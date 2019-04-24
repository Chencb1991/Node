# Node

> 启动

```
var express = require('express');
var app = new express();
//var bodyParser = require('body-parser')
// parse application/x-www-form-urlencoded
//app.use(bodyParser.urlencoded({ extended: false }))
 
// parse application/json
//app.use(bodyParser.json());

app.all('*', function(req, res, next) {//允许请求跨域访问
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8")
    next();
});

app.get('/',function(req,res){
	res.send('hello');
	//res.send(req.route);
});
app.get('/1',function(req,res){
	res.send(req.query)
});
app.get('/2',function(req,res){
	res.send('hello word2')
});
app.listen('8088',function(){
	console.log('app is listen on 8088');
})


```
> 爬虫抓取cdn

 ```
 var express = require('express');
var superagent = require('superagent');
var cheerio = require('cheerio');
var app = express();

app.get('/', function (req, res, next) {
  // 用 superagent 去抓取 https://cnodejs.org/ 的内容
  superagent.get('https://www.bootcdn.cn/jquery/')
    .end(function (err, sres) {
      // 常规的错误处理
      if (err) {
        return next(err);
      }
      // sres.text 里面存储着网页的 html 内容，将它传给 cheerio.load 之后
      // 就可以得到一个实现了 jquery 接口的变量，我们习惯性地将它命名为 `$`
      // 剩下就都是 jquery 的内容了
      var $ = cheerio.load(sres.text);
      var items = [];
      $('.container  span').each(function (idx, element) {
        var $element = $(element);
        items.push({
          title: $element.html(),
          //href: $element.attr('src')
        });
      });

      res.send(items);
    });
});

app.listen('8088',function(req,res){

})
```





