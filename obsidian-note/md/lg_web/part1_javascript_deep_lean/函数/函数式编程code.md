# 函数作为参数代码
```javascript
	//foreach 
const foreach = function(arr,fn){
	for(let i = 0 ; i < arr.length;i++){
	  fn(arr[i])
	 }
}
//调用foreach
const arr = [1,2,3,4,5,6]
foreach(arr,(item)=>{
  console.log(item)
})
//定义filter函数
const filter = function(arr,fn){
  let newArr = []
  for(let i = 0;i < arr.length;i++){

    if(fn(arr[i])){
      newArr.push(arr[i])
    }
  }
  return newArr
}
//调用
const arr1 = [1,2,3,4,5,6,7,8]
const newArr =  filter(arr1,(item)=>{
  return item %2 === 0
})
console.log(newArr);
```

# 函数作为返回值
```javascript
/*
支付场景
调用一个once函数，该函数接收一个支付函数
once函数的功能是限制传递进去的支付函数只能执行一次
*/

const once = function(fn){
  let done =false
  return function(){
    if(!done){
      done = true
      return fn.apply(this,arguments)
    }
  }
}
const pay = once(function(money){
  console.log(`支付了${money}元`);
})
pay(5)
pay(5)
pay(5)
```
# 模拟map,every,some
```javascript
//模拟map,every,some
//map作用，对数组进行遍历，对每一个元素进行处理，最后返回新数组
const map = (arr,fn)=>{
  let newArr = []
  for (const item of arr) {
    newArr.push(fn(item))
  }
  return newArr
}
//测试
let arr = [1,2,3,4,5,6]
arr = map(arr,(item)=>{
  return item * item
})
console.log(arr);

//every的作用，每一个元素都必须满足条件，结果返回一个布尔值
const every = (arr,fn)=>{
  let result = true
  for (const value of arr) {
    result = fn(value)
    if(!result){
      break
    }
  }
  return result
}
//测试
let arr1 = [1,2,3,4,5]
let result = every(arr1,(item)=>{
  return item > 10
})
console.log(result);

//some的作用，只要有一个满足，结果就返回true,否则返回false
const some = (arr,fn)=>{
  let result = false
  for (const value of arr) {
    result = fn(value)
    if(result){
      break
    }
  }
  return result
}
//测试
let arr2 = [1,2,3,4,5,6]
let result2 = some(arr2,(item)=>{
  return item %2 === 0
})
console.log(result2);
```
# 闭包两个案例
```javascript
//创建一个函数，该函数得到一个多少次方的参数，并且返回一个函数，返回的函数接受一个数字
    const makePow = function (power) {
      return function (number) {
        return Math.pow(number, power)
      }
    }
    const pow2 = makePow(2)
    const pow3 = makePow(3)
    console.log(pow2(2))
    console.log(pow2(5))
    console.log(pow3(5))

//求基本工资+绩效案例
    const makeIncome = function (base) {
      return function (workHard) {
        return base + workHard
      }
    }
    const incomeLeave1 = makeIncome(12000)
    const incomeLeave2 = makeIncome(15000)
    console.log(incomeLeave1(2000))
    console.log(incomeLeave2(3000))
```
# slice、splice演示
```javascript
/*
slice splice都可用于数组截取
slice不改变原数组，第一个参数是开始索引，第二个参数是结束索引，且取不到第二个参数,是纯函数
splice改变原数组，第一个参数是开始索引，第二个参数是取几个，不是纯函数
*/
const arr1 = [1,2,3,4,5]
console.log(arr1.slice(0,3));
console.log(arr1.slice(0,3));
console.log(arr1.slice(0,3));
const arr2 = [1,2,3,4,5,6,7]
console.log(arr2.splice(0,3));
console.log(arr2.splice(0,3));
console.log(arr2.splice(0,3));
```
# lodash部分函数演示
```javascript
/*
fist,last,toupper,reverse,each,includes,find,findIndex
*/
const _ = require('lodash')
let arr = ['jack','mick','tom','merry']
console.log(_.first(arr));
console.log(_.last(arr));
console.log(_.toUpper(_.first(arr)));
console.log(_.reverse(arr));//会影响原数组
console.log(_.each(arr,(item)=>{console.log(item);}));
```
# memoize记忆函数的演示和模拟实现
```javascript
//演示
const _ = require('lodash');
function getArea(r){
  console.log('半径为'+r);
  return Math.PI * r * r
}
const getAreaWithMemory = _.memoize(getArea)
console.log(getAreaWithMemory(5));
console.log(getAreaWithMemory(5));

//模拟实现
function memoize(fn){
  let cache = {}
  return function(r){
    cache[r] = cache[r] || fn(r)
    return cache[r]
  }
}
const getAreaWithMemory = memoize(getArea)
console.log(getAreaWithMemory(5));
console.log(getAreaWithMemory(5));
console.log(getAreaWithMemory(5));
console.log(getAreaWithMemory(5));
```
# 柯里化函数案例
```javascript
const _ = require('lodash')
//字符串的match，用来匹配字符串内是否有正则对应的规则内容
''.match(/\s+/g)//匹配字符串中是否有空格 +表示任意多个
''.match(/\d+/g)//匹配字符串是否有数字
/*
如果要匹配多个字符串是否有空格、数字，这样子直接调用match
正则会用太多遍，不太合适
可以用闭包，固定一个参数（正则），传入一个参数（字符串）
*/
const makeMatch = function(reg){
  return function(str){
    return str.match(reg)
  }
}
const matchSpace = makeMatch(/\s+/g)
const matchNum = makeMatch(/\d+/g)
console.log(matchSpace('hi joker'));
console.log(matchNum('i loveyou 520'));

//如果要对一个数组内的元素，进行判断是否有对应规则的内容
let arr =['joker king','mike_done','123af']
const filter = _.curry(function(fun,array){
  return array.filter(fun)
})
const filterSpace = filter(matchSpace)
const filterNum = filter(matchNum)
console.log(filterSpace(arr));
console.log(filterNum(arr));
```
# 柯里化推演
```javascript
//需求，对数组的每一个元素进行判断是否有空格、数字
// 普通形态
let arr = ['hello world','jone knnne','123aaas','juefft']
// 那么只需要遍历每一个数组，然后给遍历出来的元素来个match
const result =  arr.filter((item)=>{
  return item.match(/\s+/g) || item.match(/\d+/g)
})
console.log(result);
/*
以上代码可以实现，但是不能复用，例如我有一百个数组
那么需要来一百次，如此可以考虑封装成函数
*/
/*
考虑到item.match(/\s+/g)较为长，所以弄成一个变量
*/
const spaceMatch = (item)=>{
  return item.match(/\s+/g)
}
const numMatch = (item)=>{
  return item.match(/\d+/g)
}
const arrFilter = function(fun,arr){
  return arr.filter(fun)
}
const result2 =  arrFilter(spaceMatch,arr)
console.log(result2);
/*
现在已经完成需求，但进一步思考，发现如果有一百个，那么arrFilter的第一个参数
一直是重复的，这样子可以考虑柯里化
*/
const _ = require('lodash')
const arrFilterCurry = _.curry(function(fun,arr){
  return arr.filter(fun)
})
const spaceCurry = arrFilterCurry(spaceMatch)
const NumCurry = arrFilterCurry(numMatch)
console.log(spaceCurry(arr));
console.log(NumCurry(arr));
```
# 柯里化实现
```javascript
function getSum(a,b,c){
  return a+b+c
}
const curry = (fun)=>{
  return function curried(...args){
    if(args.length < fun.length){
      return function(){
        return curried(...args.concat(Array.from(arguments)))
      }
    }else{
      return fun(...args)
    }
  }
}
const curried = curry(getSum)
console.log(curried(1)(2)(3));
```
# 模拟组合函数compose实现
```javascript
//lodash中的组合函数float-right
const _ = require('lodash')
const compose_lodash =  _.flowRight( _.toUpper, _.first, _.reverse)
console.log(compose_lodash(['abc','cbd','nba']));
//自己实现组合函数
const compose = function(...args){
  return function(value){
    return args.reverse().reduce(function(acc,fn){
      return fn(acc)
    },value)
  }
}
const reverse = array => array.reverse()
const first = array => array[0]
const toUpper = str => str.toUpperCase()
const firtUpper = compose(toUpper,first,reverse)
console.log(firtUpper(['abc','cbd','nba']));
```
# 组合函数调试
```javascript
//使用函数组合，实现 'ABC BCD NBC' 转化成 'abc-bcd-nbc'
// 自定义柯里化函数
const curry = (fun)=>{
  return function curried(...args){
    if(args.length < fun.length){
      return function(){
        return curried(...args.concat(Array.from(arguments)))
      }
    }else{
      return fun(...args)
    }
  }
}
// 自定义组合函数
const compose = function(...args){
  return function(value){
    return args.reverse().reduce(function(acc,fn){
      return fn(acc)
    },value)
  }
}
// 定义调试打印
const log = curry((str ,val) =>{
  console.log(str , val);
  return val
})
// 柯里化lodash中参数是多个的函数
const _ = require('lodash')
const split = curry(function(sep,str){
  return _.split(str,sep)
})
const join = curry(function(sep,arr){
  return _.join(arr,sep)
})
 const map = curry(function(fn,arr){
  return _.map(arr,fn)
 })
//  组合函数，形成新数组
const f = compose(join('-'),log('map:'), map(_.toLower),log('split:'), split(' '))
console.log(f('ABC BCD NBC'));
```
# lodash的fp模块
```javascript
const { toLower } = require('lodash')
const fp = require('lodash/fp')
//实现 'ABC BCD NBC' 转化成 'abc-bcd-nbc'
const f = fp.flowRight(fp.join('-'), fp.map(toLower), fp.split(' '))
console.log(f('ABC BCD NBC'));
////把字符串的首字母提取，并转换为大写 world wild web -  W,W,W 这也是point free 编程样式
const fp = require('lodash/fp')
//这样子写的话有两次遍历数组，性能较低
const firstLetterToUpper = fp.flowRight(fp.join(','),fp.map(fp.first),fp.map(fp.toUpper) ,fp.split(' '))
//可以把fp.first 和 fp.toUpper合并成一个函数
const firstLetterToUpper = fp.flowRight(fp.join(','),fp.map(fp.first),fp.map(fp.flowRight(fp.first,fp.toUpper)) ,fp.split(' '))
console.log(firstLetterToUpper('world wild web'));
```
# 函子的定义调用
```javascript
/*
函子内部保存一个值，并且向外暴露一个操作该值的方法，该方法接收一个函数
这个函数是真的意义上的对值的操作函数
暴露出来的map方法返回一个新的函子，新函子内部的值是上一个函子保存的值通过
外部传递的函数处理过后的值
*/
class Container {
  static of(value){
    return new Container(value)
  }
  constructor(value){
    this._value = value
  }
  map(fn){
    return Container.of(fn(this._value))
  }
}
const funtor =  Container.of(5)
  .map(x => x+1)
  .map(x => x*x)
  console.log(funtor);
```
# MayBe函子
```javascript
//普通函子无法处理null,underfined
class MayBe{
  static of(value){
    return new MayBe(value)
  }
  constructor(value){
    this._value = value
  }
  map(fn){
    return this.isNothing() ? MayBe.of(null) : MayBe.of(fn(this._value))
  }
  isNothing(){
    return this._value === null || this._value === undefined
  }
}
const r =  MayBe.of('sss')
  .map(x => x.toUpperCase())
  console.log(r);
  //maybe函子也有问题，就是无法处理在哪里出现了null或者undefined
  const r =  MayBe.of('sss')
  .map(x => x.toUpperCase())
  .map(x => null)
  .map(x => x.toUpperCase())
  console.log(r);
```
# Either函子
```javascript
class Left{
  static of(value){
    return new Left(value)
  }
  constructor(value){
    this._value = value
  }
  map(fn){
    return this
  }
}
class Right{
  static of(value){
    return new Right(value)
  }
  constructor(value){
    this._value = value
  }
  map(fn){
    return Right.of(fn(this._value))
  }
}
//定义一个函数，用来转换json格式的字符串成json对象
function jsonParse(str){
  try {
    return Right.of(JSON.parse(str))
  } catch (error) {
    return Left.of({error: error.message})
  }
}
let r = jsonParse('{"name" : "zs"}').map(x => x.name.toUpperCase())
console.log(r);
```
# IO函子
```javascript
/*
IO函子的value是一个函数
创建实例的时候，会把传进去的值包装成函数，然后传给构造器函数
之后，在map函数里，会进行值包装成的函数和操作值的函数进行合并成一个新函数
flowRight就是执行一个函数后得到的值，传递给上一个函数
然后map会返回一个新的IO函子，其中的值就是上一个IO函子的操作函数，如果调用就会得到上一个函子的操作函数操作之后的数据
*/
const fp = require('lodash/fp')
class IO{
  static of(value){
    return new IO(function(){
      return value
    })
  }
  constructor(fn){
    this._value = fn
  }
  map(fn){
    return new IO(fp.flowRight(fn,this._value))
  }
}
const r = IO.of(process).map(p => p.execPath)
console.log(r._value());
```
# folktale
```javascript
//folk的compose和curry的使用
const {compose,curry} = require('folktale/core/lambda')
const {first,toUpper} = require('lodash/fp')
const curried = curry(2, (x,y)=>{
  return x + y
})
console.log(curried(1)(2));
const f = compose(toUpper, first)
console.log(f(['abc','cba']));
```