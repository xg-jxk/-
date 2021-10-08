# TS

```
npm i -g typescript

tsc xx.ts 生成同名js文件
node xx.js   运行文件
```

```react
npm i -g ts-node
tsc --init
ts-node xx.ts       相当于执行了 tsc+node
```

## 类型注解

```
: + 类型
```



## 原始类型

```react
let age : number = 18
let useName : string = 'jxk'
let isOk : Boolean = true
let un : undefined = undefined
let nu : null = null
let s:symbol = new Symbol()
```

## 数组类型

```js
// 方式1  只能是数字
let arr : number[] = [1,2,3,4] 
// 方式2  泛型
let arr1 : Array<number> = [1,2,3,4]
```

## 联合类型

```react
// 丨 联合类型
let arr : (string | number)[] = [1,2,'h','h'] 
```



## 类型别名

```js
type 自定义的类型别名  = 类型

type CustomArray = (number | string) []

let arr : CustomArray = [1,2,'h','h'] 
```

## 函数类型

```js
function add(n1:number,n2:number) : number {
	return n1 + n2
}
// 表达式
const add = (n1:number,n2:number) : number => {
  return n1 + n2
}

// 类型别名  注意: 只能用于函数表达式
type Fn = (n1:number,n2:number) : number

const add: Fn = (n1,n2) =>{
  return n1 + n2
}


// 函数没有返回值 
const sayHi = (name: string) : void => {
  
}
// 可选参数 ?  要放在后面
const mySlice = (start:number,end?:number) : void => {
 
}
// 默认参数
const mySlice = (start:number = 0,end:number = 2) : void => {
 
}
```

## 对象类型

```react
const user: { name: string; age: number; sayHi(): void } = {
  name: 'zs',
  age: 18,
  sayHi() {
    console.log(1)
  }
}

// type
type User = {
  name: string
  age: number
  sayHi: (name: string) => void
}
// 接口类型 只能指定对象类型 推荐I开头
interface IUser {
  name: string
  age: number
  sayHi: (name: string) => void
}
interface IPoint2d {
  x: number
  y: number
}
// 接口可以继承
interface IPoint3d extends IPoint2d {
  z: number
}
let str: IPoint3d = {
  x: 1,
  y: 1,
  z: 1
}
```

## 接口interface

## 元组类型

```react
// 元组类型   指定数组成员个数和类型
let arr: [number, number] = [123, 123]

```

## 类型推论

```
定义变量且赋值的时候会发生类型推论
```

## 字面量类型

```react
// 字面量类型
let str: string = 'jxk'
// 只能是jxk
const str1: 'jxk' = 'jxk'
// 一般配合联合类型使用
type Direction = 'up' | 'down' | 'left' | 'right'
const str2: Direction = 'up'

```

## 枚举类型

```react
// 枚举类型可以当类型也可以当值使用
// enum Gender {
//   Women = 'Women',
//   Man = 'Man'
// }

enum Gender {
  Women,
  Man
}
function send(gender: Gender) {
  console.log(gender)
}
send(Gender.Women)

```

## 类型断言

```react
// 类型断言
// <>形式少用
// const img = <HTMLImageElement>document.getElementById('root')
// 查看DOM类型可以F12点击元素  然后控制台console.log($0.__proto__)
const img = document.getElementById('root') as HTMLImageElement
console.log(img.src)
```

## typeof

```react
// typeof 获取对象或者属性的类型
const obj = {
  username: 'zs',
  age: 19,
  sayHi() {
    console.log('hi')
  }
}

type Obj = typeof obj

function get(obj: Obj) {
  console.log(obj.username)
}
```

# 泛型

## 泛型函数

```react
// 泛型
// 定义泛型
function id<Type>(n: Type): Type {
  return n
}

// let num1 = id<number>(1)  
let num1 = id(1)  // 简化
let num2 = id('1')
```

## 泛型约束

```react
// Type 类型的数组  因为只要是数组就一定存在 length 属性，因此就可以访问
function id<Type>(value: Type[]): Type[] {
  console.log(value.length)
  return value
}
```

```react
// 创建一个接口
interface ILength { length: number }

// Type extends ILength 添加泛型约束
// 解释：表示传入的 类型 必须满足 ILength 接口的要求才行，也就是得有一个 number 类型的 length 属性
function id<Type extends ILength>(value: Type): Type {
  console.log(value.length)
  return value
}
```

```react
// 约束key 只能是type里的属性  
function getProp<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key]
}
let person = { name: 'jack', age: 18 }
getProp(person, 'name')
```

# 泛型工具类型

## partial

```react 

type Props =  {
  id: string
  children: number[]
}

type PartialProps = Partial<Props>
//  PartialProps 类型  即对象可选属性id 和children
```

## Readonly

```react
type Props =  {
  id: string
  children: number[]
}

type ReadonlyProps = Readonly<Props>
  
//  只读不可修改
  let props: ReadonlyProps = { id: '1', children: [] }
// 错误演示
props.id = '2'
```



## Pick

```react
interface Props {
  id: string
  title: string
  children: number[]
}
type PickProps = Pick<Props, 'id' | 'title'>
  
  // 选择两个属性构造一个新类型
```



## Omit

```react
interface Props {
  id: string
  title: string
  children: number[]
}
type PickProps = Pick<Props,  'title'>
  
  // 排除title 属性
```

# 索引签名类型

```react
// 属性不确定的情况下使用
type Action = {
	type : string
  [key : string] : any
}
```

# 获取函数类型返回值的类型

```
type RootState =  ReturnType<typeof Fn>
```



# 1

# 类型声明文件

```
用了jsx的js文件  需要使用.tsx后缀名
```



```
xxx.d.ts    .d.ts 结尾的文件

ts的类型声明文件   只能出现类型信息

内置类型声明文件  : 数组方法  window、document 等 BOM、DOM API 

第三方库声明文件   DefinitelyTyped
@types/react   @types/lodash   
```

## declare

```
declare 关键字:用于类型声明，为其他地方(比如，.js 文件)已存在的变量声明类型，而不是创建一个新的变量

1. 对于 type、interface 等这些明确就是 TS 类型的(只能在 TS 中使用的)，可以省略 declare 关键字。
2. 对于 let、function 等具有双重含义(在 JS、TS 中都能用)，应该使用 declare 关键字，明确指定此处用于类型声明。
```



# 项目中添加ts

- [CRA 添加 ts 文档](https://create-react-app.dev/docs/adding-typescript)
- 如果要在现有的 JS 项目中，添加 TS，需要以下操作：

1. 安装包：`yarn add typescript @types/node @types/react @types/react-dom @types/jest`
2. 移除 `jsconfig.json`
3. 创建 `path.tsconfig.json` 文件，将原来 `jsconfig.json` 文件中的内容拿过来
4. 使用 `tsc --init` 创建 `tsconfig.json`【注意：项目中 tsconfig.json 和 jsconfig.json 只能有一个】
5. 将原来通过 React 脚手架创建的 TS 项目中的 tsconfig.json 中的配置，拷贝到咱们自己的项目中

```json
{
  "compilerOptions": {
    "baseUrl": "./",
    "paths": {
      "@/*": ["src/*"],
      "@scss/*": ["src/assets/styles/*"]
    }
  }
}
```

5. 在 `tsconfig.json` 中，添加以下配置：

```json
{
  // 添加这一句
  "extends": "./path.tsconfig.json",

  "compilerOptions": {
    ...
  }
}
```

6. 将通过 React 脚手架创建的 TS 项目中的 `src/react-app-env.d.ts` 拷贝到咱们自己项目的 src 目录下
7. 重启项目





## axios  

```
err: AxiosError<{ message: string }>
```





# unknown

```
安全的任意类型

需要用typeof xx === 类型做比较 明确类型才能使用
```



# 注意点

```
可能为空或者undefined   加!  非空断言  或者<Html...>  或者as 类型

```

