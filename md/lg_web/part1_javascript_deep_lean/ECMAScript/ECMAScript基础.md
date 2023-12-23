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