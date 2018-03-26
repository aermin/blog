Web 缓存大致可以分为：数据库缓存、服务器端缓存（代理服务器缓存、CDN 缓存）、浏览器缓存。

浏览器缓存也包含很多内容： HTTP 缓存、indexDB、cookie、localstorage 等等。这里只讨论 HTTP 缓存相关内容。


#### 304 状态码

该状态码表示客户端发送附带条件（指采用 GET 方法的请求报文中包含 If-Match，If-ModifiedSince，If-None-Match，If-Range，If-Unmodified-Since 中任一首部）的请求时，服务器端允许请求访问资源，但未满足条件的情况。

![image](https://user-images.githubusercontent.com/24861316/36773059-b610d46c-1c92-11e8-8c34-45d7f75f717d.png)


### 协商缓存

缓存的资源到期了，并不意味着资源内容发生了改变，如果和服务器上的资源没有差异，实际上没有必要再次请求。客户端和服务器端通过某种验证机制验证当前请求资源是否可以使用缓存。

浏览器第一次请求数据之后会将数据和响应头部的缓存标识存储起来。再次请求时会带上存储的头部字段，服务器端验证是否可用。如果返回 304 Not Modified，代表资源没有发生改变可以使用缓存的数据，获取新的过期时间。反之返回 200 就相当于重新请求了一遍资源并替换旧资源。

客户端:  If-None-Match+ If-Modified-Since
![image](https://user-images.githubusercontent.com/24861316/36773526-10db506e-1c95-11e8-9784-6e9e00cb7fb3.png)


服务器：Etag + Last-Modifired
![image](https://user-images.githubusercontent.com/24861316/36773200-74783ed6-1c93-11e8-8bd8-f09c8160e02d.png)

`If-Modified-Since` 用于确认代理或客户端拥有的本地资源的有效性。 获取资源的更新日期时间，可通过确认首部字段 Last-Modified 来确 定。

如果在 If-Modified-Since 字段指定的日期时间后， 资源发生了 更新， 服务器会接受请求

![image](https://user-images.githubusercontent.com/24861316/36773197-705f821e-1c93-11e8-8f3b-8daf8dbd57e6.png)


### 强缓存

可以理解为无须验证的缓存策略。对强缓存来说，响应头中有两个字段 Expires/Cache-Control 来表明规则。

![image](https://user-images.githubusercontent.com/24861316/36777165-695588f0-1ca3-11e8-82c4-e1db41bd6be9.png)
