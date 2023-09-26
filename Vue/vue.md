# 模块化、组件化、规范化、自动化

+ 模块化 js css 资源
+ 组件化 ui结构

# webpack

前端工程化具体解决方案

压缩、js兼容、性能优化等

![image-20221031144737624](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221031144737624.png)

-S 开发阶段及以后上线都要使用的包(--save)

-D 只开发阶段使用的包 (--save-dev)

npm install webpack@5.42.1 webpack-cli@4.7.2 -D









# vue

vue指令、组件（是对ui结构的复用）、路由、vuex、vue组件库

### 数据驱动视图

 	+ 数据变化会驱动视图自动更新
 	+ 自动渲染
 	+ 单项绑定

### 双向数据绑定

	+ form表单采集数据，Ajax提交数据
	+ 不用手动操作DOM元素双向。

> 底层原理是MVVM（model view view-model）



# 结构

```vue
<div id="app">{{username}}</div>
<script>
    const vm = {   //配置对象
        data() {  //参数
            return {
                username: 'zs'
            }
        }
    }
    Vue.createApp(vm).mount('#app') //创建应用，将配置对象组件化挂载到id为app的元素上
</script>
```



# 指令

### v-once

一次渲染不会改变



### v-html（慎用）

插入html语言

v-html="html语言"



### v-bind

绑定

：

：[变量]="变量"

@[变量]=”变量“



### v-if和v-show

v-show只是display上变换，元素还是会渲染出来（频繁切换页面使用）

v-if 会销毁元素（一次性渲染，条件很少改变）



### v-for

diss

需要key值，唯一标识，要添加（减少渲染次数，提高渲染性能）

![image-20221101112235738](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221101112235738.png)



### v-model

<input type="text" :value="w" @input="e => { w = e.target.value }">

<input type="text"v-model="w">

+ .lazy 输入框失去焦点，发生改变
+ .number 值转为数字类型
+ .trim 返回值自动 去除空格











简写或者全写

### 响应式methods

每一次调用便执行一次

### 计算属性 computed

用于计算使用，只要依赖值不变，不会重新计算（会缓存）

### 监听watch

监听数据改变，只监听不需要this

一个数据影响多个数据

不是深度监听，监听不到属性变化

深度监听时侦听器会一层一层向下遍历，给对象每一个属性加上侦听器

因此可以使用 ”obj.attribution“:{}使用字符串进行优化，只会单独监听

```
watch：{
监听的数据名字(newValue,oldValue){

}
}
```



### 样式绑定

:class="{ coser: isactive }"

会和样式合并

防止对象

### template



### 事件处理

**event**

访问DOM事件

longs(param, $event) {

   this.w -= 20;

   console.log(this.w);

  }



### 事件修饰符

<!-- 单击事件将停止传递 --> 

	<a @click.stop="doThis"></a> 



<!-- 阻止默认行为 -->

	 <form @submit.prevent="onSubmit"></form>



<!-- 只触发一次回调 -->

@click.once



# 组件

### this

是组件实例

### 引入->注册->使用

ui层面

功能模块封装

![image-20221101162804834](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221101162804834.png)

把其他组件（vue）引入到App.vue,再把App.vue挂载到页面上

先引入再声明注册才能使用

```vue
<script>
    //引入
import Contents from './components/Contents.vue' 
export default {
  data() {   //返回不同对象,每个组件返回不同对象，不会造成数据污染
    return {   
    };
},
  components: {
      //声明注册
    Contents
  }
};
</script>
```

每当使用一个组件，就创建了一个新的**实例**

<组件名></组件名>       或者      <组件名 />



### Props向子组件传递数据（单向数据流）

原因：数据传给父组件，父组件再分类所有数据把相应数据给子组件（这样就减少了向服务器请求次数）

![image-20221101170737388](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221101170737388.png)

+ 父组件对子组件引用处 动态绑定(:  v-bind)或普通复制("自定义变量名=传的数据")
+ 子组件props：{  }接收

props:[]子组件接收数据

可传动态v-bind；也可以静态

对象类型（可以限制类型）

### props校验

export default {  

​		props: {    

​			title: String,  

​		    likes: Number 

​	 }

 }

### $emit监听(v-on @)子组件(子传父)

子组件先声明emits:["自定义事件名字"]

子组件中通过$emit来触发事件

this.$emit(“自定义事件名称”,"发送的事件参数")

+ 子组件触发函数，函数下this.$emit触发自定义事件
+ 父组件监听事件 <Contents @自定义事件="方法"></Contents>通过方法接收

子组件触发函数，函数触发事件，事件又触发父组件methods







### $refs:父组件访问子组件 

+ **注册**子组件实例中加个**ref="name"**
+ **this.$refs**

也可以获取元素的信息



### $parent:子组件访问父组件

**this.$parent**直接访问

**尽量不要直接访问，用props接收**

因为子组件可能会被多个父组件调用，读取参数不一致



### $root:子组件访问根组件

 根组件：App.vue 



# 插槽

留下坑

<组件><slot></slot></组件>



### 具名插槽

```vue
<slot name="Name">默认内容</slot>


<!-- 父级 -->
<template v-slot:Name> 
<!-- header 插槽的内容放这里 --> 
</template>
```

 当没有传入插槽数据会使用插槽中的备用内容



### 作用域插槽

**数据由子组件提供**

子传父类似props

```vue
<!-- <MyComponent> 的模板 -->
<div>
  <slot :text="greetingMessage" :count="1"></slot>
</div>


<!-- 父级 -->
<MyComponent v-slot="slotProps">
  {{ slotProps.text }} {{ slotProps.count }}
</MyComponent>

```



# Provide/Inject

跨级通信

注入接收的 **变量名要一样**

**注意这并不是相应式的，不会变值**

### 两种方式

+ ```js
  //父组件
  provide:{
      me:'hello'
    }
  
  //子组件
  inject:['me']
  ```

+ 如果我们需要提供**依赖当前组件实例**的状态 (比如那些由 `data()` 定义的数据属性)，那么可以以函数形式使用 `provide`：

  ```js
  export default {
    data() {
      return {
        message: 'hello!'
      }
    },
    provide() {
      // 使用函数的形式，可以访问到 `this`
      return {
        message: this.message
      }
    }
  }
  ```

### 响应式解决

+ 传对象obj，值放在obj中，因为对象是传地址

```js
	provide(){

​			 return {

​					obj:{

​							 valuename:this.value

​					} 

​			}

​	}

obj.valuename
```



+ 把传递值变成函数

  ```js
  provide(){
  		 return {
  			message: ()=>this.message
  		}
  
  }
  
  
  // 子组件注入接收是个函数
  inject:['message']
  
  //使用(为了页面简洁，函数调用一般在computed计算属性中调用)
  computed:{
  	newmsg(){
  		return this.message()
  	}
  }
  ```

+ 直接传计算属性

  ```js
  import { computed } from 'vue'
  
  export default {
    data() {
      return {
        message: 'hello!'
      }
    },
    provide() {
      return {
        // 显式提供一个计算属性
        message: computed(() => this.message)
      }
    }
  }
  
  ```





# 生命周期

+ 创建实例
+ 初始化，数据监测，数据代理
+ created——————（data 和 methods 绑定完成到vue上但是DOM没有生成）
+ 生成虚拟DOM，开始转换为真实DOM
+ mounted
+ DOM形成，mounted不断监听页面形成更新

每个template都有一个生命周期





# 组合式api——setup函数

 将同一逻辑关注点相关代码收集在一起

setup(){}

> 注意：所需要的组成使用都需要import导入
>
> 注意：从setup中返回的refs在模板中访问时时被自动浅解包的，因此不应在模板中使用.value

### setup

是个函数，记得return导出

function也要导出

setup在实例创建之前       **要避免使用this，会找不到实例**

通过ref、reactive定义响应式变量

响应式：动态变化

以下都在setup中的内容

### ref(value)

定义num和string类型的value

ref()返回带有value属性的对象

```vue
<script>
import { ref } from 'vue'

export default {
  setup() {
      //ref()返回带有value属性的对象
    const count = ref(0)

    // 返回值会暴露给模板和其他的选项式 API 钩子
    return {
      count
    }
  },

    //当通过 this 访问时也会同样如此解包
  mounted() {
    console.log(this.count) // 0
  }
}
</script>

// 模板会自动解析value值(解包)
<template>
  <button @click="count++">{{ count }}</button>
</template>
```

### reactive({})

定义响应式对象类型

可以直接在return哪里通过...解构对象，方便后面直接拿值，但这样传出去的不是对象，因此**不再是响应式**了

### 解构...后不在响应式解决方法

##### toRefs(object)

toRef:指向源地址，响应式值

toRefs:将响应式对象转换为普通对象，这个对象里每个值都是toRef指向源地址

使解构后的数据重新会获得响应式

使用：...toRefs(object)

### 监听watch()

watch(监听数据名字,回调函数)

监听对象与选项式类似，能知道对象变了，但无法读取对象内部值。

为了能监听到对象内的值需要使用：**watchEffect** 

##### watchEffect()

watchEffect(回调函数) ——————————不需要指定监听的属性，会自动搜集依赖

> 但是组件初始化时会执行一次回调函数

类似computed的依赖，只要在回调中引用到响应式的属性，那这个依赖属性变了就监听

### 生命周期钩子

在之前生命周期钩子基础上加上on即可

都需要import导入钩子

onMounted(回调函数)

### props

 setup(props){}

props:{

}

还是要写

```js
export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}

```

但是接收变成非响应式，需要调用**toRefs**去转换props对象

```js
const {valuename} = toRefs(props)
```

> 但此时需要注意valuename是一个对象，要值需要valuename.value去取值

### 解构会破环响应式

let str = props.str

这也是解构

解决方法：

```js
import { toRefs, toRef } from 'vue'

export default {
  setup(props) {
    // 将 `props` 转为一个其中全是 ref 的对象，然后解构
    const { title } = toRefs(props)
    // `title` 是一个追踪着 `props.title` 的 ref
    console.log(title.value)

    // 或者，将 `props` 的单个属性转为一个 ref
    const title = toRef(props, 'title')
  }
}

```

### context

setup(props,context)

获取组件被使用时的属性、插槽、触发事件、函数

 console.log(context.attrs)     // 插槽（非响应式的对象，等价于 $slots）    console.log(context.slots)     // 触发事件（函数，等价于 $emit）    console.log(context.emit)     // 暴露公共属性（函数）    console.log(context.expose)



`setup` 也可以返回一个[渲染函数](https://cn.vuejs.org/guide/extras/render-function.html)，此时在渲染函数中可以直接使用在同一作用域下声明的响应式状态：

```js
import { h, ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    return () => h('div', count.value)
  }
}

```

返回一个渲染函数将会阻止我们返回其他东西。

对于组件内部来说，这样没有问题，但如果我们想通过模板引用将这个组件的方法暴露给父组件，那就有问题了。

我们可以通过调用 [`expose()`](https://cn.vuejs.org/api/composition-api-setup.html#exposing-public-properties) 解决这个问题：

##### expose()

```js
import { h, ref } from 'vue'

export default {
  setup(props, { expose }) {
    const count = ref(0)
    const increment = () => ++count.value

//暴露出去的数据
    expose({
      increment
    })
//返回渲染函数
    return () => h('div', count.value)
  }
}

```

### provide/inject

provide('name',value)

```js
<script setup>
import { ref, provide } from 'vue'
import { fooSymbol } from './injectionSymbols'

// 提供静态值
provide('foo', 'bar')

// 提供响应式的值
const count = ref(0)
provide('count', count)

// 提供时将 Symbol 作为 key
provide(fooSymbol, count)
</script>

```

const 变量名 =  inject("name")；

```js
<script setup>
import { inject } from 'vue'
import { fooSymbol } from './injectionSymbols'

// 注入值的默认方式
const foo = inject('foo')

// 注入响应式的值
const count = inject('count')

// 通过 Symbol 类型的 key 注入
const foo2 = inject(fooSymbol)

// 注入一个值，若为空则使用提供的默认值
const bar = inject('foo', 'default value')

// 注入一个值，若为空则使用提供的工厂函数
const baz = inject('foo', () => new Map())

// 注入时为了表明提供的默认值是个函数，需要传入第三个参数
const fn = inject('function', () => {}, false)
</script>

```

### script setup

语法糖

+ 引入组件不需要注册组件
+ 定义变量在模板中不需要在暴露出去（export default）
+ 定义响应式变量需要从vue中引入

##### defineProps()和definEmits()













# 路由

核心：改变url，但是页面不进行整体刷新

路由理解为指向

路由表：是一个映射表，一个路由就是一组映射关系，key：value————key：表示路径，value：可以为function或者Component

### 安装

在工程文件夹下

```
npm install vue-router@4
```

 ### router文件夹

配置路由文件(路由表，组件等)

![image-20221103170321385](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221103170321385.png)

```js
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!--使用 router-link 组件进行导航 -->
    <!--通过传递 `to` 来指定链接 -->
    <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

> router-link内部是内容，可以是文字，也可已是内容，等等

### views文件夹

路由组件.vue

![image-20221103170348259](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221103170348259.png)

### 挂载

![image-20221103170453471](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221103170453471.png)

```js
// 1. 定义路由组件.
// 也可以从其他文件导入
const Home = { template: '<div>Home</div>' }
const About = { template: '<div>About</div>' }

// 2. 定义一些路由
// 每个路由都需要映射到一个组件。
// 我们后面再讨论嵌套路由。
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
]

// 3. 创建路由实例并传递 `routes` 配置
// 你可以在这里输入更多的配置，但我们在这里
// 暂时保持简单
const router = VueRouter.createRouter({
  // 4. 内部提供了 history 模式的实现。为了简单起见，我们在这里使用 hash 模式。
  history: VueRouter.createWebHashHistory(),
  routes, // `routes: routes` 的缩写
})

// 5. 创建并挂载根实例
const app = Vue.createApp({})
//确保 _use_ 路由实例使
//整个应用支持路由。
app.use(router)

app.mount('#app')

// 现在，应用已经启动了！
```

> 注意要把路由配置文件导出挂载到app上
>
> router-link、router-view都需要挂载了才能使用

以上路由是不重新加载页面情况下更改url

### 带参数路由（$route）

/路径/:id

> 值会给id

可以理解位符号：表示动态的，不知道输入什么值

*路径参数* 用冒号 `:` 表示。当一个路由被匹配时，它的 *params* 的值将在每个组件中以 `this.$route.params` 的形式暴露出来。因此，我们可以通过更新 `User` 的模板来呈现当前的用户 ID：

this.$route表示当前实例是活跃的路由对象下

### 组合式带参数路由

访问：

```js
import {useRoute} from "vue-router"

useRoute().params.参数名
```



### NotFound 页面

要输入的网站是动态的需要：符号

输入错误网站地址都到这一界面

需要使用到正则表达式

```js
const routes = [
  // 将匹配所有内容并将其放在 `$route.params.pathMatch` 下
  { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  // 将匹配以 `/user-` 开头的所有内容，并将其放在 `$route.params.afterUser` 下
  { path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

### 多个参数

```js
path:"/index/:id+"
```

:id 绑定

"+"表示可以有多个参数

“*”表示可有可无

（正则）

### 嵌套路由

一个父路径有多个子路径可以选择

通过父路径加children[]数组得到

```js
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        // 当 /user/:id/profile 匹配成功
        // UserProfile 将被渲染到 User 的 <router-view> 内部
        path: 'profile',
        component: UserProfile,
      },
      {
        // 当 /user/:id/posts 匹配成功
        // UserPosts 将被渲染到 User 的 <router-view> 内部
        path: 'posts',
        component: UserPosts,
      },
    ],
  },
] 
```



### $router

路由实例

$router是一个全局的路由容器,里面包含着很多的路由对象;$route是$router容器中状态是active的那个路由对象.

![image-20221104120639179](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221104120639179.png)

##### $router.push()

this.$router.push("路径") 由this当前的组件实例跳转到路径

实现路由守卫，条件判断

+ this.$router.push("路径") 

+ this.$router.push({path:"路径"})
+ this.$router.push({**name**:"路径取得名字",params:{"属性名":"属性值"}})
+ this.$router.push({path:"路径",query{属性名:”属性值“}})      /路径?属性名=属性值  （问号形式）

替换当前位置(替换掉当前页面，是当前页面没有历史)：

```js
this.$router.push({path:"路径",query{属性名:”属性值“},replace:true}) 
```

##### $route.go(num)

num为1：前进

num为-1：后退

##### $router.back()

后退一步

##### $router.forward()

前进一步

### 命名路由（name）

```js
const routes = [
  {
    path: '/user/:username',
      //命名为user
    name: 'user',
    component: User,
  },
]
```

```html
//调用
<router-link :to="{ name: 'user', params: { username: 'erina' }}">
  User
</router-link>
```



### 命名视图（多个路由组件components）

同级组件路由展示

多个路由组件视图需要components

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 **s**)：

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/',
        //components:多个路由组件视图
      components: {
        default: Home,
        // LeftSidebar: LeftSidebar 的缩写
        LeftSidebar,
        // 它们与 `<router-view>` 上的 `name` 属性匹配
        RightSidebar,
      },
    },
  ],
})
```

同时占位符<router-view>需要给名字

```html
<router-view name="LeftSidebar"></router-view>
<router-view></router-view>
<router-view name="RightSidebar"></router-view>
```



### 重定向（redirect）

不同名字同一个页面

会被替换

redirect

```js
const routes = [{ path: '/home', redirect: '/' }]
```



```js
const routes = [
  {
    // /search/screens -> /search?q=screens
    path: '/search/:searchText',
    redirect: to => {
      // 方法接收目标路由作为参数
      // return 重定向的字符串路径/路径对象
      return { path: '/search', query: { q: to.params.searchText } }
    },
  },
  {
    path: '/search',
    // ...
  },
]
```



### 取别名（alias）

alias：”别名“

方便写,不会被替换



### props

在组件中使用 `$route` 会与路由紧密耦合，这限制了组件的灵活性，因为它只能用于特定的 URL。虽然这不一定是件坏事，但我们可以通过 `props` 配置来解除这种行为：

```js
const routes = [{ path: '/user/:id', component: User, props: true }]
```

接收端

```js
props:{
	id:String
}
```

> 对于有命名视图的路由，你必须为每个命名视图定义 `props` 配置：

```js
const routes = [
  {
    path: '/user/:id',
    components: { default: User, sidebar: Sidebar },
      //每个命名视图组件定义props
    props: { default: true, sidebar: false }
  }
]
```

### 历史模式

createWebHistory     html5模式

createWebHashHistory   hash模式

二者区别有无#号





# 导航守卫

### 全局路由对象

router.beforeEach

全局意味每次进入路由都调用

```js
const router = createRouter({ ... })

router.beforeEach((to, from，next) => {
  // ...
    //to到哪里来，from从哪里来
    //next()是一个函数相当于通行证
  // 返回 false 以取消导航
  return false
})
```

next("路径")；

没有路径默认继续。



### 每路守卫

beforeEnter

```js
 {
        path: '/about', component: About,
        beforeEnter: (to, from，next) => {
            console.log(to);
            console.log(from);
            if(条件){
                next();
            }

        },
```



### 组件内的守卫

- `beforeRouteEnter(){"路由进入组件之前"}`  这里拿不到this（此时组件实例还没创建）
- `beforeRouteUpdate(){"路由更新组件之前"}`
- `beforeRouteLeave(){"路由离开组件之前"}`

`beforeRouteEnter(){"路由进入组件之前"}`  这里拿不到this（此时组件实例还没创建）要拿到实例值,需要使用next回调函数获取

```js
beforeRouteEnter(to,from,next){
	next((vm)=>{
	vm.数值名；
	})

}
```



# 路由懒加载（推荐）

用到时再加载组件

```js
// 将
// import UserDetails from './views/UserDetails.vue'
// 替换成
const UserDetails = () => import('./views/UserDetails.vue')

const router = createRouter({
  // ...
  routes: [{ path: '/users/:id', component: UserDetails }],
})
```

变成函数



# 状态集中管理

**store文件夹下**

**index.js**

用来管理数据

实现响应式，在App组件中能通过provide提供

+ 响应式：reactive{}对象中存着状态

  ```js
  import {reactive} from "vue"
  
  const store = {
    //定义状态  
      state:reactive({
          msg:"helloworld"
      })
  }
  ```

  父组件import    ， provide

  子组件inject



# fetch

原生js，是http数据请求的一种

  fetch()返回promise对象

**通过json()将响应的body,解析json的promise**

fetch("网站路径").then((res)=>{return res.json()}).then((res)=>res)



# axios

基于promise的http库

```
安装： npm install axios
```



```js
import axios from "axios"
```

### 同源策略（CORS），浏览器的保护机制

### 跨域请求

解决方法

不通过浏览器请求，

代理：proxy

![image-20221104163021897](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221104163021897.png)

**配置vite.config.js**

```js

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  // 中转服务器
  server: {
    // 通过代理实现跨域
    proxy: {
      '/path': {
        target: "https://baike.baidu.com",
        // 开启代理允许跨域
        changeOrigin: true,
        //设置重写路径(把/path替换多余)
        rewrite: path => path.replace(/^\/path/, '')
      }
    }
  }
})

```



```js
import axios from "axios"
export default {
    created(){
        axios.get("/path/item/SD%E5%8D%A1/122767?fr=aladdin").then((res)=>console.log(res))
    }
}
```



# vuex

![image-20221104194239495](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/vue/vueimage-20221104194239495.png)

状态集中管理

如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 [store 模式](https://v3.cn.vuejs.org/guide/state-management.html#从零打造简单状态管理)就足够您所需了。但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。

仓库与数据间联通

安装

```
npm install vuex@next --save
```

引入(创建store文件夹下的index.jss)

```js
import { createStore } from 'vuex'
```





```js
import { createStore } from "vuex";
// 创建store实例
const store = createStore({
    // 存储单一状态，是存储的基本数据
    state() {
        return {
            //数据
        }
    },
    //store内的方法
    mutations:{
        //方法(第一个参数一定是state)
        function_example(state,params){
        }
    }
})
export default store;
```

### 访问 $store

> 不能直接改变store中的状态，改变的唯一途径是显式的提交（commit）mutation

**store中的值需要通过store内部的方式去改变**

可以通过 `$store.state` 来获取状态对象，并通过 `$store.commit` 方法触发状态变更：

### mutations

内部的函数是对state中的值进行操作，第一个参数一定要是上面定义的state对象

通过**$store.commit('函数名',(参数)载荷)**触发

同步函数

store中的值通过mutations变更

传的参数多可以传对象

### Getter

getters:{}

如果state中的状态需要过滤或者其他操作

相当于store中的计算属性

内部方式第一个参数也是state

第二个是getters

```js
    getters: {
        ReversMsg: function (state) {
            return state.msg.split("").reverse().join('')
        },
        ReversMsgLength: function (state, getters) {
            return getters.ReversMsg.length
        }
    }
```

### Action

获取数据（异步操作）

异步函数

不能直接变更状态，需要提高mutation

dispatch触发

函数名(context)——————context与store具有相同属性和方法

context.state.属性名

改变store中的状态用mutations中的方法

触发（commit）mutations

```js
context.commit('函数名'，参数)
```



### module

为了将数据细分，使得数据在state中显的不臃肿。

模块化

每一个模块就相当于一个子仓库，里面包含state、mutations、getter、action甚至子module

```js
// 模块化
const moduleA = {
    state() {
        return {
            username: "张三"
        }
    },
    getters: {
        //局部的state是moduleA中的state
        usernameage: function (state) {
            return state.username + "今年18岁了";
        }
    }
}



// 创建store实例
const store = createStore({
    modules: {
        a: moduleA
    },
}
```

**会将模块中的getters（子父）合并在一起** 

**mutations也是**

**actions也是**

但是state不会合并，需要**$store.state.模块名字**来获取，访问父模块state变成rootState（第三个参数）

模块化 文件夹分部分



### 辅助函数mapState、mapMutations、mapGetters、mapActions

来自vuex

相当于直接提取，直接拿来使用

##### mapState

```js
import {mapState} from vuex;

computed:mapState({
	//state是vux中保存起来的store中的state
	count:state=>state.count;
    //count:"count";也可以   
})

//这样也可以
computed:mapState(["count"]);
```

显示数据直接用count即可，已经**直接提取**出来了

**这样不要$store.state.count**

缺陷：

​		只能有一个computed，如果组件自身需要使用computed会出现问题

解决方法：

 		对象展开运算符：(mapState是一个对象)，对象展开拼接组成新的对象

```js
computed:{
	组件计算属性(){},
	//...对象展开运算符
	...mapState(["count","..."])
}
```



##### mapGetters

在computed中扩展







##### mapMutations

在methods中扩展

```js
import {mapMutations} from vuex;


//methods中使用
methods:{
    组件methods,
    ...mapMutations(["函数名","函数名"])
}
```



##### mapActions

在methods中扩展



### 模块命名空间 namespaced

使得模块分割开

namespaced:true

模块加了命名空间,需要改变调用名字

```js
this.$store.state["模块名/属性名"]
```

辅助函数写法也变了（第一个参数变成模块名字）

```js
...mapMutations("模块名",{"函数名","函数名"})
```







### $nextTick

作用：将回调函数延迟到下一次dom更新数据后调用

this.$nextTick(回调函数)

在dom渲染之后被触发，获取更新的数据

vue是异步渲染的，数据更新之后，dom不会立刻渲染



另解：加一个定时器





使用场景

+ 在生命周期created中进行dom操作，一定要放在$nextTick中
+ 在数据变化后要执行的某个操作，而这个操作需要使用随数据变化而变化的Dom结构，这个操作常常放在$nextTick中
