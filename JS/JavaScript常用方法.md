

# JavaScript常用方法

[TOC]

## 一、关于DOM

### 1、复制到剪切板

>在Web应用程序中，复制到剪贴板因其对用户的便利性而迅速流行起来。
>
>```js
>const copyToClipboard = (text) =>
>navigator.clipboard?.writeText && navigator.clipboard.writeText（text）。
>// 测试
>copyToClipboard("复制的内容!")
>```
>
>注意：必须检查用户的浏览器是否支持该API .为了支持所有用户，你可以使用一个输入并复制其内容

### 2、检测黑暗模式

>随着黑暗模式的普及，如果用户在他们的设备中启用了黑暗模式，那么将你的应用程序切换到黑暗模式是非常理想的。幸运的是，可以利用媒体查询来使这项任务变得简单。
>
>```js
>const isDarkMode = () =>
>window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches。
>// 测试
>console.log(isDarkMode())
>```

### 3、滚动到顶部

>初学者经常发现自己在正确滚动元素的过程中遇到困难。最简单的滚动元素的方法是使用scrollIntoView方法。添加行为。"smooth "来实现平滑的滚动动画。
>
>```js
>const scrollToTop = (element) => element.scrollIntoView({ behavior: "smooth", block: "start" })
>```

> 方法2：
>
> ```js
> export const scrollToTop = () => {
>   const height = document.documentElement.scrollTop || document.body.scrollTop;
>   if (height > 0) {
>     window.requestAnimationFrame(scrollToTop);
>     window.scrollTo(0, height - height / 8);
>   }
> }
> ```

### 4、滚动到底部

>就像scrollToTop方法一样，scrollToBottom方法也可以用scrollIntoView方法轻松实现，只需将块值切换为结束即可
>
>```js
>const scrollToBottom = (element) =>  element.scrollIntoView({ behavior: "smooth", block: "end" })
>```

### 4.2、滚动到元素位置

```js
export const smoothScroll = element =>{
    document.querySelector(element).scrollIntoView({
        behavior: 'smooth'
    });
}
smoothScroll('#target'); // 平滑滚动到 ID 为 target 的元素
```

### 5、生成随机颜色

>```js
>const generateRandomHexColor = () => `#${Math.floor(Math.random() * 0xffffff) .toString(16)}`;
>```

### 6、设置系统缩放比，适配各分辨率

> ```js
> // 设置系统缩放比，适配各分辨率
> export function refreshScale() {
> 	let baseWidth = document.documentElement.clientWidth;
> 	let baseHeight = document.documentElement.clientHeight;
> 	let appStyle = document.getElementById('Home').style;
> 	let realRatio = baseWidth / baseHeight
> 	let designRatio = 16 / 9
> 	let scaleRate = baseWidth / 1920
> 	if (realRatio > designRatio) {
> 		scaleRate = baseHeight / 1080
> 	}
> 	appStyle.transformOrigin = 'left top';
> 	appStyle.transform = `scale(${scaleRate}) translateX(-50%)`;
> 	appStyle.width = `${baseWidth/scaleRate}px`
> }
> ```

### 7、防抖

```js
export const debounce = (() => {
  let timer = null
  return (callback, wait = 800) => {
    timer&&clearTimeout(timer) // 如果存在定时器就清除，重新计时
    timer = setTimeout(callback, wait) // 然后创建新的定时器
  }
})()
```

在vue中使用

```vue
methods: {
  loadList() {
    debounce(() => {
      console.log('加载数据')
    }, 500)
  }
}
```

### 8、节流

> 其实可以把节流原理想象成王者荣耀的英雄技能，当你释放技能的时候，该技能就会进行冷却倒计时，等倒计时结束之后，就可以再次释放技能了

```js
export const throttle = (() => {
  let last = 0
  return (callback, wait = 800) => {
    let now = +new Date()
    if (now - last > wait) {
      callback()
      last = now
    }
  }
})()
```

### 9、开启全屏

```js
export const launchFullscreen = (element) => {
  if (element.requestFullscreen) {
    element.requestFullscreen()
  } else if (element.mozRequestFullScreen) {
    element.mozRequestFullScreen()
  } else if (element.msRequestFullscreen) {
    element.msRequestFullscreen()
  } else if (element.webkitRequestFullscreen) {
    element.webkitRequestFullScreen()
  }
}
```

### 10、关闭全屏

```js
export const exitFullscreen = () => {
  if (document.exitFullscreen) {
    document.exitFullscreen()
  } else if (document.msExitFullscreen) {
    document.msExitFullscreen()
  } else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen()
  } else if (document.webkitExitFullscreen) {
    document.webkitExitFullscreen()
  }
}
```

### 10-1、全屏/取消全屏

```html
<!doctype html>
<html>
    <head>
      <meta charset="UTF-8"/>
        <title>全屏不全屏</title>
    </head>
    <body>
      <button id="fullScreen">全屏模式</button>
      <button id="noFullScreen">取消全屏</button>
     </body>
</html>
<script>
       document.getElementById("fullScreen").onclick=function(){
          if(document.documentElement.RequestFullScreen){
            document.documentElement.RequestFullScreen();
          }
          //兼容火狐
          console.log(document.documentElement.mozRequestFullScreen)
          if(document.documentElement.mozRequestFullScreen){
            document.documentElement.mozRequestFullScreen();
          }
          //兼容谷歌等可以webkitRequestFullScreen也可以webkitRequestFullscreen
          if(document.documentElement.webkitRequestFullScreen){
            document.documentElement.webkitRequestFullScreen();
          }
          //兼容IE,只能写msRequestFullscreen
          if(document.documentElement.msRequestFullscreen){
            document.documentElement.msRequestFullscreen();
          }
       }
       document.getElementById("noFullScreen").onclick=function(){
          if(document.exitFullScreen){
            document.exitFullscreen()
          }
          //兼容火狐
          console.log(document.mozExitFullScreen)
          if(document.mozCancelFullScreen){
            document.mozCancelFullScreen()
          }
          //兼容谷歌等
          if(document.webkitExitFullscreen){
            document.webkitExitFullscreen()
          }
          //兼容IE
          if(document.msExitFullscreen){
            document.msExitFullscreen()
          }
       }
</script>

```

```text
// 全屏之后还可以选择性的调整样式，就和hover一样，如下所示
:-webkit-full-screen { }
:-moz-full-screen { }
:-ms-fullscreen { }
:fullscreen { }
```

### 10-2、判断是否为全屏

```js
// 判断是否为全屏
export function isFullScreen() {
    return  !! (
        document.fullscreen || 
        document.mozFullScreen ||                         
        document.webkitIsFullScreen ||       
        document.webkitFullScreen || 
        document.msFullScreen 
    );
}
```



### 11、判断手机是Andoird还是IOS

```js
/** 
 * 1: ios
 * 2: android
 * 3: 其它
 */
export const getOSType=() => {
  let u = navigator.userAgent, app = navigator.appVersion;
  let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1;
  let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);
  if (isIOS) {
    return 1;
  }
  if (isAndroid) {
    return 2;
  }
  return 3;
}
```

### 12、存储操作

```js
class MyCache {
  constructor(isLocal = true) {
    this.storage = isLocal ? localStorage : sessionStorage
  }

  setItem(key, value) {
    if (typeof (value) === 'object') value = JSON.stringify(value)
    this.storage.setItem(key, value)
  }

  getItem(key) {
    try {
      return JSON.parse(this.storage.getItem(key))
    } catch (err) {
      return this.storage.getItem(key)
    }
  }

  removeItem(key) {
    this.storage.removeItem(key)
  }

  clear() {
    this.storage.clear()
  }

  key(index) {
    return this.storage.key(index)
  }

  length() {
    return this.storage.length
  }
}

const localCache = new MyCache()
const sessionCache = new MyCache(false)

export { localCache, sessionCache }
```

**示例：**

```js
localCache.getItem('user')
sessionCache.setItem('name','张三')
sessionCache.getItem('token')
localCache.clear()
```

## 二、数组操作

### 1、数组乱序

>在使用需要某种程度的随机化的算法时，你会经常发现洗牌数组是一个相当必要的技能。下面的片段以O(n log n)的复杂度对一个数组进行就地洗牌。
>
>```js
>const shuffleArray = (arr) => arr.sort(() => Math.random() - 0.5) 。
>// 测试
>const arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
>console.log(shuffleArray(arr))
>```

### 2、数组去重

>利用ES6的set结构的唯一性特性，可以很快的实现数组去重
>
>```js
>const getUnique = (arr) => [...new Set(arr)]。
>// 测试
>const arr = [1, 1, 2, 3, 3, 4, 4, 5, 5];
>console.log(getUnique(arr))
>```

> ##### 数组对象根据字段去重
>
> 利用reduce去重
>
> ```js
> function uniqueArrayObject (arr = [], key = 'id') {
>     if (arr.length === 0) return
> 	let obj = {};
> 	const ret = arr.reduce((cur,next) => {
> 		if (!obj[next[key]]){
> 			obj[next[key]] = true;
> 			cur.push(next);
> 		}
> 		return cur;
> 	},[])
> 	return ret
> }
> let arr2 = [
>   {a: '1', b: 'q1'},
>   {a: '1', b: 'q2'},
>   {a: '2', b: 'e1'},
>   {a: '2', b: 'e2'}
> ]
> console.log( uniqueArrayObject(arr2, 'a'));
> /*
> 0: {a: '1', b: 'q1'}
> 1: {a: '2', b: 'e1'}
> */
> ```
>
> 利用foreach
>
> ```js
> export const uniqueArrayObject = (arr = [], key = 'id') => {
>   if (arr.length === 0) return
>   let list = []
>   const map = {}
>   arr.forEach((item) => {
>     if (!map[item[key]]) {
>       map[item[key]] = item
>     }
>   })
>   list = Object.values(map)
> 
>   return list
> }
> ```
>
> 

## 三、数据操作

### 1、校验数据类型

```js
export const typeOf = function(obj) {
  return Object.prototype.toString.call(obj).slice(8, -1).toLowerCase()
}
typeOf('哈哈哈')  // string
typeOf([])  // array
typeOf(new Date())  // date
typeOf(null) // null
typeOf(true) // boolean
typeOf(() => { }) // function
console.log(typeOf({})); // object
```

### 2、手机号脱敏

```js
export const hideMobile = (mobile) => {
  return mobile.replace(/^(\d{3})\d{4}(\d{4})$/, "$1****$2")
}
```

### 3、大小写转换

- str 待转换的字符串
- type 1-全大写 2-全小写 3-首字母大写

```js
export const turnCase = (str, type) => {
  switch (type) {
    case 1:
      return str.toUpperCase()
    case 2:
      return str.toLowerCase()
    case 3:
      //return str[0].toUpperCase() + str.substr(1).toLowerCase() // substr 已不推荐使用
      return str[0].toUpperCase() + str.substring(1).toLowerCase()
    default:
      return str
  }
}

turnCase('vue', 1) // VUE
turnCase('REACT', 2) // react
turnCase('vue', 3) // Vue
```

### 4、解析URL参数

```js
export const getSearchParams = () => {
  const searchPar = new URLSearchParams(window.location.search)
  const paramsObj = {}
  for (const [key, value] of searchPar.entries()) {
    paramsObj[key] = value
  }
  return paramsObj
}

// 假设目前位于 https://****com/index?id=154513&age=18;
getSearchParams(); // {id: "154513", age: "18"}
```

### 5、生成UUID

```js
export const uuid = () => {
  const temp_url = URL.createObjectURL(new Blob())
  const uuid = temp_url.toString()
  URL.revokeObjectURL(temp_url) //释放这个url
  return uuid.substring(uuid.lastIndexOf('/') + 1)
}
console.log(uuid()); //bebbc719-d92c-45cc-8277-b8cc677df40a
```

### 6、金额格式化

**参数：**

- {number} number：要格式化的数字
- {number} decimals：保留几位小数
- {string} dec_point：小数点符号
- {string} thousands_sep：千分位符号

```js
export const moneyFormat = (number, decimals, dec_point, thousands_sep) => {
  number = (number + '').replace(/[^0-9+-Ee.]/g, '')
  const n = !isFinite(+number) ? 0 : +number
  const prec = !isFinite(+decimals) ? 2 : Math.abs(decimals)
  const sep = typeof thousands_sep === 'undefined' ? ',' : thousands_sep
  const dec = typeof dec_point === 'undefined' ? '.' : dec_point
  let s = ''
  const toFixedFix = function(n, prec) {
    const k = Math.pow(10, prec)
    return '' + Math.ceil(n * k) / k
  }
  s = (prec ? toFixedFix(n, prec) : '' + Math.round(n)).split('.')
  const re = /(-?\d+)(\d{3})/
  while (re.test(s[0])) {
    s[0] = s[0].replace(re, '$1' + sep + '$2')
  }

  if ((s[1] || '').length < prec) {
    s[1] = s[1] || ''
    s[1] += new Array(prec - s[1].length + 1).join('0')
  }
  return s.join(dec)
}
```

**示例：**

```
moneyFormat(10000000) // 10,000,000.00
moneyFormat(10000000, 3, '.', '-') // 10-000-000.000
```

### 7、下载文件

**参数：**

- api 接口
- params 请求参数
- fileName 文件名

```js
const downloadFile = (api, params, fileName, type = 'get') => {
  axios({
    method: type,
    url: api,
    responseType: 'blob', 
    params: params
  }).then((res) => {
    let str = res.headers['content-disposition']
    if (!res || !str) {
      return
    }
    let suffix = ''
    // 截取文件名和文件类型
    if (str.lastIndexOf('.')) {
      fileName ? '' : fileName = decodeURI(str.substring(str.indexOf('=') + 1, str.lastIndexOf('.')))
      suffix = str.substring(str.lastIndexOf('.'), str.length)
    }
    //  如果支持微软的文件下载方式(ie10+浏览器)
    if (window.navigator.msSaveBlob) {
      try {
        const blobObject = new Blob([res.data]);
        window.navigator.msSaveBlob(blobObject, fileName + suffix);
      } catch (e) {
        console.log(e);
      }
    } else {
      //  其他浏览器
      let url = window.URL.createObjectURL(res.data)
      let link = document.createElement('a')
      link.style.display = 'none'
      link.href = url
      link.setAttribute('download', fileName + suffix)
      document.body.appendChild(link)
      link.click()
      document.body.removeChild(link)
      window.URL.revokeObjectURL(link.href);
    }
  }).catch((err) => {
    console.log(err.message);
  })
}
```

**使用：**

```js
downloadFile('/api/download', {id}, '文件名')
复制代码
```

### 8、深拷贝

```js
export const clone = parent => {
  // 判断类型
  const isType = (obj, type) => {
    if (typeof obj !== "object") return false;
    const typeString = Object.prototype.toString.call(obj);
    let flag;
    switch (type) {
      case "Array":
        flag = typeString === "[object Array]";
        break;
      case "Date":
        flag = typeString === "[object Date]";
        break;
      case "RegExp":
        flag = typeString === "[object RegExp]";
        break;
      default:
        flag = false;
    }
    return flag;
  };

  // 处理正则
  const getRegExp = re => {
    var flags = "";
    if (re.global) flags += "g";
    if (re.ignoreCase) flags += "i";
    if (re.multiline) flags += "m";
    return flags;
  };
  // 维护两个储存循环引用的数组
  const parents = [];
  const children = [];

  const _clone = parent => {
    if (parent === null) return null;
    if (typeof parent !== "object") return parent;

    let child, proto;

    if (isType(parent, "Array")) {
      // 对数组做特殊处理
      child = [];
    } else if (isType(parent, "RegExp")) {
      // 对正则对象做特殊处理
      child = new RegExp(parent.source, getRegExp(parent));
      if (parent.lastIndex) child.lastIndex = parent.lastIndex;
    } else if (isType(parent, "Date")) {
      // 对Date对象做特殊处理
      child = new Date(parent.getTime());
    } else {
      // 处理对象原型
      proto = Object.getPrototypeOf(parent);
      // 利用Object.create切断原型链
      child = Object.create(proto);
    }

    // 处理循环引用
    const index = parents.indexOf(parent);

    if (index != -1) {
      // 如果父数组存在本对象,说明之前已经被引用过,直接返回此对象
      return children[index];
    }
    parents.push(parent);
    children.push(child);

    for (let i in parent) {
      // 递归
      child[i] = _clone(parent[i]);
    }

    return child;
  };
  return _clone(parent);
};

复制代码
```

> 此方法存在一定局限性：一些特殊情况没有处理: 例如Buffer对象、Promise、Set、Map。

**如果确实想要完备的深拷贝，推荐使用 lodash 中的 cloneDeep 方法。**

### 9、模糊搜索

**参数：**

- list 原数组
- keyWord 查询的关键词
- attribute 数组需要检索属性

```js
export const fuzzyQuery = (list, keyWord, attribute = 'name') => {
  const reg = new RegExp(keyWord)
  const arr = []
  for (let i = 0; i < list.length; i++) {
    if (reg.test(list[i][attribute])) {
      arr.push(list[i])
    }
  }
  return arr
}
复制代码
```

**示例：**

```js
const list = [
  { id: 1, name: '树哥' },
  { id: 2, name: '黄老爷' },
  { id: 3, name: '张麻子' },
  { id: 4, name: '汤师爷' },
  { id: 5, name: '胡万' },
  { id: 6, name: '花姐' },
  { id: 7, name: '小梅' }
]
fuzzyQuery(list, '树', 'name') // [{id: 1, name: '树哥'}]
复制
```

### 10、遍历树节点

```js
export const foreachTree = (data, callback, childrenName = 'children') => {
  for (let i = 0; i < data.length; i++) {
    callback(data[i])
    if (data[i][childrenName] && data[i][childrenName].length > 0) {
      foreachTree(data[i][childrenName], callback, childrenName)
    }
  }
}
```

**示例：**

假设我们要从树状结构数据中查找 id 为 9 的节点

```js
const treeData = [{
  id: 1,
  label: '一级 1',
  children: [{
    id: 4,
    label: '二级 1-1',
    children: [{
      id: 9,
      label: '三级 1-1-1'
    }, {
      id: 10,
      label: '三级 1-1-2'
    }]
  }]
 }, {
  id: 2,
  label: '一级 2',
  children: [{
    id: 5,
    label: '二级 2-1'
  }, {
    id: 6,
    label: '二级 2-2'
  }]
  }, {
    id: 3,
    label: '一级 3',
    children: [{
      id: 7,
      label: '二级 3-1'
    }, {
      id: 8,
      label: '二级 3-2'
    }]
}],

let result
foreachTree(data, (item) => {
  if (item.id === 9) {
    result = item
  }
})
console.log('result', result)  // {id: 9,label: "三级 1-1-1"}   
```