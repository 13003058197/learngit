# 开发环境搭建

```bash
> npm i -g typescript
> tsc xx.ts

tsc xx.ts -watch
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

// void 表示没有值，控制，undefined
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

// 类型的别名
let a: 1 | 2 | 3 | 4;
let b: 1 | 2 | 3 | 4;

type myType = 1 | 2 | 3 | 4;
type myString = string;
let a: myType;
let b: myString


```

## 编译选项

