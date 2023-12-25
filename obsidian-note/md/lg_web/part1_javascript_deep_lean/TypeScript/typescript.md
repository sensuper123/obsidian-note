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