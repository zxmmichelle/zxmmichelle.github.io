## 微信登陆流程名词
> - openid : 用户唯一身份标识  
> - unionid ：用户在（同一公司）多平台中的唯一身份表示  
> - session_key ： 用于解密服务端传递过来的加密信息   
> - 微信登陆态 ： 调用wx.login()后，微信端会维护一个session，用户使用小程序的时间越长，这个session的有效时间越长，反之越短，可用作用户登陆状态有效期判断   
> - 第3方登陆态 ： 由开发团队（开发者或企业开发团队）维持的session    
> - code ： 一个临时的code，通过wx.login()获得，有效期为5分钟

## 关于session_key的一些问题
> - 如何获取 ？     
> wx.login()获取code，把code发送给己方服务端，服务端调用auth.code2Session，换取openid、unionid、session_key等信息  
> - 把session_key保存在服务器中， 可不可行 ？       
> 不可行，session_key会过期  
> - 首次登陆时，把用户信息保存在数据库中，不用反复获取数据，可行吗？      
> 可行，但无法保持数据库中的数据与微信的同步。  
> - 如何保持数据与微信的同步 ？       
> 每次进入小程序的时候调用wx.login()，auth.code2Session，获取新的用户数据与session_key，解析数据，然后展示数据  
> - 每次都要调用吗？  
> 可以通过wx.checkSession()判断session_key是否过期了，如果过期了就重新调用微信登陆流程。  

## 使用wx.login()需要注意
> 这个过程可能会刷新登陆态，此时服务器使用code换取的session_key不是加密时使用的session_key，会导致解密失败，所以建议在进行wx.login()之前先使用wx.checkSession进行登陆态检查

## 各类登陆流程
### 案例1
> 这个流程适用于基于openid或unionid作为创建账号的情况
![image](https://user-images.githubusercontent.com/43370259/191156174-b8f772b6-3531-485b-8914-3225d75c9d27.png)

### 案例2
> 有登陆机制，但用户在游客状态
![image](https://user-images.githubusercontent.com/43370259/191164950-e0035525-27a3-42d7-8647-f433b83bdd4a.png)

### 案例3
> 有登陆页面，用户点击登陆
> 

