# 初体验中间件

## 中间件的概念

^37a4da

中间件可以想成一个污水处理管道，从一头进入，经过处理，再从另一头出来。

## 中间件的本质

中间件本质是一个函数，带的参数有，req,res,next参数

## 中间件的定义

定义一个最简单的中间件函数

```js
const mw = function(res,req,next){
    console.log('这是一个最简单的中间件')
    //处理后，必须调用next函数
    next()
}
```

## 全局生效的中间件

使用app.use(mw)，注册中间件，就是全局生效的

也可以使用app.use(function(req,res,next){next()})这样简化的定义。


一定要在路由前面定义中间件，这样不用变量名，也会自动调用前面的中间件，调用完毕执行next()，交给后面的路由。

## 局部生效的中间件

不使用app.use()注册就是局部生效的中间件。

![image-20231218020031709](https://s2.loli.net/2023/12/18/XVcLsnzDgehYp4G.png)

## 中间件的作用

中间件共享同一个res,req，可在上游注册属性方法，下游可以通过res，req，调用

## 5个注意事项

![image-20231218020234509](https://s2.loli.net/2023/12/18/rLXmg9KOuEQPsIU.png)

# 五种中间件类型

![image-20231218031827419](https://s2.loli.net/2023/12/18/UsDXvONLytfHhol.png)

## 1.应用级别中间件

绑定带app实例上的就是应用级别中间件

## 2.路由级别中间件

![image-20231218032205743](https://s2.loli.net/2023/12/18/Hd2OWsKgkGViJnq.png)

## 3.错误级别中间件

![image-20231218033247573|500](https://s2.loli.net/2023/12/18/NrUuPjp26WD8KEV.png)

## 4.内置中间件

![image-20231218040018393|775](https://s2.loli.net/2023/12/18/y1jUZrgfblCkGcq.png)

## 5.第三方中间件

![image-20231218040107655|700](https://s2.loli.net/2023/12/18/UfbGYQEJoAd4gHu.png)

# 自定义中间件

## 1.需求描述

![image-20231218042824723](https://s2.loli.net/2023/12/18/L2dlfM6RgONZuDE.png)

## 2.定义中间件

```js
const mw = funtion(req,res,next){
}	
```

## 3.监听req的data事件

![image-20231218044833514](https://s2.loli.net/2023/12/18/DbdMZE8vz2uwL1m.png)

## 4.监听req的end事件

![image-20231218044849880](https://s2.loli.net/2023/12/18/czL9EnfCdiUg2A3.png)

## 5.使用querystring进行数据解析

![image-20231218044932575](https://s2.loli.net/2023/12/18/AKpWgRrofZVu1xi.png)

## 6.将解析出来的body对象挂载到req

![image-20231218045008450](https://s2.loli.net/2023/12/18/yARnjSMGs4OLd2P.png)

## 7.自定义中间件封装成模块

![image-20231218045034130](https://s2.loli.net/2023/12/18/dS6GuZzlFksWwfB.png)

```js
const express = require('express')

const app = express()
//node内置模块，用来处理数据
const qs = require('querystring')

// 自定义中间件
app.use((req,res,next)=>{

  // 监听req.data属性
  let str = ''
  req.on('data',(chunk)=>{
    str += chunk

  })
  //监听req.end属性
  req.on('end',()=>{
    const body =  qs.parse(str)
    req.body = body
    next()
  })
})


app.post('/user',(req,res)=>{
  console.log(req.body);
  res.send('ok')
})

app.listen(80,()=>{
  console.log('server running at http://127.0.0.1:80');
})
```

# 使用express写接口

## 主代码 

```js
const express = require('express')
//导入route中间件
const apiRouter = require("./13.apiRouter.js")

const app = express()
//将字符串处理成对象，并挂载到req.body
app.use(express.urlencoded({extended:false}))
//将路由中间件注册成全局中间件，并且在其对应规则路径上加上/api
app.use('/api',apiRouter)

app.listen(80,()=>{
  console.log('server running at 127.0.0.1:80');
})
```

## apirouter中间件代码

```js
const express = require('express')

const apiRouter = express.Router()


apiRouter.get('/get',(req,res)=>{
  res.send({
    status:0,
    message:'请求成功',
    data:req.query
  })
})

apiRouter.post("/post",(req,res)=>{

  res.send({
    status:0,
    msg:'请求成功',
    data:req.body
  })
})

module.exports = apiRouter
```

# 接口跨域问题

## 使用cors解决跨域问题

![image-20231218072058627](https://s2.loli.net/2023/12/18/cZIzxMGCQuEBPlJ.png)

![image-20231218072114438](https://s2.loli.net/2023/12/18/KFkyoYCErzsVJZ2.png)

### Access-Control-Allow-Origin

![image-20231218073953275|650](https://s2.loli.net/2023/12/18/5HlWGDRYna7AJyZ.png)

### **Access-Control-Allow-Header**

![image-20231218074018430|600](https://s2.loli.net/2023/12/18/Mk7HjuEA9cpKbCt.png)

### Access-Control-Allow-Methods

![image-20231218074054388](https://s2.loli.net/2023/12/18/s1tZ5Qbkzirx6Bc.png)

### 请求分为两类

#### 简单请求

![image-20231218074145711](https://s2.loli.net/2023/12/18/e3SyR64nFPCBraZ.png)

#### 预检请求

![image-20231218074154925](https://s2.loli.net/2023/12/18/UKE3wl6a25MBWNP.png)

## jsonp

### 什么是jsonp

![image-20231218075140910](https://s2.loli.net/2023/12/18/xZAuwMnRsSWKpHN.png)

### 创建jsonp

![image-20231218074957417](https://s2.loli.net/2023/12/18/ZXbD3lPEzBgt9p7.png)

### 实现步骤

![image-20231218075226584](https://s2.loli.net/2023/12/18/8jA4slYNPrKimoE.png)

### 具体代码

![image-20231218075410146](https://s2.loli.net/2023/12/18/9c5zURQB67lwW2a.png)

![image-20231218075418767|725](https://s2.loli.net/2023/12/18/xLskf76yG4VbOXW.png)
