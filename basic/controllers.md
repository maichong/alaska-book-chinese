# 控制器

控制器方法的HTTP访问点为 `PREFIX/CONTROLLER/ACTION` 来访问的,如果是主Service，则省略Service名称，例如：``http://localhost:8888//login``。这个就是省略掉了主Service和Action名称。

## 扩展控制器
我们可以通过配置来扩展控制器，例如，新建``config/alaska-user/controllers/sayHello.js``文件：
```js
export default async function (ctx) {
  ctx.body = 'hello world!';
}
```
或者直接在主目录下新建``controllers/sayHello.js``文件：
```js
export default async function (ctx) {
  ctx.body = 'hello world!';
}
```
