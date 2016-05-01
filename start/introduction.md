# 基础概念

## Service

我们将不同业务领域进行抽象和封装，提供基础原型组件，称之为Service。

每一个Alaska项目都由一到多个Service组成，每一个Service都是一个MVC子系统，即，每一个Service都可以包含数据模型、控制器、API、视图模板。

每一个Service都可以指定HTTP挂载点来接收、处理HTTP请求。所有挂载点必须唯一。

每个Service都有其主目录，主目录下存放该Service的配置、模型、控制器、模板等等。

每一个Service都封装在一个NPM包中。除主Service，Service所在的NPM包入口文件应当默认导出该Service类，该类继承于alaska.Service，在程序启动期间自动完成实例化。

Service可以互相依赖，在一个Alaska项目中，有一个主Service作为程序启动入口。

同一个Service在运行中的项目中只有一个实例，例如，A依赖B和C，B依赖C，那么虽然有两个Service依赖C，但是只有一个实例C。

每个Service都有一个ID，一般是其NPM包名，例如alaska-user、alaska-balance等。

Service可以为其依赖的子Service设置别名(alias)。

获取一个子Service实例使用 `service.service(ID_OR_ALIAS)` 方法。ID_OR_ALIAS是子Service的ID或别名。

## Model

我们对具体业务中的实体对象进行计算机数学建模，称之为模型(Model)。

所有模型文件存放在其所在Service的models文件夹中，每个文件中定义一个数据模型，文件名和数据模型的类名一致，所有数据模型类继承于 `service.Model` 。

Alaska采用mongoose完成底层的数据模型操作，Alaska的模型继承于[mongoose.Model](http://mongoosejs.com/docs/api.html#model-js)，所以我们可以在Alaska模型上调用所有的Mongoose方法，例如， `User.findOne()` 、 `User.count()` 、 `user.save()` 等等。

每一个Model都有 `name` 、 `id` 、 `key` 属性，name即是其类名，id是其类名转连字符小写形式，比如 `PostCat` 模型的id是 `post-cat` ，key是其所在service的id加上其id，例如 `alaska-post.post-cat` 。

在项目中，获取一个模型类可以使用 `service.model(Name)` 方法，或者直接import其文件，例如

```javascript
import Article from '../models/Article'
```

## Field

实体对象拥有多个属性，每一个属性即是一个字段(field)，每一种字段的类型称为字段类型(Field)。

Field都由各个Field类定义，所有的Field类都继承于 `alaska.Field` 。

Field类定义由各个NPM扩展包提供，例如 `alaska-field-text` 、 `alasak-field-number` 、 `alaska-field-html` 等。

模型中定义的所有fields都必须制定Field类型。

Field有两大作用，1.定义mongoose SchemaType。2.提供管理平台React UI控件。例如text类型和html类型在数据库中都储存为String类型，但是在管理员平台的数据编辑页面，text显示为一个input输入框，而html显示为一个富文本编辑框。

## Controller

用于处理HTTP请求、处理数据和事物、最后返回渲染后的HTML文档的中间件成为控制器(Controller)。

Alaska的控制器基于[Koa v2](https://github.com/koajs/koa/tree/v2.x)，每个控制器方法都是一个Koa中间件，一律采用 `async function` 实现。

所有的控制器存放于所在Service主目录的 `controllers` 目录中。

控制器方法的HTTP访问点可以描述为 `PREFIX/CONTROLLER/ACTION` 。PREFIX为所在Service的挂载点，CONTROLLER是控制器的名称，ACTION是控制器的方法名。

默认支持GET和POST方法请求控制器。

## API

Alaska中，专门用来对外提供JSON数据接口的控制器为API控制器，存放在api目录中。

所有API的HTTP访问点在Service挂载点的 `/api/` 子路径。

Alaska中API有两种RESTFUL接口和扩展接口。扩展接口的定义和普通控制器的定义相同，但是一定要始终返回JSON数据。

RESTFUL接口由系统默认提供，配置Model.api属性即可开放RESTFUL接口。

默认提供的RESTFUL接口专门用于数据的CURD操作，不包含复杂的逻辑处理，如果需要覆盖默认RESTFUL接口，可以使用前置中间件。

RESTFUL接口规格为
```bash
GET    PREFIX/api/MODEL       #数据列表
POST   PREFIX/api/MODEL       #创建记录
GET    PREFIX/api/MODEL/ID    #记录详情
GET    PREFIX/api/MODEL/count #统计数量
PUT    PREFIX/api/MODEL/ID    #更新记录
DELETE PREFIX/api/MODEL/ID    #删除记录
```
PREFIX是其Service的挂载点，MODEL是访问模型的id，ID是访问数据的id。

## Sled

我们业务逻辑从控制器和API中抽离出来，封装成单独的Sled类。

控制器、API和其他代码调用Sled执行，这样有利于代码复用和精简控制器。

所有的Sled类继承于service.Sled并且存放于sleds目录。

每一个Sled都必须实现 `exec()` 方法，所有的业务逻辑都在此方法中完成。

调用Sled使用 `sled.run()` 方法，或使用快捷方法： `service.run(sledName,data)` 或 `SledClass.run(data)` 。

例如注册用户：

```javascript
let user = await USER_SERVICE.run('Register',{ username: 'alaska', password: 'pass'});
```



