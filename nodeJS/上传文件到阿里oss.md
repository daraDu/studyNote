# z上传文件到阿里oss

> [官方文档][https://help.aliyun.com/document_detail/111267.html]

项目中一般会遇到上传图片或者文件到oss上，本次采用阿里云的oss，上传方式有两种



## client.put

client.put(name, file[, options])，put接口将（本地路径、Buffer、ReadStream）、（File、Blob只支持在浏览器端）上传到OSS

- [git文档][https://github.com/ali-sdk/ali-oss?spm=a2c4g.11186623.0.0.10843b126HGLIT#object-operations]
 - name ： 上传到oss的路径名称
 - file： {String|Buffer|ReadStream|File(only support Browser)|Blob(only support Browser)} object local path, content buffer or ReadStream content instance use in Node, Blob and html5 File 

## 流式上传：client..putStream

client..putStream(name, stream[, options])，添加一个流文件到oss上

- [git文档][https://github.com/ali-sdk/ali-oss?spm=a2c4g.11186623.0.0.520e2258VheH6u#putstreamname-stream-options]

- name ： 上传到oss的路径名称
- stream： {ReadStream} object ReadStream content instance

## 第一种方案

前端先将文件上传给后端，后端先将文件存储到服务器上，然后再将服务器中的文件上传到oss上，然后删除刚才的文件。这种方法只适用于较小的文件，因为大文件会占用服务器内存。不推荐

这里的案例直接用之前大文件切片上传的代码，具体参考：<a name="大文件切片上传" href="./大文件切片上传.md">大文件切片上传</a>

主要就是在merge方法里调用： 

node端：

```js
module.exports = (app) => {
    const express = require('express');
    const router = express.Router();
    const multiparty = require('multiparty')
    const fs = require('fs')
    const fse = require('fs-extra')
    const path = require('path')
    const OSS = require('ali-oss');

    const upload_dir = __dirname + '/../../uploadsMultipart/slice'


    const headers = {
        // 指定Object的存储类型。
        'x-oss-storage-class': 'Standard',
        // 指定Object的访问权限。
        'x-oss-object-acl': 'private',
        // 设置Object的标签，可同时设置多个标签。
        'x-oss-tagging': 'Tag1=1&Tag2=2',
        // 指定PutObject操作时是否覆盖同名目标Object。此处设置为true，表示禁止覆盖同名Object。
        'x-oss-forbid-overwrite': 'true',
    };
    const client = new OSS({
        // yourRegion填写Bucket所在地域。以华东1（杭州）为例，Region填写为oss-cn-hangzhou。
        region: 'yourregion',
        // 阿里云账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM用户进行API访问或日常运维，请登录RAM控制台创建RAM用户。
        accessKeyId: 'yourAccessKeyId',
        accessKeySecret: 'yourAccessKeySecret',
        // 填写Bucket名称。
        bucket: 'examplebucket'
    })
    router.post('/uploadSlice', (req, res) => {
        const form = new multiparty.Form({ uploadDir: upload_dir })
        form.parse(req)
        form.on('file', async (name, chunk) => {
            console.log(chunk);
            // 存放切片的目录 
            let chunkDir = `${upload_dir}/${chunk.originalFilename.split('.')[0]}`
            if (!fse.existsSync(chunkDir)) {
                await fse.mkdirs(chunkDir)
            }
            // 原文件名
            let dPath = path.join(chunkDir, chunk.originalFilename.split('.')[1])
            // 移动文件
            await fse.move(chunk.path, dPath, { overwrite: true })
            res.send('文件上传成功')
        })
    })
    router.post('/merge', async (req, res) => {
        let name = req.body.name
        let fname = name.split('.')[0]
        let chunkDir = path.join(upload_dir, fname)
        let chunks = await fse.readdir(chunkDir)
        chunks.sort((a, b) => a - b).map(chunkPath => {
            fs.appendFileSync(
                path.join(upload_dir, name),
                fs.readFileSync(`${chunkDir}/${chunkPath}`)
            )
        })
        fse.removeSync(chunkDir)
        const pathFile = upload_dir + '/' + name
        const result = await client.put(name, path.normalize(pathFile) // 此处上传到oss-----------------
            // 自定义headers
            , { headers }
        );
        console.log('result', result);
        res.send({
            msg: '合并成功',
            url: `http://localhost:9000/uploadsMultipart/slice/${name}`
        })
    })
    app.use('/admin/api', router);
}
```



## 第二种方案

前端将文件传给后端，后端直接将**流或者buffer**推送到oss上。推荐，不会占用服务端的内存

```js
module.exports = (app) => {
    const express = require('express');
    const fs = require('fs');
    const router = express.Router();
    const OSS = require('ali-oss');
    const multer = require('multer')
    
    // 将buffer转为stream流
    let Duplex = require('stream').Duplex;
    function bufferToStream(buffer) {
        let stream = new Duplex();
        stream.push(buffer);
        stream.push(null);
        return stream;
    }
    const headers = {
        // 指定Object的存储类型。
        'x-oss-storage-class': 'Standard',
        // 指定Object的访问权限。
        'x-oss-object-acl': 'private',
        // 设置Object的标签，可同时设置多个标签。
        'x-oss-tagging': 'Tag1=1&Tag2=2',
        // 指定PutObject操作时是否覆盖同名目标Object。此处设置为true，表示禁止覆盖同名Object。
        'x-oss-forbid-overwrite': 'true',
    };
    const client = new OSS({
        // yourRegion填写Bucket所在地域。以华东1（杭州）为例，Region填写为oss-cn-hangzhou。
        region: 'yourregion',
        // 阿里云账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM用户进行API访问或日常运维，请登录RAM控制台创建RAM用户。
        accessKeyId: 'yourAccessKeyId',
        accessKeySecret: 'yourAccessKeySecret',
        // 填写Bucket名称。
        bucket: 'examplebucket'
    })
    // 不保存在本地
    const storage = multer.memoryStorage()
    const upload = multer({ storage }) // 这段代码设置文件不在本地保存
    const month = new Date().getMonth() + 1
    // 以年月日作为上传文件夹的名称
    let ossPath = new Date().getFullYear() + '-' + month + '-' + new Date().getDate()
    router.post('/uploadOss', upload.single('file'), async (req, res) => {
        // 使用put方法上传buffer
        let result = await client.put('ossdemo/' + ossPath + '/' + req.file.originalname, req.file.buffer);
        //使用putStream上传流到oss
        // let result = await client.putStream('ossdemo/' + req.file.originalname, bufferToStream(req.file.buffer));
        console.log(result);
        res.send({
            code: 200,
            msg: '上传成功',
            data: {
                url: result.url
            }
        })
    })
    app.use('/admin/api', router);
}
```

