String.prototype.localeCompare 的坑
---
*2015-10-31*


使用 [`gulp-inject`](https://www.npmjs.com/package/gulp-inject) 注入到页面之前，需要对文件进行排序。在使用排序方法时发现 `String.prototype.localeCompare` 的这个方法在 `Mac` 和 `Windows` 环境结果不同。

使用 `localeCompare` 测试
> [‘A', 'a', 'B', 'b'].sort(function(a, b) { return a.localeCompare(b); })
mac
[ 'A', 'B', 'a', 'b' ]
windows
[ ‘a', 'A', 'b', 'B' ]

不使用 `localeCompare` 测试
>[‘A’, ‘a’, ‘B’, 'b'].sort();
mac
[ 'A', 'B', 'a', 'b' ]
windows
[ 'A', 'B', 'a', 'b' ]

根据 [String.prototype.localeCompare() - JavaScript | MDN][MDN] 说明，`localeCompare` 方法是和操作系统 `locale` 值相关。

我们可以使用 `localeCompare` 函数的第二个参数 `locale` 的 `BCP 47 language` 扩展 `kf` 参数来确定大小写优先顺序。

但首先需要先了解什么是 `BCP 47 language tag` ，啊，好麻烦...

也可以自定义[检测字符大小写的函数][Stack Overflow]作为 `localCompare` 的比较器
```javascript
function caseInsensitiveComparator(valueA, valueB) {
    var valueALowerCase = valueA.toLowerCase();
    var valueBLowerCase = valueB.toLowerCase();

    if (valueALowerCase < valueBLowerCase) {
        return -1;
    } else if (valueALowerCase > valueBLowerCase) {
        return 1;
    } else { //valueALowerCase === valueBLowerCase
        if (valueA < valueB) {
            return -1;
        } else if (valueA > valueB) {
            return 1;
        } else {
            return 0;
        }
    }
}
```

参考：
* [String.prototype.localeCompare() - JavaScript | MDN][MDN]
* [How to perform case insensitive sorting in Javascript? - Stack Overflow][Stack Overflow]

[MDN]: https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/String/localeCompare
[Stack Overflow]: http://stackoverflow.com/questions/8996963/how-to-perform-case-insensitive-sorting-in-javascript
