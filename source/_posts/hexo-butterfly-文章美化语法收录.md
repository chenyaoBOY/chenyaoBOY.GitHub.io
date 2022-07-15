---
title: hexo-butterfly-文章美化语法收录
keywords: 教程
date:

categories: 
- 教程
tags:
- API使用
- 语法
- hexo

---


**这里的效果是针对hexo框架并且主题是{% label butterfly %}**

-----

### node 标签
语法：
```text
{% note [class]  %}
你的文本
{% endnote %}
```
支持的 class 种类包括 default primary success info warning danger
例如：
```text
{% note success  %}
oh body! How excellent you are with the same nine years education.
{% endnote %}
```

**效果展示(该效果可能被butterfly主题美化了)**
{% note warning %}
warning 
{% endnote %}


{% note danger %}
danger
{% endnote %}


{% note info %}
info
{% endnote %}


{% note success %}
success
{% endnote %}


{% note primary %}
primary
{% endnote %}


### label 标签 
通过 label 标签可以为文字添加背景色，语法如下：
```text
语法：同九年, {% label 汝何秀 %}
```
**效果：**
同九年, {% label 汝何秀 %}

备注：应该可以选定颜色的 但是不知道咋搞

### 按钮

```text
语法：{% btn #, 按钮 %}
```
效果：
{% btn #, 按钮 %}


### tab标签
语法：
```text
{% tabs Tab标签列表 %}
  <!-- tab 标签页1 -->
    标签页1文本内容
  <!-- endtab -->
  <!-- tab 标签页2 -->
    标签页2文本内容
  <!-- endtab -->
  <!-- tab 标签页3 -->
    标签页3文本内容
  <!-- endtab -->
{% endtabs %}
```
效果：
{% tabs Tab标签列表 %}
  <!-- tab 标签页1 -->
    标签页1文本内容
  <!-- endtab -->
  <!-- tab 标签页2 -->
    标签页2文本内容
  <!-- endtab -->
  <!-- tab 标签页3 -->
    标签页3文本内容
  <!-- endtab -->
{% endtabs %}