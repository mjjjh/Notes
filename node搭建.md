# 搭建后端

### 全局命令

装一次即可（已装）

```
npm install express-generator -g
```

### 进入项目目录

```
express --view=ejs server
```

![image-20221117182653104](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20221117182653104.png)

server目录

安装好后运行后默认端口号：localhost:3000



> 写入数据后一定要重启一下



# 前端请求后端接口【本地测试】

注意：

+ 手机和电脑是一个wifi
+ 不可以使用localhost，必须使用ip地址

查ip：

+ mac：ifconfig
+ window：ipconfig







# 跨域问题

1.首先是最简单有效的通过中间键解决
在命令行中输入

```
npm i cors -s
下载cors中间键并且调用
app.use(cors())
```

即可解决

2.第二种是直接在路由中输入

```
res.setHeader("Access-Control-Allow-Origin", "*");
```

这个也比较简单但是有些解决不了

3.第三种是用app.all解决，比起第二种更加全面
包含了不同请求格式

```
app.all('*', function (req, res, next) {
	res.header("Access-Control-Allow-Origin", "*");
	res.header('Access-Control-Allow-Methods', 'PUT, GET, POST, DELETE, OPTIONS');
	res.header("Access-Control-Allow-Headers", "X-Requested-With");
	res.header('Access-Control-Allow-Headers', ['mytoken','Content-Type']);
	next();
});
```

目前遇到的跨域问题基本这三种都能解决。



# 接口文档

### 1.1 接口功能

获取首页数据

### 1.2 URL

地址：/api/index_list

### 1.3支持格式

JSON

### 1.4HTTP请求

GET

### 1.5请求参数

### 1.6返回参数

| 返回字段 | 字段类型 | 说明                           |      |
| -------- | -------- | ------------------------------ | ---- |
| code     | string   | 返回结果状态。0：正常；1：错误 |      |
| data     | object   | 首页数据                       |      |

### 1.7接口示例

```json
{
	”code“:”0“，
	”data“:{
		topBar:[
			{
			id:0,
			name:"推荐"
			}
			...
		],

		data:[{
            type:"swiperlist",
            data:[{
                imgUrl:"../as/.."
            }]      
		}]
	}
}
```

### 1.8
