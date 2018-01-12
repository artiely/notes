

# 一个在线正则表达式验证网站
http://regex.zjmainstay.cn/

## 去除回车换行
```
function getText(str) {
  var resultStr = str.replace(/\ +/g, "") // 去掉空格
  resultStr = str.replace(/[ ]/g, "")    // 去掉空格
  resultStr = str.replace(/[\r\n]/g, "") // 去掉回车换行
  return resultStr
}
```
## 去掉html 标签
```
function delHtmlTag(str) {
  return str.replace(/<[^>]+>/g, '') // 去掉所有的html标记
}
```

## 去掉html 特定属性
如 `<table width="500"></table>` 中的`width=500`
```
str.replace(/\s+width="[^"]*"/ig,'')
```