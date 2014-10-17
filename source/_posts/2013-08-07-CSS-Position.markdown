---
layout: post
title: "CSS Position"
date: 2013-11-26 14:18
comments: true
categories: [css, position]
---

最近，遇到一个问题，不同的浏览器之间对定位展现出来的页面效果不一样，甚至同是chrome，但是window和*nix的都不一样。

比如：

```css
# css file

.cont {
  position: relative;
  top: -6px;
}
```

这是简单的相对定位代码，绝对定位就更不好说了，可能出现的问题就更多了。绝对定位更多使用在如左边的导航操作区。对于不同浏览器之间的效果差别，我在这就不好贴出来了。

通过我的试验，感觉定位基本都可以用另外的方法替代。可简单由`float`、`margin`、`padding`构成：

```
.cont {
  float: right:
  padding-right: 10px;
  ...
}
```

有时会不得不用到 margin，不过在我记忆中，好像不同的浏览器中初始值也不一样，此知识点的来源是找不到了。我潜意识里能不用则不用。

听同事说，定位比一般样式要耗资源，是不是呢！！

---

最近看到一个不同浏览器内核不同的代码，如`webkit`:

```
@media screen and (-webkit-min-device-pixel-ratio:0) {
  ...
}
```

更多不同浏览器差别及相应的语法技巧，可以转到 [browserhacks](http://browserhacks.com)，对于调试浏览器的兼容性的开发者来说是十分有益的。可恶的浏览器之争，一家独大，对于开发者来说也不见得是坏事。