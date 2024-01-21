```javascript
//获取滚动条距离
function getScrollOffset(){
  // w3c规范
  if(window.scrollY){
    return {
      left : window.scrollX,
      top : window.scrollY
    }
  }else{
    //在所有浏览器中，以下存在是互斥的，只会存在一个
   return{
    left : document.body.scrollLeft + document.documentElement.scrollLeft,
    top : document.body.scrollTop + document.documentElement.scrollTop
   }
  }
}

//获取窗口高度 // 可视高度
function getViewportSize(){
  //w3c标准
  if(window.innerWidth){
    return {
      width : window.innerWidth ,
      height : window.innerHeight
    }
  }else{
    //没有<!DOCTYPE html> 向后兼容模式，浏览器怪异模式
    if(document.compatMode === 'BackCompat'){
      return {
        width : document.body.clientWidth ,
        height : document.body.clientHeight
        }
      }else{
      //有<!DOCTYPE html> 标准模式
        return {
          width : document.documentElement.clientWidth , 
          height : document.documentElement.clientHeight
        }
    }
  }
}

//获得整个文档的高度
function getScrollSize(){
  //w3c规范
  if(document.body.scrollHeight){
    return {
      width : document.body.scrollWidth , 
      height : document.body.scrollHeight
    }
  }else{
    return {
      width : document.documentElement.scrollWidth,
      height : document.documentElement.scrollHeight
    }
  }
}
//获取pageX/Y
function pagePos(e){
	var sLeft = getScrollOffset().left,
		sTop = getScrollOffset().top,
		//文档偏移量，margin
		cLeft = document.documentElement.clientLeft || 0,
		cTop = document.documentElement.clientTop || 0
	return {
		X : e.clientX + sLeft - cLeft,
		Y : e.clientY + sTop - cTop
	}
}

//封装事件监听的方法
function addEvent(el,type,fn){
  if(el.addEventListener){
    //w3c规范 IE9及以下没有
    el.addEventListener(type,fn,false)
  }else if(el.attachEvent){
    //IE9支持
    el.attachEvent('on' + type , function(){
      //attachEvent 里面的 this 指向window
      //改变 this指向
      fn.call(el)
    })
  }else{
    //最广泛的方式 element.onclick = fun
    el['on' + type] = fn
  }
}

//移除事件
function removeEvent(elem,type,fn){
	if(elem.addEventListener){
		elem.removeEventListener(type,fn,false)
	}else if(elem.attachEvent){
		elem.detachEvent('on'+type,fn)
	}else{
		elem['on' + type] = null
	}
}

//取消冒泡
function cancelBubble(e){
	var e = e || window.event
	if(e.stopPropagation){
		e.stopPropagation()
	}else{
		e.cancelBubble = true
	}
}

//取消默认行为
function preventDefaultEvent(e){
	var e = e || window.event
	if(e.preventDefault){
		event.preventDefault()
	}else{
		event.returnValue = false
	}
}
//获取元素的某个属性
function getTypes(ele, prop) {
  if (window.getComputedStyle) {
    if (prop) {
      return window.getComputedStyle(ele, null)[prop]
    } else {
      return window.getComputedStyle(ele, null)
    }
  } else {
    if (prop) {
      return ele.currentStyle[prop]
    } else {
      return ele.currentStyle
    }
  }
}

//获得节点的全部子元素
function getElemtChildrNodes(node) {
  var temp = {
    length: 0,
    push: Array.prototype.push,
    slice: Array.prototype.slice,
  }
  //缓存node.childNodes长度
  var nodeLen = node.childNodes.length,
  //得到node.childNodes
    childNodes = node.childNodes
  for (var i = 0; i < nodeLen; i++) {
    var nodeItem = childNodes[i]
    if (nodeItem.nodeType === 1) {
      // temp[temp['length']] = nodeItem
      // temp.length++
      temp.push(nodeItem)
    }
  }
  return temp
}

//获得节点父元素，n是第几层
function eleParent(node,n){
  var type = typeof(n)
  if(type === ' undefined'){
    return node.parentNode
  }else if(n <=0 || type !== 'number'){
    return undefined
  }
  while(n){
    node = node.parentNode
    n--
  }
  return node
}

//根据标签名，获得对应的父元素
function getEleParentByTagName(node,parentTag) {
    var parent = node.parentElement
    parentTag = parentTag.toUpperCase()
    while (parent) {
      if (parent.nodeName == parentTag) {
        return parent
      } else {
        parent = parent.parentElement
      }
    }
  }

//根据标签名，获取对应的子元素
 function getEleChildByTagName(node,childTag) {
    childTag = childTag.toUpperCase()
    var allChildNodes = getElemtChildrNodes(node)
    //判断是否有子节点名称，有的话返回对应子节点，没有的话返回全部节点
    if (childTag) {
      //遍历temp，找到对应子节点
      for (var i = 0; i < allChildNodes.length; i++) {
        //nodeName 返回大写
        if (allChildNodes[i].nodeName === childTag) {
          return allChildNodes[i]
        }
      }
    } else {
      return temp
    }
  }

```
