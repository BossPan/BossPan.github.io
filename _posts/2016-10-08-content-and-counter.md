---
title:      CSS 生成内容
summary:    content 属性、CSS 计数器
categories: CSS
tags:       CSS
---

## 插入生成内容

结合使用:before, :after伪类与content属性可以向文档中插入生成内容。

生成内容放在元素框的内部（相当于与之关联的元素内容区），除了列表标志，无法把生成的元素放在元素框之外。（CSS2.1明确禁止插生成内容浮动或定位。）

块级元素只接受生成内容的display值为none, inline, block，所有其他值都转换为block。行内元素只接受生成内容的display值为none, inline，所有其他值都转换为inline。

生成内容从与之关联的元素中继承值。

以下是一些简单的例子。

### 插入字符串。

```css
a[href]:before {
    content: "(link) ";
}
```

注意：串值会原样显示，即使其中包含某种标记也不例外。

```
h2:before {
    content: "<em>&para;</em> ";
    color: gray;
}
```

可以使用转义指示十六进制Unicode值，如"\00AB"显示为 « 。

```css
a:before {
    content: "\00AB";
}
```

### 插入属性值。

不存在的属性将自动替换为空字符串。

```css
a[href]:after {
    content: " [" attr(href) "]";
}
```

### 生成引号。

需要结合quote属性，以下得到的效果是在blockquote的开头和结尾各插入一个引号。

```css
blockquote {
	quotes: '"' '"'; 
}
blockquote:before {
	content: open-quote;
}
blockquote:after {
	content: close-quote;
}
```

插入弯引号需要使用 Unicode 值，对应为'\201C' '201D'。

## 计数器

counter-reset 属性用于重置计数器标识符，counter-increment 属性用于将计数器递增。

## 使用计数器生成编号

### 为标题生成编号。（[Demo](http://htmlpreview.github.io/?https://github.com/BossPan/bosspan.github.io/blob/master/demo/counter.html)）

书上的代码貌似存在问题，更正如下。差别在于设置 counter-reset 的位置，应该是在父元素或者关联元素上设置。

```css
body {
    counter-reset: chapter;
}

h1 {
    counter-reset: section subsec;
}

h2 {
    counter-reset: subsec;
}

h1:before {
    counter-increment: chapter;
    content: counter(chapter) ". ";
}

h2:before {
    counter-increment: section;
    content: counter(chapter)"." counter(section) ". ";
}

h3:before {
    counter-increment: subsec;
    content: counter(chapter) "." counter(section) "." counter(subsec) ". ";
}
```

### 为列表生成编号。（[Demo](http://htmlpreview.github.io/?https://github.com/BossPan/bosspan.github.io/blob/master/demo/counter.html)）

而如果需要在列表中生成类似的序号，可以使用 counters 属性，如下便可。

```css
ol {
    list-style-type: none;
    counter-reset: ordered;
}
ol li:before {
    counter-increment: ordered;
    content: counters(ordered, ".") " - ";
}
```

## 参考目录

- `CSS权威指南 第12章 列表与生成内容`