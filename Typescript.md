#   开发环境搭建

```js
> npm i -g typescript

// 编译单个文件
> tsc xx.ts
> tsc xx.ts -watch 或 tsc xx.ts -w

// 编译多个文件
1. 在项目下新建一个 tsconfig.json文件
2. 运行 tsc 或 tsc -w 命令
```

## tsconfig.json

```json
{
  //指定那些ts文件需要被编译，** 代表任意目录, * 代表任意文件
  "include":[
    "./src/**/*"
  ],
  // 不需要被编译的目录
  "exclude":[ ],
  // 一定被继承的配置文件
  "extends":"./config/xxx.json",
  // 指定具体需要被编辑的文件
  "files": [
    "1.ts",
    "2.ts"
  ],
  // 编译器选项
  "compilerOptions":{
    "target":"es2015 ",// 指定被编译为的es版本
    "mudule": "es2015",// 指定要使用的模块化规范
    //"lib":["dom"] //指定项目中用到的库，比如dom，一般不需要改,默认 
    "outDir": "./dist", // 编译后文件所在目录
    "outFile": "./app.js", // 代码合并成一个文件 
    "allowJs": false, // 是否对js文件进行编译，默认false
    "checkJs": false, //是否检查js语法是否符合规范  
    "removeComments": false, // 是否移除注释  
    "noEmit": true,// 不生成编译后的文件，有时只希望用ts检查语法，不编译文件时用到
    "noEmitOnError": true, // 当有错误时不生成编译文件
    
    "strict": true,// 所有严格检查的总开关
    "alwaysStrict": false,// 用来设置编译后的文件是否使用严格模式，默认false
    "noImplicitAny": true, //不允许隐式的any 
    "noImplicitThis": false, // 不允许不明确的this
    "strictNullChecks": true,// 严格检查空值
    
    "experimentalDecorator": true ,// 开启装饰器的支持
    
  }
  
}
```



# 类型声明

```typescript
let a: number;
a = 10;

let a:number = 10
 
// 参数类的类型声明
function sum(a:number, b:number){}
// 返回值的类型声明
function sum(a, b):number{}
```

# 类型

```typescript
//number
// string
// boolean

// 字面量, 限定了a的值
let a:10;
a = 10; 

//any 表示任意类型
let a: any;
let a; // 隐式any
a = 10;
a = "jay"

// unknown 类型的变量不能直接赋值给其他变量
let b: unknown

# any类型的变量可以赋值给任意类型,unknown类型不能直接赋值给其他类型
let s:string;
let a:any = 10
let b:unknown = "abs"
s = a; 正确
s = b; 错误

// 类型断言, 告诉解析器变量的实际类型
s = b as string;
s = <string> b;

// void 表示没有值，空值，undefined
function fn(): void{
  
}

// never 永远不会返回结果，不能是任何值
function fn2(): never{
  thorw new Error("报错")
}

// object
let a :object;
let b :{name: string, age?: number}; // ?表示可选属性
let c: {name:string, [propsName: string]: any} // name属性必须，其他属性随意 

// function
let f : (a:number, b:number) => number;

// array
let a: string[];
let a: Array<string>;

// tuple 元组，固定长度的数组
let a: [string, number]

// enum 枚举
enum Gender{
  Male,
  Female
}
enum Gender{
  Male = 1,
  Female = 2
}
let i :{name:string, gender:Gender}
i ={name: "zs", gender: Gender.Male}

// 联合类型 |
let a: "male" | "female";
let b: number | string;

// & 同时
let j : {name: string} & {age: number};

// 类型的别名 type
let a: 1 | 2 | 3 | 4;
let b: 1 | 2 | 3 | 4;

type myType = 1 | 2 | 3 | 4;
type myString = string;
let a: myType;
let b: myString 
type myObj = {name: string, age:number}


```

# webpack打包ts代码

## 基础

```js
npm i -D webpack webpack-cli typescript ts-loader

//webpack.config.js
const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  output: {
    path: path.resolve(__dirname, "dist")
    filename: "bundle.js"
  },
  // webpack打包时使用的模块
  module: {
    // 指定加载规则
    rules: [
      {
        test: /\.ts$/,
        use: 'ts-loader',
        exclude: /node_modules /
      }
    ]
  },
  // 设置引用的模块
  resolve: {
    extensions: [".ts",".js"]
  }
}
// tsconfig.json
// 运行命令 > webpack
```



## html-webpack-plugin

```js
npm i -D html-webpack-plugin

const HTMLWebpackPlugin = require("html-webpack-plugin")
module.exports = {
  plugins: [
    new HTMLWebpackPlugin(),
    new HTMLWebpackPlugin({
      template: "./src/index.html"
    })
  ]
}
```

## web pack-dev-server

```js
// 实时更新 
> webpack serve --open chrome.exe

```

## clean-webpack-plugin

```js
const {CleanWebpackPlugin} = require("clean-webpack-plugin")
plugins:[
  new CleanWebpackPlugin()
]
```

## js兼容性

```js
 npn i -D @babel/core @babel/preset-env babel-loader core-js

output: {
    path: path.resolve(__dirname, "dist")
    filename: "bundle.js"，
    // 告诉webpack不使用箭头函数打包，针对高版本webpack
    environment: {
      arrowFunction: false,
      const: false,
    }
  },
rules: [
      {
        test: /\.ts$/,
        use: [
          { 
            loader: "babel-loader",
            options: {
              // 设置预定义环境
              presets: [
                // 第一个环境
                [
                  "@babel/preset-env",  // 指定环境插件
                  // 配置信息
                  {
                    targets: {
                      chrome: "88", // 要兼容的目标浏览器 
                      ie: "11",// 兼容IE11
                    },
                    "corejs": "3", // 指定corejs版本
                    "useBuiltIns": "usage", // 使用corejs的方式，usage表示按需加载
                  }
                ]
              ]
            }
          },
          'ts-loader'
        ],
        exclude: /node_modules /
      }
    ]
```

## 样式

```js
npm i -D less less-loader css-loader style-loader

{
  test:/\.less$/,
    use:[
      "style-loader",
      "css-loader",
			"less-loader"
    ]
}
```

## 样式兼容

```js
npm i -D postcss postcss-loader postcss-preset-env

{
  test:/\.less$/,
    use:[
      "style-loader",
      "css-loader",
      {
        loader: "postcss-loader",
        options: {
          postcssOptions: {
            plugins:[
              [
                "postcss-preset-env",
                {
                  browsers: "last 2 versions" 
                }
              ]
            ]
          }
        }
      },
			"less-loader"
    ]
}

```



# 类

```typescript
class Person{
  name: string = "zs"; 
	static readonly age: number = 10; // readonly只读属性 
  constructor(age:number){ 
  	this.age = age;
  }
  static say(){} // 静态方法
}

class Animal{
  name: string;
  constructor(name: string){
    this.name = name;
  }
  say(){ }
}
// 继承
class Dog extends Animal{
  run(){ }
  say(){ }
}
class Dog extends Animal{
  age: number;
  constructor(name:string, age:number){
    super(name)
    this.age = age;
  }
  run(){ }
  say(){ }
}
 
// 抽象类，专门用来被继承，不能创建实例
abstract class Parent{
  // 抽象方法, 没有方法体，子类必须重写
  abstract say():string;
  // 也可以有普通方法
  say(){}
}
```

# 接口

```typescript
// 接口，用来定义类的结构，和类的别名类似
type myObj = {name: string, age:number};

interface MyObject{
  name: string,
  age: number
}

const obj: MyObject = {
  name: "zs",
  age: 18
}
// 区别，接口可以限制类的结构
interface Parent {
  name: string;
  say(): void;
}

class Child implements Parent{
  name: string;
  constructor(name: string){
    this.name = name
  }
  say(){}
}
// 属性封装, 修饰符
class A {
  private _name: string; // 私有属性，当前类可访问
  public age: number; // 公有属性
  protected gender: number; // 当前类和子类可以访问
  
  get name(){
    return this._name;
  }
  set name(name: string){
    this._name = name;
  }
}

// 简写
class A{
  constructor(private name: string){
    this.name = name;
  }
}
```

# 泛型

```typescript
// 定义函数和类时，如果遇到类型不明确就可以使用泛型
function fn<T>(a: T):T{
  return a
}
fn(10) // 自动进行类型推断
fn<string>("hello")

// 指定多个泛型
function fn<T, K>(a: T, b: K):T{
  return a;
}

// 限制泛型范围
interface Inter{
  length: number
}
// 泛型T是Iner的子类
function fn<T extends Inter>(a:T):number{
  return a.length; 
}
fn("123")

// 在类中的使用
class Person<T>{
  name: T;
  constructor(name: T){
    this.name = name
  }
}
new Person ("zs")
new Person<string>("zs")
```

# 命名空间

```typescript
// 避免命名冲突
export namespace A{
  export class Person{}
}

import { A } from ""
new A.Persion()
```



# 装饰器

 ```js
// 是一个特殊的方法，可以扩展类，属性，方法，参数的功能
普通装饰器(不可传参)
装饰器工厂(可传参)

//  类装饰器
function logClass(params: any){
  params.prototype.url = "xxxx"
}
@logClass
class A{
  
}

// 属性装饰器
function logClass(params: string){
  return function(target:any, attr:any){
    // target： 构造函数
  }
}

// 方法装饰器
function logMethod(){
  return function(target, methodName, descriptor){
    
  }
}
// 参数装饰器
function params(){
  return function params(target, methodName, index ){
    // target： 静态-构造函数，实例方法-原型对象
    // methodName： 方法名
    // 参数索引： 参数index
  }
}


@logClass("hello")
class A{
  @logClass("www.xxx.com") // 属性装饰器
  public  url: string;
	@logMethod()
	say(@params() name: string){}
	
}
// 执行顺序
属性装饰器
方法装饰器
参数装饰器
类装饰器

同类型的装饰器，先执行后面的


 ```



# 实战-贪吃蛇

## Food

```typescript
// 食物类
class Food{
  element: HTMLElement;
  constructor(){
    this.element = document.getElementById("food")!;
  }
  
  get X(){
    return this.element.offsetLeft;
  }
  
  get Y(){
    return this.element.offsetTop;
  }
  // 改变实物位置
  change(){
    // 0-290 之间间距为10的整数
    let top = Math.round(Math.Random(29)) * 10
    let left = Math.round(Math.Random(29)) * 10
    
    this.element.style.top = top +"px";
    this.element.style.left = left+ "px";
  }
}
```

## ScorePanel

```typescript
// 计分板类
ScorePanel{
  score = 0;
  level = 1;
  scoreEle: HTMLElement;
  levelEle: HTMLElement;
  constructor(){
    this.scoreEle = document.getElementById("score")!;
    this.levelEle = document.getElementById("level ")!
  }
  addScore(){
    this.scoreEle.innerHTML = ++this.score + ""
   	// 
    if(this.score % 10 === 0){
      this.levelUp()
    }
  }
  levelUp(){
    if(this.level < 10){
      this.levelEle.innerHTML = ++this.level + ""
    }
    
  }
}
```

## Sanke

```typescript
<div id="snake">
	<div>
</div>
// 蛇类
class Snake{
  head: HTMLElement;
  body: HTMLCollection;
  element: HTMLElement;
  constructor(){
    this.head = document.querySelector("#sanke > div") as HTMLElement
    this.element = document.getElementById("#snake")
    this.body = document.getElementById("sanke").getElementsByTagName("div");
  }
  // 蛇头坐标
  get X(){
    return this.head.offsetLeft;
  }
  get Y(){
    return this.head.offsetTop;
  }
  // 撞墙检测
  set X(val){
    if(this.X === val) return;
    if(val < 0 || val > 290){
      throw new Error("蛇撞墙了")
    }
    
    // 阻止掉头问题
    if(this.body[1] && this.body[1].offsetLeft ===  val){
      if(val > this.x){
        val = this.x -10
      }eles {
        val = this.x +10
      }
    }
    
    this.moveBody()
    this.head.style.left = val+"px";
    this.checkEatSelf();
  }
  set Y(){
    if(this.Y === val) return;  
    if(val < 0 || val > 290){
      throw new Error("蛇撞墙了")
    }
    
    if(this.body[1] && this.body[1].offsetTop ===  val){
      if(val > this.y ){
        val = this.y -10
      }eles {
        val = this.y +10
      }
    }
    
    this.moveBody()
    this.head.style.top = val+ "px";
    this.checkEatSelf();
  }
  addBody(){
    this.element.inserAdjacentHTML("beforeend", "<div></div>")
  }
  moveBody(){
    for(let i = this.body.length-1; i > 0; i--){
      let x = (this.body[i-1] as HTMLElement).offsetLeft;
      let y = (this.body[i-1] as HTMLElement).offsetTop;
      this.body[i].style.left = x + "px";
      this.body[i].style.top = y + "px";
    }
  }
  // 吃到自己身体
  checkEatSelf(){
    for(let i = 1; i< this.body.length; i++){
      let body = this.body[i] as HTMLELement
      if(this.x === body.offsetLeft && this.y === body.offsetTop){
        throw new Error('吃到自己')
      }
    }
  }
}
```

## GameControl

```typescript

class GameControl{
  snake: Snake;
  food: Food;
  sorcePanel: ScorePanel;
  direction: string = "";
  // 蛇活着
  isLive = true;
  constructor(){
    this.snake = new Snake();
    this.food = new Food();
    this.scorePanel = new SorePanel;
    this.init();
  }
  init(){
    document.addListenEvent("keydown", this.keydownHandler.bind(this))
    this.run();
  }
  keydownHandler(event: keybordEvent){
    this.direction = event.key;
  }
  run(){
    let x = this.snake.x;
    let y = this.snake.y;
    switch(this.direction){
        case: "ArrowUp":
          y -= 10;
          break;
        case: "ArrowDown":
        	y += 10
        	break;
        case: "ArrowLeft":
        	x -=10;
        	break;
        case: "ArrowRight":
        	x +=10;
        	break;
    }
    
    this.checkEat(x,y)
    
    try{
      this.snake.x = x;
    	this.snake.y = y;
    }catch(e){ // 蛇撞墙了 
      console.error(e.message) 
      this.isLive = false
    }
    
    # 赞
    this.isLive && setTimeout(this.run.bind(this), 300 - (this.scorePanel.level-1) * 30)
  }
  // 吃苹果
  checkEat(x: number, y:number){
    if( x === this.food.x && y === this.food.y){
      this.food.change()
      this.scorePanel.addScore()
      this.sanke.addBody()
    }
  }
  
}

new CameControl();
```











