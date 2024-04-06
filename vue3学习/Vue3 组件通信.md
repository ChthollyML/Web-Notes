1. Props 和 emit
父组件

``` vue 
<template>
  <div class="fa">
    <div style="margin: 10px;">我是父组件</div>
    <Son :fatherMessage="fatherMessage"></Son>
  </div>
</template>
<script setup lang="ts">
import Son from './Son.vue'
import {ref} from "vue";
const fatherMessage = ref<string>("我是父组件传过来的值")
</script>
<style scoped>
.fa{
  border: 3px solid cornflowerblue;
  width: 400px;
  text-align: center;
}
</style>
```
子组件
```vue
<template>
  <div style="margin: 10px;border: 2px solid red">
    我是子组件
    <div style="margin: 5px;border: 2px solid gold">
      父组件传值接收区：{{fatherMessage}}
    </div>
  </div>
</template>
<script setup lang="ts">
interface Props {
  fatherMessage?: string,
}
defineProps<Props>()
</script>
```

Emit 的是通过父组件传入函数，子组件回调函数传值

2. DefineExpose

子组件暴露属性和方法给父组件

3. 兄弟组件传参
通过子传父，父（中间件）再传给其他兄弟组件
爷孙组件传值也是这样
4. V-model 传值
子组件用 defineprops=modelvalue 接受
Definemit（“update:: modelvalue”）给父组件传递更改
6. Provide, inject
由父组件通过变量，子组件用 inject 接收
反之可以父组件提供函数，子组件调用函数反向传值

7. 插槽 slot
作用域插槽允许父组件向子组件传递数据，并在子组件中使用这些数据。在子组件中，可以使用特殊的 `v-slot` 指令来声明作用域插槽，并在插槽中访问父组件传递的数据。例如，以下是一个使用作用域插槽的示例：
<template>
  <div>
    <slot name="header" :message="message"></slot>
    <slot></slot>
  </div>
</template>
<template>
  <div>
    <slot name="header" :message="message"></slot>
    <slot></slot>
  </div>
</template>
<template>
  <div>
    <slot name="header" :message="message"></slot>
    <slot></slot>
  </div>
</template>
<template>
  <div>
    <slot name="header" :message="message"></slot>
    <slot></slot>
  </div>
</template>
```vue
<template> 
	<div> 
	<slot name="header" :message="message"></slot> 
	<slot></slot> 
	</div>
</template>
```

在父组件中，可以使用 `v-slot` 指令来为作用域插槽指定具体的内容。例如，以下是一个使用作用域插槽的示例：
```vue
<template>
  <div>
    <my-component>
      <template v-slot:header="{ message }">
        <h1>{{ message }}</h1>
      </template>
      <p>This is the default content</p>
    </my-component>
  </div>
</template>

```
