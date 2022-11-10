# 

# Vue简介

1、官网

[中文官网](https://cn.vuejs.org/)

2、介绍与描述

- vue是一套用来**动态构建用户界面**的**渐进式**JavaScript框框
  - 构建用户界面：把数据通过某种办法变成用户界面
  - 渐进式：Vue可以自底向上逐层的应用

3、Vue的特点

- 遵循MVVM模式
- 编码简洁，体积小，运行效率高，适合移动/PC端开发
- 它本身只关注UI，也可以引入其他第三方库开发

4、与其他js框架的关联

- 借鉴Angular的模板和数据绑定技术
- 借鉴React的组件化和虚拟dom技术

二、初始Vue



# 组件间通信

## a、自定义组件事件

## b、全局事件总线

所有组件都能看见

可以使用：$on、$off、$emit

### 1、定义

一种组件间通信的方式，适用于`任意组件间通信`

### 2、安装全局事件总线

```js
// main.js
new Vue({
    ...
    beforeCreate() {
    	Vue.prototype.$bus = this // 安装全局事件总线，$bus就是当前应用的vm
	}
})
```

### 3、使用事件总线

①、接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的`回调留在A组件自身`

```js
methods () {
    demo(data) {
        ....
    }
},
mounted() {
    this.$bus.$on('xxx', this.demo)
}    
```

②、提供数据：this.$bus.$emit('xxx', 数据)

### 4、解绑

最好在beforeDestory钩子中，用$off去解绑当前组件所用到的事件

```js
beforeDestory() {
    this.$bus.$off('xxx')
}
```

## c、消息订阅与发布

1、订阅消息：消息名

2、发布消息：消息内容

使用pubsub-js 插件

和事件总线类似，所以推荐使用全局事件总线

### 1、定义

一种组件间通信的方式，适用于任意组件间通信

### 2、使用步骤

- 安装pubsub-js： npm i pubsub-js

- 引入：import pubsub from 'pubsub-js'

- 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身

  ```js
  methods() {
      demo(data){...}
  }
  mounted() {
      this.pid = pubsub.subscribe('xxx', this.demo)
  }
  ```

- 提供数据：pubsub.publish('xxx',数据)

- 最好再beforeDestroy钩子中，用pubsub.unsubscribe(pid)去取消订阅

# nextTick

1、语法：this.$nextTick(回调函数)

2、作用：在下一次DOM更新结束后执行其指定的回调

3、什么时候用：当改变数据后，要基于更新后的DOM进行某些操作时，要在nextTick所指定的回调函数中执行

# 过度与动画

1、作用：在插入、更新或移除DOM元素时，在合适的时候给元素添加样式类名

2、图示：

![过渡与动画](E:\test\笔记\vue\image\过渡与动画.png)3、写法

①、准备好样式

- 元素进入的样式

  > - v-enter                 进入的起点
  > - v-enter-active     进入过程中
  > - v-enter-to            进入的终点

- 元素离开的样式

  > - v-leave                离开的起点
  > - v-leave-active    离开过程中
  > - v-leave-to          离开的终点

②、使用<transition>包裹要过渡的元素，并配置name属性，此时需要将上面样式名的<strong style="color: red"> v</strong>换为<strong style="color: red"> name</strong>

③、要让页面一开始就显示动画，需要添加appear

## a、动画

```vue
<transition name="move" appear>
	<div class="box" v-show="isShow"></div>
</transition>
<style>
.box{
	width: 400px;
	height: 100px;
	background: #bfa;
	margin-top: 20px;
}
/* 动画 */
.move-enter-active {
	animation: move 1s linear;
}
.move-leave-active {
	animation: move 1s linear reverse;
}
@keyframes move {
	0% {
		transform: translateX(-100%);
	} to {
		transform: translateX(0);
	}
}
</style>
```

## b、过渡

> 频繁点击后过渡效果最好

```vue
<transition name="move2" appear>
	<div class="box" v-show="isShow"></div>
</transition>

<style>
/* 过渡 */
/* 进入的起点 离开的终点*/
.move2-enter, .move2-leave-to{
	transform: translateX(-100%);
}

.move2-enter-active,.move2-leave-active{
	transition: 0.5s linear;
}

/* 进入的终点 离开的起点*/
.move2-enter-to, .move2-leave{
	transform: translateX(0);
}
</style>
```

## c、多个元素过渡

> 需要配置key

```vue
<!-- 多个元素的过渡 -->
<transition-group name="move2" appear>
	<div class="box" key="1" v-show="isShow">你好呀</div>
	<div class="box" key="1" v-show="isShow">哈哈哈</div>
</transition-group>
```

## d、第三方动画库Animate.css

```vue
<transition-group appear name="animate__animated animate__bounce" enter-active-class="animate__swing" leave-active-class="animate__backOutUp">
	<div class="box" key="1" v-show="isShow">你好呀</div>
	<div class="box" key="1" v-show="isShow">哈哈哈</div>
</transition-group>
```

# 四、Vue中的Ajax

解决跨域方法

- crop（后端配置）：服务器返回数据，携带一些特殊的响应头
- jsonp script src (前后端都要配置，并且只能解决get请求的跨域，其他方式的请求都不能解决)
- 代理服务器（前端配置）

  - nginx
  - vue-cli

## 4.1、Vue脚手架配置代理

vue.config.js是一个可选的配置文件，如果项目的根目录中存在这个文件，那么它会被`@vue/cli-service`自动加载。你也可以使用package.json中的vue字段，但是注意这种写法需要你严格遵照JSON的格式来写

### 方式一

编写vue.config.js配置具体代理规则

在vue.config.js中添加以下配置

```js
module.exports = {
    devServer: {
        proxy: 'http://localhost:5000'
    }
}
```

使用

```js
getStudents(){
	axios.get('http://localhost:8080/students').then(
		response => {
			console.log('请求成功了',response.data)
		},
		error => {
			console.log('请求失败了',error.message)
		}
	)
}
```

说明

- 1、优点：配置简单，请求资源时直接发给前端(8080)即可

- 2、缺点：不能配置多个代理，不能灵活的控制请求是否走代理

- 3、工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求才会发给服务器，否则优先匹配前端资源

  > 比如前端本地文件夹下面有个students的文件，就不会像服务器请求了

### 方式二

编写vue.config.js配置具体代理规则

```js
module.exports = {
    // 开启代理服务器方式2
    devServer: {
        proxy: {
            '/api': { //'/api' 请求前缀 只查找请求前缀开始的
                target: 'http://localhost:5000',
                pathRewrite: {'^/api': ''}, // 重写路径, 将所有的/api都变为''
                ws: true,//用于支持websocket，默认值为true
                changeOrigin: true //跨域伪造， 用于控制请求中的值，将值变为服务器的值。false不伪造 true 伪造， 一般为true 默认为true
            },
            '/carApi': { 
                target: 'http://localhost:5001',
                pathRewrite: {'^/carApi': ''},
            }
        }
    }
}
```

> changeOrigin：
>
> - 设置为true时，服务器收到的请求头中的host为：hocalhost:5050
> - 设置为false时，服务器收到的请求头中的host为：hocalhost:8080
> - 默认值为：true

使用

```js
getStudents(){	    axios.get('http://localhost:8080/api/students').then(
		response => {
			console.log('请求成功了',response.data)
		},
		error => {
			console.log('请求失败了',error.message)
		}
	)
},
getCar({
  axios.get('http://localhost:8080/carApi/cars').then(
		response => {
			console.log('请求成功了',response.data)
		},
		error => {
			console.log('请求失败了',error.message)
		}
	)
},
```

说明

- 1、优点：可以配置多个代理，且可以灵活的控制请求是否走代理
- 2、确定：配置略微繁琐，请求资源时必须加前缀



**node服务器代码**

server1.js

```js
const express = require('express')
const app = express()

app.use((request,response,next)=>{
	console.log('有人请求服务器1了');
	console.log('请求来自于',request.get('Host'));
	console.log('请求的地址',request.url);
	next()
})

app.get('/students',(request,response)=>{
	const students = [
		{id:'001',name:'tom',age:18},
		{id:'002',name:'jerry',age:19},
		{id:'003',name:'tony',age:120},
	]
	response.send(students)
})

app.listen(5000,(err)=>{
	if(!err) console.log('服务器1启动成功了,请求学生信息地址为：http://localhost:5000/students');
})

```

server2.js

```js
const express = require('express')
const app = express()

app.use((request,response,next)=>{
	console.log('有人请求服务器2了');
	next()
})

app.get('/cars',(request,response)=>{
	const cars = [
		{id:'001',name:'奔驰',price:199},
		{id:'002',name:'马自达',price:109},
		{id:'003',name:'捷达',price:120},
	]
	response.send(cars)
})

app.listen(5001,(err)=>{
	if(!err) console.log('服务器2启动成功了,请求汽车信息地址为：http://localhost:5001/cars');
})
```

## 4.2、插槽slot

1、作用：让父组件可以向子组件指定位置插入HTML结构，也是一种组件间通信的方式

2、分类：默认插槽、具名插槽、作用域插槽

3、使用方式

- 默认插槽

  ```vue
  // 父组件
  <demo>
      <div>
          默认插槽
      </div>
  </demo>
  // 子组件
  <template>
  	<div>
          <!--定义插槽-->
          <slot>插槽默认内容</slot>
      </div>
  </template>
  ```

- 具名插槽

  ```vue
  // 父组件
  <demo>
      <template slot="center">
      	<div>
          	具名插槽
      	</div>
      </template>
      <template v-slot:footer>
      	<div>
          	具名插槽
      	</div>
      </template>
  </demo>
  // 子组件
  <template>
  	<div>
          <!--定义插槽-->
          <slot name="center">插槽默认内容</slot>
          <slot name="footer">插槽默认内容</slot>
      </div>
  </template>
  ```

- 作用域插槽

  > 理解：数据在子组件的自身，但根据数据生成的结构需要组件的使用者（父组件）来决定。（games数据在demo组件中，但使用数据所遍历出来的结构由父组件决定）

  ```vue
  // 父组件
  <demo>
      <template scope="scopeData">
      	<ul>
          	<li v-for="item in scopeData.games" :key="item"> {{item}}</li>
      	</ul>
      </template>
  </demo>
  
  // 子组件
  <template>
  	<div>
          <!--定义插槽-->
          <slot :games="games">插槽默认内容</slot>
      </div>
  </template>
  <script>
  export default {
      name: 'demo',
      props: ['title'],
      // 数据在子组件自身
      data () {
          return {
              games: ['qq飞车'， '劲舞团']
          }
      }
  }
  </script>
  ```

# 五、vuex

## 5.1、理解vuex

### 5.1.1、vuex是什么

- 概念：专门在Vue中实现集中式状态（数据）管理的一个vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信

### 5.1.2、什么时候使用vuex

1、多个组件依赖同一状态

2、来自不同组件的行为需要变更同一状态

### 5.1.3、Vuex工作原理图

![Vuex工作原理图](E:\test\笔记\vue\image\vuex.png)

## 5.2、求和案例

### 5.2.1 搭建Vuex环境

1、下载安装vuex： npm i vuex

2、创建 src/store/index.js该文件用于创建Vuex中最为核心的store

```vue
import Vuex from 'vuex'
import Vue from 'vue'
Vue.use(Vuex) // 一定要在这个文件下调用该方法，如果在main.js中使用use(Vuex) 会报错，因为import store from './store'会提升到最前面

// 准备actions--用于响应组件动作
let actions = {}
// 准备mutations--用于操作数组（state）
let mutations = {}
// 准备state--用于存储数据
let state = {}
// 准备getters---用于操作state中的数据，再次加工
let getters = {}

export default new Vuex.Store({
  actions,
  mutations,
  state，
  getters
})
```

3、在src/main.js中创建vm时传入store配置项

```vue
//引入Vue
import Vue from 'vue'
//引入App
import App from './App.vue'
//引入store
import store from './store'
//关闭Vue的生产提示
Vue.config.productionTip = false


//创建vm
new Vue({
	el:'#app',
	render: h => h(App),
	store
})
```

## 5.2.1 使用Vuex写求和案例

Vuex的基本使用

- 初始化数据`state`，配置`actions`、`mutations`，操作文件`store.js`

- 组件中读取vuex中的数据<strong style="color:#f00">$store.state.数据</strong>

- 组件中修改vuex中的数据<strong style="color:#f00">$store.dispatch('action中的方法名',数据)或$store.commit('mutations中的方法名', 数据)</strong>

  若没有其他网络请求或其他业务逻辑，组件中也可越过`actions`，即不写`dispatch`，直接写`commit`

- 当state中的数据需要经过加工后再使用时，可以使用<strong style="color:#f00">getters加工</strong>，相当于全局计算属性

`src/store/index.js`该文件用于创建Vuex中最为核心的store

```vue
import Vuex from 'vuex'
import Vue from 'vue'
Vue.use(Vuex)

// 准备actions--用于响应组件动作
let actions = {
  incrementOdd(context, value) {
    if (context.state.total % 2) {
      context.commit('INCREMENT', value)
    }
  },
  incrementWait(context, value) {
    setTimeout(()=>{
      context.commit('INCREMENT', value)
    },300)
  }
}
// 准备mutations--用于操作数组（state）
let mutations = {
  INCREMENT(state, value) {
    state.total += value
  },
  DECREMENT(state, value) {
    state.total -= value
  }
}
// 准备state--用于存储数据
let state = {
  total: 0
}
//准备getters——用于将state中的数据进行加工
const getters = {
	bigSum(state){
		return state.sum*10
	}
}
export default new Vuex.Store({
  actions,
  mutations,
  state
})
```

`src/components/count.vue`

```vue
<template>
	<div>
		<h1>当前求和为：{{sum}}</h1>
		<h3>当前求和放大10倍为：{{bigSum}}</h3>
		<h3>我在{{school}}，学习{{subject}}</h3>
		<h3 style="color:red">Person组件的总人数是：{{personList.length}}</h3>
		<select v-model.number="n">
			<option value="1">1</option>
			<option value="2">2</option>
			<option value="3">3</option>
		</select>
		<button @click="increment(n)">+</button>
		<button @click="decrement(n)">-</button>
		<button @click="incrementOdd(n)">当前求和为奇数再加</button>
		<button @click="incrementWait(n)">等一等再加</button>
	</div>
</template>

<script>
	import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
	export default {
		name:'Count',
		data() {
			return {
				n:1, //用户选择的数字
			}
		},
		computed:{
			//借助mapState生成计算属性，从state中读取数据。（数组写法）
			...mapState(['sum','school','subject','personList']),
			//借助mapGetters生成计算属性，从getters中读取数据。（数组写法）
			...mapGetters(['bigSum'])
		},
		methods: {
			//借助mapMutations生成对应的方法，方法中会调用commit去联系mutations(对象写法)
			...mapMutations({increment:'JIA',decrement:'JIAN'}),
			//借助mapActions生成对应的方法，方法中会调用dispatch去联系actions(对象写法)
			...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
		},
		mounted() {
			// const x = mapState({he:'sum',xuexiao:'school',xueke:'subject'})
			// console.log(x)
		},
	}
</script>

<style lang="css">
	button{
		margin-left: 5px;
	}
</style>
```

