# 中间件
中间件指封装所有http请求细节处理，因为一次http请求可能包含很多工作，如记录日志、ip过滤、查询字符串、请求体解析、cookie处理、权限认证、参数验证、异常处理等，但对于web应用而言，不需要接触过多细节性处理，所以引入中间件来简化和隔离这些基础设施和业务逻辑间的细节，让开发者能够关注在业务上的开发，提高开发效率。

node的中间件是一个洋葱模型
### 中间件框架——connect
express就是基于connect开发的
### 错误处理
在某一步骤中出现错误时，会跳过接下来的常规中间件，只调用错误处理中间件。
- 用connect的默认错误处理器
- 自行处理
### node框架
常见的node框架都是服务端的开发框架，重点是对http request和http response两个对象的封装和处理，应用生命周期维护以及视图的处理等。
- express
诞生已久，是简洁灵活的web开发框架，基于Connect中间件框架，功能丰富随取随用，自身封装了大量便利的功能，如路由、视图处理等等
- koa
更为年轻，是express原班人马基于es6新特性重新开发的敏捷开发框架，基于co中间件框架，自身并没有集成太多功能，大部分功能需要用户自行require中间件解决，基于es6 generator特性的中间件机制，解决了回调地狱和错误处理问题。

[Node.js框架之express与koa对比分析](https://developer.aliyun.com/article/3062)