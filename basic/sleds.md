# Sleds

我们将业务逻辑封装为单独的Sled类，控制器，API和其它代码都可以调用Sled执行。
所有的Sled都继承于`alaska.Sled`类。

## 扩展Sleds

Sled类必须实现exec()方法，例如新建`sleds/Increment.js`：
```js
import alaska from 'alaska';

export default class Increment extends alaska.Sled{
  async exec(data){
    let user = data.user;
    user.age++;
    user.save();
    return user;
  }
}
```
或者新建`config/alaska-user/sleds/Increment.js`：
```js
import alaska from 'alaska';

export default class Increment extends alaska.Sled{
  async exec(data){
    let user = data.user;
    user.age++;
    user.save();
    return user;
  }
}
```
