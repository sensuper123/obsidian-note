# 安装
1. 初始化package文件 --- npm init -yes
2. 安装flow-bind          --- npm install flow-bin --dev 
# 初体验
```javascript
需在代码前面添加@flow
且需初始化.flowconfig文件 yarn flow init || npx flow init
开始一个后台服务监视代码  yarn flow   || npx flow
//@flow
function add(num1:number,num2:number){
  return num1 + num2
}
add('100','200')
//但是类型注解的方式不符合JavaScript语法，是无法运行的，所以在开发完后需要把类型注解去掉
```
# 编译去掉类型注解的两种方式
1. 使用flow-remove-types
```JavaScript
操作步骤
安装 ：npm install flow-remove-types --dev
使用 : npx flow-remove-types ./src -d .dist 
//   ./src          ----是要去掉注解的源码
//   -d .dist    ---- 是指定去掉注解后的新代码放在根路径dist文件夹下
```
2. 使用babel
```javascript
安装 ： npm install @babel/core @babel/cli @babel/preset-flow
//@babel/core 是babel核心代码、@babel/cli是可以在命令行使用babel的工具、@babel/preset-flow是去掉flow注解的一个预设
配置 ：初始化.babelrc 文件
{"preset":["@babel/preset-flow"]}
使用 ：npx babel src -d .dist
// src是源码入口 -d .dist 是去掉flow注解后的代码放在根目录dist文件下
```


# 类型注解
```javascript
// 参数类型注解
function add(n:number){
  return n * n
}
//返回值类型注解
function add1(n:number):number{
  return n * n
}
//返回值为underfined类型注解
function add1(n:number):void{
   n * n
}
```
# 原始类型
```javascript
let s:string = 'string'
let n:number = 123 || Nan || Infinity
let b:boolean = true || false
let nu:null = null
let e:void = undefined
let f:Symbol = Symbol()
```
# 数组类型
```javascript
const arr1:Array<number> = [1,2,3]
const arr2:string[] = ['foo','five']
//固定长度的数组，也可称为元组
const arr3:[string,number] = ['foo',123]
```
# 对象类型
```javascript
/*
@flow
*/
const obj1:{foo:string,bar:number} = {foo:'foo',bar:12}
const obj2:{foo?:string,bar:number} = {bar:100}
const obj3:{[string]:string} = {}
obj3.key1 = 'key1'
obj3.key2 = 'key2'
```
# 函数类型
```javascript
/*
@flow
*/ 
function foo(callBack:(string,number)=>void){
  callBack('string',100)
}
foo((str,n)=>{
  console.log(n);
})
```
# 特殊类型
```javascript
/** 
 * @flow
 * */ 
const a:'foo' = 'foo'
const types:'success'|'warning'|'danger' = 'warning'
type stringOrNumber = string | Number
const b:stringOrNumber = 'string'
const gender:?number = undefined
const gender : number | null | void = undefined
```
# 任意类型
```JavaScript
/*
 * 任意类型 mixed any
 * @flow
*/
// mixed 强类型
function passMixed (value: mixed) {
  if (typeof value === 'string') {
    value.slice(1)
  }
  if (typeof value === 'number') {
    value * value
  }
}
passMixed('str');
passMixed(123);


// any 弱类型 （兼容老语法）
function passAny (value: any) {
  value.slice(1)

  value * value
}
passAny('str');
passAny(123);
```