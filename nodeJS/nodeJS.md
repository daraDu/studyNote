nodeJS

（一个一个用户（前端）去饭店，一个服务员去服务（后端服务器端）， 后厨表示数据库）

统一API

非阻塞



nvm list 展示当前系统的所有node版本

nvm use 版本号 选择node版本



npm init 创建package.json

## 异步编程

进程和线程

什么是同步？





http.createServer(res, req)

1.req就是用户**获取**客户端（浏览器）传来的信息—>请求
2.res：用户向客户端（浏览器）**返回**信息------>响应



get是url传值



post是事件接受的形式

发送的数据

req.on('data', chunk=>{

})  chunk 每次的数据



数据发送完成

req.on('end', ()=>{

})



URLSearchParams：https://vimsky.com/examples/usage/nodejs-url-class-urlsearchparams-nj.html



node操作里会执行两次的原因，图标也会进行请求一次

req.url === ‘/favicon.ico’ 也会进行执行

解决： 判断req.url !==‘/favicon.ico’  执行逻辑操作



url.parse(reqUrl, true).query // 弃用 

替代API

new URL(reqUrl, 'http://localhost:9090/')

文档：[网址|节点.js v16.18.0 文档 (nodejs.org)](https://nodejs.org/dist/latest-v16.x/docs/api/url.html#url_the_whatwg_url_api)

## node 连接数据库

### 1、下载mysql

下载地址: https://dev.mysql.com/downloads/mysql/

### 2、工具: nacicat premium

### 3、node连接数据库

​		a>在目录中下载mysql包

​			npm install mysql

​		b>引入mysql

​		C>配置mysql信息

![image-20221019115450962](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221019115450962.png)



4、mysql操作代码

```js
// 注册
var mysql = require ('mysql')
var connection = mysql.createConnection({
    host :'主机名',
    user:'用户名',
    password : '密码',
    database:'库名称',
    port: '端口号'
})
// 连接
connection.connect();
// 查询
connection.query('SELECT 1', function (error, results, fields){
    if (error) throw error;
    console.log(results)
}
// 关闭                
connection.end()
```

database：库 （自己创建的test那个库）里面是表，表里面是字段

库=》表=》字段

![image-20221019115634773](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221019115634773.png)

##### 1》mysql查询数据

- select * from 表名称：查询所有

- select * from 表名称 where key1=? and key2=?

  - where ===>条件
  - ? 是本地要查询的值
  - key1是数据库里的属性名

  ```js
  connection.query('select * from 表名称 where name=? and password=?', [第一个?, 第二个?], (err, results, fields) =>{
      
  })
  ```

  

![image-20221019145656786](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221019145656786.png)

##### 2》数据库增加语句

- insert into 表名称 value （id, username, userpwd）
  - 括号里的是要添加到数据库里的字段名称
  - id 如果要自增长，在navicat中设置就好，不用传了

## express

- 基于node.js的web应用框架

- 目录结构

  - bin ->  www ===> 启动文件
  - app.js  ===> 全局配置文件
  - routers ===> 路由的配置
  - views ====> 页面  index.ejs
  - public ===> 静态资源

  
  
  
  
  ![image-20221025084911879](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025084911879.png)
  
  

![image-20221025082424997](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025082424997.png)

密码散列

![image-20221025082511083](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025082511083.png)

![image-20221025082538010](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025082538010.png)

![image-20221025084056215](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025084056215.png)

![image-20221025084207820](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025084207820.png)



![image-20221025084623655](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025084623655.png)

secret 环境变量里面





![image-20221025090338456](C:\Users\sss\AppData\Roaming\Typora\typora-user-images\image-20221025090338456.png)