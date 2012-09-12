ECMA 没有规定`Array.prototype.sort`的具体实现 ，对稳定性也没有作要求。

```
The sort is not required to be "stable". 
Elements which are equal don't have to remain in the same order.
```
详见：[http://qfox.nl/ecma/273](http://qfox.nl/ecma/273)

V8 中的`Array.prototype.sort`有一个小小的优化：在数组中数据量较小（不大于10个）时，使用稳定的插入排序；数据量较大时使用快排。

详见：[https://github.com/joyent/node/blob/master/deps/v8/src/array.js](https://github.com/joyent/node/blob/master/deps/v8/src/array.js) 第782行

进而考虑各个浏览器的 js 引擎的情况，可以使用如下代码：

```
var o = [
 { name: 'a', key: 1 },
 { name: 'b', key: 1 },
 { name: 'c', key: 1 },
 { name: 'd', key: 1 },
 { name: 'e', key: 1 },
 { name: 'f', key: 1 },
 { name: 'g', key: 1 },
 { name: 'h', key: 1 },
 { name: 'i', key: 1 },
 { name: 'j', key: 1 },
 { name: 'k', key: 1 },
 { name: 'l', key: 1 },
 { name: 'm', key: 1 },
 { name: 'n', key: 1 },
];
var s = o.sort(function (a, b) {
  return a.key - b.key;
});
for (var i = 0, l = s.length; i < l; i++) {
  document.write(s[i].name + ', ' + s[i].key);
  document.write('<br />');
}
```

得到的结果：

* IE 6 7 8 稳定
* Firefox < 3 不稳定
* Firefox >= 3 稳定，在FF3中使用了稳定的归并排序：[https://bugzilla.mozilla.org/show_bug.cgi?id=224128](https://bugzilla.mozilla.org/show_bug.cgi?id=224128)
* Chrome All 不稳定
* Safari 6 稳定