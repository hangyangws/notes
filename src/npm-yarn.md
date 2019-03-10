# npm yarn 相关

### npm 包推荐

- [cnpm](https://npm.taobao.org/)

如果你是国内用户，并且不方便翻墙，建议使用 [cnpm](https://npm.taobao.org/) 作为 npm 的备用品
安装 cnpm：`sudo npm install cnpm -g --registry=https://registry.npm.taobao.org`  
安装完成后，下面的所以命令，都可以把 npm 换成 cnpm

- [n](https://github.com/tj/n) && [nvm](https://github.com/creationix/nvm)

[n](https://github.com/tj/n) 可以用来管理 Node.js 版本

- [nrm](https://www.npmjs.com/package/nrm)

[nrm](https://www.npmjs.com/package/nrm) 可以用来管理 npm 镜像源头

### `sudo npm list --depth=0`

显示 npm 装包列表，depth 表示查看依赖的深度，可以加上 `-g` 参数查看全局安装列表

### `sudo npm uninstall -g  moudleName`

卸载某个 node 全局模块

### `sudo npm update -g moudleName`

更新某个 node 全局模块

### `sudo npm install npm@4.4.4 -g`

升级 npm 到固定版本号  
如果你已经安装某个版本，想替换版本号，也可以使用这个命令

### `npm outdated`

这个命令很不错哦，可以查看那些包可以更新，同样添加 `-g` 参数可以查看全局的

### `npm install --only=production`

只安装 dependencies 里面列出的模块

### `npm init -y` OR `npm init -f`

如果你执行 `npm init` 的时候被它的每一步都需要你确认而困扰的话，那么 `npm init -y` 可以快速生成 `package.json` 文件

### 发布版本

- 升级补丁版本号：`npm version patch`
- 升级小版本号：`npm version minor`
- 升级大版本号：`npm version major`
- 升级到版本号，包括升级信息，然后发布：

```sh
npm version 0.3.2 -m "…"
npm publish
```

# yarn

如果你既不喜欢 npm 的安装速度，也不想使用 cnpm ，那么我建议你使用 [yarn](https://yarnpkg.com/zh-Hans/)  

### `yarn upgrade-interactive`

用来选择更新相应的库  
查看那些库需要更新，用空格勾选你需要更新的库，回车确认

### 安装

npm isntall --dev || yarn add -D => devDependencies
npm isntall => dependencies

[link](https://yarnpkg.com/zh-Hans/docs/package-json#peerdependencies-a-classtoc-idtoc-peerdependencies-hreftoc-peerdependenciesa)

### 简写

`npm install pkg` => `npm i pkg`  
`npm i --global pkg` => `npm i -g pkg`  
`npm i --save pkg` => `npm i -S pkg`  
`npm i --save-dev pkg` => `npm i -D pkg`  
`npm test` => `npm t`

### 枚举可用的脚本

`npm run` => 得到一个的所有可用的脚本的列表
