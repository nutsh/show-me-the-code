# 魔方前端性能优化的几点实践
1、优先对数据做处理，而不是对节点处理。
  例如魔方之前使用的table组件，dataTables的实现是数据不处理，添加到dom树上，sort、pager等是对获取的dom节点进行操作。
  现有的实现是基于获取的数据，根据table组件的配置，对数据进行sort、pager，最后进行一次页面的渲染。
2、浏览器降级处理
  善用css 伪类，hover效果。圆角支持等
3、组件的销毁
  事件解绑，unbind，undelegate等等，慎用mouseover事件
  变量销毁，null or delete区别，删除属性的唯一方法是使用 delete 操作符；设置属性为 undefined 或者null 并不能真正的删除属性， 而仅仅是移除了属性和值的关联
```js
var obj = {
    bar: 1,
    foo: 2,
    baz: 3
};
obj.bar = undefined;
obj.foo = null;
delete obj.baz;

for(var i in obj) {
  if (obj.hasOwnProperty(i)) {
    console.log(i, '' + obj[i]);
  }
}
//result
//bar undefined
//foo null
```