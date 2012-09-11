# 使用 supertest 辅助http相关的单元测试

之前，我们一致使用类似 [test/support/http.js](https://github.com/fengmk2/urlrouter/blob/00efe57544b9ad0a6a70e741cbceb021804d8e1e/test/support/http.js) 这个简单的辅助库来进行http单元测试。

但是，这个库的api和使用方式都不够简单和统一，通常需要写很多should代码。

## 现状

如判断一个 `json api: /foo` 是否返回状态`200`，并且结果是 `{success: true, count: 100}`

我们会编写一下代码

```js
require('./support/http');
var app = require('../');

before(function (done) {
  app.listen(0, done);
});

it('should /foo return 200 and json data', function (done) {
  app.request()
  .get('/foo')
  .end(function (res) {
    res.should.status(200);
    res.headers['content-type'].should.include('application/json');
    JSON.parse(res.body).should.eql({
      success: true,
      count: 100
    });
  });
});
```

基本步骤: 

1. app.listen() 成功后才能请求
1. 必须对res进行各种should判断
1. json必须手工反序列化，然后判断结果

## 有了[supertest](https://github.com/visionmedia/supertest)

一切变得简单！结果判断使用统一的`expect()`方法处理。

将上面的用例重构:

```bash
$ npm install supertest
```

```js
var request = require('supertest');
var app = require('../');

it('should /foo return 200 and json data', function (done) {
  request(app)
  .get('/foo') // 请求接口保持不变
  .expect(200); // 能给判断状态码
  .expect('Content-Type', /^application\/json/) // 直接使用正则表达式判断
  .expect({
    success: true,
    count: 100
  }, done);
});
```

一切都是很正规的，即使不懂代码的同学，也基本能看懂。

产品经理应该很难看懂 `JSON.parse(data)` 的吧？哈哈
