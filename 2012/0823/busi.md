# C++ addon中buffer传递到js中   
* Code: 
传递非`utf8`或者`ascii`编码的buffer  
```c++
//
#include <node.h>
#include <v8.h>
#include <node_buffer.h>
#include <iostream>
using namespace std;
using namespace v8;
using namespace node;

Handle<Value> test(const Arguments& args) {
  HandleScope scope;
  char a[3] = {13, 1, 1, 0, 0, -123};
  //return scope.Close(Buffer::New(String::New(a))); //会被第四个字符0截断
  //return scope.Close(Buffer::New(String::New(a, 6))); //无法解析成utf-8或者ascii，转换错误
  return scope.Close(Buffer::New(a, 6)->handle_);
}
```
两种传递buffer的方法对比:  
```c++
#include <node.h>
#include <v8.h>
#include <node_buffer.h>
#include <iostream>
using namespace std;
using namespace v8;
using namespace node;

Handle<Value> slowBuffer(const Arguments& args) {
  HandleScope scope;
  char a[100] = {-2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111
                };
  Buffer *buf = Buffer::New(a, 100);
  return scope.Close(buf->handle_);
}

Handle<Value> array(const Arguments& args) {
  HandleScope scope;
  char a[100] = {-2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111,
                -2, 0, 1, 2, 0, 0, 0, 0, -127, 32, 0, 0, 0, -52, 77, 116, 0, 42, 99, 111
                };
    Handle<Array> bufArray = v8::Array::New(100);
  for(unsigned i=0; i!=100; ++i) {
    bufArray->Set(i, v8::Integer::New((short)a[i]));
  }
  return scope.Close(bufArray);
}
void init(Handle<Object> target) {
  target->Set(String::NewSymbol("slowBuffer"),
  FunctionTemplate::New(slowBuffer)->GetFunction());
  target->Set(String::NewSymbol("array"),
  FunctionTemplate::New(array)->GetFunction());
}
NODE_MODULE(buf, init)
```
```js
var bufTest = require('./build/Release/buf');

console.time('slowBuffer');
for (var i = 0 ; i < 100000; i++) {
  bufTest.slowBuffer();
}
console.timeEnd('slowBuffer'); //200+ms


console.time('array');
for (var i = 0 ; i < 100000; i++) {
  new Buffer(bufTest.array());
}
console.timeEnd('array');  //1000+ms
```

 1. new v8::String的时候如果没有传递长度，会调用`strlen`获取长度，导致被`\0`截断。   
 2. 需要注意buffer的编码形式，如果不是`utf8`或者`ascii`字符编码，则不可以通过`Stirng`的方式传递。   