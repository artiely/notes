# 什么是Sticky footers布局
- 在网页设计中，Sticky footers设计是最古老和最常见的效果之一，大多数人都曾经经历过。它可以概括如下：如果页面内容不够长的时候，页脚块粘贴在视窗底部；如果内容足够长时，页脚块会被内容向下推送。

# 固定高度的实现方式
html
```
<div class="page">
  <div class="sticker">
    <div class="sticker-con"></div>
  </div>
  <div class="sfooter">@Artiely</div>
</div>


```
css
```
.sticker {
    height: auto;
    padding: 0 0 40px 0;
    min-height: 100vh;
    box-sizing: border-box;
  }
  .stickerCon {
    padding-bottom: 40px;
    box-sizing: inherit;
  }
  .sfooter {
    margin-top: -40px;
    height: 40px;
    width: 100%;
    line-height: 40px;
    position: relative;
  }
```

# Flexbox解决方案
解决这类问题，Flexbox是最完美的方案。我们只需要几行CSS代码就可以完美的实现，而且不需要一些奇怪的计算或添加额外的HTML元素。
html
```
<body>
  <div class="main"></div>
  <div class="footer"></div>
</body>
```
css
```
body{
  display:flex;
  flex-flow:colum;
  min-height:100vh;
}
.main{
  flex:1;
}
```

# 其他参考
>- [https://css-tricks.com/snippets/css/sticky-footer/](https://css-tricks.com/snippets/css/sticky-footer/)
>- https://pixelsvsbytes.com/2011/09/sticky-css-footers-the-flexible-way/](https://pixelsvsbytes.com/2011/09/sticky-css-footers-the-flexible-way/)

