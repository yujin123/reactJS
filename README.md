## 项目运行过程

```
//克隆项目
git clone https://github.com/yujin123/reactJS.git

//编译
npm run build

//运行
npm run dev

//在浏览器输入
http://localhost:8080
```

## 项目的搭建过程

### 1、创建项目
```
//创建文件夹
mkdir projectName

//进入项目目录
cd projectName

//初始化化项目
npm init
```
### 2、项目结构
```
--projectName
  |--components（组件目录）
    |--Hello（组件1）
      |--imgs
        |--bg.png
      |--index.jsx
      |--index.less
    |--World（组件2）
      |--index.jsx
      |--index.less
  |--app
      |--main.js（入口文件）
  |--build（输出目录，项目目录）
    |--index.html
    |--build.js（输出文件，由 webpack 打包后生成的）
  |--package.json
  |--webpack.config.js
```

### 3、加入项目依赖
#### (1)项目依赖的第三方
```
npm install react react-dom --save
```

#### (2)开发需要的第三方
```
npm install babel-core babel-loader babel-preset-es2015 babel-preset-react --save-dev

npm install less-loader less css-loader style-loader --save-dev 

npm install url --save-dev

npm install webpack webpack-dev-server --save-dev
```

### 4、在package.json的scripts里添加如下内容：
```
"build": "webpack", //构建，生成build.js文件
"dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build" //启动服务器
```
### 5、实现自动刷新
前面实现了对文件修改的监听和自动打包，但浏览器还需要手动刷新。其实可以在 Webpack 的配置文件中增加一个入口点，实现自动刷新。

```
entry: [
  'webpack/hot/dev-server',
  'webpack-dev-server/client?http://localhost:8080',
  APP_PATH
],
```

## 附录：
### (1)package.json
```
{
  "name": "demo01_webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev": "webpack-dev-server --devtool eval --progress --colors --hot --content-base build"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "react": "^15.0.2",
    "react-dom": "^15.0.2"
  },
  "devDependencies": {
    "babel-core": "^6.8.0",
    "babel-loader": "^6.2.4",
    "babel-preset-es2015": "^6.6.0",
    "babel-preset-react": "^6.5.0",
    "less": "^2.7.1",
    "css-loader": "^0.23.1",
    "less-loader": "^2.2.3",
    "style-loader": "^0.13.1",
    "url-loader": "^0.5.7",
    "webpack": "^1.13.0",
    "webpack-dev-server": "^1.14.1"
  }
}
```
### (2)webpack.config.js
```
var path = require('path');
var webpack = require('webpack');


var ROOT_PATH = path.resolve(__dirname);
var APP_PATH = path.resolve(__dirname, './app/main.js');
var BUILD_PATH = path.resolve(__dirname, './build');

module.exports = {
  entry: [
    'webpack/hot/dev-server',
    'webpack-dev-server/client?http://localhost:8080',
     APP_PATH
  ],  
  output: {
    path: BUILD_PATH,
    filename: 'build.js'
  },
  module: {
    loaders: [{
      test: /\.jsx?$/,
      loaders: ['babel-loader?presets[]=es2015,presets[]=react']
    },{
      test: /\.less$/,
      loader: 'style!css!less'
    },{
        test: /\.(png|jpg)$/,
        loader: 'url?limit=50000'
      }]
  }
}
```

