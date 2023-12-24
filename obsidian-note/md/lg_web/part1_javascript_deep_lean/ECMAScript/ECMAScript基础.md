# 其他笔记
```
https://zhuanlan.zhihu.com/p/345859715
```
# JavaScript和ECM
AScript的关系
```javascript
ECMAScript是JavaScript的语言基础，提供了基础语法等
JavaScript是ECMAScript的扩展
在window环境下，JavaScript包含ECMAScript、webApi(bom,dom)
在node环境下，JavaScript包含ESMAScript、fs、path等等模块
```

# ES2015 / ES6的改变
![](https://s2.loli.net/2023/12/24/Yq5WwEyJTRaBGtM.png)

# let 、const块级作用域
```javascript
1.var在函数外声明的变量会变成全局变量
	这样子在for循环注册事件时如以下代码
	var arr = [{},{},{}]
	for(var i = 0;i<arr.length;i++){
	  arr[i].onclick = ()=>{
	    console.log(i);
	  }
	}
	arr[0].onclick() --3
	arr[1].onclick() --3
	arr[2].onclick() --3

2.for循环比较特殊，实际上for循环有两层块级作用域，因为判断语句也有块级作   用域，把以上代码改写其实是：
	for(let i = 0 ; i < arr.length; i++){
		let i = 'foo'
		console.log(i) - foo
	}
	let i = 0
	if(i < arr.length){
		let i = foo
		console.log(i) -foo
	}

3.var关键字声明的变量有变量提升

4.cosnt 关键字 注意点：const只是不能修改地址值，如：
	const obj = {}
	obj.name = 'zs'
	以上是允许的
	obj = {}
	报错
```

# 数组对象解构
```javascript
	--对应
		const arr = [1,2,3]
		const [first,two,three] = arr
		console.log(first,three); --1 
		
	--特定对应，省略位置加,
		const [,four] = arr
		console.log(four) --2
		
	--剩余对应
		const [five,...more] = arr
		console.log(five) -- [ 2, 3 ]
		
	--对象解构
		对象解构，要根据对象名来进行解构
		let obj = {
		  name :'zs',
		  age : 18
		}
		//若有命名冲突
		let name = 'li'
		let {name : objName = '初始值',age} = obj
		console.log(objName);
		console.log(age);
```

# 模板字符串
	1. 支持插值表达式，如${1+2}
	2. 如果要在字符串内有`` 可以用转义字符\
	3. 支持换行

# 字符串扩展方法
```javascript
/*includes(),startsWith(),endsWith()*/
let str = `Error : you message is failed.`
console.log(str.startsWith('Error'));
console.log(str.endsWith('.'));
console.log(str.includes(':'));
```

# 对象扩展方法
```javascript
Object.assign
	Object.assignconst source = {
	  a : 123,
	  b : 123
	}
	const target = {
	  a : 456,
	  c : 789
	}
	Object.assign(target,source)
	console.log(target); --{ a: 123, c: 789, b: 123 }
	
Object.is 
		console.log(Object.is(+0,-0)) -- true
		console.log(Object.is(NaN,NaN)) -- true
```
# Proxy
```javascript
const person = {
  name : 'zs',
  age : 18
}
const proxyPerson = new Proxy(person,{
  get(target,property){
    return property in target ? target[property] : 'underind'
  },
  set(target,property,value){
    if(property === 'age'){
      if(!Number.isInteger(value)){
        throw new Error(`${value} is not an int `)
      }
    }
    target[property] = value
  }
})
console.log(proxyPerson.age);
```

# Reflect
```javascript
//reflect实际上是一个操作对象的库
//如proxy里面的操作方法，本质在内部是调用reflect方法
const obj1 = {
  name :'zc',
  age:18
}
const obj1Proxy = new Proxy(obj1,{
  get(target,property){
    console.log('watch obj1');
    return Reflect.get(target,property)
  }
})
// console.log(obj1Proxy.name);
// console.log('name' in obj1);
// console.log(Object.keys(obj1));
// console.log(delete obj1.name);
console.log(Reflect.ownKeys(obj1));
console.log(Reflect.deleteProperty(obj1,'name'));
console.log(Reflect.has(obj1,'age'));
console.log(obj1);
```
# class
```javascript
//类 实例方法 静态方法 extends
class Person{
  constructor(name){
    this.name = name 
  }
  sayName(){
    console.log(this.name);
  }
}
const p1 = new Person('zs')
p1.sayName()

//继承，相当于拥有父元素的所有方法和属性，但需要调用super把父类需要的参数传递过去，相当于父类执行一遍，从而子类拥有父类的属性和方法
class Student extends Person{
  constructor(name,stuNum){
    super(name)
    this.stuNum = stuNum
  }
  sayStuNum(){
    console.log(this.stuNum);
  }
}
const p2 = new Student('li','001')

p2.sayName()
p2.sayStuNum()
console.log(p2.name);
```
# Set
```javascript
// Set 数据结构，是一个集合，可以看成是一个数组，但元素不能重复
const s = new Set()
s.add(1).add(3).add(2).add(1)
console.log(s);

const arr = [1,2,3,4,5,6,6,5,4,3,2,1]
const s1 = new Set(arr)
console.log(s1); //-Set { 1, 2, 3, 4, 5, 6 } 
const s2 =  Array.from(s1) 
console.log(s2); //[ 1, 2, 3, 4, 5, 6 ]
const s3 = [...new Set(arr)]
console.log(s3); //[ 1, 2, 3, 4, 5, 6 ]

//Set几种方法
console.log(s1.has(2));
s1.forEach((item)=>{console.log(item)})
s1.delete(2)
console.log(s1); //Set { 1, 3, 4, 5, 6 }
console.log(s1); //Set {}
```
# Map
```javascript

//Map 数据结构，类似于对象，只是对象的键只能是字符串，而Map的键可以是任何一种类型

const m = new Map()
//通过set方法来设置，键值对
const tom = {name:'tom'}
const arr = [1,2,3,4]
m.set(tom,90)
m.set(arr,1234)
m.set(true,'爱你')
//通过get来取值
console.log(m.get(tom));
console.log(m.get(arr));
console.log(m.get(true));
//遍历
m.forEach((val,key)=>{
  console.log(val,key);
})
//删除
m.delete(tom)
//查询
m.has(arr)
//清空
m.clear()
```

s
# Symbol
```javascript
// Symbol 数据类型，每一个都是独一无二的
//目前主要运用在对象的属性名，和私有成员上
const obj1 = {
  [Symbol('name')] : 'zs',
  [Symbol('name')] : 'ls'
}
console.log(obj1); //不会覆盖 { [Symbol(name)]: 'zs', [Symbol(name)]: 'ls' }

//私有成员
const name = Symbol()
const person ={
  [name] :'zs',
  sayName(){
    console.log(this[name]);
  }
}
person.sayName()

//通过Symbol.for,来复用这个变量
const person ={
  [Symbol.for('name')] :'zs',
  sayName(){
    console.log(this[Symbol.for('name')]);
  }
}
person.sayName()
```
# for of
```javascript
//for of 可遍历任何的数据类型
//遍历数据
const arr = [1,2,3]
for(const item of arr){
  console.log(item);
}
//遍历set数据结构，集合，类似数组，去重
const s = new Set([1,2,3,5,6,3,2,])
for(const item of s){
  console.log( item);
}
//遍历map数据结构，类似对象，健可以是任何值
const m = new Map()
m.set({name:'tom'},90)
m.set([1,2,3],80)
for(const item of m){
  console.log(item); //[ { name: 'tom' }, 90 ]  [ [ 1, 2, 3 ], 80 ]
}
//可用结构，得到键值
for(const [key,value] of m){
  console.log(key,value);
}
```