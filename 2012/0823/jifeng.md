# mysql连接超时问题

## mysql自身的设置
```
connect_timeout #获取连接阶段（authenticate）起作用
interactive_timeout
wait_timeout
```
有兴趣可看这篇文章：http://www.taobaodba.com/html/433_mysql_timeout_analyze.html

## 代码级别控制
``` 
var queryTimer = setTimeout(function() {
  queryTimer =  null;
  var err = new Error('Query Timeout :' + QUERY_TIMEOUT);
  process.nextTick(function() {
    cb(err);
  }); 
}, QUERY_TIMEOUT);

mysql.query(sql, function(){
  //only callback when not expired
  if (queryTimer) {
    clearTimeout(queryTimer);
    queryTimer =  null;
    var args = arguments;
    process.nextTick(function(){
      cb.apply(null, args);
    });
  }
});
```

问题：虽然这次访问可以按超时返回，但下次请求过来依然走这条连接，加重这条连接的负担

现在的解决方案：当超时断开这条连接 + 连接池 