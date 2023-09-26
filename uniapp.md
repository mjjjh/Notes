# 全局配置文件



# rpx

响应式单位

机型自动适配

是iphone两倍    375px = 375*2 = 750rpx

# scss

嵌套

变量

运算



# 内部组件

为了适配各个平台

### view  

div



### text

span

文本可选否



### scroll-view

滑块

使用竖向滚动时，需要给 `<scroll-view>` 一个固定高度，通过 css 设置 height；使用横向滚动时，需要给`<scroll-view>`添加`white-space: nowrap;`样式。





###  swiper 

加swiper-item

轮播图



### image

aspectFill:短边显示完全



### navigator

跳转非tabbar 





# 组件（easycom）

1. **通过uni-app的[easycom](https://uniapp.dcloud.io/collocation/pages?id=easycom)：** 将组件引入精简为一步。只要组件安装在项目的 `components` 目录下，并符合 `components/组件名称/组件名称.vue` 目录结构。就可以不用引用、注册，直接在页面中使用。

同级目录



# .native

@click.native

表示组件使用的是原生的方法，而不是子组件emit传过来的方法





# .sync

：state.sync="数据"

实现子组件能够更改父组件的值

子组件:		this.$emit("update:state",修改的值)





# 小程序中接收路由数据$route失效

使用onLoad(e)

e中包含传递的数据







# 字体图标

iconfontsrc   (ttf)

text：“\u字库Unicode码”





# api    界面    设置TabBar





# 数据本地缓存(Storage)

sync同步   **使用同步**

相比于异步少success

+ uni.setStorageSync

+ uni.getStorageSync

+ uni.removeStorageSync

+ uni.cleanStorageSync



# request封装

全局js文件





# 状态管理

store/index.js

```js
import {
	createStore
} from "vuex"


import cart from "./modules/cart.js"
const store = createStore({
	modules: {
		cart
	}
})

export default store;
```



store/modules/cart.js

```js
export default {
	state: {
		a: 1
	}
}
```



main.js

```js
// #ifdef VUE3
import {
	createSSRApp
} from 'vue'
export function createApp() {
	const app = createSSRApp(App)
	///////////////
    app.use(store)
	return {
		app
	}
}
// #endif
```



# 验证码

腾讯云sdk



# 支付宝

支付：

+ 沙箱
  + 登录
  + 下载工具（登录同一个号）
  + 生成私钥，复制公钥
+ 企业sdk
