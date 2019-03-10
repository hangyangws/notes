GOTO：https://www.valentinog.com/blog/react-webpack-babel/

保证 npm 版本大于 5

1. 新建文件夹
1. npm init
1. npm i 相关依赖模块
  react
  react-dom
  prop-types
1. npm i -D 相关开发依赖模块
  webpack
  webpack-cli // Since version 4 that is no longer the case: you can start developing straigh away.

  webpack-dev-server // 启动服务器
  style-loader
  css-loader

  babel-loader
  babel-core
  babel-preset-env // ES6 code down to ES5，babel-preset-es2015 is now deprecated
  babel-preset-react // compiling JSX

  html-webpack-plugin // 根据模板生成 html 文件

1. 新建 .babelrc
  {
    "presets": ["env", "react"]
  }

1. 新建 webpack.config.js

  module.exports = {
    module: {
      rules: [{
        test: /\.js$/,
        exclude: /node_modules/,
        use: ['babel-loader']
      }, {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      }]
    }
  };

1. 新建 src/app && src/index.js
1. 新建 src/app/index.js
