页面传回值需要双向绑定 `v-model` 
Computed：
+ 当计算的数据发生改变时，会重新计算
+ 有缓存，会减少使用次数

```ts
//只读计算属性
let fullname = computed(()=>{
	retrun 'fullname'
})
```

```ts
//可读可写计算属性
let fullname = computed({
	get(){
	retrun 'fullname'
	},
	set(val){
		//val为更改的值'1231'
		//每次fullname被赋值时改变时会调用set()方法，但是没有直接更改fullname的值
	}
})

fullname.value = '1231'
```
