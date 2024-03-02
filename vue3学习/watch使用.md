作用：监视数据变化
特点：Vue 3 的 watch 只能监视以下四种数据变化
+ ref 定义的数据
+ reactive 定义的数据
+ 函数返回的值
+ 一个包含上述内容的数组

Ref：
```ts
watch(name,(newValue,oldValue)=>{
	console.log('name变化了')
})
```
深度监视 `depp`，能监视到对象的属性变化，但是引用的还是同一个对象（oldValue == newValue）
立即检索 `immediate` ，初始化时就先执行一次监视
```ts
watch(name,(...),{deep:true})
```

Reactive :
对 reactive 定义的对象监听，自动开启深度监视