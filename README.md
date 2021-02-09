## 简答题
### 请简述 Vue 首次渲染的过程。
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
       1. get()方法中调用了updateComponent
       2. 然后先是调用了里面作为参数的vm.render()
       3. 之后调用vm._update(),将虚拟dom转成真实dom,并更新到视图上
     - 触发mounted钩子函数，返回vm
     
### 请简述 Vue 响应式原理。
在`this._init()`方法里，会调用`initState()`在这个方法中去初始化实例状态,处理响应式的方法`observer()`在此函数中定义
- observer()
  - 在`core/observer/index.js`中定义，核心功能是为数据添加响应式处理
  - 方法首先会判断传入value是否为对象或者VNode,如果不是直接返回
  - 然后去判断value.ob是否为Observer实例，并且value.__ob__ 是否为响应式对象，如果是 返回实例
  - 最后`new Observer(value)`创建Observer对象并返回
    - Observer类中首先为value的__ob__属性设置为当前实例
    - 判断value是数组还是对象，分别对数组和对象进行响应式处理
    - 数组：为数组方法进行修补，改变当前数组对象的原型属性，
    - 对象：执行`this.walk`遍历对象的每个属性，调用`defineReative(obj,key[i])`方法, 设置响应式数据
      1. defineReative里面主要的是重写了对象的get/set方法，为get/set增加了依赖收集的方法
      2. 在get中调用dep.depend()进行收集依赖
      3. 在set中调用dep.notify()进行触发更新
  
### 请简述虚拟 DOM 中 Key 的作用和好处。
在diff算法的时候会对比子节点的新旧VNode是否为sameVnode,sameVNode中会对比key和tag标签
- 如果不设置key,在diff算法的时候，中比较子节点的时候，增加dom操作
- 如果设置key的话，在对比新旧节点的key相同会被认为是sameVnode,所以只做比较，更新的时候只会做变化部分的dom操作
### 请简述 Vue 中模板编译的过程。
模板编译就是把模板转成渲染函数的过程，也就是template到render的过程。模板编译的通过调用`compileToFunctions`函数完成
- compileToFunctions
  - 读取缓存中的 CompiledFunctionResult对象，如果有直接返回
  - 调用compile函数编译模板
    - compile函数主要的作用是合并options选项，调用baseCompile进行编译
    - baseCompile是模板编译的核心函数，src/compiler/index.js中定义，主要做了三件事
      1. 把模板编译成ast 抽象语法树
      2. 优化语法树
      3. 把抽象语法树生成字符串形式的 js 代码，并进行返回
  - 拿到返回的对象后，调用`createFunction`把字符串形式的js代码转成js方法
  - 缓存并返回res对象(render,staticRenderFns方法)



