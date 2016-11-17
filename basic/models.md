# 扩展数据模型
所有的数据模型，我们都可以对其进行扩展，包括字段、方法及钩子等。

## 扩展字段
若要给`User`模型中扩展字段，可以新建配置文件`config/alaska-user/models/User.js`,并在其中配置需要添加的字段，例如：
```js
export const fields = {
  nick: {
    label: '昵称',
    type: String
  },
  birthday: {
    label: '生日',
    type: Date
  }
};
```
## 扩展方法
同理，想要给`User`模型扩展方法及hook等，可以在配置文件`config/alaska-user/models/User.js`中对其进行相应的配置
```js
export async function preSave() {
  if (!this.nick) {
    this.nick = this.username;
  }
}
export const methods = {
  let me = this;
  sayHello: function () {
    console.log(`我叫${me.username}，但是大家都叫我${me.nick}。`);
  }
}
```
## 隐藏模型
对于后台用不到的模型，我们可以将其隐藏掉，例如`config/alaska-user/models/User.js`：
```js
export const hidden = true;
```
同理，我们可以使后台对模型进行编辑和删除，例如`config/alaska-user/models/User.js`：
```js
export const edit = true;
export const noremove = false;
```

## 列表显示及排序
例如`config/alaska-user/models/User.js`：
```js
export const defaultColumns = 'avatar username nick roles email createdAt';
export const defaultSort = 'createdAt';
```
这样，我们就把默认的后台列表显示及排序替换掉了。

## 添加自定义按钮
后台对数据进行增加，修改及保存，都需要按钮去触发，我们称之为`action`。后台的action是有限的，如果我们想要添加自己的action，只需要在特定的模型中进行配置，例如：`config/alaska-user/models/User.js`：
```js
export const actions = {
  disenable: {
    title: '限制登录',
    list: false,
    style: 'danger',
    needRecords: 1,
    sled: 'alaska-demo.DisenableUser'
  }
}
```

* `actions`：所有的action都是它的一个属性
* `disenable`：自定义的一个action
* `title`：后台界面显示的标题
* `list`：是否只用于列表页面，如果为false，则添加和编辑页面也有。
* `style`：按钮的样式
* `needRecords`：列表页需要几条数据才能触发按钮事件
* `sled`：执行该action的sled。
