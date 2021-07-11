## 名词
> principal: 能唯一标识用户身份的属性，一个主题（用户）可以有多个principal；例如用户可以通过手机号码、邮箱、用户名。  
credential: 凭证，只有主题（用户）才知道的；例如密码。  
authentication: 身份验证，一般是指提供身份验证的服务。  
authenrization: 访问权限。  
session:  即会话，一种服务器分辨“客户端”的身份标识，服务器端需保存相关session信息。  
cookie: 指浏览器实现的一种数据存储功能，常与session结合使用（在B/S模式下）。  
token: 也是一种服务器分辨“客户端”的标识，即“令牌”，不同于session，服务器端一般不保存token信息。  

## B/S模式下，session/cookie的运作方式：
