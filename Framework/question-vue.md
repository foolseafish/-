 # Vue
- MVVM？
A: MVMM与MVC的区别就在于双向绑定，减少了Controller的工作量，原理就是响应式。原本响应式会频繁操作DOM的，得力于虚拟DOM的出现操作次数和性能有很大的提升。
- Vue生命周期？
- Vue响应式原理（发布订阅原理）？
A：发布订阅依赖收集，利用defineProperty。get收集副作用依赖，set通知更新依赖。
- Vue性能优化？首屏优化？
A：beforeDestory 清楚绑定监听，大对象别放在data里，计算类数据放computed（缓存）。频繁渲染v-show替代v-if。大型非比用组件使用异步组件减少打包成果物和加载文件大小。
- Vue-Router解决了什么问题？
1. 嵌套路由
2. 模块化基于组件的路由系统
3. 可以传参
4. 导航控制
- Vue-Router的工作机制（原理）？
1. hash
2. history
- Vuex解决了什么问题？
1. 统一的状态流管理
2. 数据可追踪
- Vuex的工作机制？
1. state, getter, mutation, action
- Vue3Composition API思路及有哪个好的改变?
优点：
>1. **解决了原先mixin模式下对外开放接口与内部方法混合，调用丑陋**。
    setup接口统一放外部接口，API清晰。
>2. **解决了原先mixin模式下mixin与组件间的命名冲突**。
    这个问题很致命，组件可能有与mixin里面私有变量或者私有函数相同的名字。
>3. **解决了事件销毁需要单独写到beforeDestoryed生命周期**。
    生命周期钩子回调注册，监听和销毁可以写一起方便维护。
>4. **与reactive模块配合可以减少Vue对象上挂载很多不必要的方法和属性**。
    因为代码都写在data,method，或者生命周期等里面，为了不丢失上下文属性和方法需要挂载到vue对象上。

- Vue3的优化点？
1. 引入Composition API,优点如上
2. 提出隐含的响应式内部逻辑作为一个独立包，并优化原有的defineProperty属性拦截，改用proxy。
3. TreeShaking减少打包体积，非基础功能拆分，只留基准体积。体积相比减少了50%;
4. 将VNode做静态节点分析，只监听节点变更。将静态节点，静态子树和数据对象独立于渲染函数，不重新创建。CPU时间优化90%


- computed实现？

- 数据变更怎么触发渲染？