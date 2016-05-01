# 数据模型

## 建立Article文章模型

在 `models` 目录下建立 `Article.js`。

然后编辑文件内容：

```js
export default class Article extends service.Model {
  static label = 'Article';
  static fields = {
    label: {
      title: 'Title',
      type: String
    },
    content: {
      label: 'Content',
      type: String
    },
    createdAt: {
      label: 'Created At',
      type: Date
    }
  };

  preSave() {
    if (!this.createdAt) {
      this.createdAt = new Date;
    }
  }
}
```

注意：

1. 文件名应与模型类名一致
2. 使用 `export default` 语法导出类定义
3. 模型类应继承于 `service.Model`
4. 模型类中，`fields` 静态属性是必须定义的，其他的可以省略。

在上面的模型类定义中

* `label` 静态属性代表在管理平台显示的模型名称，如不指定则与模型的name一直。

* `fields` 静态属性定义该模型的字段列表，该对象的每一个属性代表着该模型的每一个字段。字段的 `label` 属性代表该字段在管理平台显示的标签，`type` 代表该字段的类型。
> 类型 `String` 和 `Date` 都是简写方式，分别代表着 `alaska-field-text` 和 `alaska-field-datetime` 扩展。

* `preSave` 方法是记录保存的前置钩子，在这里用来自动设置记录的新建时间。

## 重启node

重启后，刷新浏览器，会发现多出一个Article菜单。

我们定义了Article模型后，管理平台就自动多出了针对Article的CURD操作界面。我们可以尝试录入写内容。

## 扩展Field

我们发现文章的内容字段在后台显示为一个简单的input输入框，而我们需要的文章内容应该是富文本的。

修改 `Article` 模型，将 `content` 字段类型更换为 `'html'`：

```js
content: {
  label: 'Content',
  type: 'html'
}
```

这样我们就讲内容编辑器切换成了富文本编辑器，但是重启后却发生了异常：


```
Alaska launch failed!
Error: Cannot find module 'alaska-field-html'
    at Function.Module._resolveFilename (module.js:339:15)
    ...
```

异常原因是我们的项目没有安装 `alaska-field-html` 扩展库，运行安装命令：

```sh
npm i -S alaska-field-html
```

安装扩展后，需要重新构建后台：

```sh
alaska build
```

重启node后，即可在管理平台发现文章编辑器的内容字段已经变为了富文本编辑器。

> `type: 'html'` 也是缩写形式，系统会自动为type加上前缀 `alaska-field-`。
>
> 例如 `type: 'date'` 会加载 `alaska-field-date` 扩展。
>
> `type: Date` 会加载 `alaska-field-datetime` 扩展。
>
> `type: Boolean` 会加载 `alaska-field-checkbox` 扩展。
> 
> `type: Number` 会加载 `alaska-field-number` 扩展。
> 
> `type: OtherModel` 会加载 `alaska-field-relationship` 扩展。

## 建立ArticleCat文章分类模型

在 `models` 目录下建立 `ArticleCat.js`，内容为：

```js
export default class ArticleCat extends service.Model {
  static label = 'Article Category';
  static title = 'name';
  static defaultColumns = '_id name parent';
  static fields = {
    name: {
      label: 'Name',
      type: String,
      required: true
    },
    parent: {
      label: 'Parent Category',
      type: ArticleCat
    }
  };
}
```

* `static title = 'name'` 静态属性的作用是将 `name` 字段设置为本模型的**标题字段**，如不指定，则默认认为模型的标题字段为 `title`。

* `defaultColumns` 静态属性的作用是定义管理平台列表界面默认显示的数据列，各个字段用空格分隔，如果不指定，默认为`_id title CreatedAt`。

* `name` 字段的 `requried` 属性代表该字段必填。

* `parent` 字段的 `type` 类型为 `ArticleCat` 即当前数据模型类，代表模型关联，这里是缩写形式。

> 模型关联字段的全写为
```js
parent: {
  label: 'Parent Category',
  type: 'relationship',
  ref: 'ArticleCat',
  multi: false
}
```
另外，还可以缩写为：
```js
parent: {
  label: 'Parent Category',
  ref: ArticleCat
}
```
如果一对多关联 `alaska-user` Service提供的 `User` 模型，可定义为：
```js
users: {
  label: 'Users',
  ref: ['alaska-user.User']
}
```

## 录入分类数据

重启后录入两条分类：JS、MongoDB。

## 模型关联

修改 `Article` 模型，为其添加新的字段：

```js
cat: {
  label: 'Category',
  ref: 'ArticleCat',
  required: true
}
```

然后在后台编辑文章的界面即可以为文章关联一个分类。

## 示例代码

在 `https://github.com/maichong/alaska-demo/tree/model` 可以查看到本节的示例代码，或则按照下列步骤克隆示例代码到本地：

```
git clone https://github.com/maichong/alaska-demo.git
cd alaska-demo
git checkout model
```


