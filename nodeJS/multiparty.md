# 什么是multiparty？

https://www.npmjs.com/package/multiparty

## 介绍

> multiparty模块是基于node的文件上传模块，**于解析支持流的多部分表单数据请求**

## 属性

- encoding：设置传入表单域的编码。默认为utf-8（一般不用修改）
- maxFieldsSize：设置限制所有字段分配的内存数量，如果数据超过了这个值系统会触发error事件，默认值是2MB。
- maxFields：限制在触发错误事件前将被解析的字段数量.在这种情况下文件也被记录算做一个字段。默认值是1000
- maxFilesSize：仅当autoFiles字段是true时才有意义。限制所有接受的合并文件的字节数。如果超过了设置值，系统会触发一个错误事件。默认时无穷大。
- autoFields：启动字段事件和禁用部分字段事件。如果你添加了一个字段监听器这个值将自动设置为true。
- autoFiles：启动文件事件和禁用部分文件事件。如果你添加了一个文件监听器这个值自动设置为true。
- uploadDir：仅当autoFiles 是true时有意义。文件上传的位置目录。你能在使用fs.rename以后移动他们。默认为os.tmpdir().

## 方法

- form.parse(request, [cb])

  >解析一个正在到达的包含表单数据的node.js请求。这将使表单根据到达的请求去触发事件。
  >
  >如果提供了cb回调函数，autoFields和autoFiles会设置成true并且所有的字段和文件都被收集并传递到回调函数，不需要监听表单的任何事件。当你想要读每个事件都是方便的，但一定要写清除代码，因为所有的上传文件将被写入磁盘，即使你不感兴趣。

- form.bytesReceived

  > 这个字段是到目前为止接收的是表单字节数量

- form.bytesExpected

  > 这个属性是在表单中字节的预期数量

## 事件

- #### 'error' (err)

除非你为form.parse提供一个回调函数，并且你的确想去处理这个事件。否则你的服务器将在用户提交非法请求时宕机。

仅仅一个error事件就能一直的被触发，并且如果一个error事件被触发了，则close事件就不会被触发了。

如果错误符合某个http响应码，err对象将有一个带着推荐的http响应码的值的statusCode属性被发送返回来。

注：一个error事件将从form和当前的part中被触发。

- #### 'part' (part)

当一个part事件，在线程请求时被遇到了该会被触发。part 是一个ReadableStream流，它也有以下属性：

headers - part的头信息，比如content-type头字段。

name - part的字段名

filename - 如果part是正在上传的文件时的文件名

byteOffset - 在请求体中part的字节偏移量

byteCount -假如在请求中这是最后一次part，那么这个值的尺寸就是这个part的字节。你可以使用它，比如如果你上传一个文件到S3的时候设置Content-Length 头字段就能获取请求的字节数。如果part有一个Content-Length header头则这个头将被代替。

当autoFields是开启状态字段的parts事件不被触发，同样的当autoFiles是开启状态文件parts不被触发。

part触发的error事件你一定要处理他们。

- #### 'aborted'

当请求被中断时触发。这个事件后边跟了一个简短的错误事件。在实践中你不要需要去处理它。

'progress' (bytesReceived, bytesExpected)

当一片数据从表单中被接受时触发。bytesReceived参数包含了全部到目前为止从表单提交的字节数。bytesExpected参数包含总共的被知道的预期的字节数，如果不知道此值为null。

- #### 'close'

全部的parts被解析和被触发之后‘close’才会被触发。如果一个error事件被触发了close事件将不被触发。

如果你的autoFiles状态是on，直到数据全部刷新到磁盘并且文件处理已经被关闭才会被触发。

'file' (name, file)

通过默认的multiparty将不触发你的硬盘驱动。但是如果添加了这个监听事件，multiparty自动设置form.autoFiles 字段为true并且将流上传到你的磁盘。

每次请求接收的最大字节能被maxFilesSize明确。

返回数据：

name - 文件的字段名

file - 一个对象有下面几个属性:

  fieldName - 和-name一样都是文件的字段名

  originalFilename - 用户上传时的源文件名

  path - 上传文件的磁盘路径

  headers - 我们发送文件的头信息

  size - 文件的尺寸

'field' (name, value)

name - 字段名

value - 字段值

## 示例

```js
module.exports = (app) => {
    const express = require('express');
    const router = express.Router();
    const multiparty = require('multiparty')
    const { fs } = require('fs');

    // 多文件上传
    router.post('/uploadMultipart', (req, res) => {

        // 生成multipart对象
        var multipartForm = new multiparty.Form();
        // 设置文件存储路径
        // 这个路径文件夹一定一定一定要提前创建，否则不会成功，也不提示报错
        multipartForm.uploadDir = __dirname + '/../../uploadsMultipart'

        /**
         * parse 表单解析
         * fields：普通的表单数据
         * files：上传的文件信息
         * **/
        multipartForm.parse(req, (err, fields, files) => {
            try {
                console.log('25===', fields, files);
                let upFile = files.file[0]
                // 重命名
                res.send({
                    code: 200,
                    msg: '上传成功',
                    fileSize: ((upFile.size) / 1048576).toFixed(2) + 'M'
                })
            } catch {
                console.log('err===', err);
            }
        })
    })
    app.use('/admin/api', router);
}
```



## 遇到的坑

multipartForm.uploadDir = __dirname + '/../../uploadsMultipart'  

设置的uploadsMultipart`路径一定一定一定`要提前创建好，否则fields, files  都为undefined ，也不报错提示