## 背景
现有t是一个二维数组，尝试寻找每一行的最大值。

```
var t = [
  [154, 3654, 52345, 7765, 9878],
  [2768, 454, 67654, 845, 10456],
  [743, 239, 6546, 145, 1237, 5434, 581],
  [447, 157, 6, 242, 182]
];

for (var i = 0, l = t.length; i < l; i++) {
  var largest;
  var row = t[i];
  for (var j = 0, k = row.length; j < k; j++) {
    if (row[j] > largest || !largest) {
      largest = row[j];
    }
  }
  console.log('the largest one of ' + t[i] + ' is: ' + largest);
}
```

运行结果：

```
the largest one of 154,3654,52345,7765,9878 is: 52345
the largest one of 2768,454,67654,845,10456 is: 67654
the largest one of 743,239,6546,145,1237,5434,581 is: 67654
the largest one of 447,157,6,242,182 is: 67654
```

## 原因
每次循环中，变量largest只重新定义了，没有赋值。重新定义并不会自动给它赋初始值，所以还是保留上次循环中的值。

## 总结
无论用什么语言编程，尽量在定义了变量后都给它赋初值。