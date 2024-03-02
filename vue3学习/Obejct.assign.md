整体替换
```ts
car = reactive({name:'12',age:18})
object.assign(car,{name:'baoma',age:21})
```
替换后仍保留响应式属性
