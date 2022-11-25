# Rest&Ajax

> 参考：[廖雪峰REST][https://www.liaoxuefeng.com/wiki/1022910821149312/1105003357927328]、[哔站REST和AJAX李立超][https://www.bilibili.com/video/BV1Gg411z7fA?p=1&vd_source=0446c600c159458937b7c9e7a61ce142]

前言：早些年的前后端没有分离的时候，服务器的结构是基于MVC模式

- M：Model---数据模型 
- V：View--- 视图用来呈现
- C：Controller --- 控制器，赋值加载数据并选择视图来呈现数据

![MVC.jpg](E:\test\笔记\JS\image\MVC.jpg)

MVC模式的缺点

（1）现在的应用场景，一个应用通常都会有多个端（client）存在：web端 移动app端 PCh5端，如果服务器直接返回html页面，那么服务器就只能为web端提供服务，其他类型的客户端还需要单独开发服务器，这样就提高了开发和维护的成本

（2）降低系统的性能，由于视图不能直接访问数据库需要控制器来帮助因此降低了性能

（3）不适合小型的应用程序，因为花费大量的时间将MVC用于到规模规模不大的程序中得不偿失，而且增加了代码和工作量

（4）由于模型和视图要严格分离，这样给调试增添了困难，而且级联层级的修改是自上而下的如果某块业务只需要新增一个视图，而这个新增就会因为MVC模式思想的限制而去增加业务逻辑层和数据访问层的代码。

如何解决上面的问题呢？

> 传统的服务器需要坐两件事情，第一个加载数据，第二个要将模型渲染金视图

- 解决方案就是将前后端分离，渲染视图的功能从服务器中剥离出来，服务器只负责向客户端返回数据，渲染视图的工作由客服端自行完成。分离以后，服务器只提供数据，一个服务器可以同时为多种客户端提供服务器，同时将视图渲染的工作交给客户端以后，简化了服务器代码的编写，降低了维护成本

前后端分离之后，需要统一一下前后端通信的风格，这时候就需要REST

## REST

REST：REpresentational State Transfer表示层状态的传输

- 其实就是一种服务器的设计风格
- 主要特点，服务器只返回数据，最常用的数据格式是JSON。由于JSON能直接被JavaScript读取，所以，以JSON格式编写的REST风格的API具有简单、易读、易用的特点

### REST规范

REST规范定义了资源的通用访问格式，虽然不是强制要求，但遵守规范可以让人易于理解

**请求的方法**

- GET    加载数据
- POST   新建或添加数据
- PUT    添加或修改数据
- PATCH  修改数据
- DELETE 删除数据
- OPTION 由浏览器自动发送，检查请求的一些权限

**API接口**

- 获取用户列表

  ```
  GET /api/user
  ```

- 获取用户详情

  ```
  GET /api/user/:id
  ```

- 新建用户

  ```js
  POST /api/user
  ```

- 删除用户

  ```
  DELETE /api/user/:id
  ```

。。。。。



## 跨域处理cors

什么是跨域？

- 如果两个网站的完整的域名不相同就会跨域                

  a网站：http://haha.com 

  b网站：http://heihei.com

  a访问b网站就会产生跨域

解决跨域处理前要明确一件事情，这个限制是浏览器限制还是服务器限制的跨域

先看一个案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>rest&ajax</title>
</head>
<body>
    <button id="btn">获取数据</button>
    <script>
        const oBtn = document.getElementById('btn')
        oBtn.onclick = ()=>{
            const xhr = new XMLHttpRequest()
            xhr.open('get', 'http://localhost:3000/api/user')
            xhr.send()
        }
    </script>
</body>
</html>
```

```js
// 获取用户列表
app.get('/api/user', (req, res) => {
    console.log('我收到了user请求');
    res.send({
        code: 200,
        data: userLists
    })
})
```

服务端打印出了：我收到了user请求，说明请求已经到达了服务器。所以是浏览器为了数据安全进行的限制，`跨域是在保护服务器`，避免服务器的数据被他人随意获取

### 解决方案

1、**JSONP**（现在使用比较少）

2、**服务器设置允许跨域的头**，原理就是告诉浏览器不要保护我，我可以向这个客户端发送数据（推荐使用）

- Access-Control-Allow-Origin：允许跨域的域名，只能设置一个，*表示允许所有网站都可以访问
- Access-Control-Allow-Methods：允许的请求方式
- Access-Control-Allow-Headers：允许传递的请求头

```js
app.use((req, res, next)=>{
	// res.setHeader("Access-Control-Allow-Origin", '*')
	res.setHeader("Access-Control-Allow-Origin", 'http://127.0.0.1:5501/') // 允许这个路径访问
	res.setHeader("Access-Control-Allow-Methods", "GET,POST")
	res.setHeader("Access-Control-Allow-Headers", "Content-type")
    next()
})
```



## AJAX

前后端分离后，后端只负责把数据传给前端，前端渲染页面。关键前端怎么获取到后端传输的数据呢？这时就需要AJAX了。

- A 异步 、J JavaScript、A 和、X xml，就是异步的JavaScript和xml，它的作用就是通过js向服务器发送请求来加载数据，其中xml是早期的Ajax使用的数据格式，目前数据都是使用JSON

- 可以选择的方案：
  - XMLHTTPRequest（xhr）
  - Fetch
  - axios

### XMLHTTPRequest（xhr）

- 创建一个新的xhr对象，xhr

  ```js
  const xhr = new XMLHttpRequest()
  ```

- 设置请求的信息，与服务端进行连接

  ```js
  xhr.open('get', 'http://localhost:3000/user')
  ```

- 发送请求

  ```js
  xhr.send()
  ```

- 数据加载完毕

  ```js
  xhr.onload
  ```

- 响应状态码

  ```js
  xhr.status
  ```

- 读取响应信息

  ```js
  xhr.response
  ```

- 响应数据格式（将返回的数据设置为JSON格式）

  ```js
  xhr.responseType = 'json'
  ```

  ![responseType.png](E:\test\笔记\JS\image\responseType.png)

示例：

```js
oBtn.onclick = ()=>{
    const xhr = new XMLHttpRequest()
    xhr.open('get', 'http://localhost:3000/api/user')
    xhr.send()
    // 处理返回的数据
    // 异步
    // 设置返回数据格式
    xhr.responseType = 'json'
    // 给xhr绑定一个onload事件解决异步问题，等待数据返回成功，获取response
    xhr.onload = function () {
        // xhr.status // 响应状态码
        if (xhr.status === 200) {
            // 读取响应数据 xhr.response()
            let res = xhr.response
            //如果没有设置xhr.responseType = 'json'， xhr.response默认返回的数据格式是string
            // res = JSON.parse(res)
            console.log(res);
            if (res.code === 200) {
                // 渲染数据操作
            }
        }
    }

}
```

### fetch

> [fetch mdn][https://developer.mozilla.org/zh-CN/docs/Web/API/fetch]

fetch是xhr的升级版，采用的是Promise API。作用是和xhr一样的，但是使用起来更加友好。fetch原生JS就支持的一种Ajax请求

fetch()与XMLHttpRequest的主要差异

- 1、fetch使用Promise，不适用回调函数，因此写起来更简洁
- 2、fetch采用模块化设计，API分散在多个对象上：Response对象、Request对象、Headers对象，而XMLHttpRequest的API设计不是很好，输入、输出、状态都在同一个接口管理，比较混乱
- 3、fetch通过数据量（Stream对象）处理数据，可以分块读取，有利于提高网站性能表现，减少内存占用，对于请求大文件或者网速慢的场景很有用。XMLHttpRequest对象不支持数据流，所有的数据必须放在缓存里，不支持分块读取，必须等全部拿到后，再一次性给出来

```js
fetch(url, {method,headers,body,mode}).then(response=>{}).catch
```

**Response对象**：处理服务器返回的数据

1. body: ReadableStream
2. bodyUsed: false，只读属性包含一个布尔值，指示正文是否已被读取
3. headers: Headers {}，指向一个headers对象，对应HTTP回应的所有标头
4. ok: true，返回一个布尔值，表示请求是否成功，true对应HTTP状态码200到299，false对应其他状态码
5. redirected: false，属性返回一个布尔值，表示请求是否发生
6. status: 200，属性返回一个数字，表示HTTP回应的状态码
7. statusText: "OK"，属性返回一个字符串，表示HTTP回应的状态信息，成功就返回OK
8. type: "cors"，属性返回请求的类型
9. url: "http://localhost:3000/api/user"，属性返回请求的URL

- **Response.body**

  返回数据格式

  > - [`ArrayBuffer`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
  > - [`ArrayBufferView`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) (Uint8Array 等)
  > - [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)/File
  > - string
  > - [`URLSearchParams`](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams)
  > - [`FormData`](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData)

  格式化解析

  >- [`Request.arrayBuffer()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request/arrayBuffer) / [`Response.arrayBuffer()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/arrayBuffer)
  >- [`Request.blob()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request/blob) / [`Response.blob()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/blob)
  >- [`Request.formData()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request/formData) / [`Response.formData()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/formData)
  >- [`Request.json()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request/json) / [`Response.json()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/json)
  >- [`Request.text()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Request/text) / [`Response.text()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/text)

  Response.json()使用较多的是将服务器返回的数据转为JSON格式

- **Response.type**

  返回请求的类型

  > - `basic`：普通请求，即同源请求。
  > - `cors`：跨域请求。
  > - `error`：网络错误，主要用于 Service Worker。
  > - `opaque`：如果`fetch()`请求的`type`属性设为`no-cors`，就会返回这个值，详见请求部分。表示发出的是简单的跨域请求，类似`<form>`表单的那种跨域请求。
  > - `opaqueredirect`：如果`fetch()`请求的`redirect`属性设为`manual`，就会返回这个值，详见请求部分。

- 示例

  ```js
  fetch('http://localhost:3000/api/user',{method: 'get'}).then(res=>{
      console.log('res1===', res);
      return res.json()
  }).then(res=>{
      console.log('res2===', res);
  })
  ```

  ```js
  // 调用fetch发送请求来完成登录
  fetch("http://localhost:3000/api/login", {
      method: "POST",
      headers: {
          "Content-type": "application/json"
      },
      body: JSON.stringify({ username, password })
  })
      .then((res) => res.json())
      .then((res) => {
  
          if(res.status !== "ok"){
              throw new Error("用户名或密码错误")
          }
  
          // console.log(res)
          // 登录成功
          root.innerHTML = `
              <h1>欢迎 ${res.data.nickname} 回来！</h1>
              <hr>
              <button id="load-btn">加载数据</button>
          `
      })
      .catch((err) => {
          console.log("出错了！", err)
          // 这里是登录失败
          document.getElementById("info").innerText = "用户名或密码错误"
      })
  ```

### axios

