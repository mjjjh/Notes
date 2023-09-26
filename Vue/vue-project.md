# 实现rem布局

在public文件夹下创建js文件夹

在js文件夹下创建rem.js





# axios

封装

为了方便改域名

域名抽取

### 创建实例

可以使用自定义配置新建一个 axios 实例

##### axios.create([config])

```js
const service = axios.create({
  baseURL: 'https://some-domain.com/api/',
  timeout: 1000,
  headers: {'X-Custom-Header': 'foobar'}
});
```

```js
// 发送 POST 请求
axios({
  method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
// 获取远端图片
axios({
  method:'get',
  url:'http://bit.ly/2mTM3nY',
  responseType:'stream'
})
  .then(function(response) {
  response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
});


//async await方法
onMounted(async()=>{
        let res = await getBanner();
        state.images = res.data.banners
    })
```

### 实例方法

以下是可用的实例方法。指定的配置将与实例的配置合并。

##### axios#request(config)

##### axios#get(url[, config])

##### axios#delete(url[, config])

##### axios#head(url[, config])

##### axios#options(url[, config])

##### axios#post(url[, data[, config]])

##### axios#put(url[, data[, config]])

##### axios#patch(url[, data[, config]])



# 数据集中获取（api）

首页的

其他部分的

### 使用axios实例加函数

api/home.js中代码

```js
// 首页的数据
import service from "..";

// 首页轮播图数据
export function getBanner() {
    return service({
        method: "get",
        url: "/banner?type=2"
    })
}
```



# 获取api数据记得async|await





# 页面无法跳转

> 注意App.vue里面视图

```js
<template>

 <router-view></router-view>

</template>
```

只用 <router-view></router-view>即可，实现窗口视图在这里出现



# 渲染快而数据所在层级过深，获取时会丢失

需要将数据保存到本地

将数据保存到sessionStorage中

##### 对象转成JSON属性存储

```js
const state = {example：{}} 
sesssionStorage.setItem("属性名",json形式——————JSON.stringfy(state))
```



### 在数据接收那边需要判断

如果数据没有接收到需要获取sessionStorage中的数据

##### JSON转对象获取

```js
sesssionStorage.getItem("set中的属性名",json形式——————JSON.stringfy(对象))

json转对象
JSON.parse(上面的).example
//注意这个example是上面存储数据的属性example
```



# store中存储的数据一定只能通过mutations更改





# 需要用到实时监听可以使用定时器

每隔几秒触发一次





# 需要属性就在script中添加







# [需要看的元素]

可以看到内部所有元素



# promise <pending>

还在进行中，需要async和await





# 谷歌浏览器记住密码样式改变

修改谷歌浏览器记住密码后的默认样式

```css
input:-webkit-autofill {
    -webkit-text-fill-color: #999999 !important;
    transition: background-color 5000s ease-in-out 0s;
}
```



# 刷新之后store里面的数据会丢失

