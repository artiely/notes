# mongoose多条件模糊查询

这是今天手头项目中遇到的一个问题,关于mongoose如何实现类似于SQL中 nick LIKE '%keyword%' or email LIKE '%keyword%' 这种多条件模糊搜索的问题。 查阅了mongoose文档才得以实现,特此记录一下。

[mongodb文档](https://docs.mongodb.com/manual/reference/operator/query/regex/)

[mongoose文档](http://www.nodeclass.com/api/mongoose.html#index_Mongoose-Document)

主要用到了query.$or和query.$regex这两个find参数。

其中query.$or用于实现多条件查询，其值是一个数组。[相关文档](http://www.nodeclass.com/api/mongoose.html#query_Query-or) 

示例代码：
```
query.or([{ color: 'red' }, { status: 'emergency' }])
```
query.$regex用于实现模糊查询。[相关文档](http://www.nodeclass.com/api/mongoose.html#query_Query-regex)

示例代码:

```
{ <field>: { $regex: /pattern/, $options: '<options>' } }
{ <field>: { $regex: 'pattern', $options: '<options>' } }
{ <field>: { $regex: /pattern/<options> } }
```
通过以上两个参数就可以实现多条件模糊查询了。以User表为例,通过输入一个关键字,来匹配昵称或者邮箱与关键字相近的记录。

示例代码:
```
const keyword = this.params.keyword //从URL中传来的 keyword参数
const reg = new RegExp(keyword, 'i') //不区分大小写
const result = yield User.find(
    {
        $or : [ //多条件，数组
            {nick : {$regex : reg}},
            {email : {$regex : reg}}
        ]
    },
    {
        password : 0 // 返回结果不包含密码字段
    },
    {
        sort : { _id : -1 },// 按照 _id倒序排列
        limit : 100 // 查询100条
    }
)
```
 实例代码
 ```
 var local = require('./models/local')

 app.get('/local/repeat', function (req, res) {
 var keyword = req.query.keyword // 获取查询的字段

 var _filter={
    $or: [  // 多字段同时匹配
      {cn: {$regex: keyword}},
      {key: {$regex: keyword, $options: '$i'}}, //  $options: '$i' 忽略大小写
      {en: {$regex: keyword, $options: '$i'}}
    ]
  }
  var count = 0
  local.count(_filter, function (err, doc) { // 查询总条数（用于分页）
    if (err) {
      console.log(err)
    } else {
      count = doc
    }
  })

  local.find(_filter).limit(10) // 最多显示10条
    .sort({'_id': -1}) // 倒序
    .exec(function (err, doc) { // 回调
      if (err) {
        console.log(err)
      } else {
        res.json({code: 0, data: doc, count: count})
      }
    })
})
```
local.js
```
var mongoose = require('./db.js'),
  Schema = mongoose.Schema;

var LocalSchema = new Schema({
  key : { type: String },                    //变量
  en: {type: String},                        //英文
  cn: {type: String},                        //中文
  tn : { type: String}                       //繁体
});

module.exports = mongoose.model('Local',LocalSchema);
```
db.js
```
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost/test');
var db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function (callback) {
  // yay!
});
module.exports = mongoose;
```