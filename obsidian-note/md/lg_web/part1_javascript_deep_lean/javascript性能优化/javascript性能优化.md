# 内存管理
1. 申请内存
2. 使用内存
3. 回收内存
```javascript
//申请内存
let obj = {}
//使用内存
obj.name = 'zs'
//回收内存
obj = null
//内存是一块可读写的单元组成的空间
```
# JavaScript垃圾回收
1. JavaScript垃圾回收是自动的
2. 一个对象没有被引用就是垃圾对象
3. 从根上不可达的对象也是垃圾对象
```javascript
const obj1 = {name:'obj1'}
let a = obj1 //a 引用了obj1指向的对象
obj1 = null //把obj1变量删除，obj1的对象还不是垃圾对象，因为还被a收引用

根是obj，obj引用堆里的一个对象，这个对象里面包含o1,o2各引用堆里的obj1,obj2对象，obj1的next属性引用obj2，obj2的pre属性引用obj1
当一个对象从根出发，所有能引用到它的线路都断了的时候，这个对象就变成了垃圾对象
function creatObj(obj1,obj2){
  obj1.next = obj2
  obj2.pre = obj1
  return {o1:obj1,o2:obj2}
}
let obj =  creatObj({name:'obj1'},{name:'obj2'})
console.log(obj);
delete obj.o1 
delete obj.o2.pre //obj1就变成垃圾了


```
# GC算法
1. GC是一种机制，垃圾回收器完成具体的工作
2. 工作的内容就是查找垃圾，释放空间，回收空间
3. 算法就是工作时查找和回收所遵循的规则

# 引用计数算法优缺点
1. 优点
	1. 发现垃圾立即回收
	2. 最大限度减少程序暂停
2. 缺点
	1. 无法回收循环引用的对象
	2. 时间开销大 --修改变量的计算器需要时间，如果变量大，就会耗时间

# 引用计数的理解
```javascript
全局变量下：
	全局变量的所有属性的原始计算器都不为0，因为全局对象的属性都有对应的引用
	如 a = 10 ,在全局对象下，编译过程时，a 会转变为内存地址，其内存的是10，从而找到 10 这个数值
	又如 obj = {age:10} ,在编译过程时，obj会变成内存地址，且内部存储的是{age:10}的这个地址值，{age:10 }这个对象被引用了
	综上，10和{age:10}这两个值都被引用，故不是垃圾对象
-----------------------------------------------------------------
案例分析：
	const obj1 = {age:10}
	const obj2 = {age:20}
	const obj3 = {age:30}
	const arr = [obj1.age,obj2.age,obj3.age]
	首先，全局对象下，会有三个obj属性，期内存储对应的地址值，相当于这三个对象都被引用了一次
	其次，arr的前三个元素，存储了三个对象的age属性，其本质应当是，找到obj1所存储的地址值，在指向{age:10}对象，再把age的值10复制到arr[0]上，所以，本质并没有引用obj1，obj1的计算器还是1.其他同理
-----------------------------------------------------------------
案例分析：
	const obj = { obj1 :{age:10} }
	const arr = [obj.obj1]
	obj = null
	全局下，栈内有obj，存储{obj1:{age:10}}这个地址值
	{obj1:{age:10}}里面的obj1属性，又存储了{age:10}这个地址值，这个obj1属性其实存储的位置还是在栈内存里
	arr[obj.obj1]存储的是{age:10}这个地址
	当执行obj = null 时，obj这个对象将没有任何别的变量引用他，obj.obj1引用的是{age:10}这个对象，所以obj将变成垃圾对象
	在垃圾回收器没工作时，{age:10}这个对象应该是被 obj1 和 arr[0] 两处指向的
	当垃圾回收器工作时，{age:10}就只被引用一次了

```
# 标记清除算法
1. 分两个阶段完成
2. 遍历根上所有可达对象，并标记
3. 遍历所有对象，清除没有标记的对象
4. 回收相应的空间

# 标记清除算法优缺点
1. 优点
	1. 能解决引用计数循环引用不能回收的问题
2. 缺点
	1. 空间碎片化，回收的空间是有大小的，如下图，左右不可达，且大小分别为2，1，回收的空间地址不连续，如果我们需要申请一个1.5大小的空间，那么左边的话就大了，造成浪费，右边的话就小了，不够使用
	2. ![](https://s2.loli.net/2023/12/28/iG3YEAQjb6r7skZ.png)


# 标记整理算法原理
1. 标记阶段的操作与标记清除一致，都是从根出发遍历可达对象并标记
2. 清除阶段会先整理，移动对象位置
3. 优点是回收空间地址连续，可以更好利用空间

# V8垃圾回收策略
1. 采用分代回收的思想
2. 分为新生代、老生代
3. 针对不同代，采用不同的垃圾回收策略

# V8内存分配
1. V8内存一分为二
2. 小空间用于存储新生代对象（32M | 16M）
3. 新生代指的是存活时间较短的对象
# 新生代对象回收实现
1. 回收过程采用复制算法 + 标记整理算法
2. 新生代内存分为两个等大小空间
3. 使用空间为From，空闲空间为To
4. 活动对象存储于From空间
5. 标记整理后将活动对象拷贝到To
6. From 与 To交换空间完成释放
# 新生代回收细节说明
1. 拷贝过程中可能出现晋升
2. 晋升就是将新生代对象转移到老生代
3. 一轮GC还存活的新生代需要晋升
4. To空间使用率超过25%也需要晋升

# 老年代对象说明
1. 老年代对象存放在右侧老生代区域
2. 64位操作系统1.4G，32位操作系统700M
3. 老年代对象指的是存活时间较长的对象
# 老年代对象回收实现
1. 主要采用标记清除、标记整理、增量标记算法
2. 首先使用标记清除完成垃圾空间的回收
3. 采用标记整理进行空间优化 
	1. 当新生代要晋升老生代时，且老生代剩余空间不足以给新生代存储时，会运用标记整理优化空间，把之前的碎片化空间，组合成连续的空闲空间
4. 采用增量标记进行效率优化
	1. 增量标记主要是为了减少程序卡顿，例如老生代开始垃圾回收时，需要遍历可达对象，这时候如果全部遍历完成，会给程序造成较长卡顿现象，所以使用增量标记，让垃圾回收遍历一层对象后重新执行程序，之后再继续往下遍历更深层的对象且标记
# 新老生代垃圾回收细节对比
1.  新生代区域垃圾回收使用空间换时间
	1. 因为新生代空间分为两个等大的空间，From空间用来存储所有对象，To空间等Form空间遍历可达对象且标记后，将标记的对象全部复制到To空间
2. 老生代区域回收不适合复制算法
	1. 因为复制算法的话要一分为二，老生代空间如果一分为二，代表有几百兆的空间被浪费，而新生代被浪费的也就几十兆

# 内存问题的外在表现和评判标准
1. 页面出现延迟加载或经常性暂停  （频繁垃圾回收）
	1. 通过内存变化图进行分析
	2. ![image.png](https://s2.loli.net/2023/12/28/2O9zEAWo4SYdZyx.png)

2. 页面持续性出现糟糕的性能  （内存膨胀）
	1. 在多数设备上都存在性能问题
3. 页面的性能随时间延长越来越差 （内存泄露）
	1. 内存使用持续升高，走势图一直升高
# 监视内存的方式
1. 任务管理器监控内存 shift + esc
2. Performance - Timeline
3. 堆快照查找分离dom，分离dom界面不体现，但内存确实存在

# V8引擎工作流程
1. parser解析器
	1. 词法分析
		1. V8引擎将源代码分解成一系列的词元（tokens）。
	2. 语法分析
		1. parser语法分析直接生成AST
			1. 立即执行的函数直接编译生成AST
		2. pre-parser(预编译)只变量提升
			1. 如果不是立即执行的函数，就会进行预编译，等到函数调用时，才进行parse生成AST
2. ignition解释器（预编译）
	1. 将AST翻译成字节码
3. TurboFan编译器
	1. 将字节码转成机器码
# 代码执行栈示意图
1. 刚开始栈示意图
	1. 浏览器会在内存中申请内存空间，堆栈全局对象都在内存中
	2. 代码开始执行时，会往栈内压入一个全局执行上下文，且有VO全局变量对象，用来存储全局变量，浏览器会自动在VO挂载window属性，指向GO全局对象
	3. ![](https://s2.loli.net/2023/12/29/QJNLhv4ZbnPXe1p.png)
	4. 引用类型堆栈处理
		1. ![image.png](https://s2.loli.net/2023/12/29/7ywuxjVdCZI4TiX.png)
2. 函数类型堆栈处理
	1. 如果代码内有函数，在变量声明的时候，首先会在堆内存创建一个对象，然后在VO(G)声明一个变量如foo ，它的值指向刚刚创建的对象 foo = 0x000,这个对象内部存储着函数代码体
	2. 当执行到函数时，会进行以下步骤
		1. 会形成一个全新私有上下文，内部有个AO对象，用于存放函数内的变量
		2. 确定作用域链
		3. 确定this
		4. 初始化arguments
		5. 形参赋值：它就相当于是变量声明，然后将声明的变量放置于AO
		6. 变量提升
		7. 代码执行
# 对于VO,GO,AO及它们存在位置的问题
统一理解coderwhy的流程内容
![image.png|750](https://s2.loli.net/2024/01/02/8okA6WfhwpZDBQO.png)


# 变量私有化
```javascript
/*
对于能存储在局部作用域的变量尽量存储在局部作用域
这么做的目的是为了减少变量查找的层级，从而使代码运行的更快
*/
var i ,str = ''
function makeStr(){
  for(i = 0 ; i < 1000; i++){
  str += i
  }
}

function makeStr1(){
  let str = ''
  for(var i = 0;i< 1000; i++){
    str += i
  }
}

//前者需要从本地makeStr的AO找i，str,找不到再去全局作用域找
//后者只需要从本地makeStr的AO找i,str


```
# 变量缓存
1. 当一个变量在一个作用域内需要多次使用，且这个变量在其他的作用域内，或者说在其他的堆地址内，就可以复制一份，缓存在本地作用域内
2. 这样子做其实本质也是减少了变量查找的层级，加快代码运行效率
3. 缺点就是在本地AO多存储了一个变量，用空间换时间，浪费了些空间

# 防抖和节流
1. 防抖是只多次点击无效，只进行第一次后最后一次点击操作
2. 节流是可以自行设置事件间隔，例如每隔一秒钟就能进行一次事件操作
3. 应用场景
	1. 滚动事件
	2. 输入的模糊匹配
	3. 轮播图切换
	4. 点击操作

# 防抖实现
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <button id="btn">点击</button>
  </body>
  <script>
    let btn = document.getElementById('btn')
    function myDebounce(handle, wait, immediate) {
      // 参数判断
      if (typeof handle !== 'function')
        return new Error('handle is must be function')
      if (typeof wait === 'undefined') {
        wait = 300
      } else if (typeof wait === 'boolean') {
        immediate = wait
        wait = 300
      } else if (typeof wait !== 'number') {
        return new Error('wait must be a number')
      }
      if (typeof immediate !== 'boolean') immediate = false

      
      let timer = null
      return function proxy(...args) {
        let self = this
		//实现在一段时间内，只执行第一次
        if (immediate && !timer) {
          handle.call(self, ...args)
          timer = setTimeout(() => {
            timer = null
          }, wait)
          //实现在一段时间内，只执行最后一次
        } else if (!immediate) {
          clearTimeout(timer)
          timer = setTimeout(() => {
            handle.call(self, ...args)
          }, wait)
        }
      }
    }
    function btnClick(ev) {
      console.log(ev)
      console.log('点击了btn')
    }
    btn.onclick = myDebounce(btnClick, 3000, true)
  </script>
</html>

```