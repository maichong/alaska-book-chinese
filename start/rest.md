# REST接口

## 为Article模型打开REST接口

Alaska内置RESTFUL接口，只需要配置数据模型的 `api` 静态属性即可为模型开放接口。

修改 `Article` 模型代码为：

```js
import alaska from 'alaska';

export default class Article extends alaska.Model {
  static label = 'Article';
  static api = {
    list: alaska.PUBLIC,
    show: alaska.PUBLIC
  };

  static fields = {
    title: {
      label: 'Title',
      type: String
    },
    cat: {
      label: 'Category',
      ref: 'ArticleCat',
      required: true
    },
    content: {
      label: 'Content',
      type: 'html'
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

重启node后，打开浏览器查看文章列表接口： `http://localhost:5000/api/article`

在上方的 `api` 静态属性中 `list` 和 `show` 分别为列表和详情API开关。

> api的属性有：`list`，`show`，`create`，`count`，`update`，`remove`。
> 
> 开关值分别为 `alaska.CLOSED`，`alaska.PUBLIC`，`alaska.AUTHENTICATED`，`alaska.OWNER`。分别可以简写为 0、1、2、3。代表四种接口权限：关闭（默认）、开放、需要验证、需要认证资源所有权。
> 
> 例如 `list:3` 则代表列表接口需要登录，并且只允许列出登录用户自己的文章列表。

## 示例代码

在 `https://github.com/maichong/alaska-demo/tree/rest` 可以查看到本节的示例代码，或则按照下列步骤克隆示例代码到本地：

```
git clone https://github.com/maichong/alaska-demo.git
cd alaska-demo
git checkout rest
```



