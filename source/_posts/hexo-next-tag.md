---
title: Hexo-Next Tag 插件的使用
author: Ackerman
date: 2021-12-08 12:40:34
categories:
- [Hexo, Next]
---

> 标签插件和 Front-matter 中的标签不同，它们是用于在文章中快速插入特定内容的插件。
>
> 虽然你可以使用任何格式书写你的文章，但是标签插件永远可用，且语法也都是一致的。

### 文本居中引用 -Centered Quote

#### 使用方法

```
{% cq %}No pain, No gain{% endcq %}
```

#### 显示效果

{% cq %} No pain, No gain{% endcq %}

<!-- more -->

### 提示块-Note

#### 使用方法

```
{% note default %}
This is default note
{% endnote %}

{% note primary %}
This is primary note
{% endnote %}

{% note success %}
This is success note
{% endnote %}

{% note info %}
This is info note
{% endnote %}

{% note warning %}
This is warning note
{% endnote %}

{% note danger %}
This is danger note
{% endnote %}
```

#### 显示效果

{% note default %}
This is default note
{% endnote %}

{% note primary %}
This is primary note
{% endnote %}

{% note success %}
This is success note
{% endnote %}

{% note info %}
This is info note
{% endnote %}

{% note warning %}
This is warning note
{% endnote %}

{% note danger %}
This is danger note
{% endnote %}

### 标签-Label

#### 使用方法

```
{% label default@默认 %} {% label primary@主要 %} {% label success@成功 %} {% label info@信息 %} {% label warning@警告 %} {% label danger@危险 %}
```

#### 展示效果

{% label default@默认 %} {% label primary@主要 %} {% label success@成功 %} {% label info@信息 %} {% label warning@警告 %} {% label danger@危险 %}



### 选项卡-Tabs

#### 使用方法

```
{% tabs Unique name, [index] %}
<!-- tab [Tab caption] [@icon] -->
Any content (support inline tags too).
<!-- endtab -->
{% endtabs %}

Unique name   : Unique name of tabs block tag without comma.
                Will be used in #id's as prefix for each tab with their index numbers.
                If there are whitespaces in name, for generate #id all whitespaces will replaced by dashes.
                Only for current url of post/page must be unique!

[index]       : Index number of active tab. # 默认显示哪个tab，如果不指定，默认是第一个，-1的话则不显示
                If not specified, first tab (1) will be selected.
                If index is -1, no tab will be selected. It's will be something like spoiler.
                Optional parameter.

[Tab caption] : Caption of current tab. 当前tag标题
                If not caption specified, unique name with tab index suffix will be used as caption of tab.
                If not caption specified, but specified icon, caption will empty.
                Optional parameter.

[@icon]       : FontAwesome icon name (without 'fa-' at the begining). fontAwesome图标
                Can be specified with or without space; e.g. 'Tab caption @icon' similar to 'Tab caption@icon'.
                Optional parameter.
```

#### Demo

```
{% tabs 选项卡💕,2 %} 生成的tab名称为“选项卡💕”+序号
<!-- tab -->
我的名字叫选项卡1
<!-- endtab -->

<!-- tab -->
我的名字叫选项卡2，我默认展示
<!-- endtab -->

<!-- tab Ackerman-->
我的名字叫 Ackerman 
<!-- endtab -->
{% endtabs %}
```

#### 显示效果

{% tabs 选项卡💕, 2 %} 生成的tab名称为“选项卡💕”+序号
<!-- tab -->
我的名字叫选项卡1
<!-- endtab -->

<!-- tab -->
我的名字叫选项卡2，我默认展示
<!-- endtab -->

<!-- tab Ackerman-->
我的名字叫 Ackerman 
<!-- endtab -->
{% endtabs %}



### 按钮-Button

#### 使用方法

```
{% button url, text, icon [class], [title] %}
```

- `url`: 绝对或相对 url，锚点也可以
- `text`: 按钮文字，如果未指定 icon 则为必须
- `icon`: FontAwesome 图标名称（开头没有"fa-"），如果未指定 text 则为必须
- `[class]` : FontAwesome 类：`fa-fw | fa-lg | fa-2x | fa-3x | fa-4x | fa-5X` ，可选参数
- `[title]`: 鼠标悬停时的提示，可选参数

#### Demo

```
{% button #按钮-Button, 我是一个按钮, bug fa-fw, 我是一个showtooltip %}
{% button #按钮-Button, book, book fa-lg fa-fw, 我是一个showtooltip %}
```

#### 显示效果

{% button #按钮-Button, 我是一个按钮, bug fa-fw, 我是一个showtooltip %}

{% button #按钮-Button, book, book fa-lg fa-fw, 我是一个showtooltip %}

