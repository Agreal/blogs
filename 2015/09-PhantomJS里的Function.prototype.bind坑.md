# PhantomJS 里的 Function.prototype.bind 坑

发布于: 2015-09-09

在用 PhantomJS 运行 `stdfn.CHAR = String.fromCharCode.bind(String);` 时报错：
`TypeError: 'undefined' is not a function (evaluating 'String.fromCharCode.bind(String)')`
经 google 一番发现文章 [Function.prototype.bind is undefined #10522 ](https://github.com/ariya/phantomjs/issues/10522)

原因：PhantomJS 调用 `bind` 返回 `undefined`

解决：[Add Shim for Function.prototype.bind()](https://github.com/ariya/phantomjs/issues/10522#issuecomment-16125137)，参考如下 [MDN 的代码](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#Polyfill)

```Javascript
if (!Function.prototype.bind) {
  Function.prototype.bind = function(oThis) {
    if (typeof this !== 'function') {
      // closest thing possible to the ECMAScript 5
      // internal IsCallable function
      throw new TypeError('Function.prototype.bind - what is trying to be bound is not callable');
    }

    var aArgs   = Array.prototype.slice.call(arguments, 1),
        fToBind = this,
        fNOP    = function() {},
        fBound  = function() {
          return fToBind.apply(this instanceof fNOP
                 ? this
                 : oThis,
                 aArgs.concat(Array.prototype.slice.call(arguments)));
        };

    if (this.prototype) {
      // native functions don't have a prototype
      fNOP.prototype = this.prototype;
    }
    fBound.prototype = new fNOP();

    return fBound;
  };
}
```

不得不吐槽，PhantomJS 的 bind 实现方法居然都是错误的，ES5都多少年了，还不 fix。
