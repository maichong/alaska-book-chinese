# 初始化项目

一个Alaska项目就是一个普通的NPM包。这个NPM包中的内容即是主Service的定义。

## 环境

Alaska需要node v5.0+ 和NPM v3+ 环境。你可以运行以下命令检测你的当前环境：

```sh
node -v
npm -v
```

## 初始化项目

我们使用 `alaska init`命令完成初始化项目，在此之前，我们需要在我们的系统中全局安装alaska-cli包。

```
npm i -g alaska-cli
```

然后建立alaska-demo目录，并在其中执行初始化命令，然后按步骤、按提示输入参数：

```sh
mkdir alaska-demo #Windows系统请使用md命令代替mkdir
cd alaska-demo
alaska init
```

> 如果改目录尚未初始化为npm项目（没有找到package.json）alaska会首先调用 `npm init` 。

## 项目目录

初始化后，项目的目录结构如下：

```js
alaska-demo
├── api/                 //api控制器
├── config/              //项目配置文件目录
│   ├── alaska-admin.js  //alaska-service配置
│   └── alaska-demo.js          //主Service配置
├── controllers/         //控制器
├── locales/             //本地化语言设置
├── middlewares/         //中间件
├── models/              //数据模型
├── node_modules/
├── public/              //静态文件
├── runtime/             //临时文件
├── sleds/               //Sled目录
├── templates/           //swig模板目录
├── updates/             //自动更新脚本，供alaska-update启动时执行
├── views/               //Admin后台扩展视图控件
├── alaska-demo.js       //项目入口文件，需要babel-node环境
├── index.js             //项目入口文件，已加载babel-register，可直接执行
├── package.json
├── webpack.admin.dev.js //Admin后台控件webpack配置，development模式
└── webpack.admin.pro.js //Admin后台控件webpack配置，production模式
```

## 编译后台

> alaska-admin Service提供一个基于React的可扩展管理平台。在默认初始化配置中，我们的项目已经自动依赖alaska-admin。

在这个新项目中，我们需要首先编译一下后台，运行：

```sh
alaska build
```

> `alaska build` 命令主要执行两个任务，首先会扫描项目，记录下所有需要的React控件，然后调用 `webpack --config webpack.admin.pro.js` 将这些控件打包生成后台所需要的 `app.min.js` 。

> 当安装了新的后台扩展控件后，需要重新运行 `alaska build`。

> 如果node运行环境是开发模式（NODE_ENV=development）则需要执行 `webpack --config webpack.admin.dev.js` 编译开发模式需要的 `app.js`。

## 启动

初始化后，执行启动命令：

```sh
node index.js
```

然后在浏览器中打开 `http://localhost:5000` 查看效果。

> 服务端口号依据初始化时设置而定，或者在 `config/alaska-demo.js` 配置文件中修改 `port` 参数。

## 登录管理平台

alaska-admin 默认的挂载点在 `/admin`，打开 `http://localhost:5000/admin/` 即可进入管理平台登录界面。 登录账号和密码依据初始化时的设置。

> 我们建议您修改 alaska-admin 的默认挂载点以利于安全，编辑 `config/alaska-admin`，修改 `prefix` 参数，注意 `prefix` 参数必须以 `/` 开头，并不能以 `/` 结尾。

> 如果管理平台一直显示 loading 界面，则有可能是因为您没有成功执行 `alaska build`。

## 示例代码

在 `https://github.com/maichong/alaska-demo/tree/init` 可以查看到本节的示例代码，或则按照下列步骤克隆示例代码到本地：

```
git clone https://github.com/maichong/alaska-demo.git
cd alaska-demo
git checkout init
```
