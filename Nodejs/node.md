# Node

运行 V8 JavaScript 引擎，它是 Google Chrome 的核心

![](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/node/nodeimage-20230322191442517.png)







### 使用node：

- 交互，简单验证
- 解释js文件  node js文件名



### 使用Nvm

> 安装前需要卸载node

包容所有的node版本，管理node版本存在在电脑里面

```
nvm list   -----查看所有安装的node版本

nvm install node版本号   ------安装node

nvm use 版本号  ------使用特定版本

nvm uninstall 版本号 -------卸载
```







### Code Runner

插件，在编译器中直接运行js



### 注意

node 只会运行ECMAscript（核心语法），

不会运行bom，dom，会报错





### CommonJs（模块化的标准，服务器端的模块化规范）的模块的使用

- 导出exports

  ```js
  let a = 10;
  function getSum(a,b){
      return a+b;
  }
  
  //导出方式一
  exports.a = a;
  exports.getSum = getSum;
  
  //导出方式二
  //通过module.exports等于一个对象，来导出数据
  module.exports = {
      a,getSum
  }
  ```

- 导入数据require

  ```js
  //导出的模块一般需要使用一个变量来接收，一般把接收的变量定义常量
  const m1 = require("./m1.js");
  console.log(m1);
  
  //{ a: 10, getSum: [Function: getSum] }
  ```



####  this指向

在node中this代表当前这个模块，也就是exports对象

```js
//运行在node中
consloe.log(this)
//{}   也即exports对象
```



> node里面没有window对象，但是有全局对象**global**
>
> nodejs里面声明这个变量，不会添加到global全局对象中，但可以给global添加成员（global.a = 10;）





# node 模块

先require引入

- 内置模块
- 自定义模块（自己书写）
- 第三方模块（npm安装使用）





### nodemon 包

监听代码的改动,自动更新

```
npm install -g nodemon
```



改变code runner插件配置（使js在nodemon中运行）

![image-20221206195852758](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/node/nodeimage-20221206195852758.png)

```json
{
    "code-runner.executorMap": {
        "javascript": "node",     -------->改成nodemon
        "php": "C:\\php\\php.exe",
        "python": "python",
        "perl": "perl",
        "ruby": "C:\\Ruby23-x64\\bin\\ruby.exe",
        "go": "go run",
        "html": "\"C:\\Program Files (x86)\\Google\\Chrome\\Application\\chrome.exe\"",
        "java": "cd $dir && javac $fileName && java $fileNameWithoutExt",
        "c": "cd $dir && gcc $fileName -o $fileNameWithoutExt && $dir$fileNameWithoutExt"
    }
```



### 内置模块

- fs：文件操作
- http：网络操作
- path：路径操作（提供了用于处理文件和目录的路劲的实用工具）
- querystring：查询参数解析（？后面的参数）
- url：解析



#### path模块

```js
// 引入path模块
const path = require('path');

//两个特殊的变量
console.log(__dirname);//当前执行的文件绝对路径，不包含文件名

console.log(__filename); //当前执行的文件绝对路径，包含文件名
```



##### path.extname() 

后缀名



##### path.basename()

 文件名



##### path.dirname() 

指定绝对路径



##### path.parse(指定文件) 

包含所有路径信息的对象



##### 拼接  path.join(路径1，一级路径，二级路径，...)

可以拿到某一个文件路径的完整态

**多一层目录多一层对象 **





#### fs(file system) 模块

用于对系统文件及目录进行读写操作

##### 读

```js
// 引入fs模块
const fs = require('fs');
// 引入path模块
const path = require('path')

// 使用fs的方法（sync同步）
let filepath = path.join(__dirname, 'content.js')

let filecontent = fs.readFileSync(filepath)
console.log(filecontent.toString());//通过toString转换Buffer对象

// 直接转成utf-8
filecontent = fs.readFileSync(filepath, 'utf-8')
console.log(filecontent);
```

- 同步读取：只有等读取完才能执行下面的
- 异步读取

```js
// 异步读取需要三个参数
fs.readFile(filepath, 'utf-8', (error, data) => {
    // 如果读取文件成功，error时null
    if (error) {
        console.log(error);
        return 
    }
    console.log(data);
})
```



##### 写

一般用于临时处理或者存储少量数据

不是叠加而是覆盖

```js
fs.writeFile('文件路径','写入内容','utf-8')


/////////////////////////////////////////////
writeFile('message.txt', data,'utf-8', (err) => {
  if (err) throw err;
  console.log('The file has been saved!');
});
```



##### 常用方法

fs.renameSync(旧文件名，新文件名)



**获取路径下的所有文件名**

fs.readdirSync() 





> str.endsWith()
>
> str.startsWith()
>
> str.substring()





#### http模块

**把数据传到指定端口上，进行页面和服务器的数据连接（数据传到端口，服务器监听端口）**

**搭建第一个的后端程序**

 ip地址+端口

ip地址唯一性

端口唯一性，但是针对于同一台设备

端口号：标识同一台设备上不同的网络进程（可以联网的，运行起来的程序）

0-65535

​		0-1024（知名端口）

端口号被占用了就换一个

**后端：服务器程序**（解析请求）

##### 流程

| 1    | 导入http模块                                   |
| ---- | ---------------------------------------------- |
| 2    | 定义服务程序的端口号                           |
| 3    | 创建服务器对象                                 |
| 4    | 调用服务器的监听方法，让服务器监听浏览器的请求 |

```js
// 导入http模块
const http = require('http');

// 定义端口号
const port = 8080;

//创建服务器对象
const server = http.createServer((request, response) => {
    // 这里的代码每收到一次请求就会执行一次
    //request请求对象，response响应对象
    response.end('hello')//表示响应工作结束，这个方法后面不要再写响应的一些操作
})

//调用服务器的监听方法，让服务器监听浏览器的请求
server.listen(port, (error) => {
    //用来监听有没有出错，如果没有报错undefined
    console.log(error);
    console.log('服务器已经运行成功了');
})
```



> 一个端口号只能一个服务器使用
>
> end永远放在响应最后面，多次调用没有效果



##### 获取请求路径

```js
request.url
```



##### 获取请求方法

```js
request.method
```



##### 获取get请求参数

```js
//需要url模块
let parse = url.parse(请求路径，true)
parse.query //得到参数对象
```

> true表示解析成一个对象，false解析成一个字符串





##### 获取post请求参数

```js
//以事件方式来接收，事件名就是data，触发post请求就会触发这个代码
request.on('data',(postData)=>{
    //postData就是接收的参数
})
```





#### url模块

获取路径，从而获取get请求参数

```js
let parse = url.parse(路径，true)
parse.query //得到参数对象
```

> true表示解析成一个对象，false解析成一个字符串



# web服务器

**把数据传到指定端口上，进行页面和服务器的数据连接（数据传到端口，服务器监听端口）**

**端口是数据的中转站**

文件分级

- assets
- - css
  - html
  - js

请求路径需要判断

> 注意href会使的浏览器自动发送请求，css引入注意

```js
const http = require('http')
const path = require('path');
const fs = require('fs');

const port = 8080;

const server = http.createServer((request, response) => {
    let reqUrl = request.url
    if (reqUrl == '/' || reqUrl == '/index.html') {
        let pathall = path.join(__dirname, 'assets', 'html', 'index.html')
        let content = fs.readFileSync(pathall)
        response.end(content)
    }
    else if (reqUrl == '/center.html') {
        let pathall = path.join(__dirname, 'assets', 'html', 'center.html')
        let content = fs.readFileSync(pathall)
        response.end(content)
    }
    // css引入的href会自动请求需要判断
    else if (reqUrl.endsWith('css') || reqUrl.endsWith('js')) {
        let pathall = path.join(__dirname, 'assets', 'css', 'center.css')
        let content = fs.readFileSync(pathall)
        response.end(content)
    }
    // 找不到页面404
    else {
        response.end('404')
    }
})

server.listen(port, (error) => {
    console.log(error);
    console.log('服务器运行成功');
})
```





# npm

包管理工具

- 新建npm文件夹

```
npm init   -----项目初始化
```



- 生成package.json文件

​		记录项目相关信息



- 安装依赖

```
npm install 包名字
```

node_modules（存放项目依赖）



- 在npm中新建app.js文件

​		引入（require）





# yarn

包管理工具

快速、可靠、安全

```
npm install -g yarn  ------全局安装
```



- 新建yarn文件夹

  ```
  yarn init ------初始化
  ```



- 生成package.json文件



- 安装依赖

```
yarn add 包名字
```



- 在yarn中新建app.js文件

​		引入（require）







# async和await

异步终极方案

async 函数(){

​	let variy = await ...

}

**直接拿到promise对象成功的数据**

> await必须放在async function函数内部
>
> async function（异步函数 ）需要调用，里面的await是异步的，不是同步的
>
> 多了一个await，异步同步化表达，外观同步执行，实质上还是异步的
>
> 执行完第一个异步操作后，再去执行第二个异步操作
>
> async await 一般后面接promise对象

**如果await后面接的是基本数据类型，会对这个基本数据类型进行包装，包装成一个promise对象**

同步也是要顺序执行的同步，异步等待同步结束或回调

### 特点：

- 异步中级方案（简化promise操作）
- 异步操作同步化表达



### 作用：

- 替代then()
- 异步操作同步化表达

- 保证各个异步操作顺序执行，减少回调地狱



### 好处：

- 代码更节省，更加便于阅读及理解
- 提高开发效率



### 错误处理

#### 内部处理

try/catch

可以保证项目代码可以完整运行

```js
async function func(){
    try{
    	let data = await new Promise((resolve,reject)=>{reject("错误")})
	}catch(error){
    	console.log(error);
	}
}

```



#### 外部处理

```js
//接收到promise对象后
promise对象.catch((reason)=>{
	console.log(reason)
})
```









# express

框架（会省略？后参数请求）

搭建服务器程序

### 流程

1. 项目下安装


```
 npm install express --save
 
 
 yarn add express
```




2. 导入express
3. 创建app对象（app项目对象）
4. 监听端口是否有请求
5. 处理请求

```js
//导入express
const express = require("express")

//创建app对象（app项目对象）
const app = express()

//处理请求
//第一个参数是请求路径，第二个参数是针对于这个路径的处理函数
app.get('/',(req,res)=>{
    //req请求对象
    //res响应对象
    res.send("hello")
})

//监听端口是否有请求
app.listen(3000,()=>{
    console.log("express 启动成功")
})

```



### get请求处理

```js
//获取请求参数
req.query
```



### post请求处理

获取参数需要第三方包：**body-parser**

- 引入

- 把body-parser功能添加到项目app中

  ```js
  app.use(bodyParser.urlencoded({extended:false}))//false则接收是字符串或者数组，true则为任意类型
  
  app.use(bodyParser.json())//解析json格式
  ```

- 在接口获取参数

  ```js
  app.post('/register.html',(req,res)=>{
      let body = req.body
      //可以使用解构赋值(即定义的变量结构和body中的一样)
      let { ， ，} = body;
      //获取请求参数进行处理
      
      //查询数据库，判断用户名是否注册
  })
  ```

  

### 重定向

```js
res.redirect(路径)
```



### all()方法

合并同个请求路径不同请求方式

```js
app.all('/register.html',(req,res)=>{
    //获取请求方式
    let method = req.method
    //判断请求方式
    if(method == 'GET'){
        
    }else if(method == 'POST'){
        
    }

})
```







### 使用express获取静态资源

public文件夹---->css,js,html,img

需要：

```js
//获取静态资源
app.use(express.static(public路径))   //指定本地public下找静态资源
```







### 使用express渲染模板页面

**art-template**

直接把页面渲染，不需要在读入写入

- 安装

  ```
  yarn add art-template
  yarn add express-art-template
  
  
  npm install art-template
  npm install express-art-template
  ```

- 使用

  ```js
  var express = require('express');
  const path = require('path');
  var app = express();
  
  // view engine setup
  //模板引擎初始化的工作
  app.engine('html', require('express-art-template'));
  
  //项目环境的设置,出问题view改成view options
  //production生产模式，线上模式
  //development 开发模式
  app.set('view options', {
      debug: process.env.NODE_ENV !== 'production'
  });
  
  //设置在哪一个目录下查找html文件（当前目录文件夹中的views文件夹下）
  app.set('views', path.join(__dirname, 'views'));
  
  //设置模板后缀名
  app.set('view engine', 'html');
  
  // routes
  app.get('/', function (req, res) {
      res.render('index.html',{
          user: {
              name: 'aui',
              tags: ['art', 'template', 'nodejs']
          }
      });
  });
  ```

  

  在html中也可判断

  ```html
  {{if user}}
    <h2>{{user.name}}</h2>
  {{/if}}
  ```

[语法介绍](http://aui.github.io/art-template/docs/syntax.html)







### 路由（服务器的）

访问服务器，获取数据的方式

新建routes文件夹，创建文件

抽取路由

#### 流程

```js
////passport.js////

//第一步引入express
const express = require('express');

// 创建路由对象
const routerPort = express.Router();

// 放在路由对象上
routerPort.get('/index.html', (req, res) => {

})


// 导出路由模块
module.exports = routerPort





////有app的文件////
//引入
const routerPort = require('./routes/passport')

//将路由对象注册到app对象
app.use(routerPort)
```











### pathinfo参数

```js
'/index/01/play'

接收：app.get('/index/:id/:type',(req,res)=>{
    //参数在params中
    req.params
})

```







### 模板过滤器

对后台传递的数据进行处理筛选过滤

```js
template.defaults.imports.dateFormat = function(date, format){/*[code..]*/};
template.defaults.imports.timestamp = function(value){return value * 1000};


//看的是return的值
```



使用

```
{{value | filter}}
```







### 模板继承使用

```js
{{extend '父类路径'}}
{{block 'head'}} 可替换的内容 {{/block}}   //相当于占位符，通过变量head寻找
```







# 数据库







# 服务器连接数据库

流程:

- 安装数据库

  ```
  yarn add mysql
  
  npm install mysql
  ```

- 引入

  ```js
  const mysql = require('mysql');
  ```

- 数据库连接基本配置

  ```js
  var pool = mysql.createPool({
      host:'localhost',
      user:'root',
      password:'123456',
      database:'数据库名字'
  })
  ```

- 对数据库进行增删改查操作

  ```js
  function query(sql,callback){
      //getConnection连接，pool连接池
      pool.getConnection(function(err,connection){
          connection.query(sql,function(err , rows){
              //错误，还有一个是记录，也就是查询到的记录
              callback(err,rows)
              connection.release()
      	})
  	})
  
  ```

- 导出

  ```js
  exports.query = query;
  ```

- 引入

  ```js
  const db = require('./db/db')
  ```

- 使用

  ```js
  db.query('sql语句',(err,data)=>{
      if(err){
          console.log(err);
          return;
      }
      console.log(data);
  })
  ```

  



### ORM

Object Relation Mapping(对象关系映射)

记录：对象

字段：属性

可以把数据库表每条记录映射成一个模型对象 

#### 查询

```js
app.get('/', (req, res) => {
    // 使用ORM查询数据库
    // 创建模型，需要操作哪一个数据库表
    let user = db.model('user');
    // 通过模型做一系列操作

    //查询
     user.find(['username', 'age'], (err, data) => {
         res.send(data)
     })
    // 条件查询
     user.find('id=4', (err, data) => {
         res.send(data)
     })
    // 分页查询
     user.limit({ where: '', number: 3, count: 5 }, (err, data) => {
         res.send(data)
     })
})
```





#### 增加

```js
// 增加单条记录
    user.insert({ username: '士大夫', password: '111', real_name: '四大', gender: '男', address: '浙江', phone: '121255', age: 20 }, (err, result) => {
        console.log(err);
        res.send(result);
    })
    // 增加多条记录
    user.insert([{ username: '士大夫', password: '111', real_name: '四大', gender: '男', address: '浙江', phone: '121255', age: 20 }, { username: '士夫', password: '111', real_name: '四', gender: '男', address: '浙江', phone: '11255', age: 22 }], (err, result) => {
        console.log(result);
        res.send('添加成功');
    })

```





#### 删除

```js
// 按条件删除
    user.delete('id=21', (err, result) => {
        console.log(result);
        res.send("删除成功");
    })
    // 清空表里所有的内容(慎用)
    user.delete((err, result) => {
        console.log(result);
        res.send("删除成功");
    })
```







#### 修改

```js
    // 修改全部数据
    user.update({age:18},(err,result)=>{
        console.log(result);
        res.send('修改成功');
    })
    // 按条件修改
    user.update('id = 4',{age:18},(err,result)=>{
        console.log(result);
        res.send('更新成功');
    })
```







#### sql

```js
 // 基本增删改查满足不了基本需求，可以直接使用sql语句
    user.sql('select group_concat(age),count(*) c from user group by gender', (err, result) => {
        res.send(result[0])
    })
```



### 回调地狱

回调函数嵌套回调函数

#### 使用async await解决



```js
// async await 函数自调用
    (async function getData() {
          let user = db.model('user');
        // 如果查询成功result接收数据库返回的数据data
        // 如果查询失败result接收错误err
        // try/catch捕获异常保障程序不会崩掉
        let result;
        try{
            result = await new Promise((resolve, reject) => {
                    user.sql('select * from user', (err, data) => {
                        if (err) {
                            reject(err)
                            return
                        }
                        resolve(data)
                    })
               
            })
        }catch(err){
            console.log(err);
            // 将错误打印给前端，errmsg->错误信息
            res.send({errmsg:'数据库查询出错'})
            return
        }
        
        //第二步 
        let result2 = ...

 
        res.send(result)
    })();

    // 或手动调用
    // getData();
```





#### 进行封装

handleDB.js

```js
const db = require('./nodejs-orm/index');
/**
 * @description 数据库操作
 * @param {*} res ：响应对象
 * @param {String} tableName ：操作表名
 * @param {String} methodName : 进行操作
 * @param {String} errmsg : 报错原因
 * @param {*} n1 : 参数1 条件/对象
 * @param {*} n2 ：参数2 对象
 * @returns 
 */

async function handleBD(res, tableName, methodName, errmsg, n1, n2) {

    let user = db.model(tableName);
    // 如果查询成功result接收数据库返回的数据data
    // 如果查询失败result接收错误err
    // try/catch捕获异常保障程序不会崩掉
    let result;
    try {
        result = await new Promise((resolve, reject) => {
            // 一个参数都没有
            if (!n1) {
                user[methodName]((err, data) => {
                    if (err) {
                        reject(err)
                        return
                    }
                    resolve(data)
                })
                return
            }
            // 传了一个参数（n1）
            if (!n2) {
                user[methodName](n1, (err, data) => {
                    if (err) {
                        reject(err)
                        return
                    }
                    resolve(data)
                })
                return
            }
            // 传两个参数
            user[methodName](n1, n2, (err, data) => {
                if (err) {
                    reject(err)
                    return
                }
                resolve(data)
            })

        })
    } catch (err) {
        console.log(err);
        // 将错误打印给前端，errmsg->错误信息
       res.send({
            errmsg: errmsg
        })
        return
    }
    return result
}

module.exports = handleBD

```

#### 调用

```js
 // async await 函数自调用
    (async function getData() {
        let result = await handleDB(res, 'user', 'update', '数据库更新出错', 'id=4', { username: '李四', age: '18' })
        let result01 = await handleDB(res, 'user', 'sql', 'sql语句出错', 'select * from user')
        res.send(result01)
    })();

    // 或手动调用
    // getData();
```











# Ajax

**异步请求，局部刷新**

 ### 步骤

1. 创建AJAX对象
2. 设置请求路径，请求方式
3. 绑定监听状态改变的处理函数，在处理函数可获取响应数据
4. 发送请求

### 创建AJAX对象

```js
function createAjax(){
    var ajax;
    ajax = new XMLHttpRequest();
    return ajax;
}
```





### 响应处理和响应流程

响应处理，即对服务响应回浏览器的数据根据状态码和AJAX对象的状态信息进行不同的处理，在绑定状态改变的处理函数中写出对应的逻辑代码即可。

AJAX对象有四个属性：

- readyState总共有5个状态值，分别为0~4.每个值代表了不同的含义：onreadyStatechange
  - 0：初始化，AJAX对象还没有完成初始化
  - 1：载入，AJAX对象开始发送请求
  - 2：载入完成，AJAX对象的请求发送完成
  - 3：解析，AJAX对象开始读取服务器的响应
  - 4：完成，AJAX对象读取服务器的响应结束
- status表示响应的HTTP状态码，常见状态码如下：
  - 200：成功
  - 302：重定向
  - 404：找不到资源
  - 500：服务端错误
- responseText获得字符串形式的响应数据
- responseXML获得XML形式的响应数据

在状态改变的处理函数一般在针对**readyState == 4**且**status == 200** 的情况才处理，再根据后台返回的数据类型决定从responseText或者responseXML获取服务器响应回来的数据。

![AJAX请求流程图](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/node/nodeAJAX%E8%AF%B7%E6%B1%82%E6%B5%81%E7%A8%8B%E5%9B%BE.png)







### 使用AJAX发送get请求

```js
 // 发送AJAX请求
            // 1.创建AJAX对象
            let ajax = new XMLHttpRequest();
            // 2.设置请求路径和方式
            ajax.open('GET', '/get_data');
            // 3.绑定监听状态改变的处理函数
            ajax.onreadystatechange = () => {
                // 获取相应回来的数据
                if (ajax.readyState === 4 && ajax.status === 200) {
                    console.log(JSON.parse(ajax.responseText));
                    // 进行渲染

                }
            }
            // 4.发送请求
            ajax.send()
```



 

### 使用AJAX发送post请求

```js
let username = document.querySelector('#username').value;
let password = document.querySelector('#password').value;
let params = { username, password }
let ajax = new XMLHttpRequest();
ajax.open('post', '/login_post');
ajax.onreadystatechange = () => {
}
// 转换成json格式
ajax.send(JSON.stringify(params));
```







# CSRF防护

### 生成cookies

```
yarn add cookie-parser
yarn add cookie-session


npm install cookie-parser
npm install cookie-session
```



#### 流程

- 引入（req)

  ```js
  const cookieSession = require('cookie-session');
  const cookieParser = require('cookie-parser');
  ```

- 注册

  ```js
  app.use(cookieParser())
  app.use(cookieSession({
      name:'my_session',
      keys:['sdf#4$#sds%sddasds$$%$$%sDDs'],
      maxAge:1000*60*60*24*2 //设置过期时间为两天
  }))
  ```

- 生成随机字符串

  ```js
  // 生成随机字符串
  function getRandomString(n) {
      var str = '';
      while (str.length < n) {
          str += Math.random().toString(36).substring(2);
      }
      return str.substring(str.length - 1)
  }
  ```

- 使用

  ```js
  app.get('/login', (req, res) => {
      // 设置token，将token保存在cookie中
      let csrf_token = getRandomString(48);
      res.cookie('csrf_token',csrf_token)
      res.render('login')
  })
  ```

- 在请求中请求头也要使用

  ```js
  headers:{'X-CSRFToken':getCookie('csrf_token')}
  
  //获取cookie函数
  function getCookie(name) {
      var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
      return r ? r[1] : undefined;
  }
  ```

- 请求为post情况判断

  ```js
  //判断响应头中CSRFToken的值，和cookies中的cookie进行对比
  if(req.method == "POST"){
      req.headers['x-csrftoken'];
      req.cookies['csrf_token'];
    //进行对比，如果一致说明合法
    //不一致，请求伪造，直接return
  }
  ```

  





# 抽取封装

### 使用函数进行封装

```js
let appConfig = (app) => {
    app.use(express.static(path.join(__dirname, 'public')))
    app.use(bodyParser.urlencoded({ extended: false }))
    app.use(bodyParser.json())//解析json格式
    app.use(cookieParser())
    app.use(cookieSession({
        name: 'my_session',
        keys: ['dsas@##@$#%$^^%^%DFvscdwqdq'],
        maxAge: 1000 * 60 * 60 * 24 * 2
    }))

    // view engine setup
    //模板引擎初始化的工作
    app.engine('html', require('express-art-template'));
    //项目环境的设置,出问题view改成view options
    //production生产模式，线上模式
    //development 开发模式
    app.set('view options', {
        debug: process.env.NODE_ENV !== 'production'
    });
    //设置在哪一个目录下查找html文件（当前目录文件夹中的views文件夹下）
    app.set('views', path.join(__dirname, 'views'));
    //设置模板后缀名
    app.set('view engine', 'html');
}

```







### 使用面向对象

```js
class AppConfig{
    //对象一new就会执行constructor
    constructor(app){
        this.app = app;
        this.app.use(express.static(path.join(__dirname, 'public')))
    	this.this.this.this.this.app.use(bodyParser.urlencoded({ extended: false }))
    	this.this.this.this.app.use(bodyParser.json())//解析json格式
    	this.this.this.app.use(cookieParser())
    	this.this.app.use(cookieSession({
       		name: 'my_session',
        	keys: ['dsas@##@$#%$^^%^%DFvscdwqdq'],
        	maxAge: 1000 * 60 * 60 * 24 * 2
    	}))

    	// view engine setup
    	//模板引擎初始化的工作
    	this.app.engine('html', require('express-art-template'));
    	//项目环境的设置,出问题view改成view options
    	//production生产模式，线上模式
    	//development 开发模式
    	this.app.set('view options', {
        	debug: process.env.NODE_ENV !== 'production'
    	});
    	//设置在哪一个目录下查找html文件（当前目录文件夹中的views文件夹下）
    	this.this.app.set('views', path.join(__dirname, 'views'));
    	//设置模板后缀名
    	this.app.set('view engine', 'html');
    }
}
```









# 项目流程

### 初始化(init)

### express框架

### 模板引擎

### public文件夹

### 静态资源使用

### 请求参数配置

### session和cookie

```js
router.get('/set_cookie', (req, res) => {
    res.cookie('name', 'nodejs');
    req.session['age'] = 20
    res.send('设置了cookie和session');
})

router.get('/get_cookie', (req, res) => {
    let name = req.cookies['name'];
    let age = req.session['age'];
    res.send(`获取的cookie值为${name},session值为${age}`)
})
```

### 抽取封装(配置文件config+路由routes)

### 数据库mysql

### AJAX

```JS
ajax({
            url:'/passport/register',
            type:'post',
            data:JSON.stringify(params),
            contentType:'application/json',
            //headers:{'X-CSRFToken':getCookie('csrf_token')},
            success: function (resp) {
                console.log("回调成功了");

                //判断是否注册成功
                if(resp.errno == '0'){
                    //重新加载当前页面
                    alert(resp.errmsg);
                    window.location.reload()
                }else{
                    alert(resp.errmsg);
                }
            }
        })



//获取cookie函数
function getCookie(name) {
    var r = document.cookie.match("\\b" + name + "=([^;]*)\\b");
    return r ? r[1] : undefined;
}
```





### 注册功能

```js
// 注册
router.post('/passport/register', (req, res) => {
    (async function () {
        // 1.获取参数，判空
        let body = req.body
        let { username, image_code, password } = body
        if (!username || !image_code || !password) {
            res.send({ errmsg: '缺少参数' })
            return
        }
        // 2.验证图片验证码是否正确，不正确直接return
        if (image_code.toLowerCase() !== req.session['IMG_CODE'].toLowerCase()) {
            res.send({ errmsg: '验证码错误' })
            return
        }
        // 3.查询数据库用户名是否存在
        // 查询到，result->数组[{字段...}]
        // 没有，result->空数组[]
        let result = await handleDB(res, 'info_user', 'find', '数据库查询出错', `username = "${username}"`)
        // 4.如果已经存在，返回用户名已经存在，进行return
        if (result.length > 0) {
            res.send({ errmsg: '用户名已注册' })
            return
        }
        // 5.如果不存在，向数据库中添加一条记录
        let result2 = await handleDB(res, 'info_user', 'insert', '数据更新出错', { username, nick_name: username, password_hash: password })
        // 6.保持登陆状态,insertId
        console.log(result2);
        req.session['user_id'] = result2.insertId
        // 7.返回响应成功
        res.send({ errno: 0, errmsg: '注册成功' })
    })();

})
```











### 项目添加CSRF防护

在utils下新建common.js(公共的工具函数)目录

```js
function getRandomString(n){
    var str = '';
    while (str.length < n) {
        str += Math.random().toString(36).substring(2);
    }
    return str.substring(str.length - 1)
}
//csrfProtect钩子函数，执行某一个函数之前和之后可能会执行其他指定的函数
function csrfProtect(req,res,next){
    //console.log("-------------------------------------csrfProject")
    let method = req.method
    if(method == "GET"){
        let csrf_token = getRandomString(48)
        res.cookie('csrf_token',csrf_token)
        next()
    }else if(method =="POST"){
        console.log(req.headers["x-csrftoken"])
        console.log(req.cookies["csrf_token"])
        
        if(req.headers["x-csrftoken"] === req.cookies["csrf_token"]){
            console.log("csrf验证通过！");
            next()
        }else{
            res.send({errmsg:"crsf验证不通过！"})
            return
        }
    }
    
}

module.exports = {
    csrfProtect
}
```

在配置文件中引入

```js
req

//get
app.use(common.csrfProtect,indexRouter)

//钩子函数在登录注册(post)防护
app.use(common.csrfProtect,passportRouter)
```



### 密码加密

- 不可逆算法：单项散列函数（常见md5和sha256），只能加密，不可解密，加密之后得到的密文长度是一定的，明文相同，密文就相同，明文不相同，密文也不相同
- 可逆加密
  - 对称加密![image-20221213131156321](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/node/nodeimage-20221213131156321.png)
  - 非对称加密![image-20221213131543677](https://raw.githubusercontent.com/mjjjh/NoteImage/main/image/node/nodeimage-20221213131543677.png)

md5使用（**每次相同明文使用，密文相同**）

- 引入（req）

- 双重md5加盐

  ```js
  //#@$@#dfdf#%@#$%^Fg7^%f为盐  
  let ret = md5(md5(password)+'#@$@#dfdf#%@#$%^Fg7^%f')
  ```

- 抽取出keys

  ```js
  //keys.js
  module.exports={
      session_keys : '',
      password_salt : ''
  }
  ```

  

 







# 小程序

### 跨域问题

```js
// 中间件 解决跨域问题
function middleware(req, res, next) {
    console.log('这里是中间件')
    // 跨域处理
    res.header("Access-Control-Allow-Origin", "*");//设置允许跨域的域名，*代表允许任意域名跨域
    res.header("Access-Control-Allow-Headers", "X-Requested-With");//允许的header类型
    res.header("Access-Control-Allow-Methods", "PUT,POST,GET,DELETE,OPTIONS");//跨域允许的请求方式
    res.header("X-Powered-By", ' 3.2.1');
    res.header("Content-Type", "application/json;charset=utf-8");
    next(); // 执行下一个路由
}



//all是路由中指代所有的请求方式
    app.all(middleware) //使用全局自定义中间件，放到起始位置



//不行则加
const cors = require('cors');
//将cors挂载
app.use(cors())
```



### post

```js
uni.request({
	header: {
		'content-type': 'application/x-www-form-urlencoded' //后端接收的是（表单）字符串类型，例如'id=1231454&sex=男'
	},
	url: 'http://localhost:3000/register',
	method: 'POST',
	data: {
		testValue: this.testValue
	},

	dataType: 'json',
	success() {
		console.log('成功');
	}
})
```





### get

```js
uni.request({
	url: 'http://localhost:3000/data', //
    header: {
		'content-type': 'application/x-www-form-urlencoded' //后端接收的是（表单）字符串类型，例如'id=1231454&sex=男'
	},
    method: 'GET',
	success: (res) => {
		console.log(res.data);
		this.title = 'request success';
	}
});
```





































# 模块

### md5 加密 

[MD5破解](https://www.cmd5.com)



### 图片验证码

```
npm install svg-captcha
```

使用说明

```js
let captchaobj = new Captcha();
let captcha = captchaobj.getCode();//调用获取验证码的方法

captcha.text //就是验证码的文本
captcha.data //就是验证码的图片内容

//注意：配合img标签返回给浏览器需要设置响应头
res.setHeader('Content-Type','image/svg+xml');
```



需要保存到session中

```js
const express = require('express');
const router = express.Router();
const Captcha = require('../utils/captcha/index')

router.get('/passport/image_code/:float', (req, res) => {
    let captchaObj = new Captcha();
    let captcha = captchaObj.getCode();
    //存
    req.session['IMG_CODE'] = captcha.text;
    res.setHeader('Content-Type', 'image/svg+xml');
    res.send(captcha.data)
})

```





