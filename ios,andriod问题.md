# ios 下定位的层如果有表单，在输入时键盘弹起，表单的光标不会跟随（系统bug）
  解决办法：有表单数据尽量在页面完成，不用类似popup的浮层

# ios 下滑动不平滑
```
html,
body {
  -webkit-overflow-scrolling: touch;
}
```
# 文字超出隐藏
```
// 文字超出两行隐藏
.textover2{
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
  line-height:1.5;
}
// 文字超出一行隐藏
.textover1{
  overflow: hidden;
  text-overflow:ellipsis;
  white-space: nowrap;
}
```
# 请浮动
```
// 清浮动
.clearfix{
  &:after{
    content:'';
    height: 0;
    display: block;
    overflow: hidden;
    clear: both;
  }
}
```