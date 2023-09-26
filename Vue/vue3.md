# 响应式



ref reactive

### ref

支持所有类型

返回是一个类，使用需要.value

内部已经调用了triggerRef

![image-20230307100603100](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20230307100603100.png)



### shallowRef	

浅层次，到value是个对象

不能和ref共用



### triggerRef

将浅层次的强制联系



### customRef

自定义ref





### reactive

只支持引用类型：Array、Object、Map、Set

不需要加.value

> 不能直接赋值，reactive proxy，赋值会被覆盖，破坏响应是对象

解决方法：

- 数组 可以使用push 加解构
- 添加一个对象，把数组当一个属性去解决



### shallowreactive

浅层响应



  # 响应式 to

### toref（{},attr）

只能修改响应式对象的值，非响应式视图毫无变化

因为内部没有trackRef和triggerRef实现收集和触发依赖

**对象值单独拿出来就变成非相应式了**

```ts
const li = reactive({name:"mmm",age:12})

let age = toRef(li,"age")

```

 

### toRefs({})

把{}内的所有属性转换成响应式

适用于解构赋值



### toRaw

转换成原始对象，不在响应式







# 响应式原理

本质上是将执行命令函数effect保存记录下来，在数值改变时调用执行

响应：track和trigger作用

### effect函数

用于执行命令

```ts
let activeEffect;
export const effect = (fn:Function) => {
     const _effect = function () {
        activeEffect = _effect;
        fn()
     }
     _effect()
}
```



### track

每次读取时，加入当前的执行命令

```ts
const targetMap = new WeakMap()
export const track = (target,key) =>{
   let depsMap = targetMap.get(target)
   if(!depsMap){
       depsMap = new Map()
       targetMap.set(target,depsMap)
   }
   let deps = depsMap.get(key)
   if(!deps){
      deps = new Set()
      depsMap.set(key,deps)
   }
 	//加入命令effect
   deps.add(activeEffect)
}
```



### trigger

每次写入数据触发，调用执行函数

```ts
export const trigger = (target,key) => {
  const depsMap = targetMap.get(target)
  const deps = depsMap.get(key)
  //触发effect
  deps.forEach(effect=>effect())
}
```



![img](https://img-blog.csdnimg.cn/a2355ce6b8eb489d98fceaf8d2ced235.png)



### reactive实现

```ts
export const reactive = <T extends object>(target:T) => {
    return new Proxy(target,{
        get (target,key,receiver) {
          const res  = Reflect.get(target,key,receiver) as object
          //捕获依赖，记录命令
          track(target,key)
 
          return res
        },
        set (target,key,value,receiver) {
           const res = Reflect.set(target,key,value,receiver)
           //触发依赖，执行命令
           trigger(target,key)
 
           return res
        }
    })
}
```



### 使用

```ts
const user = reactive({
            name: "小满",
            age: 18
        })
//存下的依赖
effect(() => {
	document.querySelector('#app').innerText = `${user.name} - ${user.age}`
})
```







# watch

```ts
watch(attr,()=>{
	
},{
	flush:"pre"			//组件更新前调用
		  "sync"		//同步执行
		  "post"		//组件更新后执行
})
```



### watchEffect

立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。





# 生命周期

beforeCreate created   在setup语法糖模式是没有这两个生命周期的，用setup去代替

​	创建钩子

- onBeforeMount
- onMounted

​	更新钩子

- onBeforeUpdate
- onUpdated

​	销毁

- onBeforeUnmounted

- onUnmounted

  

# 组件

###  父传子

defineProps<{

​		title:string

}>()

##### 默认值

ts特有

```ts
withDefaults(defineProps<{

		title:string

}>(),{
    title:()=>"jsjdi"
})
```



### 子传父

```ts
const emit = defineEmits(['on'])
emit('on',params)
```

##### TS特有

```ts
const emit = defineEmits<{
	(e:'on',params:any):void
}>()
```



### 暴露接收

全局，可控制

父

defineExpose({

})

子

ref<InstanceType<typeof ...>>()



### 局部组件

父组件引入即可



### 全局组件

main.ts中引入

app.component(name，address)



### 递归组件

自动排布

后台管理左侧菜单栏部分

```ts
<template>
    <div>
        <div :key="i" v-for="item, i in data">
            <input type="checkbox" :checked="item.check">{{ item.name }}
            /////////////////////////////////////////////////////////////
            <Inter v-if="item?.child?.length" :data="item.child"></Inter>
        </div>
    </div>
</template>

<script setup lang="ts">
interface Tree {
    name: string
    check: boolean
    child?: Tree[]
}
const datas = defineProps<{
    data: Tree[]
}>()

</script>
```





### 动态组件

可实现tab页

<component :is="comid"></component>

const comid = ref(component name)

>  shallowRef, markRaw 





### 插槽

v-slot -------- #

v-slot="{}" -------- #default



##### 动态

父组件上

<template  #[name]> </template>





### 异步组件

骨架屏

引入需要使用

```ts
defineAsyncComponent(()=> import(''))
```

使用需要

```html
   <Suspense>
        <template #default>

        </template>
        <template #fallback>

        </template>
    </Suspense>
```



##### 异步引入 import()

实现主包拆解，当使用到组件是才会调用，提高性能





### 传送组件

```html
<Teleport to="body" disabled="">

</Teleport>
```





### 缓存组件

缓存组件内的值

```html
<keep-alive :include=['',''] :exclude=['',''] :max=''(最大缓存数)>
        <!-- 只能有一个子节点可以用v-if -->
</keep-alive>
```

新增两个生命周期：onActivated、onDeactivated





### 动画组件

```html
<transition name="fade">

</transition>

<style>
/* 出现 */
.fade-enter-from{

}
.fade-enter-active{

}
.fade-enter-to{
  
}
/* 消失 */
.fade-leave-from{

}
.fade-leave-active{
  
}
.fade-leave-to{
  
}


</style>



```

##### 动画钩子



##### appear属性

一进入页面就显示效果



##### 动画库

官方文档 [Animate.css | A cross-browser library of CSS animations.](https://animate.style/)

使用加前缀：animate__animated



gsap

```
npm install gsap -S
```

```
import gsap from 'gsap'
```



##### 渲染列表

注意要指定key

```html
<transition-group>
     <div style="margin: 10px;" :key="item" v-for="item in list">{{ item }</div>
</transition-group>
```



##### 移动

move-class

###### Lodash库



##### 随机打乱案例

```ts
<template>
    <div>
        <button @click="rad">random</button>
    </div>
    <div class="ran">
        <transition-group move-class="mm">
            <div v-for="item in items" :key="item.id" class="one">
                {{ item.id }}
            </div>
        </transition-group>
    </div>
</template>
<script setup lang="ts">
import { ref } from 'vue';
import _ from 'lodash'

let items = ref(Array.apply(null, { length: 81 } as number[]).map((_, index) => {
    return {
        id: index
    }
}))
const rad = () => {
    items.value = _.shuffle(items.value)
}

</script>
<style>
.ran {
    display: flex;
    flex-wrap: wrap;
    width: calc(20px * 10 + 20px);
}

.one {
    width: 20px;
    height: 20px;
    margin-right: 2px;
    border: 1px solid #eee;
}

.mm {
    transition: transform .5s ease;
}
</style>
```





### provide/inject







### 兄弟组件传参

- 通过父组件
- 消息订阅[Mitt](https://www.bilibili.com/read/cv16107098?spm_id_from=333.999.0.0)



### tsx

- 返回一个渲染函数
- optionsAPI
- setup 函数模式
- v-show 支持
- ref template 自动解包.value  tsx并不会 ref.value
- v-if 不支持 三元表达式代替
- map 代替v-for
- {} 代替 v-bind



### BABEL

![image-20230314161506386](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20230314161506386.png)





### v-model

本质是props和emit

因此子组件可接收父组件的v-model值

也可改变父组件的值

实现双向数据流动

```ts
defineProps<{
    //默认名字
	modelValue:type
    //自定义修饰符
    *****Modifiers?:{
    
}
}>()
```

```ts
//startwith--update:
const emit = defineEmits(['update:modelValue'])
const close = ()=>{
	emit('update:modelValue',params)
}
```





### 自定义指令

类似ref，能获取元素内容

钩子函数

一般使用mounted和updated

![image-20230314165321549](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20230314165321549.png)

```ts
//元素上 v-move="内容"
//名字v开头，上面下面对应
const vMove:Directive = {
    created(){

    },
    beforeMount(){
        
    }
}
```







### hook函数

可以理解成是方法的封装

**就是将文件的一些单独功能的js代码进行抽离出来，放到单独的js文件中**

例如图片转成base64格式

```ts
import { reject } from 'lodash'
import { onMounted } from 'vue'
type Options = {
    el: string
}
export default function (options: Options): Promise<{ imgUrl: string }> {
    return new Promise((resolve, reject) => {
        onMounted(() => {
            const file: HTMLImageElement = document.querySelector(options.el) as HTMLImageElement
            file.onload = (): void => {
                resolve({
                    imgUrl: toBase64(file)
                })
            }
        })
        const toBase64 = (el: HTMLImageElement): string => {
            const canvas: HTMLCanvasElement = document.createElement('canvas')
            canvas.height = el.height
            canvas.width = el.width
            const ctx = canvas.getContext('2d') as CanvasRenderingContext2D
            ctx.drawImage(el, 0, 0, canvas.width, canvas.height)

            return canvas.toDataURL('image/png')
        }
    })

}
```





# 全局变量和函数

```ts
//main.ts
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'

const app = createApp(App)

///////////////$name
app.config.globalProperties.$name = 'value'

///////////////$filter
app.config.globalProperties.$filter = {
    format<T>(str: T) {
        return str + 'mm'
    }
}

///需要声明
type Filter = {
    format<T>(str: T): string
}
// 声明要扩充@vue/runtime-core包的声明.
// 这里扩充"ComponentCustomProperties"接口, 因为他是vue3中实例的属性的类型.
declare module 'vue' {
    export interface ComponentCustomProperties {
        $name: string,
        $filter: Filter
    }
}

app.mount('#app')

```

### 调用

html上直接{{$name}}

script上

```ts
import { getCurrentInstance } from 'vue'

const app = getCurrentInstance()
console.log(app?.proxy?.$name);			//value
console.log(app?.proxy?.$filter.format(1213));			//1213mm

```





# 插件

app.use()

能够实现全局控制的组件

每一个页面都能使用同时能够控制的组件

- 插件创建

  ![image-20230315111233519](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20230315111233519.png)

- 注册（use）

```ts
import loading from './components/loading'

const app = createApp(App)

//这里把app回传给loading
app.use(loading)
```

- 编写

```ts
//loading/index.ts
import type { App } from 'vue'

import { createVNode, render } from 'vue'
import loading from './index.vue'
export default {
    //固定入口
    install(app: App) {
        // 转换成虚拟dom类型
        const Vnode = createVNode(loading)

        // 渲染到页面上render(组件，挂载点)
        render(Vnode, document.body)
        
        //全局配置
        app.config.globalProperties.$load = {
            show: () => Vnode.component?.exposed?.show(),
            close: () => Vnode.component?.exposed?.close()
        }
    }
}
```

- 声明

```ts
//main.ts
type Lod = {
    show(): void
    close(): void
}

declare module 'vue' {
    export interface ComponentCustomProperties {
        $load: Lod
    }
}
```









# 组件库

[一个 Vue 3 UI 框架 | Element Plus](https://element-plus.gitee.io/zh-CN/)：ts setup语法糖

[Ant Design Vue](https://next.antdv.com/docs/vue/introduce-cn) :ts setup()

[iView / View Design 一套企业级 UI 组件库和前端解决方案](https://www.iviewui.com/):options api

 [Vant 3 - Lightweight Mobile UI Components built on Vue](https://vant-contrib.gitee.io/vant/#/zh-CN/home)











# 样式穿透

外层css会自动生成哈希值区分，下载样式内层没有哈希值，但是自己去修改下载的组件内部样式时，浏览器上是会带有哈希值，从而导致无法命中这个属性。需要使用样式穿透

用于自定义修改组件

- vue2

  ```css
  style里面
  /deep/ .class_name{
  
  }
  ```

- vue3

  ```css
  style里面
  :deep(.class_name){
  
  }
  ```

deep属性选择器，放在哪个后面，把哈希值放到那个后面，挪了位置









# 异步任务

先宏后微

### 宏任务

script、setTimeout、setInterval、UI交互事件、Ajax

### 微任务

Promise.then catch finally、MutationObserver、process.nextTick(Node.js环境)

先宏后微

js脚本运行的时候就是第一个宏任务开始，同步结束后直接开始微任务。

![image-20230315170440149](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20230315170440149.png)

不用nextTick：改值=>innerHTML=>组件重新渲染=>diff后转换成真实节点

用nextTick：改值=>组件重新渲染diff变成真实节点=>微任务开始执行=>nextTick



# css原子化

接入unocss
tips：最好用于vite webpack属于阉割版功能很少

安装 

```
npm i -D unocss
```

vite.config.ts 

```ts
import unocss from 'unocss/vite'

 plugins: [vue(), vueJsx(),unocss({
      rules:[       

  ]
  })],
```

main.ts 引入 

```ts
import 'uno.css'
```

配置静态css

```ts
rules: [
  ['flex', { display: "flex" }]
]
```



配置动态css（使用正则表达式）

m-参数*10   例如 m-10 就是 margin:100px

```ts
rules: [
  [/^m-(\d+)$/, ([, d]) => ({ margin: `${Number(d) * 10}px` })],
  ['flex', { display: "flex" }]
]
```



shortcuts 可以自定义组合样式

```ts
 plugins: [vue(), vueJsx(), unocss({
    rules: [
      [/^m-(\d+)$/, ([, d]) => ({ margin: `${Number(d) * 10}px` })],
      ['flex', { display: "flex" }],
      ['pink', { color: 'pink' }]
    ],
    shortcuts: {
      btn: "pink flex"
    }
  })],
```





# 函数式编程

h函数

利用了creatVNode



# 桌面程序

eletron







# 环境变量

import.meta.env

手动设置

文件：

- .env.development
- .env.production

参数名大写

package.json配置

```json
 "dev": "vite --mode development",
 "bulid": 不需要
//生成环境会自己去读文件内容
```



**在vite.config.ts中是nodejs环境**

**需要使用process.env和loadEnv 获取环境变量**





# 打包优化分析

```
npm install rollup-plugin-visualizer
```

vite.config.ts

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'
import { visualizer } from 'rollup-plugin-visualizer'

// https://vitejs.dev/config/
export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  },
  plugins: [vue(), visualizer({ open: true })],


})

```



vite优化

```ts
build:{
       chunkSizeWarningLimit:2000,
       cssCodeSplit:true, //css 拆分
       sourcemap:false, //不生成sourcemap
       minify:false, //是否禁用最小化混淆，esbuild打包速度最快，terser打包体积最小。
       assetsInlineLimit:5000 //小于该值 图片将打包成Base64 
},
```

### PWA离线存储技术







# Vue3 Web Components

js当组件使用

隔离







# router路由

```
npm install vue-router@4
```

在src目录下面新建router 文件 然后在router 文件夹下面新建 index.ts

//引入路由对象

```ts
import { createRouter, createWebHistory, createWebHashHistory, createMemoryHistory, RouteRecordRaw } from 'vue-router'

//vue2 mode history vue3 createWebHistory
//vue2 mode  hash  vue3  createWebHashHistory
//vue2 mode abstact vue3  createMemoryHistory

//路由数组的类型 RouteRecordRaw
// 定义一些路由
// 每个路由都需要映射到一个组件。
const routes: Array<RouteRecordRaw> = [{
    path: '/',
    component: () => import('../components/a.vue'),
    children:[{
        
    },{
        
    }]
},{
    path: '/register',
    component: () => import('../components/b.vue')
}]

 

const router = createRouter({
    history: createWebHistory(),
    routes
})

//导出router
export default router
```

router-link#

请注意，我们没有使用常规的 a 标签，而是使用一个自定义组件 router-link 来创建链接。这使得 Vue Router 可以在**不重新加载页面**的情况下更改 URL，处理 URL 的生成以及编码。我们将在后面看到如何从这些功能中获益。

router-view# (可有多个)
router-view 将显示与 url 对应的组件。你可以把它放在任何地方，以适应你的布局。

<template>
  <div>
    <h1>小满最骚</h1>
    <div>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
      <router-link tag="div" to="/">跳转a</router-link>
      <router-link tag="div" style="margin-left:200px" to="/register">跳转b</router-link>
    </div>
    <hr />
    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>
  </div>
</template>

最后在main.ts 挂载

```ts
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
createApp(App).use(router).mount('#app')
```



### hash

#

监听左右箭头(前进，返回)

```ts
window.addEventListener('hashchange',(e)=>{

})
```





### web

createWebHistory

调整   但不会被箭头

```ts
history.pushState({state:1},'','路径')
```

监听左右箭头(前进，返回)

```ts
window.addEventListener('popstate',(e)=>{

})
```





### 编程式导航
除了使用 <router-link> 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

1.字符串模式

```ts
import { useRouter } from 'vue-router'
const router = useRouter()

const toPage = () => {
  router.push('/reg')
}
```

2.对象模式（传参）

```ts
import { useRouter } from 'vue-router'
const router = useRouter()

const toPage = () => {
  router.push({
    path: '/reg'
  })
}
```

3.命名式路由模式

```ts
import { useRouter } from 'vue-router'
const router = useRouter()

const toPage = () => {
  router.push({
    name: 'Reg'
  })
}

```

a标签跳转



### 历史记录

不保留（replace）

```html
 <router-link replace tag="div" to="/">跳转a</router-link>
```

或

```ts
const toPage = () => {
  router.replace('/reg')
}
```





### 传参

query:{} 或者 params:{}(数据会丢失)

params:{}(刷新数据会丢失)需要动态传参，路径设置/:id



useRoute()





### route 和 router

route 活跃的那个

router 全局







### 重定向

```ts
redirect:""

redirect:{
	path:""
}

redirect:to=>{
	return ''
}

redirect:to=>{
	return {
    	path:'',
        query:{
            
        }
    }
}
```



### 别名

alias:[]



### 路由导航

检验登录状态，是否跳转到登录界面

可以先搞页面白名单，允许跳转的页面列表

top上进度条



### 元信息

meta



### 滚动距离记忆

scrollBehavior:(to,from,savePosition)=>{}





# 细节

### 可选链操作符

?.    					前面读不到会返回undefined，防止报错

??  左边若是null或者undefined会返回右边的值

```ts
a.name?.length ?? []

//前面如果是undefined 会返回[]
```



### 非空断言

(!.)

```ts
color!.value = params
```



### axios 封装

```ts
export const axios = {
    get<T>(url: string): Promise<T> {
        return new Promise((resolve, reject) => {
            const xhr = new XMLHttpRequest()
            xhr.open('Get', url)

            xhr.onreadystatechange = () => {
                if (xhr.readyState == 4 && xhr.status == 200) {
                    setTimeout(() => {
                        resolve(JSON.parse(xhr.responseText))
                    }, 2000);
                }
            }
            xhr.send(null)
        })
    }
}
```

get(url,{params:{}})

post(url,{})



### CSS 新特性

##### 绑定script

v-bind()

```css
border: 1px solid v-bind(col);
```



##### :slotted(.class_name)

插槽选择器



##### :global(.class_name)

全局选择器



##### module

```vue
<template>
  <!-- :class="mm.div" -->
  <div :class="[$style.div]">
    2121
  </div>
</template>


<!-- module="mm" -->
<style module>
.div {
  color: pink;
}
</style>
```







### 声明文件注入

```ts
npm install --save @types/lodash
 
 这将告诉Typescript编译器什么是lodash以及库公开了哪些模块和组件。  
 此外， typings不再是推荐的方法。 您应该卸载并删除@types只应通过将tsconfig更新为：切换到@types包：
"typeRoots": [
  "node_modules/@types"
],
```





### 自动导入插件

unplugin-auto-import



### 移动控件案例

```ts
const vMove: Directive = {
    mounted(el: HTMLElement) {
        let moveel = el
        let movedown = (e: MouseEvent) => {
            let X = e.clientX - el.offsetLeft
            let Y = e.clientY - el.offsetTop

            let move = (e2: MouseEvent) => {

                el.style.left = e2.clientX - X + 'px'
                el.style.top = e2.clientY - Y + 'px'

            }
            document.addEventListener('mousemove', move)
            document.addEventListener('mouseup', () => {
                document.removeEventListener('mousemove', move)
            })
        }
        moveel.addEventListener('mousedown', movedown)

    }

}
```



### @路径设置

tsconfig.json

```json
  "paths": {
      "@/*":["./src/*"]
    }
```

vite.config.ts

```ts
export default defineConfig({
  resolve: {
     ////////
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  },
  plugins: [vue()],
})
```

### 异步输出

```js
  for (let i = 0; i < 5; i++) {
    setTimeout(() => {
      console.log(i);
    });
  }
// 0,1,2,3,4
```

```js
  for (var i = 0; i < 5; i++) {
    setTimeout(() => {
      console.log(i);
    });
  }
// 5 5 5 5 5
```



### $

可解构reactive()





### 起服务

http-server

```
npm install http-server -g
```

```
http-server -p 端口号
```





### 函数柯里化

函数柯里化（currying）是一种将接受多个参数的函数转换为一系列接受单个参数的函数的技术。这些接受单个参数的函数被称为部分应用函数，它们可以通过提供一部分参数来创建新的函数。

举个例子，考虑以下函数：

```ts
function add(x, y) {
  return x + y;
}
```

我们可以使用柯里化来将其转换为一个接受单个参数的函数序列：

```ts
function add(x) {
  return function(y) {
    return x + y;
  }
}
```

现在，我们可以这样调用：

```ts
add(2)(3); // 5
```

注意到原来的 `add` 函数接受两个参数，而柯里化之后的版本返回了一个闭包，内部的函数仅接受单个参数。当我们调用 `add(2)` 时，它返回一个新的函数，该函数可以将传递给它的参数与 `2` 相加。

柯里化可以使函数更加灵活和抽象化，因为它允许我们在运行时动态生成函数。







### json格式模板

插件：json to ts

ctrl+shift+alt+s





### 计时

window.requestAnimationFrame()
