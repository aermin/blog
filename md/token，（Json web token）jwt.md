前一个项目用的是cookie+session,由于想试试新花样，现在这个项目用的是Json web token。也做一下总结。

讲Json web token 之前先来了解下什么是token，因为jwt本质就是一个token

### 什么token

token的意思是“令牌”，是用户身份的验证方式，最简单的token组成:uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign(签名，由token的前几位+盐以哈希算法压缩成一定长的十六进制字符串，可以防止恶意第三方拼接token请求服务器)。还可以把不变的参数也放进token，避免多次查库

### 什么是JSON Web Token

JSON web Token,简称JWT，本质是一个token，是一种紧凑的URL安全方法，用于在网络通信的双方之间传递。一般放在HTTP的headers 参数里面的authorization里面，值的前面加Bearer关键字和空格。除此之外，也可以在url和request body中传递。

### JSON Web Token的组成

一个JWT实际上就是一个字符串，它由三部分组成，头部、载荷与签名依顺序用点号（"."）链接而成：1.header，2.payload，3.signature。

#### 头部（Header）里面说明类型和使用的算法，比如：

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

说明是JWT(JSON web token)类型，使用了HMAC SHA 算法。然后将头部进行base64加密（该加密是可以对称解密的),构成了第一部分.

```string
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9
```

#### 载荷（Payload）载荷就是存放有效信息的地方,含三个部分:

1.标准中注册的声明,2.公共的声明,3.私有的声明

1.标准中注册的声明 (建议但不强制使用) ：

* iss: jwt签发者
* sub: jwt所面向的用户
* aud: 接收jwt的一方
* exp: jwt的过期时间，这个过期时间必须要大于签发时间
* nbf: 定义在什么时间之前，该jwt都是不可用的.
* iat: jwt的签发时间
* jti: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。

2.公共的声明 ：
公共的声明可以添加任何的信息，一般添加用户的相关信息或其他业务需要的必要信息.但不建议添加敏感信息，因为该部分在客户端可解密.

3.私有的声明 ：
私有声明是提供者和消费者所共同定义的声明，一般不建议存放敏感信息，因为base64是对称解密的，意味着该部分信息可以归类为明文信息。

定义一个payload:

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```
然后将其进行base64加密，得到Jwt的第二部分。

```string
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9
```

把场景2的操作描述成一个json对象。其中添加了一些其他的信息，帮助今后收到这个JWT的服务器理解这个JWT。 当然，你还可以往载荷放非敏感的用户信息，比如uid

#### signature

这个部分需要base64加密后的header和base64加密后的payload使用.连接组成的字符串，然后通过header中声明的加密方式进行加盐secret组合加密，然后就构成了jwt的第三部分。

```js
// javascript
var encodedString = base64UrlEncode(header) + '.' + base64UrlEncode(payload);

var signature = HMACSHA256(encodedString, 'secret');//TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ

```

`注意`secret是保存在服务器端的，jwt的签发生成也是在服务器端的，secret就是用来进行jwt的签发和jwt的验证，所以，它就是你服务端的私钥，在任何场景都不应该流露出去。一旦客户端得知这个secret, 那就意味着客户端是可以自我签发jwt了。

将这三部分用.连接成一个完整的字符串,构成了最终的jwt:

```json
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

### 应用场景：

1.浏览器将用户名和密码以post请求的方式发送给服务器。

2.服务器接受后验证通过，用一个密钥生成一个JWT。

3.服务器将这个生成的JWT返回给浏览器。

4.浏览器存储JWT并在使用时将JWT包含在authorization header里面，然后发送请求给服务器。

5.服务器可以在JWT中提取用户相关信息。进行验证。

6.服务器验证完成后，发送响应结果给浏览器。



### jwt 相比 session 的有点在哪

* 能解决csrf[浅谈CSRF攻击方式](http://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html)
* 因为json的通用性，所以JWT是可以进行跨语言支持的，像JAVA,JavaScript,NodeJS,PHP等很多语言都可以使用。
* 因为有了payload部分，所以JWT可以在自身存储一些其他业务逻辑所必要的非敏感信息。
* 便于传输，jwt的构成非常简单，字节占用很小，所以它是非常便于传输的。
* 它不需要在服务端保存会话信息, 所以它易于应用的扩展

### 安全相关

* 不应该在jwt的payload部分存放敏感信息，因为该部分是客户端可解密的部分。
* 保护好secret私钥，该私钥非常重要。
* 建议的方式是通过SSL加密的传输（https协议），从而避免敏感信息被嗅探。


### 代码实例

客户端 这里引用axios

```js
axios.interceptors.request.use(
	config => {
		const token = localStorage.getItem('userToken');
		if (token) {
			config.headers.common['Authorization'] = 'Bearer ' + token;
		}
		return config;
	},
	error => {
		return Promise.reject(error);
	}
);
```

后端（koa）  这里引用[jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken) 这个npm包

登录时生成一个token

```js
const jwt = require("jsonwebtoken");
const secret = require("../config").secret;

const token = jwt.sign(payload, secret, {
    expiresIn: Math.floor(Date.now() / 1000) + 24 * 60 * 60 // 一天
});
```
写一个jwt验证的中间件

```js
/**
 * @file 处理jwt验证的中间件
 */

const jwt = require("jsonwebtoken");
const secret = require("../config").secret;

module.exports = async function(ctx, next) {
	// 同步验证
	const auth = ctx.get('Authorization')
	const token = auth.split(' ')[1];
	try {
		//解码取出之前存在payload的user_id 和 name
		const payload = jwt.verify(token, secret)
		ctx.user_id = payload.id;
		ctx.name = payload.name;
		await next()
	} catch (err) {
		ctx.throw(401, err)
	}
}
```

在路由（koa-router）这边使用这个中间件

```js
const verify = require('../middlewares/verify');
router.post('/register', register) //注册
	.post('/login', login) //登录
	.get('/message', verify, message) // 获取首页列表信息
```

### 其他

还有一个 oauth token ：一个关于授权(authorization)的开放网络标准协议。

> 有一个"云冲印"的网站，可以将用户储存在Google的照片，冲印出来。用户为了使用该服务，必须让"云冲印"读取自己储存在Google上的照片。

具体看这个 [理解OAuth 2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)

#### 参考阅读&&鸣谢：

[什么是 JWT -- JSON WEB TOKEN](https://www.jianshu.com/p/576dbf44b2ae)

[Token 认证的来龙去脉](https://juejin.im/post/5a6c60166fb9a01caf37a5e5?utm_source=gold_browser_extension)

[json web token是用来做什么的？](https://www.zhihu.com/question/36135526)

[JSON Web Token - 在Web应用间安全地传递信息](http://blog.leapoahead.com/2015/09/06/understanding-jwt/)

[八幅漫画理解使用JSON Web Token设计单点登录系统](http://blog.leapoahead.com/2015/09/07/user-authentication-with-jwt/?utm_source=tuicool&utm_medium=referral#disqus_thread)
