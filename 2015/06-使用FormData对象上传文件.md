# 使用FormData对象上传文件

发布于: 2015-06-03

`FormData`对象，是可以使用一系列的键值对来模拟一个完整的表单，然后使用`XMLHttpRequest`发送这个"表单"。

在 **Mozilla Developer** 网站  [使用FormData对象](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Using_FormData_Objects) 有详尽的`FormData`对象使用说明。

但上传文件部分只有底层的`XMLHttpRequest`对象发送上传请求，那么怎么通过`jQuery`的`Ajax`上传呢？
本文将介绍通过`jQuery`使用`FormData`对象上传文件。

## 使用`<form>`表单初始化`FormData`对象方式上传文件

HTML代码

```HTML
<form id="uploadForm" enctype="multipart/form-data">
    <input id="file" type="file" name="file"/>
    <button id="upload" type="button">upload</button>
</form>
```

javascript代码

```javascript
$.ajax({
    url: '/upload',
    type: 'POST',
    cache: false,
    data: new FormData($('#uploadForm')[0]),
    processData: false,
    contentType: false
}).done(function(res) {
}).fail(function(res) {});
```

这里要注意几点：

* `processData`设置为`false`。因为`data`值是`FormData`对象，不需要对数据做处理。
* `<form>`标签添加`enctype="multipart/form-data"`属性。
* `cache`设置为`false`，上传文件不需要缓存。
* `contentType`设置为`false`。因为是由`<form>`表单构造的`FormData`对象，且已经声明了属性`enctype="multipart/form-data"`，所以这里设置为false。

上传后，服务器端代码需要使用从查询参数名为`file`获取文件输入流对象，因为`<input>`中声明的是`name="file"`。

如果不是用`<form>`表单构造`FormData`对象又该怎么做呢？

## 使用`FormData`对象添加字段方式上传文件

HTML代码

```HTML
<div id="uploadForm">
    <input id="file" type="file"/>
    <button id="upload" type="button">upload</button>
</div>
```

这里没有`<form>`标签，也没有`enctype="multipart/form-data"`属性。

javascript代码

```javascript
var formData = new FormData();
formData.append('file', $('#file')[0].files[0]);
$.ajax({
    url: '/upload',
    type: 'POST',
    cache: false,
    data: formData,
    processData: false,
    contentType: false
}).done(function(res) {
}).fail(function(res) {});
```

这里有几处不一样：

* `append()`的第二个参数应是文件对象，即`$('#file')[0].files[0]`。
* `contentType`也要设置为‘false’。

从代码`$('#file')[0].files[0]`中可以看到一个`<input type="file">`标签能够上传多个文件，
只需要在`<input type="file">`里添加`multiple`或`multiple="multiple"`属性。

## 服务器端读文件

从`Servlet 3.0` 开始，可以通过 `request.getPart()` 或 `request.getPars()` 两个接口获取上传的文件。
这里不多说，详细请参考官网教程 [Uploading Files with Java Servlet Technology](https://docs.oracle.com/javaee/7/tutorial/servlets011.htm#BABFGCHB) 以及示例 [The fileupload Example Application](https://docs.oracle.com/javaee/7/tutorial/servlets016.htm#BABDGFJJ)

### 参考

* <https://developer.mozilla.org/zh-CN/docs/Web/Guide/Using_FormData_Objects>
* <http://stackoverflow.com/questions/10292382/html-5-formdata-and-java-servlets>
* <https://docs.oracle.com/javaee/7/tutorial/servlets011.htm#BABFGCHB>
* <https://docs.oracle.com/javaee/7/tutorial/servlets016.htm#BABDGFJJ>
