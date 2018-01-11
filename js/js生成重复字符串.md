需求：
我司对其客户公司有设备维修记录文档。特定为该公司维修工程师有权查看文档全部信息 其他工程可查看不敏感信息学习操作流程
因此对文本私密信息（无权限查看的）转义成符号
如：一篇内部技术文档中的账号和密码
```
function repeatStr(str,len){
   return new Array(len+1).join(str)
 }
// repeatStr('*',10)
// "*********"
```
在python你直接 print('*'*10)