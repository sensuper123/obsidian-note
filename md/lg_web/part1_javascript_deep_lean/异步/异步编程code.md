# promise 基本用法
```JavaScript
function ajax(url){
  return new Promise((resolve,reject)=>{
    const xhr = new XMLHttpRequest()
    xhr.open('get',url)
    xhr.responseType = 'json'
    xhr.onload = function(){
      if(xhr.status === 200){
        resolve(this.response)
      }else{
        reject(new Error(this.statusText))
      }
    }
    xhr.send()
  })
}
ajax('/api/users.json').then((value)=>{
  console.log(value);
},(err)=>{
  console.log(err);
})
```
# promise all 和 race使用
```javascript
ajax('/api/urls.json')
  .then((value)=>{
    let urlArr =  Object.values(value)
    return Promise.all(urlArr.map((url)=>ajax(url)))
  })
  .then((value)=>{
    console.log(value);
  })
  .catch((err)=>{
    console.log(err);
  })
 //race
   const timeout = new Promise((resolve,reject)=>{
    setTimeout(()=>{
      reject(new Error('timer'))
    },1000)
  })
  const users =  ajax('/api/users.json')
  Promise.race([timeout,users])
    .then((value)=>{
      console.log(value);
    })
    .catch((err)=>{
      console.log(err);
    })
```
# generator-promise
```javascript
function ajax(url){
  return new Promise((resolve,reject)=>{
    const xhr =new XMLHttpRequest()
    xhr.responseType = 'json'
    xhr.open('get',url)
    xhr.onload= ()=>{
      if(xhr.status === 200){
        resolve(xhr.response)
      }else{
        reject(new Error(xhr.statusText))
      }
    }
    xhr.send()
  })
}
function * main(){
  try {
    let data =  yield ajax('/api/users.json')
    console.log(data);
    let data1 = yield ajax('/api/posts.json')
    console.log(data1);
    let data2 = yield ajax('/api/urls.json')
    console.log(data2);
  } catch (e) {
    console.log(e);
  }
}
// p.value.then((data)=>{
//   if(p.done) return
//   const p1 = g.next(data)
//   console.log(p1);
//   p1.value.then((data)=>{
//     if(p.done) return
//     const p2 = g.next(data)
//     console.log(p2);
//     p2.value.then((data)=>{
//       if(p.done) return
//       const p3 = g.next(data)
//       console.log(p3);
//     })
//   })
// })
function co(gennerator){
  const g = gennerator()
  function handleResult(result){
    if(result.done) return
    result.value.then((value)=>{
      handleResult(g.next(value))
    },(err)=>{
      g.throw(err)
    })
  }
  handleResult(g.next())
}
co(main)
```