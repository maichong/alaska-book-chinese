# 中间件

除下控制器和API，在Alaska中还有两种定义中间件的方式：App中间件和Service中间件。

## App中间件

App中间件是直接调用Koa app对象定义中间件。如：

```js
alaska.app.use(async function(ctx,next){
  await next();
});
```

>请在alaska.listen()之前调用上方代码，否者将有可能无效，具体请看 [启动步骤](../advanced/startup.md)

或者在主Service的配置文件中定义 `appMiddlewares` 选项，以配置的方式加载中间件。

## Service中间件

Service中间件是指该中间件只在Service的挂载点起作用，并且使用的是[koa-router](https://github.com/alexmingoia/koa-router/tree/master/)。

定义方法是在 `middlewares` 目录下的 `index.js` 文件中导出一个默认方法，这个默认函数在系统启动时会被调用，并且会将 `koa-router` 实例传入作为参数。例如：

```js
export default function (router) {
  router.use(async function (ctx, next) {
    let start = Date.now();
    await next();
    console.log('Elapsed time %s ms.', Date.now() - start);
  });
}

```
> 如上所示，新建项目中默认已经定义了一个中间件用来记录请求的耗时。

然后我们修改代码加入一个新的中间件，用来显示404页面：

```js
export default function (router) {
  router.use(async function (ctx, next) {
    let start = Date.now();
    await next();
    console.log('Elapsed time %s ms.', Date.now() - start);
  });

  //404
  router.use(async function (ctx, next) {
    await next();
    if (ctx.status === 404 && !ctx.body) {
      await ctx.show('404');
    }
  });
}

```

然后我们建立404页面模板文件 `templates/404.swig`;

```html
<!doctype html>
<html lang="en">
<head>
  <title>404</title>
</head>
<body>
<style>
  h1 {text-align: center;margin: 100px 0;font-size: 24px;color: #333;}
</style>
<h1>404 Not Found</h1>
</body>
</html>

```

这样，当有无效请求时，就会显示默认的404页面。

此外还可以修改Service的配置文件，在 `middlewares` 数组中加入中间件的配置。

也可以在程序启动时候调用 `service.router` ：

```js
service.router.use(async function(ctx,next){
	await next();
});
```


## 示例代码

在 `https://github.com/maichong/alaska-demo/tree/middleware` 可以查看到本节的示例代码，或则按照下列步骤克隆示例代码到本地：

```
git clone https://github.com/maichong/alaska-demo.git
cd alaska-demo
git checkout middleware
```
