# 本地化

Alaska提供了多语言支持，可以配置默认语言。例如`config/alaska-demo.js`：

	export default {
	  defaultLocale: 'zh-CN',
	  locales: ['zh-CN','en']
	}

这样，我们就在项目中启用了中文简体和英文两种语言。当然，我们还需要添加相应的内置标题。

## 内置标题

每个Service的`locales`目录下都有对应的语言目录，我们也可以对其进行扩展
例如添加`config/alaska-user/locales/zh-CN/messages.js`文件：

	export default{
	  Username: '都知道的用户名'
	  Password: '鬼知道的密码'
	};

这样，我们就修改了`alaska-user`Service中`Username`及`Password`的简体中文标题。
当然，我们也可以将其修改为内置中没有的语言。只需要在`config/alaska-user/locales`目录下创建语言文件目录并启用对应语言。