# 安装Service

Service是Alaska最重要的概念和最方便的特性。我们已经将很多业务封装成了各种Service，所以使用Alaska构建项目，除了自己构建业务代码外，更方便的是使用已经封装好的Service组件。

例如我们需要一个CMS，我们可以安装专门用于CMS的原型组件 `alaska-post`，然后在其基础上根据业务需要做一些调整设置。

## 安装 alaska-post

安装 alaska-post Service只需要运行一条命令即可：

```
alaska install alaska-post:post
```

我们调用 `alaska install` 命令为当前项目安装了一个新的Service扩展，命令中的 `alaska-post:post` 意义为：安装id为`alaska-post`的Service，并为其设置别名`post`。

> 安装时，别名可以省略，也可以修改主Service的配置文件，在 `services` 参数中设置子Service别名。

重启后，我们在管理员后台即可查看到文章相关的管理项。

## 配置 alaska-post

若要修改alaska-post Service的默认设置，我们可以新建配置文件 `config/alaska-post.js` 或 `config/alaska-post/config.js` 文件，并在其中设置需要覆盖的选项，例如：

```js
export default {
  cache: {
    type: 'alaska-cache-mongo',
    url: 'mongodb://localhost/alaska-demo',
    collection: 'post_cache',
    maxAge: 3600
  }
};
```

这样我们就将alaska-post Service的默认缓存驱动设置为了MongoDB。

> Alaska为我们提供了强大的配置功能，除了默认的配置文件，我们还可以以配置文件的方式扩展或重写子Service的几乎一切功能，包括：数据模型、控制器、API、本地化、中间件、Sleds甚至模板文件。 详细请参考进阶使用。


## 示例代码

在 `https://github.com/maichong/alaska-demo/tree/service` 可以查看到本节的示例代码，或则按照下列步骤克隆示例代码到本地：

```
git clone https://github.com/maichong/alaska-demo.git
cd alaska-demo
git checkout service
```
