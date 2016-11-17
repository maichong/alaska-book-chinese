# 配置

所有的Service配置文件都在该Service下的config目录里，并且会自动挂载主Service下的config中同名的扩展配置；这样，我们就可以对相应的Service进行扩展及修改。

## 配置主Service

我们知道，安装一个Service只需要`alaska install`命令即可，但是启用，则需要在config中配置，例如`config/alaska-demo.js`：
```js
	export default {
	  session: {
	    cookie: {},
	    store: {
	      type: 'alaska-cache-mongo',
	      url: 'mongodb://localhost/alaska-demo',
	      collection: 'app_session',
	      maxAge: 3600
	    }
	  },
	  services: {
	    'alaska-admin': {},
	    'alaska-user': {}
	  },
	  statics: [
	    {
	      root: 'public',
	      prefix: ''
	    }
	  ],
	}
```
这样，我们就挂载上了`admin`及`user`Service；并且还设置了session缓存为mongo，当然，也可以设置为redis，lru等。

| 属性 | 类型 | 描述 |
| -------------- | :--------: | :-------------|
| id             | String | `Service`ID |
| db             | String | 数据库链接地址      |
| appMiddlewares | [Object] | 中间件      |
| session        | Object | 会话控制      |
| autoLogin      | Object | 记住密码      |
| services       | Object | 启用的service|
| domain         | String | 域名      |
| prefix         | String | 挂载点      |
| redirect       | String | 跳转      |
| statics        | [Object] | 静态资源      |
| superUser      | String | 超级用户      |
| autoUpdate     | Boolean | 自动更新      |
| port           | Number | 端口      |

### id
service的ID

### db
数据库链接地址```

### appMiddlewares
中间件

### session
会话控制

### autoLogin
记住密码

### services
挂载的`service`

## 示例代码

在`https://github.com/maichong/alaska-demo/tree/service/config`可以查看到本节的示例代码，或则按照下列步骤克隆示例代码到本地：

	git clone https://github.com/maichong/alaska-demo.git
	cd alaska-demo
	git checkout service