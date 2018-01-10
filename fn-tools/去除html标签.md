
```
function delHtmlTag(str) {
  return str.replace(/<[^>]+>/g, '') // 去掉所有的html标记
}
```