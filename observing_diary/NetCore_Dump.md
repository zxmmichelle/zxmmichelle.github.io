## 2021年7月22日
> 我司上线了一个新项目，项目启动没多久cpu就跑满了，奇怪的是开发环境跟测试环境都没出现这种情况。  
> 经过一下午的搜索，我非常庆幸项目用的是.net core3.0以上的sdk。  
> 现在，总结一下今日所学~   
> ### 背景 
>- linux服务器
>- ubuntu 16.04
>- 项目运行的在docker container中
>- 项目所用sdk为net core3.1
>- 所需工具：wget, procps, dotnet-dump    
> #### 预备工作
> ##### 第一步，进入docker container
> ```shell 
> docker exec -it <container_name> /bin/bash
> ```
> ##### 第二步，下载wget（有则跳过）
> ```shell 
> apt-get update
> apt-get install -y wget
> ```
> ##### 第三步，下载procps（有则跳过）
> ```shell
> apt-get install -y procps
> ```
> ##### 第四步，下载dotnet-dump且修改权限
> ```shell
> wget https://aka.ms/dotnet-dump/linux-x64 dotnet-dump
> chmod 777 dotnet-dump
> ```
> 
