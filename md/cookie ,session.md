## cookie

cookie是指Web浏览器存储的少址数据。
Cookie支持跨域名访问，例如将domain属性设置为“.biaodianfu.com”，则以“.biaodianfu.com”为后缀的一切域名均能够访问该Cookie。跨域名Cookie如今被普遍用在网络中，例如Google、Baidu、Sina等。

####  浏览器使用

cookie可以使用 js 在浏览器直接设置（用于记录不敏感信息，如用户名）

大小和数量会有限制

如果没有设置有效期，那就是会话储存，关了浏览器就没了。

尽管浏览器对cookie做了大小限制，不过最好还是尽可能在coki中少存储信息，以免避免影响性能。

#### 前后端协同使用

cookie数据会自动在Web浏览器和Web服务器之间传输，在服务端通使用 HTTP 协议规定的 set-cookie 来让浏览器种下cookie，每次网络请求 Request headers 中都会带上cookie。所以如果 cookie 太多太大对传输效率会有影响。

因此服务端脚本就可以 读、 写存储在客户端的 cookie的值。

前后端协同使用流程

![前后端cookie](http://www.hxvin.me/posts/img/前后端cookie.png)

##### Response headers

![服务器响应头](http://www.hxvin.me/posts/img/服务器响应头.png)

##### Set-Cookie 字段的属性

![Set-Cookie 字段的属性](http://www.hxvin.me/posts/img/set-cookie字段的属性.png)

##### Request headers

![客服端请求头](http://www.hxvin.me/posts/img/客服端请求头.png)

#### 使用场景

1.自动填写用户名。比如你某次登陆过一个网站，下次登录的时候不想再次输入账号了，怎么办？这个信息可以写到Cookie里面，访问网站的时候，网站页面的脚本可以读取这个信息，就自动帮你把用户名给填了，能够方便一下用户。

2.自动登录。Cookie是浏览器保存信息的一种方式，可以理解为一个文件，保存到客户端了啊，服务器可以通过响应浏览器的set-cookie的标头，得到Cookie的信息。你可以给这个文件设置一个期限，这个期限呢，不会因为浏览器的关闭而消失啊。

## session

session 是一种基于cookie的让服务器能识别某个用户的「机制」，当然也可以特指服务器存储的 session数据。
Session不会支持跨域名访问，仅在他所在的域名内有效。

#### 使用场景举例：购物

当一个用户打开淘宝登录后，刷新浏览器仍然展示登录状态。然后把购物车的东西下单支付了。

刷新的时候服务器如何分辨这次发起请求的用户是刚才登录过的用户呢？下单的时候又怎么知道是哪个用户下单的呢？

> 鉴于 HTTP 是无状态协议，之前已认证成功的用户状态无法通过协 议层面保存下来。即，无法实现状态管理，因此即使当该用户下一次 继续访问，也无法区分他与其他的用户。

于是我们会使用 Cookie 来 管理 Session，以弥补 HTTP 协议中不存在的状态管理功能。

![cookie and session](http://www.hxvin.me/posts/img/cookieAndSession.png)

1.用户在输入用户名密码提交给服务端，服务端验证通过后会创建一个session用于记录用户的相关信息的对象，这个session对象中放有生成的sessionid，也可以放一些非机密的userinfo。session对象可保存在服务器内存中（容易产生内存泄露），生产环境一般是保存在数据库中。

![存在数据库的session](http://www.hxvin.me/posts/img/存在数据库的session.png)
 上图为使用koa-session-minimal 后存在数据库的session store

2.创建session后，会把关联的session_id 通过setCookie 添加到http响应头部中。
浏览器在加载页面时发现响应头部有 set-cookie字段，就把这个cookie 种到浏览器指定域名下

3.客户端接收到从服务器端发来的 Session ID 后，会将其作为 Cookie 保存在本地。之后你发起刷新或者下单的请求时，浏览器会自动发送被种下sessionid的cookie，后端接受后去存session的地方根据sessionid查找是否有此session（有既证明处于登录状态的真实用户，否则拒绝此次请求），如果有，还可以读取登录时放的userinfo，获取用户身份（user_id）。

![koa-session-minimal相关源码](http://www.hxvin.me/posts/img/koa-session-minimal相关源码.png)

#### 需要注意

1.如果 Session ID 被第三方盗走，对方就可以伪装成你的身份进 行恶意操作了。因此必须防止 Session ID 被盗，或被猜出。为了做到 这点，Session ID 应使用难以推测的字符串，且服务器端也需要进行 有效期的管理（即使不幸被盗，之后也因有效期已过而失效），保证其安全性。

2.另外，为减轻跨站脚本攻击（XSS）造成的损失，建议事先在 Cookie 内加上 httponly 属性。

3.如果客户端的浏览器禁用了 Cookie 怎么办？一般这种情况下，会使用一种叫做URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户。

#### cookie + session  方式的局限性

* 是存储式的有状态验证,由于一定时间内它是保存在服务器上的，当访问增多时，会较大地占用服务器的性能。

* 如果web服务器做了负载均衡，那么下一个操作请求到了另一台服务器的时候session会丢失。

* 有安全隐患：CSRF 。因为是基于cookie来进行用户识别的, cookie如果被截获，用户就会很容易受到跨站请求伪造的攻击。
[浅谈CSRF攻击方式](http://www.cnblogs.com/hyddd/archive/2009/04/09/1432744.html)


##### 参考阅读&&鸣谢：

[Cookie 与 Session 的区别](https://juejin.im/entry/5766c29d6be3ff006a31b84e)
[session 、cookie、token的区别](http://blog.csdn.net/jikeehuang/article/details/51488020)
