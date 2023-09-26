# 安装

```
npm install -g pm2
```



# 启动

```
pm2 start filename
```



### 实时监听启动 

```
pm2 start filename --watch
```

文件改变自动启动





# 查看日志

```
pm2 log
```



# 监控

```
pm2 monit
```



# 停止

```
pm2 stop id/filename
```



# 删除

```
pm2 delete id/filename
```



# 重启

```
pm2 restart id/filename
```





# ts文件

要使用PM2与TypeScript一起使用，您需要将TypeScript文件转换为JavaScript文件。然后，您可以使用PM2运行生成的JavaScript文件。

以下是建议的步骤：

1. 安装所需的依赖项：

```
bashCopy Codenpm install pm2 typescript ts-node @types/node --save-dev
```

1. 在项目根目录下创建一个 `tsconfig.json` 文件，内容如下：

```
jsonCopy Code{
  "compilerOptions": {
    "module": "commonjs",
    "esModuleInterop": true,
    "target": "es2017",
    "outDir": "dist",
    "baseUrl": ".",
    "paths": {
      "*": ["node_modules/*", "src/types/*"]
    }
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.test.ts"]
}
```

1. 创建一个名为 `src/index.ts` 的文件，并添加一些示例代码：

```
typescriptCopy Codeconsole.log('Hello world!');
```

1. 在 `package.json` 文件中添加以下脚本：

```
jsonCopy Code"scripts": {
  "build": "tsc",
  "start": "pm2 start dist/index.js --name my-app",
  "stop": "pm2 stop my-app"
}
```

1. 运行 `npm run build` 命令将 TypeScript 文件转换为 JavaScript 文件。
2. 运行 `npm run start` 命令以使用 PM2 启动应用程序。

希望这有所帮助！如果您有任何进一步的问题，请告诉我。







# HTTP :80

# HTTPS :443

# SSH :22



