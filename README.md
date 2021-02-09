## 简答题
### 请简述 Vue 首次渲染的过程。
1. Vue首次渲染的时候会先去调用this._init(),这个方法里面会调用this.$mount()方法把模板编译成render函数
2. this.$mount在`/platforms/web/entry-runtime-with-compiler.js`中定义
   - 这个函数会调用compileToFunctions把template编译成render
   - 之后去执行`/platforms/web/runtime/index.js`里面的vm.$mount
3. vm.$mount
   - 这个函数主要是执行`mountComponent`去渲染模板
   
Vue首次渲染的时候先去调用`vm._init()`方法，然后去执行两个实例方法`vm.$mount`，这两个`$mount`在不同的地方定义，一个作用是将模板编译成render函数，一个是渲染模板
- vm.$mount
   - `/platforms/web/entry-runtime-with-compiler.js`中定义
   - 核心作用是调用了`compileToFunctions`将模板编译成render函数
- vm.$mount
   - 在`/platform/web/runtiem/index.js`中重写了`$mount`方法
   - 之后去执行`mountComponent`去渲染模板
   - `mountComponent`
     - 这函数在`core/instance/lifecycle.js`中定义
     - 首先判断是否传入了render选项，如果没传开发环境会报警告
     - 触发beforMount
     - 定义updateComponent方法
     - 创建Watcher对象，传入updateComponent方法(创建Watcher的时候，会触发一次get方法）
       1.get()方法中调用了updateComponent
       2.然后先是调用了里面作为参数的vm.render()
       3.之后调用vm._update(),将虚拟dom转成真实dom,并更新到视图上
     - 触发mounted钩子函数，返回vm
     
### 请简述 Vue 响应式原理。
### 请简述虚拟 DOM 中 Key 的作用和好处。
### 请简述 Vue 中模板编译的过程。


