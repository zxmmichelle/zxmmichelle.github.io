#  Blog
## 算法
[动态规划(分月饼)](https://github.com/zxmmichelle/zxmmichelle.github.io/blob/main/algorithm/mooncakes.md)


## 观察日记
[RabbitMQ](https://github.com/zxmmichelle/zxmmichelle.github.io/blob/main/observing_diary/RabbitMQ_Middleware.md)


[ElasticSearch](https://github.com/zxmmichelle/zxmmichelle.github.io/blob/main/observing_diary/Elasticsearch.md)

## 产品日记
日常碎碎念(https://github.com/zxmmichelle/zxmmichelle.github.io/blob/main/pm_diary.md)

# 关于我
## 个人信息
> 学历：本科  
> 毕业学校：五邑大学  
> 状态：在职(考虑机会)  
> 工作经验：5年(截至2021年8月8日)  

## 专业经验
### 前端
#### 语言
> 了解CSS, HTML5, JavaScript, TypeScript等前端语言


#### 框架
> ExtJs, JQuery, ZUI


### 后端
#### 语言
> 熟悉Java, C#  


#### 中间件
> 熟悉RabbitMQ, Memcached, Quartz  
> 了解Redis, Elasticsearch


#### 数据库
> 熟悉使用SQL Server, Oracle, PostgreSQL


#### 框架
> Java: SpringBoot, SpringMVC  
> .Net: .Net Framework, .Net Core


#### ORM
> Java: MyBatis  
> .Net: SqlSuger, PetaPoco


#### IDE
> Java: Eclipse, STS, Netbeans  
> .Net: Visual Studio


## 项目经验
### Demo_Repo
<b>项目地址：</b> 
> _https://github.com/zxmmichelle/Demo_Repo_  

<b>技术栈：</b> 
> SpringBoot, SpringMVC, Gradle    

<b>总结：</b>
> 该项目为个人练习项目。  
> 其中CacheRequestWeb为云食城系统中ScheduleApp业务执行框架的Java版本；  
> 项目ConcurrentRequest主要用于模拟并发测试。

### 云食城系统(Java版本)
<b>技术栈：</b> 
> SpringBoot, SpringMVC, RabbitMQ, PostgreSQL, Redis  

<b>总结：</b>
> 该项目为个人练习项目。  

### 云食城系统(.Net)
<b>技术栈：</b>
> .NetCore, PetaPoco, SqlSugar, Elasticsearch, RabbitMQ, PostgreSQL, Quartz, Memcached, Docker, Linux   

<b>负责描述：</b>
> 本人在该项目中担任项目开发组长，参与核心业务功能分析；   
> 负责数据库、系统架构设计；  
> 负责财务模块、复杂业务的实现，解决中间件使用不当等问题；  
> 负责输出开发文档，带新人，把控代码质量等；

<b>总结：</b>
> 该项目主要围绕 To B 业务开展，为经销商开发通用的 ERP 系统，配合仓储服务，孵化不同的渠道业务线辅助经销商更好地经营产品，降低其采购与仓储成本。  
该项目囊括了 7 个应用：ERP 系统、运营系统、单点登录系统、定时任务系统、触发型后台服务、资金服务系统、同步数据服务。同时，该项目需与 WMS 系统、SAAS 系统、1.0系统以及中信银行支付系统进行数据同步。  
>> ERP 系统：[WebApp]为经销商提供商品、采购、销售以及通用的财务应收应付等信息管理；  
>> 运营系统：[WebApp]为项目的运营人员提供的重要信息的审核管理功能以及部分渠道业务的管理；  
>> 单点登录系统：[WebApp]为所有 Web 系统提供单点登录服务；  
>> 定点任务系统：[WebApp]主要执行定时服务，并提供 Web 界面进行管理，以及由request 驱动的后台服务；    
>> 触发型后台服务：[ConsoleApp]主要是 RabbitMQ 的 Consumer，负责业务中的末端节点处理以及 API 的调用；    
>> 资金服务系统：[ConsoleApp]为项目中涉及到资金的功能，提供统一的查询、充值、提现、扣费、转账等服务；  
>> 同步数据服务：[ConsoleApp]为新旧系统提供同步数据的服务；  
>    
> 在项目中使用.Net Core 框架进行开发，实现跨平台部署。根据业务形态梳理建立可复用的流程，采用 RabbitMQ 对重事务、重网络 I/O 的功能进行异步处理。针对因需要与多个第三方系统同步数据导致触发点零散、维护困难的问题，在项目中使用Statemachine 进行 API 触发的管理；在部分项目中使用了领域驱动设计进行重构.

<b>成绩</b>
> 在该项目中本人重构了核心库存表, 使之能够快速适应不同的渠道模式;   
> 成功将一个单体程序在业务角度与项目结构层面上解耦. 

### China IPVPN(Workflow)
<b>技术栈：</b>
> ExtJs, Spring MVC, MyBatis, Oracle, Spring Batch, Maven

<b>负责描述：</b>
> 熟悉业务逻辑，日常维护该项目的代码，根据 PM 的需求，添加新功能;    
> 根据小组长的分配的需求进行详细分析，分配适当的模块给新人完成，并对其代码
进行质量管控;   

<b>总结：</b>
> 该项目的主要开发目标是为母公司的宽带业务提供系统支持，提高参与宽带业务的同事的工作效率; 其主要功能是为客户提交新增/更新宽带业务的申请，组织管理新增/更新带宽所需的各项资源;   
> 在项目中使用 Extjs 开发前端，方便统一前端风格，简化前端布局工作，使得前端代码更加模块化；使用 Maven 进行包的依赖管理。使用 SpringMVC 进行 Web 项目的后端开发，项目结构主要被划分 Dao、Entity、Service、Controller 四层以提高代码的可复用性。

### QPoster
<b>技术栈：</b>
> Android, NDK

<b>负责描述：</b>
> 负责设计加解密系统的流程，与部分代码的实现；  
> 根据不同的使用场景开发不同版本进行适配；  
> 对 app 进行简单的反编译测试。   

<b>总结：</b>
> 该项目主要实现为公司生产的 LED 屏提供移动切换海报的目标，其主要功能是上传图片至 LED 屏，且可对图片进行简单的编辑。  
> 在项目中使用 NDK 开发加解密的核心代码，使用反射机制与代码混淆以提高 apk 的安全性。

## 工作经历
|时间 | 公司 |职位 |  
| ----| ---| --|  
|2018.07~至今| 云食城数字科技(广州)有限公司(原广州八味缘供应链管理有限公司)| 高级软件工程师（C#）  
|2016.07~2018.06| 电讯盈科萃锋科技有限公司|Java开发工程师  
|2015.07~2015.02| 江门彩立方光电科技有限公司| 实习Java开发工程师  

## 证书
> 中级软件设计师  
> CET6  

## 自我评价
> 本人对待工作积极主动，不畏困难，善于沟通，富有团结合作精神, 喜欢技术、热爱挑战，有较强的自我驱动力。  
作为开发人员，能够独立设计完成功能模块，能够快速熟悉开发环境与新项目，有设计中小型项目架构的经验。  
作为开发小组长期间，负责统筹安排上级派下的任务，协调组员完成功能任务；积极与产品经理沟通需求与优化方向，解决相关问题.
