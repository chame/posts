---
layout: post
title: "word2007段前段后设置"
date: 2012-08-17 21:13
comments: true
categories: [work, word2007, Paragraph]
---
微软官网上Paragraph中有：

> Public property SpaceAfter Returns or sets the amount of spacing (in points) after the specified paragraph or text column.

> Public property SpaceAfterAuto Determines if Microsoft Word automatically sets the amount of spacing after the specified paragraphs.

> Public property SpaceBefore Returns or sets the spacing (in points) before the specified paragraphs.

> Public property SpaceBeforeAuto Determines if Microsoft Word automatically sets the amount of spacing before the specified paragraphs.

实际设置时，直接设置SpaceAfter能成功，但设置SpaceBefore却不成功。搜了一下，没有相关的问题。

仔细看看了API，Paragraph有属性Format， 即ParagraphFormat接口。其中有属性：

> Public property SpaceAfter Returns or sets the amount of spacing (in points) after the specified paragraph. (Inherited from _ParagraphFormat.)

> Public property SpaceAfterAuto True if Microsoft Word automatically sets the amount of spacing after the specified paragraphs. (Inherited from _ParagraphFormat.)

> Public property SpaceBefore Returns or sets the spacing (in points) before the specified paragraphs. (Inherited from _ParagraphFormat.)

> Public property SpaceBeforeAuto True if Microsoft Word automatically sets the amount of spacing before the specified paragraphs. (Inherited from _ParagraphFormat.)

描述基本是一样的。

运行代码：

    paraInstance.Format.SpaceBefore = 24;

但运行此代码就能设置成功。 汗死 ms!!
