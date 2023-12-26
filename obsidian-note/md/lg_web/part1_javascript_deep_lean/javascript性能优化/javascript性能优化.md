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

# 引用计数算法
