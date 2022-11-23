# Web Worker

> [mdn文档][https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers#web_workers_api]
>
> 参考：[阮一峰Web Worker 使用教程][http://www.ruanyifeng.com/blog/2018/07/web-worker.html]
>
> 使用参考：https://zhuanlan.zhihu.com/p/41431253

## 1、概述

众所周知JavaScript是单线程模型，也就是说，所有任务都只能在一个线程上完成，一次完成一件事情。前面的事情没有完成后面的事情只能等着，所以这也导致setTimeout的时间并不是真正的准确，当前面的事件没有完成时，即使setTimeout的时间到了，也只能老老实实的在任务队列里面排队等着。有些耗时的操作就会导致页面阻塞

所以Web Worker产生了，Web Worker的作用，就是为JavaScript创建多线程环境，允许`主线程`创建`Worker线程`，将一些任务分配给worker线程运行，在主线程运行的同时，Worker 线程在后台运行，两者互不干扰。等到 Worker 线程完成计算任务，再把结果返回给主线程。这样的好处是，一些计算密集型或高延迟的任务，被 Worker 线程负担了，主线程（通常负责 UI 交互）就会很流畅，不会被阻塞或拖慢。

打个比方：一个工厂，主线程相当于厂长，worker线程相当于工人。厂长可以将任务分配给下面的工人，工人干好活之后再反馈给厂长，你交给我干的活干完了。工人干活的时候，不耽误厂长的工作，厂长做的工作也不耽误工人流水线的活。互不干扰

Worker 线程一旦新建成功，就会始终运行，不会被主线程上的活动（比如用户点击按钮、提交表单）打断。这样有利于随时响应主线程的通信。但是，这也造成了 Worker 比较耗费资源，不应该过度使用，而且**一旦使用完毕，就应该关闭**。

**要解决的问题是：**

- 解决页面卡死问题
- 发挥多核CPU的优势，提高js性能

## 2、分类

- 专用worker（Dedicated Worker）
- 共享worker（Shared Worker）
- 嵌入式worker（Embedded worker）
- [ServiceWorkers (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API) （服务 worker）一般作为 web 应用程序、浏览器和网络（如果可用）之前的代理服务器。它们旨在（除开其他方面）创建有效的离线体验，拦截网络请求，以及根据网络是否可用采取合适的行动并更新驻留在服务器上的资源。他们还将允许访问推送通知和后台同步 API。
- Chrome Workers 是一种仅适用于 firefox 的 worker。如果您正在开发附加组件，希望在扩展程序中使用 worker 且有在你的 worker 中访问 [js-ctypes](https://developer.mozilla.org/zh-CN/js-ctypes) 的权限，你可以使用 Chrome Workers。
- [Audio Workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Audio_API#audio_workers) （音频 worker）使得在 web worker 上下文中直接完成脚本化音频处理成为可能。

### 专用worker（Dedicated Worker）

> 一个专用 worker 仅仅能被生成它的脚本所使用，通俗的说就是要与它实例化的主线程脚本文件同源

### 共享worker（Shared Worker）

> 一个共享 worker 可以被多个脚本使用——即使这些脚本正在被不同的 window、iframe 或者 worker 访问，如果共享 worker 可以被多个浏览上下文调用，所有这些浏览上下文必须属于同源（相同的协议，主机和端口号）。
>
> 通俗的说共享worker 可以被同一域（浏览器中不同的 tab、iframe 或其他共享worker）下的所有线程访问

## 3、基本用法

- 发消息：worker.postMessage()，参数是传送的消息
- 接收消息：worker.onmessage()，参数是接收到的消息

#### 3.1、主线程

主线程采用`new`命令，调用`Worker()`构造函数，新建一个 Worker 线程。

```js
var worker = new Worker('worker1.js')
```

Worker 完成任务以后，主线程就可以把它关掉。

```javascript
worker.terminate();
```

#### 3.2、worker线程

self  全局对象相当于浏览器的window对象，可以获取到主进程

worker 线程中，workers 也可以调用自己的 [`close`](https://developer.mozilla.org/zh-CN/docs/Web/API/WorkerGlobalScope) 方法进行关闭

```
close();
```

## 4、示例

- 主进程

  index.html

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>web Worker</title>
</head>
<body>
<script>
	if (window.worker) {
		var worker = new Worker('worker1.js')
		worker.postMessage('我是主线程')
		worker.onmessage= e =>{
			console.log('我是主线程接收消息',e);
		}
		setTimeout(()=>{
			// 4秒钟之后关闭worker进程
			worker.terminate();
			console.log('worker进程被关闭了');
		}, 4000)
		//当worker异常时的时候，会触发onerror事件    
        worker.onerror = function() {
             console.log('警告警告，当前worker发生了异常');
        }
		console.log('红红火火恍恍惚惚');
	} else {
		console.log('你的浏览器不支持webworker');
	}
</script>

</body>
</html>
```

- worker1进程-

  worker1.js

```js
//self  全局对象相当于浏览器的window对象
self.onmessage = e => {
    console.log('我是worker1线程接收消息', e);
}
// 递归测试
function fn(n) {
    if (n == 1 || n == 2) {
        return 1
    }
    return fn(n - 1) + fn(n - 2)
}
console.time('fn执行时间')
fn(43)
console.timeEnd('fn执行时间')
self.postMessage('哈哈哈我是worker1')

setTimeout(() => {
    console.log('测试看看我有米有被关闭呀呀呀哈哈哈哈1111');
}, 5000)

//worker 线程也可以调用close 方法来结束worker线程。
// close()


//同样的，在worker 线程中也可以监听错误信息。
onerror = err => {
    console.log(err)
}
```

- 目录结构

├── _index.html

├── _worker1.js

