## 2021年07月22日
> 我司上线了一个新项目，项目启动没多久cpu就跑满了，奇怪的是开发环境跟测试环境都没出现这种情况。  
> 经过一下午的搜索，我非常庆幸项目用的是.net core3.0以上的sdk。  
> 现在，总结一下今日所学~   
> ### 背景 
>- linux服务器
>- ubuntu 16.04
>- 项目运行的在docker container中
>- 项目所用sdk为net core3.1
>- 所需工具：wget, procps, dotnet-dump    
>- 注意！docker run时，一定要带上--cap-add=SYS_PTRACE 或 --privileged，不然无法进行转储
> #### 预备工作
> ##### 第一步，进入docker container
> ```shell 
> docker exec -it <image_name> /bin/bash
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
> #### 探险开始~
> 用以下命令找到跑满cpu的线程
> ```shell
>  ps H -eo user,pid,ppid,tid,time,%cpu,cmd --sort=%cpu
> ```
> 记住cpu占比最高那一行的pid、tid，假设分别为1、15    
> 接着用dotnet-dump转储pid为1的进程！记得要连续多collect几个。
> ```shell
> ./dotnet-dump collect -p <pid>
> ```
> 1s后将生成一个core_yyyyMMdd_HHmmss文件    
> 现在，可以开始分析这个dump文件了！！  
> ````shell
> ./dotnet-dump analyze core_yyyyMMdd_HHmmss 
> ````
> 列出所有还活着的thread
> ```shell
> clrthreads -live 
> ```
> 查看tid对应的dbg，_括号里的是tid，最前面的就是dbg_
> ```shell
> threads
> ```
> 切换查看的线程
> ```shell
> setthread <dbg>
> ```
> 查看这个线程的调用栈，如果发现里面只有一行内容，建议换一个dump文件再查一次，可能那1s这条线程正在sleep。
> ```shell
> clrstack
> ```
