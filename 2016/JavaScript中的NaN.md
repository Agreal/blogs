JavaScript 中的 NaN
---
*2016-05-04*


1. 什么是 `NaN`
2. 什么时候会返回 NaN
3. 判断 NaN
4. 参考

## 什么是 NaN

在 MDN 的 [NaN][2] 文章中有描述：
> - 全局属性 NaN 表示 Not-A-Number 的值。
> - NaN 是一个全局对象的属性。
> - NaN 属性的初始值就是 NaN，和 Number.NaN 的值一样。
> - 在 ES5 中， NaN 属性是一个不可配置（non-configurable），不可写（non-writable）的属性。但在 ES3 中，这个属性的值是可以被更改的，但是也应该避免覆盖。
> - 通常都是在计算失败时，作为 Math 的某个方法的返回值出现的（例如：`Math.sqrt(-1)`）或者尝试将一个字符串解析成数字但失败了的时候（例如：`parseInt("blabla")`）。

### NaN 的类型

```JavaScript
typeof NaN   // "number"
```
也就是说 `NaN` 是一种特殊的 `Number` 类型值。

## 什么时候会返回 NaN

[JavaScript 权威指南][1] 中『3.1.3 JavaScript 中的算数运算』章节里有描述：
> 无穷大除以无穷大、给任意负数做开方运算 或者 算数运算符与不是数字或无法转换为数字的操作数一起使用时都将返回 `NaN`。

可分开解释为以下等情况：

- 无穷大除以无穷大
- 给任意负数做开方运算
- 算数运算符与不是数字或无法转换为数字的操作数一起使用
- 字符串解析成数字

以下结果都是 `NaN`：

```JavaScript
Infinity / Infinity;   // 无穷大除以无穷大
Math.sqrt(-1);         // 给任意负数做开方运算
'a' - 1;               // 算数运算符与不是数字或无法转换为数字的操作数一起使用
'a' * 1;
'a' / 1;
parseInt('a');         // 字符串解析成数字
parseFloat('a');
```

这里的『无法转换为数字的操作』又是什么鬼？

先看下面可以转换为数字的操作例子：

```JavaScript
Math.sqrt('4');        // 2
2 * '2';               // 4
'4' / 2;               // 2
```

### 无法转换为数字的操作
这里涉及到 JavaScript 的 **类型转换** 的概念。

[JavaScript 权威指南][1] 『3.8 类型转换』章节有描述：
> 如果 JavaScript 期望使用一个数字，它把给定的值将转换为数字（如果转换结果无意义的话将返回 NaN）。

可以使用 [`Number()`][5] 方法做显式类型转换，举例：

```JavaScript
Number('1');           // 1
Number(null);          // 0
Number('a');           // NaN
```

也可以使用一元运算符 `+` 做隐式转换，举例：

```JavaScript
+'1';                  // 1
+null;                 // 0
+'a';                  // NaN
```

也可以使用全局函数 [`parseInt()`][6] 和 [`parseFloat()`][7] 解析整数和浮点数，举例：

```JavaScript
parseInt('12');        // 12
parseInt('12a');       // 12
parseInt('a12');       // NaN
parseInt(null);        // NaN
```

`parseInt()` 和 `parseFloat()` 可以简单理解为：
> 尽可能解析更多数值字符，并且忽略后面的内容；如果第一个非空格字符是非数字，则会返回 `NaN`

需要注意的是 `Number()` 和 `parseInt()``parseFloat()` ，对某些输入值的处理不同，如 `null`。


非数字类型转换 为 数字类型，如下表汇总：

| 值 | 数字 |
|:-----------|------------:|
| undefined   | NaN |
| null        | 0   |
| true        | 1   |
| false       | 0   |
| "" (空字符串)       | 0   |
| "1.2" (非空，数字)   | 1.2 |
| "one" (非空，非数字) | NaN |
| \[\] (任意对象)     | 0 |
| \[9\] (一个数字元素)     | 9 |
| \['a'\] (其他数组)     | NaN |
| function(){} (任意函数)     | NaN |


## 如何判断 NaN

首先全局的 `isNaN()` 函数不能严格判断输入值是否为 `NaN`。

### `isNaN()` 的怪异行为
在 MDN 的 [isNaN()][4] 文章中对 **非数值参数** 所表现的『怪异行为』有解释：

> 它会先尝试将这个参数转换为数值，然后才会对转换后的结果是否是 `NaN` 进行判断。

> 因此，对于能被强制转换为有效的非 `NaN` 数值来说，返回 `false` 值也许会让人感觉莫名其妙。

如下例子：

```JavaScript
isNaN('37');      // false: 可以被转换成数值 37
isNaN('');        // false: 空字符串被转换成 0
```

推荐使用 ES5 中的 `Number.isNaN()`函数判断。

### 严格判断 NaN

参考 MDN 中 [`Number.isNaN()`][3] 的 Polyfill 代码：

```JavaScript
typeof value === 'number' && isNaN(value);
```

## 参考
- [JavaScript 权威指南][1]
- [MDN NaN][2]
- [MDN Number.isNaN][3]
- [MDN isNaN][4]
- [MDN Number][5]
- [MDN parseInt()][6]
- [MDN parseFloat()][7]

[1]: https://book.douban.com/subject/10549733/ (JavaScript 权威指南)
[2]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN (MDN NaN)
[3]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN (MDN Number.isNaN)
[4]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN (MDN isNaN)
[5]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number (MDN Number)
[6]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt (MDN parseInt)
[7]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat (MDN parseFloat)
