源码剖析

1、vue内部与运行机制：

Vue.js 全局运行机制

响应式系统的基本原理

什么是 Virtual DOM？

如何编译template 模板？

## diff算法

* 比较只会在同层级进行, 不会跨层级比较

diff 算法的本质是找出两个对象之间的差异  
diff 算法的核心是子节点数组对比,思路是通过 首尾两端对比  
key 的作用 主要是  
决定节点是否可以复用  
建立key-index的索引,主要是替代遍历，提升性能

vdom是通过snabbdom.js库实现的,大概过程有以下三步：

* compile（把真实DOM编译成Vnode）
* diff（利用diff算法，比较oldVnode和newVnode之间有什么变化）
* patch（把这些变化用打补丁的方式更新到真实dom上去）

```js
function sameVnode(oldVnode, vnode){
  return vnode.key === oldVnode.key && vnode.sel === oldVnode.sel
}
```

* **patch**: 

```js
function patch(oldVnode, vnode) {
  if (sameVnode(oldVnode, vnode)) {
    patchVnode(oldVnode, vnode)
  } else {
    const oEl = oldVnode.el;
    let parentEle = api.parentNode(oEl);
    createEle(vnode);
    if (parentEle !== null) {
      api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl));
      api.removeChild(parentEle, oldVnode.el);
      oldVnode = null
    }
  }
  return vnode
}
```

* sameVnode

  ```js  
  function sameVnode(oldVnode, vnode){  
    return vnode.key === oldVnode.key && vnode.sel === oldVnode.sel  
  }``

* patchNode

  ```js
  patchVnode (oldVnode, vnode) {
      const el = vnode.el = oldVnode.el
      let i, oldCh = oldVnode.children, ch = vnode.children
      if (oldVnode === vnode) return
      if (oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text) {
          api.setTextContent(el, vnode.text)
      }else {
          updateEle(el, vnode, oldVnode)
          if (oldCh && ch && oldCh !== ch) {
              updateChildren(el, oldCh, ch)
          }else if (ch){
              createEle(vnode) //create el's children dom
          }else if (oldCh){
              api.removeChildren(el)
          }
      }
  }
  ```

  节点的比较有5种情况：

  1、if \(oldVnode === vnode\)，他们的引用一致，可以认为没有变化。

  2、if\(oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text\)，文本节点的比较，需要修改，则会调用Node.textContent = vnode.text。

  3、if\( oldCh && ch && oldCh !== ch \), 两个节点都有子节点，而且它们不一样，这样我们会调用updateChildren函数比较子节点，这是diff的核心，后边会讲到。

  4、else if \(ch\)，只有新的节点有子节点，调用createEle\(vnode\)，vnode.el已经引用了老的dom节点，createEle函数会在老dom节点上添加子节点。

  5、else if \(oldCh\)，新节点没有子节点，老节点有子节点，直接删除老节点。

  遍历子节点：子节点其实是一个数组，是否首尾遍历的方式判断是否相等并检查顺序，不符合的情况下，会在 hash 表中通过 key 查找，找到则复用，没找到则创建新元素

* updateChildren

  ```js
  updateChildren(parentElm, oldCh, newCh) {
  	let oldStartIdx = 0, newStartIdx = 0
  	let oldEndIdx = oldCh.length - 1
  	let oldStartVnode = oldCh[0]
  	let oldEndVnode = oldCh[oldEndIdx]
  	let newEndIdx = newCh.length - 1
  	let newStartVnode = newCh[0]
  	let newEndVnode = newCh[newEndIdx]
  	let oldKeyToIdx
  	let idxInOld
  	let elmToMove
  	let before
  	while (oldStartIdx <= oldEndIdx && newStartIdx <= newEndIdx) {
  		if (oldStartVnode == null) { //对于vnode.key的比较，会把oldVnode = null
  			oldStartVnode = oldCh[++oldStartIdx]
  		} else if (oldEndVnode == null) {
  			oldEndVnode = oldCh[--oldEndIdx]
  		} else if (newStartVnode == null) {
  			newStartVnode = newCh[++newStartIdx]
  		} else if (newEndVnode == null) {
  			newEndVnode = newCh[--newEndIdx]
  		} else if (sameVnode(oldStartVnode, newStartVnode)) {
  			patchVnode(oldStartVnode, newStartVnode)
  			oldStartVnode = oldCh[++oldStartIdx]
  			newStartVnode = newCh[++newStartIdx]
  		} else if (sameVnode(oldEndVnode, newEndVnode)) {
  			patchVnode(oldEndVnode, newEndVnode)
  			oldEndVnode = oldCh[--oldEndIdx]
  			newEndVnode = newCh[--newEndIdx]
  		} else if (sameVnode(oldStartVnode, newEndVnode)) {
  			patchVnode(oldStartVnode, newEndVnode)
  			api.insertBefore(parentElm, oldStartVnode.el, api.nextSibling(oldEndVnode.el))
  			oldStartVnode = oldCh[++oldStartIdx]
  			newEndVnode = newCh[--newEndIdx]
  		} else if (sameVnode(oldEndVnode, newStartVnode)) {
  			patchVnode(oldEndVnode, newStartVnode)
  			api.insertBefore(parentElm, oldEndVnode.el, oldStartVnode.el)
  			oldEndVnode = oldCh[--oldEndIdx]
  			newStartVnode = newCh[++newStartIdx]
  		} else {
  			// 使用key时的比较
  			if (oldKeyToIdx === undefined) {
  				oldKeyToIdx = createKeyToOldIdx(oldCh, oldStartIdx, oldEndIdx) // 有key生成index表
  			}
  			idxInOld = oldKeyToIdx[newStartVnode.key]
  			if (!idxInOld) {
  				api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
  				newStartVnode = newCh[++newStartIdx]
  			} else {
  				elmToMove = oldCh[idxInOld]
  				if (elmToMove.sel !== newStartVnode.sel) {
  					api.insertBefore(parentElm, createEle(newStartVnode).el, oldStartVnode.el)
  				} else {
  					patchVnode(elmToMove, newStartVnode)
  					oldCh[idxInOld] = null
  					api.insertBefore(parentElm, elmToMove.el, oldStartVnode.el)
  				}
  				newStartVnode = newCh[++newStartIdx]
  			}
  		}
  	}
  	if (oldStartIdx > oldEndIdx) {
  		before = newCh[newEndIdx + 1] == null ? null : newCh[newEndIdx + 1].el
  		addVnodes(parentElm, before, newCh, newStartIdx, newEndIdx)
  	} else if (newStartIdx > newEndIdx) {
  		removeVnodes(parentElm, oldCh, oldStartIdx, oldEndIdx)
  	}
  }
  ```

  首先，在新老两个VNode节点的左右头尾两侧都有一个变量标记，在遍历过程中这几个变量都会向中间靠拢。

  当oldStartIdx <= oldEndIdx或者newStartIdx <= newEndIdx时结束循环。

  索引与VNode节点的对应关系：

  oldStartIdx => oldStartVnode

  oldEndIdx => oldEndVnode

  newStartIdx => newStartVnode

  newEndIdx => newEndVnode

  在遍历中，如果存在key，并且满足sameVnode，会将该DOM节点进行复用，否则则会创建一个新的DOM节点。

  首先，oldStartVnode、oldEndVnode与newStartVnode、newEndVnode两两比较一共有2*2=4种比较方法。

  当新老VNode节点的start或者end满足sameVnode时，也就是sameVnode(oldStartVnode, newStartVnode)或者sameVnode(oldEndVnode, newEndVnode)，直接将该VNode节点进行patchVnode即可。

  如果oldStartVnode与newEndVnode满足sameVnode，即sameVnode(oldStartVnode, newEndVnode)。

  这时候说明oldStartVnode已经跑到了oldEndVnode后面去了，进行patchVnode的同时还需要将真实DOM节点移动到oldEndVnode的后面。

proxy代理？

2、vuex工作原理详解

