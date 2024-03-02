响应式：页面数据与前端数据绑定，同时改变
```ts
import {ref} form 'vue'
let age =ref(18)
```
将数据转换为实例对象 refimpl，为其赋值 value
age 不再是单纯的 int 类型了
再 html 中引用 `age` 不需要用 value 属性
但是在 js 中需要使用 value 属性来修改 `age` 的数据
```ts
function change(){
age.value++;
}
```
