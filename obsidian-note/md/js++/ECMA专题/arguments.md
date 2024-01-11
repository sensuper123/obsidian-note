# arguments
1. 形式参不共享的情况，这种情况形参和实参列表割裂开来，更改任何一方，都不会影响到另一方
	1. 形参列表最少有一个默认值
	2. 解构
	3. 剩余参数
	4. 严格模式
2. 将类数组转成数组的方法
	1. arguments有迭代器，可通过遍历push到新数组
	2. [].slice.call(arguments)，slice如果不穿参，会将整个数组截取，然后返回一个新数组，call改变this指向，因为arguments没有slice方法，所以只能通过这种方式改变this指向调用[]里的方法。不推荐这种方法，因为arguments是内置的函数局部变量，不推荐使用方法修改和暴露arguments，这会影响到V8引擎对代码的优化
	3. Array.from(arguments) 这个应该是最常见的
# parseInt
```javascript
/**
 * parseInt(string, redis)
 * parseInt接收一个字符串，第二个参数是把字符串看成几进制转成十进制
 * 如果第一个参数不是字符串，会进行类型转换成字符串
 * 返回number类型或者NaN
 * 返回NaN的情况：
 *              开头不是数字的字符串 'a1'
 *              redis 的范围超过 2 - 31
 *              当超出设置的redix时，例如'123' , 2 ,将字符串看成二进制，但二进制
 *              有效的只有01，所以会阶段，2，3，无效就剔除
*/
console.log(parseInt('123',2)); //1
console.log(parseInt('213',2));//NaN
console.log(parseInt(true,2));//NaN
// 5 * Math.pow(16,4) + 2 * Math.pow(16,3) + 10 * Math.pow(16,2) + 3 * Math.pow(16,1)+ 11 * Math.pow(16,0)
console.log(parseInt('0x52A3B')); //338491
console.log(5 * Math.pow(16,4) + 2 * Math.pow(16,3) + 10 * Math.pow(16,2) + 3 * Math.pow(16,1)+ 11 * Math.pow(16,0));//338491
```
# module
```javascript
----------------html----------------------
  <body>
    <div class="wrap">
      <div class="res">0</div>
      <input type="number" name="" id="num1" />
      <input type="number" name="" id="num2" />
      <div class="btns">
        <button>+</button>
        <button>-</button>
        <button>*</button>
        <button>/</button>
      </div>
    </div>
    <script type="module" src="./src/app.js"></script>
  </body>
-----------------app.js------------------------
import {add,mins,mul,div} from "./computer" 
;(function(){
  const num1Input = document.querySelector('#num1')
  const num2Input = document.querySelector('#num2')
  const btns = document.querySelector('.btns')
  const res = document.querySelector('.res')
  btns.addEventListener('click',handle)
  function handle(e){
      let type = ''
      if(e.target.nodeName === 'BUTTON'){
        type = e.target.innerHTML
      }else{
        return
      }
      res.innerHTML = comuputer(type)
     
  }
  function comuputer(type){
    let num1 = Number(num1Input.value)
    let num2 = Number(num2Input.value)
    switch(type){
      case '+' :
        return add(num1,num2)
      case '-' :
        return  mins(num1,num2)
      case '*' :
        return mul(num1,num2)
      case '/' :
        return div(num1,num2)
      default :
        break
    }
  }
})()

-------------------------computer.js-------------
export function add(a , b){
  return a + b
}
export function mins(a , b){
  return a - b
}
export function mul(a , b){
  return a * b
}
export function div(a , b){
  return a / b
}

/*
总结: export 和 export default 的区别
export 导出的是一个集合
export default 导出的是一个数据类型，例如对象
export 对应引入可用{} 从集合中拿出来
export default 应入需先把对象拿出来，再进行解构
*/
```