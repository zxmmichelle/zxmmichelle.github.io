## 名词
> principal: 能唯一标识用户身份的属性，一个主题（用户）可以有多个principal；例如用户可以通过手机号码、邮箱、用户名。  
credential: 凭证，只有主题（用户）才知道的；例如密码。  
authentication: 身份验证，一般是指提供身份验证的服务。  
authenrization: 访问权限。  
session:  即会话，一种服务器分辨“客户端”的身份标识，服务器端需保存相关session信息。  
cookie: 指浏览器实现的一种数据存储功能，常与session结合使用（在B/S模式下）。  
token: 也是一种服务器分辨“客户端”的标识，即“令牌”，不同于session，服务器端一般不保存token信息。  

## B/S模式下，session/cookie的运作方式（大部分网站在用的模式)：
> ![session](https://user-images.githubusercontent.com/43370259/125188162-5de82000-e265-11eb-8230-0ce4976fe9d0.PNG)  
> 在这个模式下，会出现服务器压力大、cookie被截取、CSRF（跨域请求伪造）、扩展性差等问题。  

## token的运作方式：
> ![token](https://user-images.githubusercontent.com/43370259/125188178-6c363c00-e265-11eb-93ca-ad109feb742f.PNG)  
> 在这个模式下，会出现无法在服务器端注销、token长度比sessionId更大等问题。  

## token与session的区别
> token：无状态、支持跨域访问、不需要绑定特定的身份验证方案、适用于不支持cookie的客户端。    
> cookie/session：有状态、不支持跨域访问、无法在不支持cookie的客户端上使用。  

## jwt
> 全称Json Web Token，一种基于JSON的开放标准（(RFC 7519)的“令牌”技术。
> 其结构如下：
> ```
> [header].[payload].[signature]
> ```

## 前后端分离token方案
> 

