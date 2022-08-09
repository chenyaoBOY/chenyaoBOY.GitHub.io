---
title: mybatis动态数据源

categories: 数据源
tags:
- datasource
- mybatis
- spring
- 读写分离

---

## 动态数据源的实现

实现 {% label AbstractRoutingDataSource %} 或者 {% label AbstractDataSource %}
前者定义好了将数据源存储到Map中以key,value形式存在，后者是父抽象类，可以自由发挥

### 原理

当系统只有一个数据库时，不读写分离的前提下，数据源只有一个。
这个时候一般是这么配置

- DataSource使用Druid/C3P0等
- 配置 {% label SqlSessionFactory %}，指定mapper的包位置,xml位置和插件interceptor等，并指定上面配置的数据源
- 配置 {% label SqlSessionTemplate %}，指定 {% label SqlSessionFactory %}
- 配置事物管理器 {% label PlatformTransactionManager %} 指定上面的DataSource


**当代码执行xxxMapper.queryXXX时，最终应该是通过SqlSessionTemplate执行的。
所以datasource在一开始的时候就已经塞进去了，属于硬编码，那多数据源是咋搞的呢？或者在哪一步实现的？**

------

一开始这么想其实还是没有弄明白，框架是如何使用数据源的。实际上DataSource是一个接口，要提供Connection也就是与数据库的连接。
所以我们在实现getConnection的时候就可以做一些策略，返回不同的Connection

```java
public interface DataSource  extends CommonDataSource, Wrapper {
  Connection getConnection() throws SQLException;
  Connection getConnection(String username, String password) throws SQLException;
}
```
我们实现了 {% label AbstractRoutingDataSource %}，虽然在声明数据源的时候仅声明了一个DataSource，
但是在获取连接的时候根据存储在ThreadLocal中的策略数据源key，可以选择指定的Datasource返回Connection。



**AbstractRoutingDataSource的实现逻辑**
```java
public abstract class AbstractRoutingDataSource extends AbstractDataSource implements InitializingBean {
    private Map<Object, Object> targetDataSources;//目标数据源，需要在声明DataSource的时候 指定
    /**
     * 1 获取连接
     */
    @Override
    public Connection getConnection() throws SQLException {
        return determineTargetDataSource().getConnection();
    }
    /**
     * 2 获取数据源 子类实现determineCurrentLookupKey方法
     * @return
     */
    protected DataSource determineTargetDataSource() {
        Object lookupKey = this.determineCurrentLookupKey();
        DataSource dataSource = (DataSource)this.resolvedDataSources.get(lookupKey);
        if (dataSource == null && (this.lenientFallback || lookupKey == null)) {
            dataSource = this.resolvedDefaultDataSource;
        }
    }

    /**
     * 3 根据数据源key返回
     */
    protected abstract Object determineCurrentLookupKey();
}
```

**DynamicDatasource继承AbstractRoutingDataSource，关于取key的逻辑 可以自由发挥**
```java
public class DynamicDatasource extends AbstractRoutingDataSource {
    private ThreadLocal<String> threadLocal = ThreadLocal.withInitial(() -> "");
    @Override
    protected Object determineCurrentLookupKey() {
        return threadLocal.get();//逻辑自己实现
    }
}
```

**声明数据源**
```java
@Configuration
public class DynamicDatasourceConfig {
    @Bean
    public DataSource dataSource(){
        DynamicDatasource dd = new DynamicDatasource();
        dd.setTargetDataSources(new HashMap<>());//注入多数据源
        return dd;
    }
}
```


明白了数据源的使用，然后再去定义注解，注解上使用哪个数据源 方法就用哪个数据源

然后再去定义切面就好了，大致的实现原理就这样。

切面的作用是为了确定该使用哪个数据源的key


------

所以要实现读写分离，不论是主库还是从库的数据源都要配置好，根据策略判定是读主库还是从库

一开始我想着只配置主库，从库不配，交给dbproxy处理，但是不太合适，因为这中间没有拼装SQL，所有的切面都是为了确定是用哪个datasouce


