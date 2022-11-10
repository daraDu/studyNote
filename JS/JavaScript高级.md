# JavaScript高级

[TOC]



## 一、JavaScript基础总结深入

### Ⅰ- 数据类型相关知识点

####  1、数据分类

- ##### 基本（值）数据类型

  字符串、number、boolean、undefined、null

- ##### 对象（引用）数据类型

  > Object：所有的对象
  >
  > Function: 特殊的对象，可执行的代码块
  >
  > 数组：特殊的对象，key为下标属性，内容是有序的

#### 2、判断数据类型方法

- ##### typeof

  - 可以判断undefined、string、number、function返回对应的`字符串`

  - 不可以判断null、array、object，都会返回`object`

  - 代码示例

    ```js
    
            let a = 1
            let b
            let c = null
            let d = 'hhh'
            let obj = {
                a: 1
            }
            let arr = [1, 2]
            function fn () {
                console.log('===');
            }
            console.log(typeof a); // 'number'
            console.log(typeof b); // 'undefined'
            console.log('=c=',typeof c); // 'object'
            console.log(typeof d); // 'string'
            console.log(typeof obj); // 'object'
            console.log(typeof arr); // 'object'
            console.log(typeof fn); // 'function'
    ```

- ##### instanceof（判断实例的方法 ）

  - 用于检测构造函数的`prototype`属性是否出现在某法实例对象的原型链上

  - 代码示例：

    ```js
    let obj2 = {
        a: 1
    }
    let arr = [1, 2]
    function fn () {
        return 'hhh'
    }
    console.log(Object());
    console.log(Array());
    console.log(obj2 instanceof Object, obj2 instanceof Array); // true， false
    console.log('37==',arr instanceof Array, arr instanceof Object); // true // true ->Array的原型链下面最终指向Object
    console.log(fn instanceof Function, fn instanceof Object); // true true->Function的原型链下面最终指向Object
    ```

- #### ===

  - 全等判断，左右两边不一致返回false，可以判断null和undefined，其他类型就要加typeof转换进行全等判断

  - 代码示例

  ```js
  let obj = {
      a: null,
      b: undefined,
      c: '我是字符串',
      d: 111,
      e: false,
      f: function () {},
      g: []
  }
  console.log(obj === Object, typeof obj === 'object'); // false true
  console.log(obj.a === null); // true
  console.log(obj.b === undefined); // true
  console.log(obj.c === String, typeof obj.c === 'string'); //false true
  console.log(obj.d=== Number, typeof obj.d=== 'number'); // false true
  console.log(obj.e=== Boolean, typeof obj.e=== 'boolean'); // false true
  console.log(obj.g=== Array, typeof obj.f=== 'array'); // false false 因为typeof obj.f=返回object
  ```

#### 3、相关问题

- ##### undefined与null的区别

  - undefined代表定义未赋值

  - null：代表定义了，并且赋值了null

  - 代码示例

    ``````js
      var a
      console.log(a)  // undefined
      var b = null
      console.log(b) // null
    ``````

    

- ##### 什么时候给变量赋值为null

  - 1、初始值的时候不确定变量的类型
  - 2、对象，函数、数组不使用的时候赋值为null，减少堆内存使用

- ##### 严格区别变量类型与数据类型

  - js的变量本身是没有类型的，变量的类型实际上是变量内存中数据的类型（js是弱类型的语言）。var a; 判断变量类型，实际上 是判断值的类型。
  - 数据的类型（数据对象）：
      \* 基本类型
      \* 对象类型
  - 变量的类型（变量内存值的类型）：
      \* 基本类型：保存基本类型的数据（保存基本类型数据的变量）。
      \* 引用类型：保存对象地址值（保存对象地址值的变量）。


### Ⅱ、数据,变量, 内存的理解

#### 1、什么是数据

- 数据就是存储在内存中保存特定信息的东西，本质就是二进制010101
- 数据的特点：可传递，可读，可运算

#### 2、什么是内存

- 内存就是内存条通电后产生的可存储数据的空间（临时的）

  ![image-20220726093802780](.\image\1.png)

- 内存的产生和死亡：内存条 -》 通电  -》 产生一定容量的临时存储（内存）空间 -》存储数据 -》处理数据 -》断电 -》 内存空间和数据全部消失

- 内存与硬盘的区别：内存的空间是临时的，断电后空间消失。硬盘的空间是永久的

- 内存中2个数据

  - 内部数据（基本数据类型，变量数据）
  - 地址值（指向引用数据类型）

- 内存分类：

  - 栈：全局变量/局部变量

  - 堆：对象

    ![image-20220726093522407](.\image\2.png)

#### 3、什么是变量

- 值可以变化的量，由变量名和变量值构成
- 一个变量对应一小块内存，变量名可以用来查找变量，变量值就是保存数据的内容

#### 4、内存、数据、变量三者的关系

- 内存是存储数据
- 变量是数据的标识（名字），用来查找数据，进而操作（读写）内存的数据

#### 5、相关问题引出

- 关于赋值和内存的问题

  var a = xxx(赋值操作)，a内存中到底保存的是什么？

  > - xxx是基本数据类型，保存的就是这个具体数据信息
  > - xxx是引用数据类型，保存的就是这个对象的地址值

- 关于引用变量赋值的问题

  - 2个引用变量指向同一个对象，通过一个变量修改对象内部属性，另一个变量看到的也是修改后的数据

  - 2个引用变量指向同一个对象，让其中一个变量指向另一个对象，另一个变量依然指向前一个对象，这时修改，两个之间互不干扰

  - 代码示例

    ```js
    let obj = {name: '小明'}
    let a = obj, b = obj
    console.log(a===b); // true
    a.name = '张三'
    console.log(b); // {name: '张三'} a和b指向同一个对象
    let obj2 = {
        name: 'lisa'
    }
    b = obj2 // b修改了地址值，指向了另一个对象obj2，a还是指向之前的obj对象
    console.log(a===b); // false
    a.name = '小黑'
    console.log(b); // {name: 'lisa'}
    ```

    

- 在js调用函数时传递变量参数时, 是值传递还是引用传递

  - 理解一：都是值（基本值/地址值）传递
    - 变量是存储在栈内存中，如果是基本类型，就是变量名和变量值。如果是引用类型，就是变量名和地址值，然后再根据地址值到堆内存中查找
  - 理解二
    - 基本值：是值传递
    - 对象：引用传递（地址值）

- JS引擎如何管理内存?

  - 1、内存的声明周期

    - 分配小内存空间得到它的使用权（分配需要的内存）
    - 存储数据，可以进行反复的读写操作（使用分配的内存）
    - 释放小内存空间（不需要时将其释放归还）

  - 2、释放内存

    - 局部变量：函数执行完自动释放（为执行函数分配的栈内存空间）
    - 对象：成为垃圾对象 ===》垃圾回收器回收（存储对象的堆空间内存：当内存没有引用指向时，对象成为垃圾对象，垃圾回收器就会释放此内存）
    - 全局变量：不会自动释放，全局变量保存在静态存贮区，程序开始运行时为其分配内存，程序结束释放该内存(会一直占用创建时给它分配的内存，只有程序结束时才会释放)

    ```js
     var a = 3 // a不会被释放，只有程序结束才会释放
     var obj = {name:"小明"}
     obj = undefined ||null  //此时,obj没有被释放,但是之前声明的`{name:"小明"}`由于没有人指向它,会在后面你某个时刻被垃圾回收器回收
    
    function fn () { var b = {}}
    fn() // b是自动释放, b所指向的对象是在后面的某个时刻由垃圾回收器回收
    ```

    

### Ⅲ、对象

#### 1、对象的概念

- 什么是对象
  - 多个数据的封装体
  - 用来保存多个数据的容器
- 为什么要用对象
  - 统一管理多个数据
- 对象的组成
  - 属性：属性名（字符串类型）和属性值（任意类型）组成
  - 方法：一个特别的属性（属性值是函数）

#### 2、如何访问对象内部数据

- `.属性名`：编码简单，（属性名为动态的时候不能使用）
- [属性名]：通用

#### 3、什么时候必须使用`['属性名']`的方式?

- 属性名包含特殊字符: `-` `空格`

- 属性名不确定

  ```js
   var p = {}
   //1. 给p对象添加一个属性: content type: text/json
   // p.content-type = 'text/json' //不能用
   p['content-type'] = 'text/json'
   console.log(p['content-type'])
  
   //2. 属性名不确定
   var propName = 'myAge'
   var value = 18
   // p.propName = value //不能用
   p[propName] = value
   console.log(p[propName]) // 18
  ```

### Ⅳ、函数

#### 1、函数的概念

- 什么是函数

  > - 用来实现特地功能的，n条语句的封装体
  >
  > - 只有函数类型的数据是可以执行的，其他的都不可以

- 为什么要用函数

  > - 提高代码复用
  > - 便于阅读交流

- 如何定义函数

  > - 函数声明
  >
  > - 表达式
  >
  >   ``````js
  >      function fn1 () { console.log('fn1()' )//函数声明
  >   
  >      const fn2 = ()=> console.log('fn2()')  //表达式
  >   ``````

#### 2、如何调用（执行）函数

- 1、一般函数：直接调用

- 2、构造函数：通过new调用

- 3、对象：通过`.内部属性/方法`

  ```js
  function fun () {}
  fun()  // 直接调用
  function Fun2() {}
  let a = new Fun2() // new调用
  let obj = {
      fn: fucntion () {}
  }
  obj.fn() // 属性调用
  ```

#### 3、回调函数

- 什么函数是回调函数？

  > - 你定义的
  >
  > - 你没有调用
  >
  > - 但它最终执行了（在一定条件或某个时刻）

- 常见的回调函数

  > - dom事件回调函数（点击事件等等，操作dom执行）
  >
  > - 定时器回调函数
  >
  > - ajax请求回调函数
  >
  > - 生命周期回调函数
  >
  >   ``````js
  >     // dom事件回调函数
  >    document.getElementById('btn').onclick = function () {alert(this.innerHTML)}
  >    // 定时器回调函数
  >    setTimeout(function () {   alert('到点了'+this)}, 2000)
  >   ``````

#### 4、IIFE (匿名函数自调用)

- 全称：IIFE (Immediately Invoked Function Expression) 立即调用函数表达式

- 好处：

  > - 防止全局变量污染，不会污染外部命名空间
  > - 隐藏实现
  > - 可以封装js模块

```js
 let a = 1
 let b = 2
 //不加分号 2 is not a function: 可能会被认为这样 2(IIFE)
 ;(function(b, c){ // 务必加分号
     console.log(a); // 1
     console.log(b); // 123
     console.log(c); // 456
 })(123, 456)
console.log(a); // 1
console.log(b); // 2
console.log(c); // c is not defined
```

#### 5、函数中的this

具体查看：https://www.jianshu.com/p/1faa863d4a4e

- 显示指定：谁调用就是谁
- 通过call/apply修改this指定谁调用：: xxx.call(obj)
- 不指定谁调用: xxx()  : window
- 回调函数: 看背后是通过谁来调用的: window/其它

```js
function Person(color) {
	console.log(this)
	this.color = color;
    this.getColor = function () {
     console.log(this)
     return this.color;
    };
    this.setColor = function (color) {
     console.log(this)
     this.color = color;
    };
}

Person("red"); //this是谁? window

const p = new Person("yello"); //this是谁? p

p.getColor(); //this是谁? p

const obj = {};
//调用call会改变this指向-->让我的p函数成为`obj`的临时方法进行调用
p.setColor.call(obj, "black"); //this是谁? obj

const test = p.setColor;
test(); //this是谁? window  -->因为直接调用了

function fun1() {
	function fun2() {  console.log(this); }
	fun2(); //this是谁? window
}
fun1(); //调用fun1
```

#### 6、函数也是对象

- instanceof  Object === true
- 函数有属性：prototype
- 函数有方法：call()和apply()修改this指向
- 可以添加新的属性、方法

#### 7、关于语句分号

- js语句后面分号可加可不加
- 分号必加情况
  - `小括号开头的前一条语句`
  - `中方括号开头的前一条语句`
  - 解决方案：在行首加分号

## 二、函数高级

### Ⅰ- 原型与原型链[#prototype]

#### 1、原型[prototype]

> 原型对象就相当于一个公共的区域，所有同一个类的实例（p1,p2等函数）都可以访问到这个原型对象
>
> 当我们访问一个对象函数的属性方法时，如果自身没有这个属性方法，就会去原型上面找，如果原型有就会直接调用
>
> 如果自身有这个属性方法就优先使用自己的
>
> <strong style="color:#DD5145">以后我们创建构造函数时，可以将这些对象共有的属性和方法，统一添加到构造函数的原型对象中，这样就不用分别为每一个对象添加，也不会影响到全局作用域，就可以使每一个对象都具有这些属性和方法了    </strong>

- 1、函数的`prototype`属性

  > - 我们创建的每一个函数（构造函数，普通函数），解析器都会向函数中添加一个属性prototype（原型对象）
  >
  > - 普通函数的prototype属性没有作用可以忽略
  >
  > - 原型对象中有一个属性constructor，它指向实例对象
  >
  >   ![image-20220726163849403](.\image\3.png)

- 2、 给原型对象添加属性(一般都是方法)

  - 作用: 函数的所有实例对象自动拥有原型中的属性(方法)

- 3、代码示例

  ``````js
    // 每个函数都有一个prototype属性, 它默认指向一个Object空对象(即称为: 原型对象)
    console.log(Date.prototype, typeof Date.prototype)
    function Fun () {//alt + shift +r(重命名rename)
  
    }
    console.log(Fun.prototype)  // 默认指向一个Object空对象(没有我们的属性)
  
    // 原型对象中有一个属性constructor, 它指向函数对象
    console.log(Date.prototype.constructor===Date)
    console.log(Fun.prototype.constructor===Fun)
  
    //给原型对象添加属性(一般是方法) ===>实例对象可以访问
    Fun.prototype.test = function () {
      console.log('test()')
    }
    var fun = new Fun()
    fun.test()
  ``````

#### 2、显式原型与隐式原型

- 1、每一个函数都有一个prototype属性，即显示原型

- 2、每一个实例对象都有一个[`__ proto __`]，即隐式原型

- 3、对象的隐式原型的值为其对象构造函数的显示原型的值

- 4、内存结构

  ![显式原型与隐式原型](.\image\显式原型与隐式原型.png)

- 5、总结：

  > 1、函数的[`prototype`]属性：在定义函数时，自动添加的，默认指向一个空Object对象
  >
  > ![原型总结1](.\image\原型总结1.png)
  >
  > 2、实例对象的`[__proto__]`属性：创建实例对象时自动添加的，默认值为构造函数的[`prototype`]属性值
  >
  > 3、可以直接操作显示原型，不推荐直接操作`[__proto__]`属性

  

- 6、代码示例

  ```js
   //定义构造函数
    function Fn() {   // 内部语句: this.prototype = {}
  
    }
    // 1. 每个函数function都有一个prototype，即显式原型属性, 默认指向一个空的Object对象
    console.log(Fn.prototype)
    // 2. 每个实例对象都有一个__proto__，可称为隐式原型
    //创建实例对象
    var fn = new Fn()  // 内部语句: this.__proto__ = Fn.prototype
    console.log(fn.__proto__)
    // 3. 对象的隐式原型的值为其对应构造函数的显式原型的值
    console.log(Fn.prototype===fn.__proto__) // true
    //给原型添加方法
    Fn.prototype.test = function () {
      console.log('test()')
    }
    //通过实例调用原型的方法
    fn.test()
  
  let Fn = function () {}
  let f1 = new Fn()
  console.log(f1.__proto__.__proto__.__proto__);//null  // f1的隐式原型 是Fn的显示原型 Fn的显示原型的隐式原型是Object,Object的隐式原型为null
  console.dir(Fn.__proto__);
  ```

#### 3、原型链

- 1、原型链

  - 每个对象都有原型，原型对象也是对象，所以它也有原型。以此类推成一个原型链

  - 访问一个对象时：

    > - 先在自身属性中查找，找到返回
    >
    > - 如果没有找到，再沿着[`__ proto __`]这条链进行查找，找到返回
    >
    > - 如果最终都没有找到，返回undefined
    >
    > - 别名： 隐式原型链
    >
    > - 作用：查找对象的属性方法
    >
    > - 原型链分析
    >
    >   ![原型链分析](.\image\原型链分析.png)

- 2、构造函数`a`、原型`b`、实例对象`c`的关系

  - 实例对象`c`的隐式原型是构造函数`a`的显示原型`c`

  - 原型`b`的constructor属性指向a

  - 图解

    ```js
    var o1 = new Object();
    var o2 = {};
    ```

    ![原型链分析](.\image\构造函数原型实例对象的关系1.png)

    ```js
    function Foo(){  }
    ```

    ![原型链分析](.\image\构造函数原型实例对象的关系2.png)


- 属性问题

  - 读取对象的属性时：会自动到原型链中查找

  - 设置对象属性时：不会查找原型链, 如果当前对象中没有此属性, 直接添加此属性并设置其值

  - 方法一般定义在原型中, 属性一般通过构造函数定义在对象本身上

  - 代码示例

    ![属性问题](.\image\4.png)

    ```js
    function Fun () {}
    Fun.prototype.a = 'xxx'
    var f1 = new Fun()
    console.log(f1.a, f1) //xxx Fun
    
    var f2 = new Fun() 
    f2.a = 'yyy'
    console.log(f1.a, f2.a, f2) //xxx yyy Fun {a: 'yyy'}
    
    function Person (name, age) {
        this.name = name
        this.age = age
    }
    Person.prototype.setName = function (name) {
        this.name = name
    }
    var p1 = new Person('张三', 20)
    p1.setName('李四')
    console.log(p1) // Person{name: '李四', age: 20}
    
    var p2 = new Person('王五', 21)
    p2.setName('哈哈哈')
    console.log(p1) // Person{name: '哈哈哈', age: 21}
    console.log(p1.__proto__ === p2.__proto__) // true
    ```

#### 4、instanceof

- instanceof是如何判断的？

  > - 表达式：A instanceof B
  >
  >   如果B函数的显示原型对象在A实例对象的原型链上，返回true，否则返回false

- Function是通过new自己产生的实例

- 案例

  ```js
  // 案例1
  function Foo () {}
  var f1 = new Foo()
  var f2 = new Function()
  var f3 = new Object()
  console.log(f1 instanceof Foo) // true
  console.log(f1 instanceof Object) // true
  console.log(f1 instanceof Function) // false
  console.log(f2 instanceof Function) // true
  console.log(f3 instanceof Function) // false
  
  // 案例二
  console.log(Object instanceof Function) // true
  console.log(Object instanceof Object) // true
  console.log(Function instanceof Function) // true
  console.log(Function instanceof Object) // true
  
  function Foo () {}
  console.log(Object instanceof Foo) // false
  ```

#### 5、相关题目

```js
/*
  测试题1
   */
function A () {}
A.prototype.n = 1
var b = new A()
A.prototype = {
    n: 2,
    m: 3
}
var c = new A()
console.log(b.n, b.m, c.n, c.m) // 1 undefined 2 3

/*
   测试题2
   */
function F (){}
Object.prototype.a = function(){
    console.log('a()')
}
Function.prototype.b = function(){
    console.log('b()')
}
var f = new F()
f.a() // a()
f.b() // f.b is not a function
F.a() // a()
F.b() // b()
console.log('f--->', f) // F{}
console.log('Object.prototype--->', Object.prototype)
console.log('Function.prototype--->', Function.prototype)  // 使用console.dir可以访问
```

![输出](.\image\5.png)

### Ⅱ、执行上下文与执行上下文栈

#### A、JavaScript 代码的执行流程

参考：https://blog.csdn.net/qq_41161604/article/details/120116104

<strong>1、JavaScript的执行机制：先编译，再执行    </strong>

![执行过程](.\image\Ⅱ-Ⅰ执行过程.png)

- JavaScript代码执行过程中，先进行编译，编译的时候会进行变量提升

  - 变量提升：在变量定义语句之前，就可以访问到这个变量（undefined）
  - 函数提升：在函数定义语句之前，就可以执行该函数
  - 先有变量提升，再有函数提升<strong style="color: #f00">（提升如果变量名和函数名重复，函数会覆盖变量的值，但是赋值的时候是在后，变量的赋值就会覆盖函数，最终显示的还是变量）</strong>

- 在编译阶段，`变量和函数`会被存放到变量环境中，变量的默认值会被设置为undefined；在代码执行阶段，JavaScript引擎会从变量环境中查找自定义的变量和函数

- 如果在编译阶段，同一个上下文中存在两个相同的变量名或者函数，那后定义的会覆盖掉之前定义的

- 作用域：作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。

- JavaScript是**词法作用域**（**静态作用域**）：函数的作用域在函数**定义**的时候就决定了

  ![执行过程](.\image\Ⅱ-Ⅰ执行过程2.png)

#### B、执行上下文与执行上下文栈

##### 1、变量提升与函数提升

- 变量提升

  - 通过`var`定义的变量，在定义语句之前就可以访问到
  - 值：undefined

- 函数提升

  - 通过function声明的函数，在函数定义之前就可以访问
  - 值：函数

- 代码示例：

  ***函数和变量同名，最后显示的变量的值***
  
  ```js
  console.log(fu); // fu()提升的时候 函数覆盖了同名的变量
  function fu () {
      console.log('我是函数');
  }
  var fu = '我是字符串'
  console.log('===', fu) // 我是字符串 提升之后执行，又把同名的函数覆盖了
  console.log(a); // undefined
  console.log(b); // Cannot access 'b' before initialization 
  var a = '1'
  let b = 2 // let创建的变量不能提升
  console.log(fun2); // undefined 
  fun2() //  fun2 is not a function  不能调用，表达式创建的是变量提升
  var fun2 = function () {
      console.log('我是通过表达式创建的');
  }
  ```

##### 2、执行上下文（代码执行的环境）

- 根据创建位置分类
  - 全局代码
  - 局部（函数）代码

- 全局执行上下文

  - 在执行代码前

  - 将window确定为全局执行上下文对全局

  > - 对全局数据进行预处理
  >
  > - var定义的全局变量===》undefined，添加为window的属性
  > - function声明的全局函数===》赋值（fun），添加为window的方法
  > - this===》赋值（window）

  - 开始执行全局代码

  - 代码示例

    ```js
    console.log(a1, window.a1) // undefined undefined
    window.a2() // a2()
    console.log(this) // window
    
    var a1 = 3
    function a2() {
        console.log('a2()')
    }
    console.log(a1) // 3
    ```

- 函数（局部）执行上下文

  - 在调用函数，准备执行函数体之前，创建对应的函数执行上下文对象（虚拟的，存在于栈中）

  - 对局部的数据进行预处理

    > - 形参变量===》赋值（实参）===》添加为执行上下文的属性
    > - arguments===》赋值（实参列表），添加为执行上下文的属性
    > - var定义的局部变量===》undefined，添加为执行上下文的属性
    > - function声明的函数===》赋值（fun），添加为执行上下文的属性
    > - this==》赋值（调用函数的对象）

  - 开始执行函数体代码

  - 代码示例

    ```js
    a2()
    var obj = {
        b1: 10,
        a1: a2
    }
    console.log('=====我是分割线========')
    obj.a1()
    function a2() {
        console.log('this===', this) // 第一个this是window，第二个是obj
        console.log(b1); // undefined undefined
        b2()
        var b1 = 1
        function b2 () {
            console.log('我是b2,我运行了');
        }
        console.log(b1); // 1 1
    }
    ```

    ![输出](.\image\Ⅱ-执行上下文1.png)

##### 3、执行上下文栈（后进先出）

- 在全局代码执行前，JS引擎就会创建一个栈来存储管理所有的执行上下文对象

- 在全局执行上下文（window）确定后，将其添加到栈中（进栈）

- 在当前函数执行上下文创建后，将其添加到栈中（进栈）

- 当前函数执行完后，将栈定的对象移除（出栈）

- 当所有的代码执行完成后，栈中只剩下了window

- **`上下文栈数 = 函数调用数 + 1`**

- 代码示例

  ```js
  var a = 10
  var bar = function (x) {
    var b = 5
    foo(x + b)
  }
  var foo = function (y) {
    var c = 5
    console.log(a + c + y)
  }
  bar(10)
  ```

- 图示：

  ![执行上下文栈](.\image\Ⅱ-执行上下文栈1.png)

- 相关题目

  ```js
  /*
  1. 依次输出什么?
  gb: undefined
  fb: 1
  fb: 2
  fb: 3
  fe: 3 // fe第一次输出是因为foo3执行完成了就销毁，走到了函数后面的console，下面fe:2,fe:1一样
  fe: 2
  fe: 1
  ge: 1
  2. 整个过程中产生了几个执行上下文?  5
  */
  console.log('gb: '+ i) // undefined
  var i = 1
  foo(1)
  function foo(i) {
    if (i == 4) {
      return
    }
    console.log('fb:' + i)
    foo(i + 1) //递归调用: 在函数内部调用自己
    console.log('fe:' + i)
  }
  console.log('ge: ' + i) // 1
  ```

![执行上下文栈](.\image\执行栈与事件队列.gif)

- 题目2：

  ```
    /*
     测试题1:  先执行变量提升, 再执行函数提升
     */
    function a() {}
    var a
    console.log(typeof a) // 'fucntion'，这里是因为变量没有赋值
  
    /*
     测试题2:
     */
     // b = undefined 提升到了这里
    if (!(b in window)) {
      var b = 1 // b先执行了变量提升
    }
    console.log(b) // undefined
  
    /*
     测试题3:
     */
    var c = 1
    function c(c) {
      console.log(c)
      var c = 3
    }
    c(2) //报错 这里的c是1所以  c is not a function
  
  ```

### Ⅲ、作用域与作用域链

#### 1、作用域

- 理解

  - 作用域就是一块“地盘”，一个代码所在的区域
  - 它是静态的（相对于上下文对象），**定义**的时候就决定了（词法作用域）

- 分类

  - 全局作用域
  - 函数作用域
  - 块作用域ES6使用let，const`（这里的为块作用域）`

- 作用

  - 隔离变量，防止变量名污染（不同作用域下同名变量不会有冲突）

- 代码示例：

  ```js
  var a = 10,b = 20
  function fn(x) {
    var a = 100,  c = 300;
    console.log('fn()', a, b, c, x) // 100 20 300 10
    function bar(x) {
      var a = 1000, d = 400
      console.log('bar()', a, b, c, d, x) //bar() 1000 20 300 400 100
        									//bar() 1000 20 300 400 200
    }
  
    bar(100)
    bar(200)
  }
  fn(10)
  ```

#### 2、作用域与执行上下文

- 区别

  > 区别1
  >
  > > - 全局作用域之外，每个函数都会创建自己的作用域，作用域在函数***`定义`***时就已经确定了。而不是在函数执行的时候创建
  > > - 全局执行上下文环境是在全局作用域确定之后，js代码马上执行之前创建
  > > - 函数执行上下文是在***`调用函数`***时，函数体代码执行之前创建
  >
  > 区别2
  >
  > > - 作用域是***`静态的`***，只要函数定义好了就一直存在，且不会再变化
  > >
  > > - 执行上下文是***`动态的`***，调用函数时创建，函数调用结束时就会自动释放

- 联系

  > - 执行上下文（对象）是从属于所在的作用域（仔细查看下方图片）
  > - 全局上下文环境===》全局作用域
  > - 函数上下文环境===》对应的函数`使用域`

  ![执行上下文栈](.\image\作用域与执行上行文.png)

#### 3、作用域链

- 理解
  - 多个上下级关系的作用域形成的链，它的方向是从下向上的（从内到外）
  - 查找变量时就是沿着作用域链来查找的
- 查找一个变量的规则
  - 在当前作用域下的执行上下文中查找对应的属性，如果有直接返回，否则进入2
  - 在上一级作用域的执行上下文中查找对应的属性，如果有直接返回，否则进入3
  - 再次执行2的相同操作，直到全局作用域，如果还找不到就抛出找不到的异常

```js
var a = 1
function fn1() {
  var b = 2
  function fn2() {
    var c = 3
    console.log(c) // 3
    console.log(b) // 2
    console.log(a) // 1
    console.log(d) // d is not defined
  }
  fn2()
}
fn1()
```

![执行上下文栈](.\image\作用域与执行上行文2.png)

- 相关题目

  ####  `作用域在函数定义时就已经确定了。而不是在函数调用时`

  ```js
  var x = 10;
  function fn() {
      console.log(x); // 10
  }
  function show(f) {
      var x = 20;
      f();
  }
  show(fn);
  ```

####        ` 对象变量不能产生局部作用域`

```js
var fn = function () {
  console.log(fn) // ƒ () {console.log(fn)} 
}
fn()

var obj = {
  fn2: function () {
    console.log(fn2)//  fn2 is not defined
    //console.log(this.fn2) // function () { console.log(fn2)}
  }
}
obj.fn2()
```

### Ⅳ、闭包

#### 1、引出闭包

-  点击某个按钮, 提示"点击的是第n个按钮"

  - #### `错误使用`

  ```js
  /*
  <button>测试1</button>
  <button>测试2</button>
  <button>测试3</button>
  */
  var btns = document.getElementsByTagName('button')
  for (var i = 0,length=btns.length; i < length; i++) {
    var btn = btns[i]
    btn.onclick = function () {
      alert('第'+(i+1)+'个') // 全是4
    }
  }
  // 上面代码相当于
  for (var i = 0,length=btns.length; i < length; i++) {
    var btn = btns[i]
    btn.onclick = btnClick
  }
  function btnClick (){
      // 这个i就是全局的i,4
      // i 的作用域在函数创建的时候就确定了
     alert('第'+(i+1)+'个') // 全是4
  }
  ```

  - #### 利用自身属性来解决

    ```js
    for (var i = 0,length=btns.length; i < length; i++) {
      var btn = btns[i]
      btn.index = i
      btn.onclick = btnClick
    }
    function btnClick (){
        console.log('this', this.index) // 这里的this，是获取调用的地方，btn
       alert('第'+(this.index)+'个')// 1 2 3
    }
    ```

  - #### 利用闭包解决

    ```js
    // 利用闭包
    for (var i = 0,length=btns.length; i < length; i++) {
      (function(j){
        var btn = btns[j]
        btn.onclick = function (){
            alert('第'+(j + 1)+'个') // 1 2 3
        }
      })(i)
    }
    ```

#### 2、理解闭包

- 如何产生闭包
  
  - 当一个嵌套的子（内部）函数，引用嵌套的外部（父）函数的变量（函数），并且这个子函数在父函数中被调用时，就产生了闭包
  
    ```js
    function fn1 () {
        var a = 2
        var b = 'abc'
        function fn2 () { 
          console.log(a)
        }
        // fn2() 情况1
        fn2() // 情况2
      }
      fn1()
    ```
  
    情况1
  
    ![闭包1](.\image\Ⅳ-闭包1.png)
  
    情况2
  
    ![闭包2](.\image\Ⅳ-闭包2.png)

#### 3、常见的闭包

- ##### 将函数作为另一个函数的返回值

  ```js
  function fn () {
    var a = 2 //使用函数内部的变量在函数执行完后, 仍然存活在内存中(延长了局部变量的生命周期)
    function fn2(){
      a++
      console.log(a);
    }
    return fn2
  }
  var f1 = fn()
  f1() // 3
  f1() // 4
  ```

  

- ##### 将函数作为实参传递给另一个函数调用

  ```js
  function showDelay(msg, time) {
      setTimeout(function () {
          alert(msg)
      }, time)
  }
  showDelay('你好呀', 2000)
  ```

#### 4、闭包的作用

- 使用函数内部的变量在函数执行完后，仍然存活在内存中（延长了局部变量的生命周期）
- 让函数外部可以操作（读写）到函数内部的数据（变量/函数）

#### 5、闭包的生命周期

- 产生：在嵌套内部函数定义执行时就产生了

- 死亡：在嵌套的内部函数成为垃圾对象时

  ```js
    function fn1() {
      //此时闭包就已经产生了(函数提升, 内部函数对象已经创建了)
      var a = 2
      function fn2 () {
        a++
        console.log(a)
      }
      return fn2
    }
    var f = fn1()
    f() // 3
    f() // 4
    f = null //闭包死亡(包含闭包的函数对象成为垃圾对象)
  ```

#### 6、闭包的应用

- 定义JS模块

  > - 具有特定功能的js文件（例如jQuery等）
  > - 将所有的数据和功能都封装在一个函数内部（私有的）
  > - 只向外暴露一个包含n个方法的对象或函数
  > - 模块的使用者，只需要通过模块暴露的对象调用方法实现对应的功能

  ```js
  // myModule.js
  function myModule() {
    //私有数据
    var msg = 'My atguigu'
    //操作数据的函数
    function doSomething() {
      console.log('doSomething() '+msg.toUpperCase())
    }
    function doOtherthing () {
      console.log('doOtherthing() '+msg.toLowerCase())
    }
  
    //向外暴露对象(给外部使用的方法)
    return {
      doSomething: doSomething,
      doOtherthing: doOtherthing
    }
  }
  ```

  ```html
  <script type="text/javascript" src="myModule.js"></script>
  <script type="text/javascript">
    var module = myModule()
    module.doSomething()
    module.doOtherthing()
  </script>
  ```

#### 7、闭包的缺点及解决

- 缺点
  - 函数执行完成后，函数内的局部变量没有释放，占用内存时间会变长
  - 容易造成内存泄漏

- 解决
  - 能不使用闭包就不使用
  - 使用完成后及时释放（具体看上面5、闭包的生命周期）

#### 8、内存溢出与内存泄漏

- ##### 内存溢出

  - 一种程序运行出现的错误

  - 当程序运行需要的内存超过了剩余的内存时，就会抛出内存溢出的错误

- ##### 内存泄漏

  - 占用的内存没有及时释放

  - ***内存泄漏积累多了就容易导致内存溢出***

- 常见的内存泄漏

  - 意外的全局变量
  - 没有及时清理的计时器或回调函数
  - 闭包

```html
<script type="text/javascript">
 // 1. 内存溢出
 var obj = {}
 for (var i = 0; i < 10000; i++) {
   obj[i] = new Array(10000000)
   console.log('-----')
 }

 // 2. 内存泄露
   // 意外的全局变量
 function fn() {
   a = new Array(10000000)  //不使用var let const去承接
   console.log(a)
 }
 fn()

  // 没有及时清理的计时器或回调函数
 var intervalId = setInterval(function () { //启动循环定时器后不清理
   console.log('----')
 }, 1000)

 // clearInterval(intervalId)

   // 闭包
 function fn1() {
   var a = 4
   function fn2() {
     console.log(++a)
   }
   return fn2
 }
 var f = fn1()
 f()
 // f = null

</script>
```

#### 9、相关题目

```js
  //代码片段一
  var name = "The Window";
  var object = {
    name : "My Object",
    getNameFunc : function(){
      return function(){
        return this.name;
      };
    }
  };
// 内部函数里的this是window
// 没有产生闭包，因为内部函数没有调用外部函数的变量
  console.log(object.getNameFunc()()); // The Window


  //代码片段二
  var name2 = "The Window";
  var object2 = {
    name2 : "My Object",
    getNameFunc : function(){
      var that = this; // that是object2
      return function(){
        return that.name2;
      };
    }
  };
// 产生了闭包，使用了外部函数变量
  console.log(object2.getNameFunc()()); // My Object
```

题目2

```js
function fun(n,o) {
    console.log(o)
    return {
        fun:function(m){
            return fun(m,n)
        }
    }
}
var a = fun(0) // undefined
a.fun(1) // 0
a.fun(2) // 0 
a.fun(3) // 0

var b = fun(0).fun(1).fun(2).fun(3) // undefined 0 1 2

var c = fun(0).fun(1) // undefined 0
c.fun(2) // 1 -->经过上方定义后 n固定为1
c.fun(3) // 1
```



## 三、对象高级

### Ⅰ- 对象创建模式

#### 1、Object构造函数模式

- 方式一：Object构造函数模式

  > 套路：先创建空Object对象，再动态添加属性/方法
  >
  > 适用场景：起始时不确定对象内部数据
  >
  > 问题：语句太多

  ```js
  // 一个人：name: '张三'， age: 10
  // 先创建空Object对象
  var p = new Object()
  p.name = '张三'
  p.age = 10
  p.sayInfo = function () {
      console.log('我叫'+ p.name + '，我今年' + p.age +'岁了')
  }
  p.setName = function (name) {
      this.name = name
  }
  p.sayInfo() // 我叫张三，我今年10岁了
  p.setName('李四')
  console.log(p.name) // 李四
  ```

#### 2、对象字面量

- 方式二：对象字面量模式

  > 模式：使用{}创建对象
  >
  > 适用场景：起始时对象内部数据确定，可以赋初始值（其他和new Object没有区别）
  >
  > 问题：创建多个对象，有重复代码

  ```js
  var p = {
      name: '张三',
      age: 10,
      setName: function (name) {
      	this.name = name
  	}
  }
  p.sayInfo = function () { // 也可以使用.的方式添加属性和方法
      console.log('我叫'+ p.name + '，我今年' + p.age +'岁了')
  }
  ```

#### 3、工厂模式

- 方式三：工厂模式

  > 套路：通过工厂函数动态创建对象并返回
  >
  > 适用场景：需要创建多个对象
  >
  > 问题：对象没有一个具体的类型，都是Object类型

  ```js
  function createPerson(name, age) {
      var obj = {
          name: name,
          age: age,
          setName: function (name) {
              this.name = name
          }
      }
      return obj
  }
  var p1 = createPerson('张三', 10)
  var p1 = createPerson('李四', 11)
  ```

#### 4、自定义构造函数模式

- 方式四：自定义构造函数模式

  > 套路：自定义构造函数，通过new创建对象
  >
  > 适用场景：需要创建多个类型确定的对象
  >
  > 问题：每个对象都有相同的数据，浪费内存

  ```js
   //定义类型
    function Person(name, age) {
      this.name = name
      this.age = age
      this.setName = function (name) {
        this.name = name
      }
    }
    var p1 = new Person('Tom', 12)
    p1.setName('Jack')
    console.log(p1.name, p1.age)
    console.log(p1 instanceof Person)
  
    function Student (name, price) {
      this.name = name
      this.price = price
    }
    var s = new Student('Bob', 13000)
    console.log(s instanceof Student)
  
    var p2 = new Person('JACK', 23)
    console.log(p1, p2)
  
  ```

#### 5、构造函数+原型的组合模式

- 方式五：构造函数+原型的组合模式

  > 套路：自定义构造函数，属性在函数中初始化，方法添加到原型上
  >
  > 适用场景：需要创建多个类型确定的对象

  ```js
    function Person(name, age) { //在构造函数中只初始化一般函数
      this.name = name
      this.age = age
    }
    Person.prototype.setName = function (name) {
      this.name = name
    }
  
    var p1 = new Person('Tom', 23)
    var p2 = new Person('Jack', 24)
    console.log(p1, p2)
  
  ```

  

### Ⅱ、继承模式

#### 1、原型链继承

- 方式：原型链继承

  > 套路
  >
  > > - 1、定义父类型构造函数
  > > - 2、给父类型的原型添加方法
  > > - 3、定义子类型的构造函数
  > > - 4、创建父类型的对象赋值给子类型的原型
  > > - 5、将子类型原型的构造属性设置为子类型
  > > - 6、给子类型原型添加方法
  > > - 7、创建子类型的对象：可以调用父类型的方法
  >
  > 关键
  >
  > > - `子类型的原型为父类型的一个实例对象`

  ```js
  // 父类型
  function Parent () {
      this.parentProp = 'Parent property'
  }
  
  Parent.prototype.showParentProp = function () {
      console.log(this.parentProp)
  }
  
  // 子类型
  function Child () {
      this.childProp = 'child property'
  }
  
  // 子类型的原型为父类型的一个实例对象
  Child.prototype = new Parent() // 这里导致Child的prototype的constructor属性指向了Parent
  
  // 让子类型的原型的constructor指向子类型
  
  Child.prototype.constructor = Child
  
  Child.prototype.showChildProp = function () {
      console.log(this.childProp)
  }
  var fn = new Child()
  
  fn.showParentProp()
  
  console.log(fn)
  ```

  ![原型链继承](.\image\原型链继承1.png)

#### 2、借用构造函数继承

- 方式2：借用构造函数继承（假的）

  > 1、套路
  >
  > > - 定义父类型构造函数
  > > - 定义子类型构造函数
  > > - 在子类型构造函数中调用父类型构造
  >
  > 2、关键：
  >
  > > - 在子类型构造函数中通用call()调用父类型构造函数
  >
  > 3、作用：
  >
  > > - 能借用父类中的构造方法，但是不灵活

  ```js
  function Person(name, age) {
      this.name = name
      this.age = age
  }
  function Student(name, age, city){
    //此处利用call(),将 [Student]的this传递给Person构造函数
      Person.call(this, name, age)  // 相当于: this.Person(name, age)
      /*this.name = name
      this.age = age*/
      this.city = city
  }
  var p1 = new Student('张三', 10, '北京')
  console.log(p1.name, p1.age, p1.city)
  ```

- ##### [`Person`]中的this是动态变化的,在[`Student`]中利用[`Person.call(this, name, age)`]改变了其this指向,所以可以实现此效果

#### 3、组合继承

- 方式三：原型链+借用构造函数的组合继承

  > - 1、利用原型链失效对父类型对象的方法继承
  > - 2、利用super()借用父类型构建函数初始化相同属性

  ```js
    function Person(name, age) {
      this.name = name
      this.age = age
    }
    Person.prototype.setName = function (name) {
      this.name = name
    }
  
    function Student(name, age, price) {
      Person.call(this, name, age)  // 为了得到属性
      this.price = price
    }
    Student.prototype = new Person() // 为了能看到父类型的方法
    Student.prototype.constructor = Student //修正constructor属性
    Student.prototype.setPrice = function (price) {
      this.price = price
    }
  
    var s = new Student('Tom', 24, 15000)
    s.setName('Bob')
    s.setPrice(16000)
    console.log(s.name, s.age, s.price)
  ```

## 四、线程机制与事件机制

<a name="线程机制与事件循环机制" href="./JavaScript线程机制与事件循环机制.md#test">线程机制与事件循环机制---点我传送</a>

### Ⅰ- 进程与线程

![进程与线程](.\image\进程与线程1.png)

#### 1、进程

- 程序的一次执行，它占有一片独有的内存空间
- 可以通过windows任务管理器查看进程
- 可以看出每个程序的内存空间是相互独立的
- 进程是CPU资源分配的最小单位

![进程与线程](.\image\进程与线程2.png)

#### 2、线程

- 是进程内的一个独立执行单元
- 是程序执行的一个完成流程
- 是CPU的最小的调度单元

#### 3、进程与线程

- 应用程序必须运行在某个进程的线程上
- 一个进程中一般至少有一个运行的线程：主线程（进程启动后自动创建）
- 一个进程中也可以同时运行多个线程：此时我们会说这个程序时多线程运行的
- 一个进程内的数据可以供其中的多个线程直接共享
- 多个线程之间的数据是不能直接共享的（内存相互独立隔离）

#### 4、引出的问题

- 何为多进程与多线程？

  > 多进程运行：一个应用程序可以同时启动多个实例运行
  >
  > 多线程：在一个进程内，同时有多个线程运行

- 比较单线程与多线程

  > 多线程：
  >
  > - 优点：能有效提升CPU的利用率
  >
  > - 缺点
  >
  >   > 创建多线程开销
  >   >
  >   > 线程间切换开销
  >   >
  >   > 死锁与状态同步问题
  >
  > 单线程
  >
  > - 优点：顺序编程，简单易懂
  > - 缺点：效率低

- ##### JS是单线程还是多线程？

  > js是单线程运行的，但使用H5中的web workers可以多线程运行
  >
  > - 只能由一个线程去操作DOM界面
  >
  > - 如何证明js执行是单线程的？
  >
  >   > - setTimeout()的回调函数是在主线程执行的
  >   >
  >   > - 定时器回调函数只有在运行栈中的代码全部执行完后才有可能执行
  >
  > - 为什么js要用单线程模式，而不用多线程模式？
  >
  >   > - JavaScript的单线程，与它的用途有关
  >   > - 作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM
  >   > - 这决定了它只能是单线程，否则会带来很复杂的同步问题

- 浏览器运行是单线程还是多线程？

  > 都是多线程运行的

- 浏览器运行是单进程还是多进程？

  > 单进程：
  >
  > - Firefox
  > - 老版IE

  >多线程：
  >
  >- Chrome
  >
  >- 新版IE

  

### Ⅱ、浏览器内核

> 支持浏览器运行的最核心的程序

#### 1、不同浏览器的内核

>- Chrome，Safari：webkit
>
>- Firefox：Gecko
>
>- IE：Trident
>
>- 360，搜狗等国内浏览器：trident+webkit

2、内核组成模块

> 主线程
>
> > - 1、js引擎模块：负责js程序的编译与运行
> > - 2、html,css文档解析模块：负责页面文本的解析
> > - 3、dom/css模块：负责dom/css在内存中的相关处理
> > - 4、布局和渲染模块：负责页面的布局和效果的绘制
>
> 分线程
>
> >定时器模块：负责定时器的管理
> >
> >网络请求模块：负责服务器请求（常规/Ajax）
> >
> >事件响应模块：负责事件的管理

### Ⅲ、定时器引发的思考

https://www.sohu.com/a/275659421_463987

#### 1、定时器真是定时执行的吗？

```html
<body>

<button id="btn">启动定时器</button>
<script type="text/javascript">

  document.getElementById('btn').onclick = function () {
    var start = Date.now()
    console.log('启动定时器前...')
    setTimeout(function () {
      console.log('定时器执行了', Date.now()-start)
    }, 200)
    console.log('启动定时器后...')

    // 做一个长时间的工作
    for (var i = 0; i < 1000000000; i++) {

    }
  }
</script>
</body>
```

- 定时器真是定时执行的吗?

  > - 定时器并不能保证真正定时执行
  > - 一般会延迟一丁点(可以接受), 也有可能延迟很长时间(不能接受)

  * 定时器回调函数是在分线程执行的吗？

    > - 在主线程执行的, js是单线程的

- 定时器是如何实现的?

  > 事件循环模型(后面讲)

#### 2、js是单线程的

- 如何证明js执行是单线程的

  > setTimeout()的回调函数是在主线程执行的
  >
  > 定时器回调函数只有在运行栈中的代码全部执行完成后才有可能执行

```js
// 如何证明JS执行是单线程的
setTimeout(function () { //4. 在将[timeout 1111]弹窗关闭后,再等一秒 执行此处
   console.log('timeout 2222')
   alert('22222222')
 }, 2000)
 setTimeout(function () { //3. 过了一秒后 打印 timeout 1111并弹窗,此处如果不将弹窗关闭,不会继续执行上方222
   console.log('timeout 1111')
   alert('1111111')
 }, 1000)
 setTimeout(function () { //2. 然后打印timeout() 00000
   console.log('timeout() 00000')
 }, 0)
 function fn() { //1. fn()
   console.log('fn()')
 }
 fn()
//----------------------
 console.log('alert()之前')
 alert('------') //暂停当前主线程的执行, 同时暂停计时, 点击确定后, 恢复程序执行和计时
 console.log('alert()之后')
```

> 流程结果:
>
> 1. 先打印了[`fn()`],然后马上就打印了[`timeout() 00000`]
> 2. 过了一秒后 打印 timeout 1111并弹窗,此处如果不将弹窗关闭,不会继续执行上方222
> 3. 在将[timeout 1111]弹窗关闭后,`再等一秒` 执行此处
>
>   - 问:为何明明写的是2秒,却关闭上一个弹窗再过一秒就执行?
>   - 解:并不是关闭后再计算的,而是一起计算的,alert只是暂停了主线程执行

- JS引擎执行代码的基本流程与代码分类

  > 代码分类:
  >
  > - 初始化代码
  > - 回调代码
  >
  > js引擎执行代码的基本流程
  >
  > 1. 先执行初始化代码: 包含一些特别的代码   回调函数(异步执行)
  >
  >  * 设置定时器
  >  * 绑定事件监听
  >  * 发送ajax请求
  >
  > 2. 后面在某个时刻才会执行回调代码

- 为什么js要用单线程模式，而不是多线程模式？

  >1. JavaScript的单线程，与它的用途有关。
  >2. 作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。
  >3. 这决定了它只能是单线程，否则会带来很复杂的同步问题
  >   * 举个栗子:如果我们要实现更新页面上一个dom节点然后删除,用单线程是没问题的
  >   * 但是如果多线程,当我删除线程先删除了dom节点,更新线程要去更新的时候就会出错