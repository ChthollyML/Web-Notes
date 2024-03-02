使用原则
1. 如需要使用一个基本类型的响应式数据，必须用 ref
2. 若是层级不深的响应式对象 ref 和 reactive 都可以
3. 若是层级很深则使用 reactive

区别：
1. Ref 创建变量必须要. Value
2. Reactive 重新分配一个新对象，会失去响应式（要用 [[vue3学习/Obejct.assign]] 去整体）
3. Ref 可以重新分配新对象，还是响应式

[[vue3学习/reactive 对象类型的响应式数据]]
[[vue3学习/ref创建对象类型的响应式数据]]
[[vue3学习/ref创建 基本类型的响应式数据]]
