# 初始配置和编译
```javascript
1.初始化package文件
	 npx init -y
2.安装 ：npm install typescript --dev 
	 安装完成后在module下有个bin目录，里面有个tsc,tsc是用来编译的
3.初始化tsconfig.json文件
	outDir    -- 编译后代码输出的位置
	rootDir   -- 编译入口
	sourceMap -- 生成映射文件
	target    -- 编译的目标代码 如 'ES2015'
4.编译单个文件
	npx tsc filePath  || yarn tsc filePath
5.编译整个项目
	npx tsc || yarn tsc
6.只有编译整个文件，tsconfig.json 文件才会生效，编译单个文件是不生效的
```
# 原始数据类型
```javascript
const s:string = 'string'
const b:number = 123 || NaN || Infinity
const c:boolean = true || false
//在非严格模式下，以上三种数据类型也可存储空值
const d:string = null || undefined
const f:void = null || undefined
const e:undefined = undefined
const g:null = null
const h:symbol = Symbol()

注意，当使用了内置对象，而没有引入对应的标准库时，typescript会找不到对应的对象声明，从而无法进行类型检查，就会报错
如ES2015新增的Promise、Symbol内置对象，如果在tsconfig的target设置的是ES5时，就会报错，因为ES5里面没有ES6新增对象的类型声明
可以在lib里面引入'ES2015',但只引入这一个标准库，console.log 又会报错，因为console.log 是浏览器给的内置对象，ES2015没有对其的声明，这时就需要再引入DOM标准库
typescript默认使用的是一个全局类型声明库，因为包含了ES5的内置对象声明和DOM、BOM对象声明
当在lib使用ES2015声明库时，会覆盖掉全局类型声明库，这样就需要额外引入DOM标准库
```
# 对象object类型
```javascript
//object 指广泛的对象，可以是对象，数组，函数
const foo:object = {} || [] || function(){}
//对象限制可以如以下
const bar:{x:number,y:string} = {x:12,y:'123'}
```
# 数组类型
```javascript
//第一种定义方式
const arr:Array<number> = [2,3,3,4,5]
//第二种定义方式
const arr1:number[] = [12,3,45,6]
//固定内部参数个数和类型，也可称元组
const arr2:[number,string] = [1,'213']
//案例
function add(...args:number[]){
  return args.reduce((pre,cur)=> pre + cur , 0)
}
add(1,2,3,4,5)
```
# 元组类型
```javascript
export {}
//元组类型，明确元素个数和元素类型的数组
const arr:[number,string] = [123,'string']
console.log(arr[0]);
console.log(arr[1]);
const [num,str] = arr //结构
const obj = {name:'zs',age:18}
Object.entries(obj) //返回的就是一个元组类型的数据

```
# 枚举类型
```javascript
enum publishStatus {
   draf = 0,
   ubpublich = 1,
   publish = 2
}
const ifpubilis  = {
  status:publishStatus.draf
}
//以上代码会编译成以下代码
var publishStatus;
(function (publishStatus) {
	//会生成双向键值对，通过值可以访问健，通过键可以访问值
    publishStatus[publishStatus["draf"] = 0] = "draf";
    publishStatus[publishStatus["ubpublich"] = 1] = "ubpublich";
    publishStatus[publishStatus["publish"] = 2] = "publish";
})(publishStatus || (publishStatus = {}));
var ifpubilis = {
    status: publishStatus.draf
};

//如果不想生成双向键值对，可以使用常量枚举
const enum publish {
  draf = 0,
  ubpublich = 1,
  publish = 2
}
注意，menu类型是会侵入到原代码的，其他typescript代码大部分在编译后都会去掉类型检测所使用的类型限制，而eunm会编译到es5代码中
```
# 函数类型
```javascript
function add(num1:number,num2:number):string {
  return '123'
}
add(1,2,66) //不会报错，不太清楚
add(5) //不会报错，不太清楚，问了AI说没打开strictFunctionTypes，打开了没报错，这个的功能主要是对函数参数的严格检查，包括个数类型

const add1 = (n:number,n1:number):string=>{
  return '123'
}
//add1也有类型，是一个函数类型，并返回一个字符串，本质上add1就是它存储的那个函数，可手动指定，也可通过类型推断
```
# any类型
```javascript
//any 接收任意类型数据，也是动态类型语言
function jsonStyFly(value:any):string{
  return JSON.stringify(value)
}
```
# 隐式类型推断
```JavaScript
export {}
let age = 18 //类型推断为number
age = '' // 报错
let foo // 类型推断为any
//以下都不报错
foo = 18
foo = 'str'
foo = true 
```
# 类型断言
```javascript
export {}
const nums = [5,2,3,4,5]
const res = nums.find(i => i > 1) //类型推断res为number || undefined
//这时我们明确知道res肯定是一个数字,可以使用as类型断言
const num1 = res as number //num1 的类型为number
num1 * num1
//可以使用<>类型断言
const num2 = <number>res //num2 的类型为number
//推荐使用as关键字，因为<>会与JSX的语法冲突
```
# 接口interface
```javascript
//接口，就是一种规范，当一个对象运用了某一个接口，就必须遵守这个接口的所有要求,相当于Post是一个磨具、模板，对象运用了接口，就变成了这个接口的形状
interface Post{
  title : string
  url : string
}
function urlPost(post:Post){
  console.log(post.title);
  console.log(post.url);
}
//接口补充 可选，只读，动态
interface Message{
  name:string
  age:number
  //可选成员
  sex?:string
  //只读成员
  readonly phone:string
}
const peoper:Message = {
  name : 'zs',
  age : 18,
  phone : '1333333333'
}
//动态成员
interface Cached {
  [key:string] : string
}
const cached:Cached = {
}
cached.name = 'zs'
cached.phone = '123333'

```
# class
```javascript
typescript类使用的细微区别，调用前需要先声明变量，这是为了做类型注解，如果没有先声明变量然后给它个类型，那么构造器内的赋值就会报错，因为不知道这个this.name 是什么类型的
class Person{
  name:string
  age:number
  constructor(name:string,age:number){
    this.name = name
    this.age = age
  }
  sayHi(msg:string){
	  console.log(`Hi,I am ${this.name}`)
  }
}
```
# class访问修饰符
```javascript
export {}
//private 私人的，只在本类中能使用
//protected 受保护的，能够继承，在子类中访问到
//readonly 只读，可在声明时赋值，或者在构造函数中赋值，只能选一种，并且如果成员已经有属性修饰符，readonly只能跟在其后面
class Person{
  name:string
  private age:number
  protected readonly gender:boolean
  constructor(name:string,age:number){
    this.name = name
    this.age = age
    this.gender = true
  }
  sayHi(msg:string){
    console.log(`I am ${this.name} I am ${this.age} ${msg}`);
  }
}
class Student extends Person {
  constructor(name:string,age:number){
    super(name,age)
    
  }
  testProtected(){
    console.log(this.gender); //之类访问父类protected数据
  }
}
```
# class 和 interface
```JavaScript
//class和interface可以定义多个接口，对class进行约束
interface Run{
  run(method:string):void
}
interface Eat{
  eat(food:string):void
}
/*
class Peoper implements Run,Eat{
  constructor(){}
  run(method: string): void {
    console.log(`直立行走:${method}`);
  }
  eat(food: string): void {
    console.log(`优雅的进餐：${food}`);
    
  }
  learning(book:string):void{
    console.log(`学习${book}`);
  }
}
*/
  
/*
class dog implements Eat,Run{
  constructor(){}
  run(method: string): void {
    console.log(`爬行:${method}`);
  }
  eat(food: string): void {
    console.log(`吭哧吭哧：${food}`);
  }
  catchMouse(name:string):void{
    console.log(`抓住了${name}`);
    
  }
}
*/


interface other{
  meg:{name:string,age:number}
  arr:number[]
  learning(book:string):void
}
const student:other = {
  meg:{
    name:'zs',
    age:18
  },
  arr:[90,80,70,60],
  learning(book:string):string{
    return `学习${book}`
  }
}
console.log(`我是${student.meg.name},今年${student.meg.age}
,数学考了${student.arr[3]}比较差,所以我要${student.learning('高数')}`); 
```
# 抽象类
1. 抽象类和接口有点类型，但接口里面没有具体的实现，抽象类有具体的实现
2. 抽象类不可用new关键字创建实例，只能通过继承
3. 抽象方法只定义不实现，继承后由子类根据需要自定义实现
```javascript
export {}
abstract class Animal{
  //一样的方式可以在抽象类实现
  breath(nose:string):void{
    console.log(`都一样用${nose}呼吸`);
  }
  //不一样的方式自定义实现，例如狗是四肢爬行，人是直立行走
  abstract run(method:string):void
}
class Dog extends Animal{
//不一样的方式
  run(method: string): void {
    console.log(`四肢爬行:${method}`);
  }
}
class Peoper extends Animal{
//不一样的方式
  run(method: string): void {
    console.log(`直立行走:${method}`);
  }
}
const dog =  new Dog()
const peoper = new Peoper()
dog.breath('鼻子')
peoper.breath('鼻子')
	dog.run('xiuxiuxiu')
peoper.run('优雅')
```
# 泛型
```javascript
//创建数字类型数组
function creatNumberArray(length:number,value:number):number[]{
  return Array<number>(length).fill(value)
}
//创建字符串类型数组
function creatStringArray(length:number,value:string):string[]{
  return Array<string>(length).fill(value)
}
function creatArray<T>(length:number,value:T):T[]{
  return Array<T>(length).fill(value)
}
//泛型就是把不确定的类型参数通过T传递进去
creatArray<string>(3,'123')
creatArray<number>(3,123)
```
# 类型声明
```javascript
import {camelCase} from "lodash"
declare function camelCase(input:string):string
const res = camelCase('hello types')
```