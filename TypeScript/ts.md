# 基础类型

数据类型

- string
- boolean
- null  *
- undefined  *
- void
- number



```ts
let str:string = 'mmm'

console.log(str);


function fun():void{
}
```



```
tsc -init		//生成tsconfig.json文件
tsc -w   //实时监听转换
```



### 严格模式   *

*是否可穿插

![image-20230307102840309](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/ts/tsimage-20230307102840309.png)





# 准备

### 换源

小满

```
npm i xmzs -g
mmp ls	//看源
mmp use	//换源
```



### ts-node

```
npm i ts-node -g
```

可直接操作ts文件



### 初始化

```
npm init -y
```



### 声明文件

```
npm i @types/node -D
```



# 任意类型

向下包含

- top type 顶级类型
  - any任意类型
  - unknown不知道类型（只能赋值自身或any）(没有办法读任何属性，方法也不可调用)（更安全，优先使用）

- object

- Number String Boolean
- number string boolean
- 1         ‘sssss’     false
- never



### Object

可包含所有类型



### object

只支持[]、{}、()=>{}



### {}

等同Object

支持所有

但是无法增加属性

> 少用为好



# interface

实例和本体形状要一样

重名合一起

任意key**索引签名**

? 可选可不选

只读设置

extends继承

定义函数

```js
interface Bbb {
    fa?: string
}

interface Aaa {
    name: string
    // 任意key
    [attribute: string]: any
    // 只读
    readonly fun: () => boolean
    readonly id: number
}

// extends 继承
interface Aaa extends Bbb {
    // ?可选可不选
    age?: number
}

let a: Aaa = {
    name: "mmm",
    age: 16,
    fun: () => false,
    id: 111,
    a: 1,
    b: 'wwwww'
}
```



方法和函数使用

```js
interface Fun {
    (name: string): number[]
}
const fn: Fun = function (name: string) {
    return [1, 2, 3]
}
```





# 数组

普通

```js
let arr: number[] = [1, 2, 3]
let arr1: Array<number> = [2, 3, 4]
```



interface使用定义对象数组

```js
interface A {
    name:string
}
let arr2:A[] = [{name:"SS"},{name:"asss"}]
```



多维数组

```js
let arr3: number[][] = [[1, 2, 3], [3, 4, 5], [4, 4, 4]]
let arr4: any[] = [[1, 2], '222', {}, true]
```



函数参数

```js
function x(...args: number[]) {
    // 内置对象IArguments
    let a: IArguments = arguments
}

x(1, 2, 3)
```





# 函数

```js
//b默认值为20
function add(a: number, b: number = 20): number {
    return a + b
}

add(1) //1 + 20 = 21
```

> 默认值和可选?不能一起使用



### 传对象

```js
interface User {
    name: string
    age: number
}

function compute(user: User): User {
    return user
}
compute({ name: 'm', age: 12 })
```



### this类型

```js
interface Obj {
    num: number
    add: (this: Obj, num: number) => void
}

let obj: Obj = {
    num: 13,
    add(this: Obj, numb: number) {
        console.log(numb);
        this.num += numb;
    }
}

obj.add(5)
console.log(obj);
```



### 函数重载

一个函数,不同参数,实现多个功能

```js
let user: number[] = [1, 2, 3]
function findNum(id?: number | number[]): number[] {
    if (typeof id == 'number') {
        return user.filter(v => v == id)
    }
    else if (Array.isArray(id)) {
        user.push(...id)
        return user
    }
    else {
        return user
    }
}

console.log(findNum(2));
console.log(findNum([4, 5, 6]));
console.log(findNum());
```





# 参数类型

### 联合类型

| 实现

```js
let phone:number|string = '12345678912'
```



```js
!!0  是false
!!1  是true
```





### 交叉类型

& 实现

将两个interface对象合起来





### 类型断言

param as 类型

```js

let phone: number | string = '12345678912'

let inter = function (num: number | string): void {
    console.log((num as string).length);
}
inter('1234')   //4
inter(1234)     //undefined
```



或者

```js
interface A {
    name: string
}
interface B {
    age: number
}
let fff = (type: A | B): void => {
    console.log((<A>type).name);
}

fff({
    age: 12
})
```





# Promise定义

```js
let res = (): Promise<number> => {
    return new Promise<number>((resolve, reject) => {
        resolve(1)
    })
}
console.log(res());
```









# 类

```js
class Aaaa {
    name: string
    age: number
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
}
let ito = new Aaaa('MM', 20)
console.log(ito);
```



### 属性

默认类内的参数是public--------------------内外部均可访问

私有的private------------------只能内部访问

protected----------------------内部和子类中访问



```js

class Aaaa {
    protected name: string
    private age: number
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
}
// let ito = new Aaaa('MM', 20)
// console.log(ito);

//继承
class Aaaaaa extends Aaaa {
    people: string
    constructor() {
        //需要先调用super父类构造函数
        super('MM', 20)
        this.people = this.name
    }
    say(): void {
        console.log(this.people);
    }
}

let ii = new Aaaaaa()
console.log(ii.say());
```



### 静态属性和静态函数

static

可直接通过类访问

**静态通过this只能访问静态**

```js
class Aaaa {
    protected name: string
    private age: number
    static aaa:string = '123456'
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
}

Aaaa.aaa
```



### 约束类

interface

```js
interface Pers {
    run(type: boolean): boolean
}
interface H {
    set(): void
}

class S {
    param:string
    constructor(params:string){
        this.param = params
    }
}

class Man extends S implements Pers, H {
    run(type: boolean): boolean {
        return type
    }
    set(): void {

    }
}
```





### 基类 抽象类

abstract

抽象类无法被实例化

abstract所定义的方法都只能描述不能进行实现

```js
abstract class Vue {
    name: string
    constructor(name?: string) {
        this.name = (<string>name)
    }
    abstract init(name: string): void
}
```



### 派生类

用于使用抽象类

```js
abstract class Vue {
    name: string
    constructor(name?: string) {
        this.name = (<string>name)
    }
    get(): string {
        return this.name
    }
    abstract init(name: string): void
}

// 派生类调用抽象类
class React extends Vue {
    constructor() {
        super()
    }
    // 需要先实现抽象类方法
    init(name: string): void {

    }
    setName(name: string) {
        this.name = name
    }
}
// 派生类可创建实例
const react = new React()
react.setName('aaaa')
console.log(react.get());
```





# 元组

```js
let arr5: [x: number, y?: boolean] = [1, false]
// 只能push number或boolean属性的值
arr5.push(22)
arr5.push(true)
arr5.push(undefined)

// 获取元组元素属性
type first = typeof arr5[0]   //number

// 获取元组长度
type len = typeof arr5['length']	//1|2



// readonly只读无法增删改
let arr6: readonly [number, boolean] = [1, false]
```





# 枚举

```ts
// 枚举 默认从零开始
enum Color {
    red = 2,    //2
    green,		//3
    blue = 5	//5
}

//反向映射
let key = Color[3] //green
```

> 要注意的是 不会为**字符串枚举**成员生成反向映射。





# 类型别名

```ts
type s = string | number
let str:s = 'asdsd'
```





### type 高级用法

```ts
type num = 1 extends number ? 1 : 0     //1
```

![img](https://img-blog.csdnimg.cn/5e0a471d4f894d6492543f6ee1243f34.png)





# never类型

不应该存在的状态

number & string

```ts
// 返回never的函数必须存在无法达到的终点
 
// 因为必定抛出异常，所以 error 将不会有返回值
function error(message: string): never {
    throw new Error(message);
}
 
// 因为存在死循环，所以 loop 将不会有返回值
function loop(): never {
    while (true) {
    }
}
```

在联合类型会被忽略



常用于兜底逻辑

```ts
type ls = 'ss' | 'as'
function na(A: ls) {
    switch (A) {
        case "as": break
        case "ss": break
        default:
            // 兜底逻辑
            const error: never = A
            break
    }
}
```





# Symbol

表示唯一

```ts
console.log(Symbol(1) === Symbol(1));   //false
```



要为true可使用for搭配

```ts
// for Symbol for 全局symbol有没有注册过这个key，如果有直接拿来用，没有就去创建一个
// 第一次前面的去找没有创建
// 第二次后面的去找找到直接拿来使用
console.log(Symbol.for("1") === Symbol.for("1"));   //true
```



> for in 不能读到symbol



```ts
Object.getOwnPropertySymbols(obj)	//只能读到symbol

Reflect.ownKeys(obj)		//能同时读到string和symbol
```





# 生成器

```ts
next()
```



```ts
// 生成器
function* gen() {
    yield Promise.resolve('a')
    yield 'b'
    yield 'c'
    yield 'd'
}

const man = gen()
console.log(man);			//Object [Generator] {}
//next调用
console.log(man.next());		//{ value: Promise { 'a' }, done: false }

```

**done为false则没有东西能迭代了，可通过done判断**







# 迭代器

遍历结构

```ts
value[Symbol.iterator]().next()
```



```ts
const each = (value: any) => {
    let cont: any = value[Symbol.iterator]()
    let next: any = { done: false }
    while (!next.done) {
        next = cont.next()
        if (!next.done) {
            console.log(next.value);
        }
    }
}
each(set)
```



### 语法糖

**for of**

```ts
for (let value of set) {
    console.log(value);
}
```

> 对象不能用，因为对象身上没有Symbol.iterator

但对象可以手动加Symbol.iterator实现迭代



### 解构

底层原理也是去调用iterator





# 泛型

动态类型

更灵活

### 一般使用

```ts
// 泛型
function ma<T>(a: T, b: T): Array<T> {
    return [a, b]
}

ma(1,2)
```



### 联合泛型

```ts
type YY<T> = number|string|T
let ex:YY<boolean> = 1
```



### interface泛型

```ts
interface LL<T> {
    msg: T
}
let sss: LL<number> = {
    msg: 1
}
```



### 多个泛型

```ts
//可使用默认值 T = number
function addss<T, K>(a: T, b: K): Array<T | K> {
    return [a, b]
}
addss(1, false)
```



### interface扩展

```ts
interface asa {
    length: number
}

function addss<T extends asa>(a: T) {
    return a.length
}

console.log(addss("sddssdd"));
```



### 对象使用

直接定死属性名字

keyof ：直接为对象属性名

```ts

let objs = {
    name: "ma",
    sex: "男"
}

type key = keyof typeof objs    // "name" | "sex"

function ob<T extends object, K extends keyof T>(obj: T, key: K) {
    return obj[key]
}

ob(objs,'name')
```



### 对象属性操作

重新改变

```ts
interface Date {
    name: string
    age: number
    sex: string
}
// 对象内部属性操作
type Options<T extends object> = {
    [key in keyof T]?: T[key]
}
type BW = Options<Date>

/*
type BW = {
    name?: string | undefined;
    age?: number | undefined;
    sex?: string | undefined;
}
*/
```





# tsconfig.json

- 1.include
  指定编译文件默认是编译当前目录下所有的ts文件

- 2.exclude
  指定排除的文件

- 3.target
  指定编译js 的版本例如es5  es6

- 4.allowJS
  是否允许编译js文件

- 5.removeComments
  是否在编译过程中删除文件中的注释

- 6.rootDir
  编译文件的目录

- 7.outDir
  输出的目录

- 8.sourceMap
  代码源文件

- 9.strict
  严格模式

- 10.module
  默认common.js  可选es6模式 amd  umd 等

```
//命令行创建文件
echo ''>文件名
```



# 命名空间

 TypeScript与ECMAScript 2015一样，任何包含顶级`import`或者`export`的文件都被当成一个模块。相反地，如果一个文件不带有顶级的`import`或者`export`声明，那么它的内容被视为全局可见的（因此对模块也是可见的）

namespace

```ts
namespace A {
    export namespace B {
        export const sss2 = 2
    }
}
import AAA = A.B;

console.log(AAA.sss2);


```





# 三斜线指令

类似import

引入库

```ts
/// <reference path="..." />
```



引入声明文件

```ts
/// <reference types="..." />
```





# 声明文件（d.ts）

declare

也可以扩充

当使用第三方库时，我们需要引入它的声明文件，才能获得对应的代码补全、接口提示等功能

如果没有声明文件可以：

1 .下载

形式：

```
npm i --save-dev @type/库名
```



2.手写声明文件

库名.d.ts

例如：

```ts
import express from 'express';

const app = express()

const router = express.Router()

router.get("/api", (req, res) => {
    res.send()
})

app.use('/api', router)

app.listen(6650, () => {

})
```

写声明

```ts
declare module 'express' {
    interface Router {
        get(path: string, cb: (req: any, res: any) => void): void
    }
    interface App {
        use(path: string, router: any): void
        listen(port: number, cb?: () => void): void
    }
    interface Express {
        (): App
        Router(): Router
    }
    const express: Express
    export default express
}
```





# 混入（Mixins）

### 对象

object.assign(type1,typ2,...)

```ts
interface Name {
    name: string
}
interface Age {
    age: number
}
interface Sex {
    sex: number
}
 
let people1: Name = { name: "小满" }
let people2: Age = { age: 20 }
let people3: Sex = { sex: 1 }
 
const people = Object.assign(people1,people2,people3)
```



### 类

extends只能接收一个类

使用implements，把类当接口使用

获得内部属性：Object.getOwnPropertyNames(对象.prototype)

混入函数

```ts
function Mixins(curCls: any, itemCls: any[]) {
    itemCls.forEach(item => {
        Object.getOwnPropertyNames(item.prototype).forEach(name => {
            curCls.prototype[name] = item.prototype[name]
        })
    })
}
```



```ts
// function mas<T, K>(a: T, b: K): Array<T | K> {
//     return [a, a]
// }


class A {
    name: string
    age: number
    change(): void {
        this.name = 'MMMM'
    }
}

class B {
    address: string
    sex: boolean
    changeadd(newadd: string): void {

    }
}

class C implements A, B {
    name: string = 'AAA';
    sex: boolean
    address: string
    age: number
    //这里只是占位符通过Mixins混入实际内容
    change(): void {

    }
    changeadd(newadd: string): void {

    }
}
function Mixins(curCls: any, itemCls: any[]) {
    itemCls.forEach(item => {
        Object.getOwnPropertyNames(item.prototype).forEach(name => {
            curCls.prototype[name] = item.prototype[name]
        })
    })
}

Mixins(C, [A, B])

let ccc = new C()
console.log(ccc.name);
ccc.change()
console.log(ccc.name);



```

混入成功

![image-20230308104656748](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/ts/tsimage-20230308104656748.png)

把其他类的属性导入到一个属性，而这个属性只需要写属性占位即可





# 装饰器

### 类装饰器

**ClassDecorator**

target <=> 构造函数

****

```ts
const Base: ClassDecorator = (target) => {
    target.prototype.name = 'mmm'
    target.prototype.fn = () => {
        console.log("add");
    }
}
//装饰器
@Base
class A {

}

const a = new A() as any
console.log(a.name);

```



##### 装饰器工厂

```ts
const Base = (params: string) => {
    const fn: ClassDecorator = (target) => {
        target.prototype.name = params
        target.prototype.fn = () => {
            console.log("add");
        }
    }
    return fn
}
//可传参数
@Base("params")
class A {

}
```





### 方法装饰器

**MethodDecorator**

descriptor.value()---------------------------回传值

****

```ts
const get = (url:string) => {
    //descriptor数据类型PropertyDescriptor
    const fn: MethodDecorator = (target, key, descriptor:PropertyDescriptor) => {
        console.log(target);        //{}
        console.log(key);           //gets
        console.log(descriptor);    //{
                                    //     value: [Function: gets],
                                    //     writable: true,
                                    //     enumerable: false,
                                    //     configurable: true
                                    // }
        axios.get(url).then(res=>{
            // .value 就是回传值
            descriptor.value(res.data)
        })
    }
    return fn
}

class A {
    @get("https://www.baidu.com")
    // data即为回传接收的值
    gets(data:any): void {
        console.log(data);
        
    }
}
```





### 参数装饰器

**ParameterDecorator**

需要库

```ts
npm i reflect-metadata

//import 'reflect-metadata'
//可以快速存储元数据然后在用到的地方取出来 defineMetadata getMetadata
```

减少一层

```ts
const get = (url: string) => {
    const fn: MethodDecorator = (target, key, descriptor: PropertyDescriptor) => {
        const keys = Reflect.getMetadata('key', target)
        axios.get(url).then(res => {
            // .value 就是回传值
            descriptor.value(keys ? res.data[keys] : res.data)
        })
    }
    return fn
}

const Result = () => {
    const fn: ParameterDecorator = (target, key, index) => {
        // 解析出.result里的值
        // key 是 result
        Reflect.defineMetadata('key', 'result', target)
    }
    return fn
}

@Base("params")
class A {
    @get("https://www.baidu.com")
    gets(@Result() data: any): void {
        // 这里就不需要.result
        console.log(data);

    }
}

```





### 属性装饰器

**PropertyDecorator**

```ts
const Name = () => {
    const fn: PropertyDecorator = (target, key) => {

    }
    return fn
}

class A {
    @Name()
    name: string
    constructor() {
        this.name = 'mmm'
    }
}
```







# Rollup构建TS项目

npm 初始化文件

### package.json文件

```json
{
  "name": "rollupTs",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
      //-w watch 实时监听
    "dev": "cross-env NODE_ENV=development  rollup -c -w",
    "build":"cross-env NODE_ENV=produaction  rollup -c"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "cross-env": "^7.0.3",
    "rollup-plugin-livereload": "^2.0.5",
    "rollup-plugin-node-resolve": "^5.2.0",
    "rollup-plugin-replace": "^2.2.0",
    "rollup-plugin-serve": "^1.1.0",
    "rollup-plugin-terser": "^7.0.2",
      // 能使用ts文件
    "rollup-plugin-typescript2": "^0.31.1",
    "typescript": "^4.5.5"
  }
}
```





### rollup.config.js文件

```js
//process.env 环境变量
//console.log(process.env);
import ts from 'rollup-plugin-typescript2'
import path from 'path'
import serve from 'rollup-plugin-serve'
import livereload from 'rollup-plugin-livereload'
import { terser } from 'rollup-plugin-terser'
import resolve from 'rollup-plugin-node-resolve'
import repacle from 'rollup-plugin-replace'
 
//build（打包）的时候不去直接return，不去更新网页
const isDev = () => {
    return process.env.NODE_ENV === 'development'
}
export default {
    //入口文件
    input: "./src/main.ts",
    //出口文件
    output: {
        file: path.resolve(__dirname, './lib/index.js'),
        format: "umd",
        //追踪源文件
        sourcemap: true
    },
 	//插件
    plugins: [
        //转换ts
        ts(),
        //代码压缩
        terser({
            compress: {
                drop_console: !isDev()
            }
        }),
        repacle({
            //往浏览器注册东西
            //全局使用
            //版本
            'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV)
        }),
        resolve(['.js', '.ts']),
        //热更新服务
        isDev() && livereload(),
        // 网页读取地址
        isDev() && serve({
            open: true,
            openPage: "/public/index.html"
        })
    ]
}
```





### tsconfig.json文件

开启sourceMap

```json
 "sourceMap": true, /* Create source map files for emitted JavaScript files. */
```



### module

**编译后的 JS 使用哪种模块系统**。

模块系统常用的有两种：ESModule 和 CommonJS。前者是 ES 的标准（使用了 import 关键字），后者则是 Nodejs 的使用的模块系统（使用了 require）。此外还有 AMD、UMD 等。

支持的值有：none、commonjs、amd、umd、system、es6/es2015、es2020、es2022、esnext、node16、nodenext。

> 它们的具体不同可以看官方文档的代码示例：
> [https://www.typescriptlang.org/tsconfig#module](https://link.zhihu.com/?target=https%3A//www.typescriptlang.org/tsconfig%23module)

如果 target 是 ES3 或 ES5，默认值是 CommonJS（毕竟 ES6 后才有的 ESModule）；否则为 ES6/ES2015。



### moduleResolution

 "moduleResolution": "node", // 模块解析策略，ts默认用node的解析策略，即相对的方式导入











# 实战---过期清除缓存插件

### 文件划分

![image-20230308204954189](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/ts/tsimage-20230308204954189.png)

### enum>index.ts

枚举，存放对应数据

```ts
export enum Dictiona {
    permanent = "permanent",
    expire = "__expire__"
}
```



### type>index.ts

用来规范类型

**[Dictiona.expire]**  索引签名

```ts
import { Dictiona } from "../enum"

export type Key = string

export type ex = Dictiona.permanent | number

export interface Data<T> {
    value: T,
    [Dictiona.expire]: ex
}
export interface Result<T> {
    massage: string
    value: T | null
}

export interface Storage {
    set<T>(key: Key, value: T, expire: ex): void
    get<T>(key: Key): Result<T>
    del(key: Key): void
    clr(): void
}
```



### index.ts

```ts
import { Storage, Key, ex, Data, Result } from "./type";
import { Dictiona } from "./enum"

// function get<T>() {
//     let data: Data<T>
//     console.log([Dictiona.expire]);

// }
// get()
export class storage implements Storage {
    public set<T>(key: Key, value: T, expire: ex = Dictiona.permanent) {
        const data: Data<T> = {
            value,
            [Dictiona.expire]: expire
        }
        localStorage.setItem(key, JSON.stringify(data))
    }
    public get<T>(key: Key): Result<T> {
        const value = localStorage.getItem(key)
        if (value) {
            let data: Data<T> = JSON.parse(value)
            //data[Dictiona.expire] 就是一个属性名
            if (typeof data[Dictiona.expire] == "number" && data[Dictiona.expire] < new Date().getTime()) {
                this.del(key)
                return {
                    massage: `您的${key}已过期`,
                    value: null
                }
            }
            else {
                return {
                    massage: `读取成功`,
                    value: data.value
                }
            }
        } else {
            return {
                massage: `无效`,
                value: null
            }
        }
    }
    public del(key: Key) {
        localStorage.removeItem(key)
    }
    public clr() {
        localStorage.clear()
    }
}
```





### rollup.config.js

> 新版注意问题

```js
//process.env 环境变量
//console.log(process.env);
import ts from 'rollup-plugin-typescript2'
import path from 'path'
import { fileURLToPath } from 'url'
const metaUrl = fileURLToPath(import.meta.url)
const dirName = path.dirname(metaUrl)


export default {
    //入口文件
    input: "./src/index.ts",
    //出口文件
    output: {
        file: path.resolve(dirName, './lib/index.js'),
        //追踪源文件
        sourcemap: true
    },

    plugins: [
        ts(),
    ]
}
```



### package.json

**![image-20230308205504703](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/ts/tsimage-20230308205504703.png)**



### tsconfig.json





# 实战---发布订阅模式

本质上是通过on增加监听key的函数，在通过emit调用（apply）监听key的对应函数

```ts
// 类的类型
interface Events {
    on: (key: string, fn: Function) => void
    emit: (key: string, ...arg: Array<any>) => void
    once: (key: string, callback: Function) => void
    off: (key: string, fn: Function) => void
}
// 每个监听的存储
interface Eventlist {
    [key: string]: Array<Function>
}

class Dispatch implements Events {
    eventlist: Eventlist
    constructor() {
        this.eventlist = {}
    }
    // 本质上只是存起来
    on(key: string, callback: Function) {
        const eventfn: Array<Function> = this.eventlist[key] || []
        eventfn.push(callback)
        this.eventlist[key] = eventfn
    }
    // 这边调用，传入函数参数
    emit(key: string, ...arg: Array<any>) {
        const eventN = this.eventlist[key]
        // 触发调用on存储的函数
        if (eventN) {
            eventN.forEach(fn => {
                fn.apply(this, arg)
            })
        }
        else {
            console.error("该事件未监听");
        }
    }
    off(key: string, fn: Function) {
        let eventName = this.eventlist[key]
        if (eventName && fn) {
            let index = eventName.findIndex(fns => fns === fn)
            eventName.splice(index, 1)
        }
        else {
            console.error("该事件未监听");
        }
    }
    // 本质上是存入 调用后会删除 的函数
    once(key: string, callback: Function) {
        let onc = (...arg: Array<any>) => {
            callback.apply(this, arg)
            // emit触发一次就删掉
            this.off(key, onc)
        }
        // 监听的是onc函数，删掉的也是onc函数
        this.on(key, onc)
    }

}

const o = new Dispatch()

o.once("ss", (...arg: Array<any>) => {
    console.log(arg, "once");
})

const fn = (...arg: Array<any>) => {
    console.log(arg, "point1");
}
// 只是创建监听
o.on('point', fn)
o.on('point', (...arg: Array<any>) => {
    console.log(arg, "point2");
})

// 触发
o.emit('point', "point send")

o.off("point", fn)

o.emit('point', "point send")

o.emit('ss', "sa", { q: 22 }, false)
o.emit('ss', "222")
```



### 函数使用

##### ().apply()

```js
Person.apply(this,arguments)

//把arguments展开通过Person内部的方法加到this中
//或者是在this中带参数arguments使用Person
```



```js
f.apply(this,arg) this代表当前类要执行f类中的内容：
如 定义一个类：function man(sex,age){
    this.sex=sex;this.age=age;
}
那么调用man类中内容的方法如下：
apply：
function human(sex,age){
    man.apply(this,arguments);//this代表当前human类，去执行man类中的内容，arguments为sex，age参数数组
}
call：
function human(sex,age){
    man.apply(this,sex,age);//this代表当前human类，去执行man类中的内容，arguments为sex，age参数数组
}
```



应用

```js
	 vararr1=new Array("1","2","3");
 
	 vararr2=new Array("4","5","6");
 
	 Array.prototype.push.apply(arr1,arr2);
```

```js
var max=Math.max.apply(null,array)
```

```js
 var min=Math.min.apply(null,array)
```









# Proxy Reflect

proxy  代理 13个方法  参数一模一样

Reflect  反射 13个方法 参数一模一样

### Proxy

拦截操作

支持 **对象**、**数组**、**函数**、**set**、**map**

```ts
let person = { name: "mm", age: 23 }

let personProxy = new Proxy(person, {
    // 取值
    //person  属性名  person|实际对象
    get(target,key,receiver) {
        //取值操作管理
        return Reflect.get(target,key,receiver)

    },
    // 赋值
    set() {
        return true
    },
    // 拦截函数调用
    apply() {

    },
    // 拦截 in 操作符
    has() {
        return true
    },
    // 拦截 for in 
    ownKeys() {

    },
    // 拦截删除操作
    deleteProperty() {
        return true
    }

})
```



##### 使用

```ts
let person = { name: "mm", age: 23 }

let personPro = new Proxy(person, {
    get(target, key, receiver) {
        if (target.age > 18) {
            return '已成年'
        }
        return Reflect.get(target, key, receiver)
    }
})


console.log(person.age);    //23
console.log(personPro.age);     //已成年
```





##### receiver

它正是可以修改属性访问中的 this 指向为传入的 receiver 对象。

保证上下文一致

```ts
const parent = {
  name: '19Qingfeng',
  get value() {
    return this.name;
  },
};
const handler = {
  get(target, key, receiver) {
    return Reflect.get(target, key);
    // 这里相当于return target[key]
  },
};
const proxy = new Proxy(parent, handler);
const obj = {
  name: 'wang.haoyu',
};
// 设置obj继承与parent的代理对象proxy
Object.setPrototypeOf(obj, proxy);
console.log(obj.value);  // 19Qingfeng
```

分析下上边的代码：

- 当我们调用 obj.value 时，由于 obj 本身不存在 value 属性。
- 它继承的 proxy 对象中存在 value 的属性访问操作符，所以会发生屏蔽效果。此时会触发 proxy 上的 get value() 属性访问操作。
- 同时由于访问了 proxy 上的 value 属性访问器，所以此时会触发 get 陷阱。进入陷阱时，target 为源对象也就是 parent ，key 为 value 。
- 陷阱中返回 `Reflect.get(target,key)` 相当于 `target[key]`。此时，不知不觉中 this 指向在 get 陷阱中被偷偷修改掉了！！
- 原本调用方的 obj 在 get 陷阱中被修改成为了对应的 target 也就是 parent 。自然而然打印出了对应的 `parent[value]` 也就是 19Qingfeng 。

　　这显然不是我们期望的结果，当我访问 obj.value 时，我希望应该正确输出对应的自身上的 name 属性也就是所谓的 obj.value => wang.haoyu 。那么，Relfect 中 get 陷阱的 receiver 就大显神通了。

```ts
const handler = {
  get(target, key, receiver) {
-   return Reflect.get(target, key);
+   return Reflect.get(target, key, receiver);
  },
};
```

（1）首先，之前我们提到过在 Proxy 中 get 陷阱的 receiver 不仅仅会表示代理对象本身，同时也还有可能表示继承于代理对象的对象，具体需要区别于调用方。这里显然它是指向继承与代理对象的 obj 。

（2）其次，我们在 Reflect 中 get 陷阱中第三个参数传递了 Proxy 中的 receiver 也就是 obj 作为形参，它会修改调用时的 this 指向。

> 你可以简单的将 `Reflect.get(target, key, receiver)` 理解成为 `target[key].call(receiver)`，不过这是一段伪代码，但是这样你可能更好理解。



### **mobx观察者模式**

值改变会做出反应

```ts
const watchlist: Set<Function> = new Set()

// 加入观察函数
function autowatch(fn: Function) {
    if (fn) {
        watchlist.add(fn)
    }
}

const personProxy = (obj: any) => {
    return new Proxy(obj, {
        set(target, key, value, receiver) {
            let result = Reflect.set(target, key, value, receiver)
            // 调用观察函数
            watchlist.forEach(fn => fn())
            return result
        }
    })
}

const personal = personProxy({
    name: "mm",
    age: 23
})

console.log(personal);

// 创建观察函数
autowatch(() => {
    console.log("值发生了改变");
})

personal.age = 28

personal.name = "majh"
```





****



# 协变（只多不少）

值上

鸭子类型

interface 里 一个大的能包含一个小的，可以实现赋值

```ts
interface A {
    name:string
    age:number
}
 
interface B {
    name:string
    age:number
    sex:string
}
 
let a:A = {
    name:"老墨我想吃鱼了",
    age:33,
}
 
let b:B = {
    name:"老墨我不想吃鱼",
    age:33,
    sex:"女"
}

//主类型 A 
//子类型 B
//B 里面包含着 A
a = b
```





# 逆变（只多不少）

函数上

```ts
interface A {
    name:string
    age:number
}
 
interface B {
    name:string
    age:number
    sex:string
}

let fna = (params:A)=>{
}
let fnb = (params:B)=>{
}
//逆变
fnb = fna
//此时调用fnb()，实现的还是fna()
//A还是主类型

```



# 双向协变

fna = fnb

fnb = fna

最好不用







# Partial

变成可选（？）参数

源码

```ts
/**
 * Make all properties in T optional
  将T中的所有属性设置为可选
 */
type Partial<T> = {
    [P in keyof T]?: T[P];
};
```



使用前

```ts
type Person = {
    name:string,
    age:number
}
 
type p = Partial<Person>
```



使用后

```ts
type p = {
    name?: string | undefined;
    age?: number | undefined;
}
```



# Pick

摘选出指定属性的类型

源码

```ts
/**
 * From T, pick a set of properties whose keys are in the union K
 */
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};
```

使用

```ts
type Person = {
    name:string,
    age:number,
    text:string
    address:string
}
 
type Ex = "text" | "age"
 
type A = Pick<Person,Ex>
```





# Readonly

设置每个属性只读

源码

```ts
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};
```

1 keyof我们讲过很多遍了 将一个接口对象的全部属性取出来变成联合类型

2 in 我们可以理解成for in P 就是key 遍历 keyof T 就是联合类型的每一项

3 Readonly 这个操作就是将每一个属性变成只读

4 T[P] 索引访问操作符，与 JavaScript 种访问属性值的操作类似





# Record

倒转

多条相同格式数据记录

```ts
type Record<K extends keyof any, T> = {
    [P in K]: T;
};

//keyof any 返回 string number symbol 的联合类型 
type person{
	name:string
}

type K = 1 | 2 | 3
type B = Record<K,person>

let obj:B = {
    1:{name:"ma"},
    2:{name:"ma"},
    3:{name:"ma"}
}
```

做到了约束 对象的key 同时约束了 value



# infer

infer 是[TypeScript](https://so.csdn.net/so/search?q=TypeScript&spm=1001.2101.3001.7020) 新增到的关键字 充当占位符

常用于extends后

```ts
type Infer<T> = T extends Array<any> ? T[number] : T		//T[number] 指数组元素值
 
type A = Infer<(boolean | string)[]>
 
type B = Infer<null>
```



```ts
type Infer<T> = T extends Array<infer U> ? U : T
 
type A = Infer<(string | Symbol)[]>
```



提取一个元素

```ts
type Arr = ['a', 'b', 'c']

type one<T extends any[]> = T extends [infer U, ...any[]] ? U : T

let aaaa: one<Arr> = "a"
```



### 递归

```ts
type Arr = ['a', 'b', 'c']

type reserve<T extends any[]> = T extends [infer first, ...infer rest] ? [...reserve<rest>, first] : T

type Arrb = reserve<Arr>
```




### declare
```ts
//Type.d.ts文件
export declare interface sss {
    one:number
}


//使用文件
//可以直接导入这个interface
import type {sss} from './Type'
```

```ts
//模块化
//Type.d.ts文件
declare module "aaa" {
	interface sss{
		one:number
	}	
}


//使用文件
//可以直接导入这个interface
import type {sss} from 'aaa'
```

