# 控制器

## 新建article控制器

在 `controllers` 目录下建立 `article.js` 文件。

```js
import Article from '../models/Article';

export default async function (ctx) {
  let articles = await Article.paginate({
    page: ctx.query.page || 1,
    perPage: 10
  });
  await ctx.show('article/list', { articles });
}

```

首先导入Article模型类供下文使用。

在导出的默认方法就是一个普通的Koa中间件。

在中间件中占有两条语句，首先调用 `Article.paginate` 查询文章列表，然后调用 `ctx.show` 方法渲染输出模板。

> `Article.paginate` 方法是Alaska模型中封装的分页查询方法
> 
> `ctx.show` 是Alaska中扩展的方法，作用是渲染swig模板，并将结果返回至`ctx.body`


## 新建模板

在 `templates` 目录下建立 `article/list.swig` 文件。

```twig
<ul>
  {% for article in articles.results %}
    <li>{{ article.title }}</li>
  {% endfor %}
</ul>

Total:{{ articles.total }}
```

> 更多swig语法请查阅 [Swig](http://paularmstrong.github.io/swig/docs/)

重启后查看 `http://localhost:5000/article`

## 示例代码

在 `https://github.com/maichong/alaska-demo/tree/controller` 可以查看到本节的示例代码，或则按照下列步骤克隆示例代码到本地：

```
git clone https://github.com/maichong/alaska-demo.git
cd alaska-demo
git checkout controller
```
