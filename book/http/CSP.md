# CSP(内容安全策略)
内容安全策略是一个额外的安全层，用于检测并削弱某些特定类型的攻击，包括跨站脚本（XSS）和数据注入攻击等，这些攻击可以盗取数据、污染网站内容、散发恶意软件。
### 配置CSP
- 服务器返回 `Content-Security-Policy` http头部
- `<meta>`标签配置

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; img-src https://*; child-src 'none';">
```

##### 原理:
CSP通过**指定有效域**——即浏览器认可的可执行脚本的有效来源，使服务器管理者有能力减少或消除XSS攻击所依赖的载体。CSP兼容的浏览器会仅执行从白名单域获取到的脚本文件，忽略所有其他脚本（包括内联脚本和html的事件处理属性）。
### 使用CSP
通过配置 Content-Security-Policy http头部，控制浏览器可以为该页面获取哪些资源。

参考：[CSP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)