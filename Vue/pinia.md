# 一般使用

### 引入

```js
import { createPinia } from 'pinia'

const pinia = createPinia()

//先使用在挂载
app.use(pinia)

app.mount("#app")
```





### 定义（options）

```js
import { defineStore } from "pinia";
export const userUserStore = defineStore('ma', {
    state: () => {
        return {
            uname: "ma"
        }
    },
    // 计算属性
    getters: {
        getUname(state) {
            return state.uname + 'jia'
        }
    },
    // 方法（this访问）
    actions: {
        changeUname() {
            this.uname = 'Ma'
        }
    }
})
```



### 定义（setup函数）

```js
export const userAgeStore = defineStore('main', () => {
    const age = ref(20)
    // 计算属性，需要引入
    const gettersAge = computed(() => {
        return age.value + 3
    })
    // 方法
    function addAge() {
        age.value++;
    }
    return { age, gettersAge, addAge }
})
```





- `ref()` 就是 `state` 属性
- `computed()` 就是 `getters`
- `function()` 就是 `actions`



### 使用

```js
<script setup>
import { userUserStore, userAgeStore } from "../stores/index";
const user = userUserStore();
const age = userAgeStore();
console.log(user);
console.log(age);
//store数据可直接修改，不用commit
//批量修改：实例名.$patch({attribute1：value1，attribute2：value2，......})
//批量修改$patch(函数)（强烈推荐）：实例名.$patch((state)=>{state.value1,state.value2,......})
</script>

<template>
  <main>
    <h2>{{ user.uname }}</h2>
    <h2>{{ user.getUname }}</h2>
    <button @click="user.changeUname">改变</button>
  </main>
</template>
```





### $reset()

恢复store里的值为默认值





### $subscribe()

store里值变化就会触发这个

```ts
$subscribe((args,state)=>{

})
```



### $onAction()

调用action就会触发

```ts
$onAction((arg)=>{

})
```

