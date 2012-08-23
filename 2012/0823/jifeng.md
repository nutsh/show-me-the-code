# mysql连接超时问题

## mysql自身的设置
```
connect_timeout
interactive_timeout
wait_timeout
net_read_timeout
net_write_timeout
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