## [forwards](https://github.com/dead-horse/forwards): 流量转发模块   
测试环境没有问题，上到生产环境就挂了？   
预发环境没有流量，无法充分测试？   
需要一个工具，将线上的流量导入到预发环境进行详细的测试。如果是http服务，可以通过nginx在前面做反向代理的时候转发，而其他服务呢？   
`forwards`就是做这件事情的一个模块，它将一个TCP连接的socket服务接收到的数据转发到另一个socket服务。   
### Usage   

```js
var forwards = require('forwards');
var net = require('net');
var server = net.createServer();
server.listen(8080);
forwards.connect(server, 'localhost:8081');
```

```js
var forwards = require('forwards');
var http = require('http');
server = http.createServer();
server.listen(8080);
forwards.connect(server, 'localhost:8081');
```

### Install   

```
npm install forwards
```

