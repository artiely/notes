# 题1
```
var Foo = function () {
  getName = function () {
    console.log(111)
  }
  return this
}
Foo.prototype.getName = function () {
  console.log(222)
}
var getName = function () {
  console.log(333)
}
function getName () {
  console.log(444)
}
Foo.getName = function () {
  console.log(555)
}
getName()
Foo().getName()
Foo.getName()
new Foo().getName()
```
## 分析

```
第一个getName() 只会去下面两个函数里面找
var getName = function () {
  console.log(333)
}
function getName () {
  console.log(444)
}
由于函数声明由于变量（变量提升同名的函数先提升）
所以先提升的函数被后面的变量覆盖了 结果333

第二个Foo().getName()
我的第一时间认为也是 333
因为Foo()调用返回的this是window
而我任务window下还是第一个上面的两个getName
其实Foo()调用后window下生成一个全局的
getName = function () {
    console.log(111)
}
所以结果 111
第三个Foo.getName()
乍一看如下3个都是
var Foo = function () {
  getName = function () {
    console.log(111)
  }
  return this
}
Foo.prototype.getName = function () {
  console.log(222)
}
Foo.getName = function () {
  console.log(555)
}
第一个函数Foo 结果为
function () {
  getName = function () {
    console.log(111)
  }
  return this
}
是个函数根本没法.getName
第二个 Foo.prototype.getName = function () {
  console.log(222)
}
只能通过new Foo().getName()获取
所以结果555
最后一个 222
```
# 题2 ajax的原理 手写ajax

ajax的技术核心是 XMLHttpRequest 对象；   
ajax 请求过程：创建 XMLHttpRequest 对象、连接服务器、发送请求、接收响应数据；
```
function creatXml() {
  if(XMLHttpRequest){
    return  new XMLHttpRequest()
  }else{
    throw new Error('浏览器不支持XHR对象！')
  }
}
function ajax(obj) {
  var xhr = creatXml()
  xhr.open("POST", url, false)
  xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
           console.log('数据正在加载')
            if (xhr.status == 200) {
                document.write(xhr.responseText)
            }
        }
    }
    xmlhttp.send()
}

```