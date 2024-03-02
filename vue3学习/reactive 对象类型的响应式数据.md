语法使用
```html
<h1>一辆{car.brand}车，价格{car.price}万</h1>
```

```ts
import {reactive} from 'vue'
let car = reactive({brand:'奔驰',price:100})
function change(){
	car.price +=10
}
```
Proxy 代理对象类型的数据
数组，对象都可以使用 reactive 转化为响应式
但是 reactive 不能把基本数据转化为响应式数据
Reactive 定义的响应式对象不能对对象整体修改，只能修改具体属性