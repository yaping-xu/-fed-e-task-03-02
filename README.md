## 简答题
### 请简述 Vue 首次渲染的过程。
1. Vue初始化，实例成员，静态成员
2. new Vue()
3. this._init()
4. vm.$mount
	-  这是``/platforms/web/entry-runtime-with-compiler.js``的$mount, 这个函数的核心作用是帮我们把模板编译成`render`函数
	- 如果没有传递`render`，把模板编译成`render`函数
	- `compileToFunction()`生成`render()`渲染函数
	- `options.render = render`
5. vm.$mount
	- 这个是`/platforms/web/runtime/ind ex.js`中的$mount
	- 调用`mountComponent()`
6. mountComponent(this,el)
   - `/core/instance/lifecycle.js`中定义
   -  判断是否有`render`选项, 如果没有但是传入了模板，并且当前是开发环境会发出警告
   - 触发`beforeMount`
   - 定义 `updateComponent`
   		- `vm._update(vm.render(), ... )`
   		- `vm.render()`渲染，渲染虚拟`DOM`
   		- ` vm._update()`更新，将虚拟`DOM`转换成真实`DOM `
   - 创建`Watcher`实例
   		- 传入了`updateComponent方法`
   		- 调用`get()`方法
   - 触发`mounted`
   - `return vm`
7. watcher.get()
  	- 创建完`watcher`会调用一次`get`
  	- 调用`updateComponent()`
  	- 调用 `vm.render()`创建VNode
  	- 调用 `vm._update(vnode, ...)` 

### 请简述 Vue 响应式原理。
### 请简述虚拟 DOM 中 Key 的作用和好处。
### 请简述 Vue 中模板编译的过程。


