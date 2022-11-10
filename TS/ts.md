https://juejin.cn/post/7018805943710253086?share_token=c4029c41-9cc7-4e39-9e2d-c57b6617d2d8

# 第一章快速入门

## 1、TypeScript简介

1.1、 TypeScript是JavaScript的超集

1.2、它对JS进行了扩展，向JS中引入了类型的概念，并添加了许多新的特性

1.3、TS代码需要通过编译器变异为JS，然后再交由JS解析器执行

1.4、TS完全兼容JS，换言之，任何的TS代码都可以直接当初JS使用

1.5、相较于JS而言，

- TS拥有了静态类型，更加严格的语法，更强大的功能；
- TS可以在代码执行前就完成代码的检查，减小了运行时异常的出现几率；
- TS代码可以编译为任意版本的JS代码，可有效解决不同JS运行环境的兼容问题
- 同样的功能，TS的代码量哟啊大于JS，但由于TS的代码结构更加清晰，变量类型更加明确，在后期代码的维护中TS远远胜于JS

## 2、TypeScript开发环境搭建

2.1、进入nodejs官网下载node.js：https://nodejs.org/en/      http://nodejs.cn/

2.2、安装node

2.3、使用npm全局安装typescript

- 进入命令行
- 输入：npm i -g typescript
- 查看是否安装成功： typescript -v

2.4、创建一个ts文件

2.5、使用tsc对ts文件进行编译

- 进入命令行
- 进入当前ts文件所在目录
- 执行命令：tsc xxx.ts

## 3、基本类型

- 类型声明

  - 类型声明是TS非常重要的一个特点

  - 通过类型声明可以指定TS中变量（参数、形参）的类型

  - 指定类型后，当为变量赋值时，TS编译器会自动检查值是否符合类型声明，符合则赋值，否则报错

  - 简而言之，类型声明给变量设置了类型，使得变量只能存储某种类型的值

  - 语法：

    ```typescript
    let 变量: 类型;
    let 变量: 类型 = 值;
    function fn(参数: 类型, 参数: 类型): 类型{
    	...
    }
    ```

    ```typescript
    let a:number
    // a只能是number类型
    a = 10
    a = 100
    // a = 'aaa'  // a只能是number类型
    
    let b:string
    // b = 1
    b = '111'
    
    var c:Boolean
    c = false
    
    // 给变量参数定义 a:number, b:number 
    // :number 给返回值定义类型
    function sum(a:number, b:number):number {
        return a + b
    }
    sum(11, 22)
    ```

- 自动类型判断

  - TS拥有自动的类型判断机制
  - 当对变量的声明和赋值是同时进行的，TS编译器会自动判断变量的类型
  - 所以如果你的变量声明和赋值同时进行，可以省略类型声明

- 类型：

  |  类型   |       例子        |                            描述                             |
  | :-----: | :---------------: | :---------------------------------------------------------: |
  | number  |     1,-1,1.1      |                          任意数字                           |
  | string  |    'hi', '123'    |                         任意字符串                          |
  | boolean |    true,false     |                      布尔值true或false                      |
  | 字面量  |      其本身       | 限制变量的值就是该字面量的值（类似const，定义之后不能更改） |
  |   any   |         *         |                          任意类型                           |
  | unknown |         *         |                        类型安全的any                        |
  |  void   | 空值（undefined） |    没有值（或undefined）（函数返回值定义就是没有返回值）    |
  |  never  |      没有值       |                        不能是任何值                         |
  | object  |  {name: '张三'}   |                        任意的JS对象                         |
  |  array  |      [1,2,3]      |                         任意JS数组                          |
  |  tuple  |       [4.5]       |               元组，TS新增类型，固定长度数组                |
  |  enum   |     enum{a,b}     |                      枚举，TS新增类型                       |

- number

  ```typescript
  let num1: number = 6;
  let num2: number = 0xf00d;
  let num3: number = 0b1010;
  let num4: number = 0o744;
  let big: bigint = 100n;
  ```

- boolean

  ```typescript
  let isTrue: boolean = false;
  ```

- string

  ```typescript
  let color: string = "green";
  color = "red"
  
  let fullName: string = `张三`;
  let age: number = 37;
  let sentence: string = `Hello, my name is ${fullName}.
  I'll be ${age + 1} years old next month.`;
  ```

- 字面量

  也可以使用字面量去指定变量的类型，通过字面量可以确定变量的取值范围

  ```typescript
  let color: 'red' | 'blue' | 'black';
  let num: 1 | 2 | 3 | 4 | 5;
  ```

- any（尽量不推荐使用，会污染其他变量类型）

  ```typescript
  let d: any = 4;
  d = 'hello';
  d = true;
  ```

- unknown

  ```typescript
  let notSure: unknown = 4;
  notSure = 'hello'
  ```

- void

  ```typescript
  let unusable: void = undefined
  ```

- object（没什么用）

  ```typescript
  let obj: object = {}
  ```

- array

  ```typescript
  let list: number[] = [1, 2, 3];
  let list: Array<number> = [1, 2, 3];
  ```

- tuple（元组）

  ```typescript
  let x: [string, number];
  x = ["hello", 10]; 
  ```

- enum

  ```typescript
  enum Color {
    Red,
    Green,
    Blue,
  }
  let c: Color = Color.Green;
  
  enum Color {
    Red = 1,
    Green,
    Blue,
  }
  let c: Color = Color.Green;
  
  enum Color {
    Red = 1,
    Green = 2,
    Blue = 4,
  }
  let c: Color = Color.Green;
  ```

- 类型断言

  有些情况下，变量的类型对于我们来说是很明确的，但是TS编译器却并不清楚，此时，可以通过类型断言来告诉编译器变量的类型，断言有两种形式：

  - 第一种

    ```typescript
    let someValue: unknown = "this is a string";
    let strLength: number = (someValue as string).length;
    ```

  - 第二种

    ```typescript
    xxxxxxxxxx let someValue: unknown = "this is a string";let strLength: number = (<string>someValue).length;
    ```

```typescript
// 也可以直接使用字面量进行类型声明
let a: 10;
a = 10;

// 可以使用 | 来连接多个类型（联合类型）
let b: "male" | "female";
b = "male";
b = "female";

let c: boolean | string;
c = true;
c = 'hello';

// any 表示的是任意类型，一个变量设置类型为any后相当于对该变量关闭了TS的类型检测
// 使用TS时，不建议使用any类型
// let d: any;

// 声明变量如果不指定类型，则TS解析器会自动判断变量的类型为any （隐式的any）
let d;
d = 10;
d = 'hello';
d = true;
a=d // 改变了a的类型，所以不建议使用原因2

// unknown 表示未知类型的值
let e: unknown;
e = 10;
e = "hello";
e = true;

let s:string;

// d的类型是any，它可以赋值给任意变量
// s = d;

e = 'hello';

// unknown 实际上就是一个类型安全的any
// unknown类型的变量，不能直接赋值给其他变量
if(typeof e === "string"){
    s = e;
}

// 类型断言，可以用来告诉解析器变量的实际类型
/*
* 语法：
*   变量 as 类型
*   <类型>变量
*
* */
s = e as string;
s = <string>e;

// void 用来表示空，以函数为例，就表示没有返回值的函数
function fn(): void{
}

// never 表示永远不会返回结果
function fn2(): never{
    throw new Error('报错了！');
}
```

- 对象类型的声明

```typescript
// object表示一个对象
let a4: Object;
a4 = {};
a4 = function () {}

// {}用来指定对象中可以包含哪些属性
// 语法：{属性名:属性值,属性名:属性值}
let b4: {name: string, age: number}

// 必须完全符合上面定义的，不能多也不能少
// b4 = {name: '张三', hobby: ''}  // "{ name: string; }" 中缺少属性 "age"，但类型 "{ name: string; age: number; }" 中需要该属性  “hobby”不在类型中
b4 = {name: '张三', age: 18}

// 在属性名后边加上?，表示属性是可选的
let c5: {name: string, age?: number}
c5 = {name: '李四'} 

// 多个属性，不一一定义， [prorpName: string]: unknown  表示任意类型的属性
//name必须定义，其他属性可选
let d5: {name: string, [propName: string]: unknown}
d5 = {name: '王五', hobby: ['学习', '睡觉', '打豆豆']}

/* 设置函数结构的类型声明
    语法： (形参: 类型, 形参: 类型)=>返回值
*/
let sum4 : (nm1: number, num2: number) => number

sum4 = function (a, b) {
    return a + b
}


/**
 * 数组的类型声明
 *  类型 []
 *  Array <类型>
 * * */
// string类型的数组
let arr4: Array <string>
arr4 = ['1', '2']
let arr41: number[];
// number类型的数组
arr41 = [1,2,3]

/*
*   元组，元组就是固定长度的数组
*       语法：[类型, 类型, 类型]
* */
let e5: [string, number];
e5 = ['hello', 123];

/**
 * 枚举
 */

 enum Gender{
    Male,
    Female
}

let f5: {name: string, gender: Gender};
f5 = {
    name: '张三',
    gender: Gender.Male // 'male'
}

// &表示同时
let g5: { name: string } & { age: number };
g5 = {name: '张三', age: 18};

// 类型的别名
type myType = 1 | 2 | 3 | 4 | 5;
let k: myType;
let l: myType;
let m: myType;

k = 2;
```

## 4、编译选项

- 自动编译文件

  - 编译文件时，使用 -w 指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译

  - 示例：

    ```typescript
    tsc xxx.ts -w
    ```

- 自动编译整个项目

  - 如果直接使用tsc指令，则可以自动将当前项目下的所有ts文件编译为js文件

  - 但是能直接使用tsc命令的前提是：要先在项目根目录下创建一个ts的配置文件tsconfig.json

  - tsconfig.json是一个JSON文件，添加配置文件后，只需tsc命令即可完成对整个项目的编译

  - tsc --init(自动创建)

  - 配置选项：

    - 4.1、include

      - 定义要编译的文件所在目录（用来指定哪些ts文件要被编译）

      - 默认值：["**/*"]

      - 示例

        - ```typescript
          "include":["src/**/*", "tests/**/*"]
          ```

        - *任意文件   ** 任意目录

    - 4.2、exclude

      - 定义需要排除在外的目录（除了哪些ts文件不被编译，不希望被编译的文件，可选）

      - 默认值：["node_modules", "bower_components", "jspm_packages"]

      - 示例

        - ```json
          "exclude": ["./src/test/**/*"]
          ```

        - 上述示例中，src下的test目录下的文件都不会被编译

    - 4.3、extends

      - 定义被继承的配置文件

      - 示例：

        - ```json
          "extends": "./configs/base"
          ```

        - 上述示例中，当前配置文件中会自动包含config目录下base.json中的所有配置信息

    - 4.4、files

      - 指定被编译文件的列表，只有需要编译的文件少时才会用到

      - 示例：

        ```json
        "files": [
            "core.ts",
            "sys.ts",
            "types.ts",
            "scanner.ts",
            "parser.ts",
            "utilities.ts",
            "binder.ts",
            "checker.ts",
            "tsc.ts"
          ]
        ```

      - 列表中文件会被TS编译器所编译

    - 4.5、compilerOptions

      - 编译选项时配置文件中非常重要也比较复杂的配置选项

      - 在compilerOptions中包含多个子选项，用来完成对编译的配置

        - 项目选项

          > - 4.5.1、target
          >
          >   - 设置ts代码编译的目标版本
          >
          >   - 可选值： 'es3', 'es5', 'es6', 'es2015', 'es2016', 'es2017', 'es2018', 'es2019', 'es2020', 'es2021', 'esnext'
          >
          >   - 示例：
          >
          >     ```json
          >     "compilerOptions": {
          >         "target": "ES6"
          >     }
          >     ```
          >
          >   - 如上设置，我们所编写的ts代码将会被编译为ES6版本的js代码
          >
          > - 4.5.2、lib
          >
          >   - 指定代码运行时所包含的库（宿主环境）
          >
          >   - 可选值：ES5、ES6/ES2015、ES7/ES2016、ES2017、ES2018、ES2019、ES2020、ESNext、DOM、WebWorker、ScriptHost ......
          >
          >   - 示例：
          >
          >     ```json
          >     "compilerOptions": {
          >         "target": "ES6",
          >         "lib": ["ES6", "DOM"],
          >         "outDir": "dist",
          >         "outFile": "dist/aa.js"
          >     }
          >     ```
          >
          > - 4.5.3、module
          >
          >   - 设置编译后代码使用的模块化系统
          >
          >   - 可选值：'none', 'commonjs', 'amd', 'system', 'umd', 'es6', 'es2015', 'es2020', 'es2022', 'esnext', 'node12', 'nodenext'
          >
          >   - 示例：
          >
          >     ```json
          >     "compilerOptions": {
          >         "module": "CommonJS"
          >     }
          >     ```
          >
          > - 4.5.4、outDir
          >
          >   - 编译后文件的所在目录
          >
          >   - 默认情况下，编译后的js文件会和ts文件位于相同的目录，设置outDir后可以改变编译后文件的位置
          >
          >   - 示例：
          >
          >     ```json
          >     "compilerOptions": {    
          >         "outDir": "dist"
          >     }
          >     ```
          >
          >   - 设置成功后编译后的js文件会生成到dist目录
          >
          > - 4.5.5、outFile
          >
          >   - 将所有的文件编译为一个js文件
          >
          >   - 默认会将所有的编写在全局作用域中的代码合并为一个js文件，如果module制定了None、System或AMD则会将模块一起合并到文件之中
          >
          >   - 示例：
          >
          >     ```json
          >     "compilerOptions": {
          >         "outFile": "dist/app.js"
          >     }
          >     ```
          >
          > - 4.5.6、rootDir
          >
          >   - 指定代码的根目录，默认情况下编译后文件的目录结构会以最长的公共目录为根目录，通过rootDir可以手动指定根目录
          >
          >   - 示例：
          >
          >     ```json
          >     "compilerOptions": {
          >         "rootDir": "./src"
          >     }
          >     ```
          >
          > - allowJs
          >
          > - 

      ​			

      - 

      - 设置

    - 

