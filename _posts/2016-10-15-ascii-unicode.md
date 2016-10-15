---
title:      字符编码
summary:    常见的字符编码
categories: Computer
tags:       Computer 计算机基础知识
---

## 字符编码

在计算机内部，所有的信息最终都表示为一个二进制的字符串。而各类语言的字符集（比如英文字母、汉字等）在计算机中具体对应哪一个字符串，需要我们自己去定义，我们定义字符集与二进制字符串的对应关系的过程，就是对字符进行编码的过程，而这个对应关系就是字符编码。

## ASCII 码

ASCII = American Standard Code for Information Interchange（美国信息交换标准代码）。ASCII码用单个字节共8位来表示字符，其中最高位为0，ASCII一共包含 2<sup>7</sup> = 128 个字符的编码。其中 33 个字符是无法显示的控制字符，控制字符的用途主要是用来操控已经处理过的文字。在 33 个字符之外的是 95 个可显示的字符，包含用键盘敲下空白键所产生的空白字符也算 1 个可显示字符（显示为空白）。

ASCII 码对英语来说是足够了，但是对于其他语言，显然是不够的。

## Unicode

Unicode（中文：万国码、国际码、统一码、单一码）是计算机科学领域里的一项业界标准。它对世界上大部分的文字系统进行了整理、编码，使得电脑可以用更为简单的方式来呈现和处理文字。

Unicode 的编码方式与 ISO 10646 的通用字符集概念相对应。目前实际应用的统一码版本对应于 UCS-2，使用 16 位的编码空间，也就是每个字符占用2个字节。这样理论上一共最多可以表示 216（即65536）个字符。基本满足各种语言的使用。实际上当前版本的统一码并未完全使用这16位编码，而是保留了大量空间以作为特殊使用或将来扩展。

Unicode 的实现方式不同于编码方式。一个字符的 Unicode 编码是确定的。但是在实际传输过程中，由于不同系统平台的设计不一定一致，以及出于节省空间的目的，对 Unicode 编码的实现方式有所不同。Unicode 的实现方式称为 Unicode 转换格式（Unicode Transformation Format，简称为 UTF）

## UTF-8

UTF-8是Unicode的实现方式之一。

UTF-8（8-bit Unicode Transformation Format）是一种针对 Unicode 的可变长度字符编码，也是一种前缀码。它可以用来表示 Unicode 标准中的任何字符，且其编码中的第一个字节仍与 ASCII 兼容。

UTF-8使用一至六个字节为每个字符编码（尽管如此，2003年11月UTF-8被RFC 3629重新规范，只能使用原来Unicode定义的区域，U+0000到U+10FFFF，也就是说最多四个字节）

1. 128个 US-ASCII 字符只需一个字节编码（Unicode 范围由 U+0000 至 U+007F）。
2. 带有附加符号的拉丁文、希腊文、西里尔字母、亚美尼亚语、希伯来文、阿拉伯文、叙利亚文及它拿字母则需要两个字节编码（Unicode 范围由 U+0080 至 U+07FF）。
3. 其他基本多文种平面（BMP）中的字符（这包含了大部分常用字，如大部分的汉字）使用三个字节编码（Unicode 范围由 U+0800 至 U+FFFF）。
4. 其他极少使用的 Unicode 辅助平面的字符使用四至六字节编码（Unicode 范围由 U+10000 至 U+1FFFFF 使用四字节，Unicode 范围由 U+200000 至 U+3FFFFFF 使用五字节，Unicode范围由 U+4000000 至 U+7FFFFFFF 使用六字节）。

## 参考目录

- [字符编码详解](http://www.crifan.com/files/doc/docbook/char_encoding/release/html/char_encoding.html)
- [ASCII](https://zh.wikipedia.org/wiki/ASCII) 、[Unicode](https://zh.wikipedia.org/wiki/Unicode) 、[UTF-8](https://zh.wikipedia.org/wiki/UTF-8)
- [字符编码笔记：ASCII，Unicode和UTF-8](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)