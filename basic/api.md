# API

## 扩展API接口

若要扩展API接口，和扩展`controller`步骤是一样的，例如新建配置文件`config/alaska-user/api/info.js`：
```js
import service from '../';

export default async function(ctx) {
  if (!ctx.user) {
    service.error(403);
  }
  ctx.body = ctx.user.data('info');
}
```
这是挂载在`user`Service下得api，如果想要扩展主Service下的api，和这个步骤是一样的，例如新建`api/user/info.js`：
```js
import service from '../';

export default async function(ctx) {
  if (!ctx.user) {
    service.error(403);
  }
  ctx.body = ctx.user.data('info');
}
```
