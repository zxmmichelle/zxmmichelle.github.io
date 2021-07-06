## 2021年7月2日
> 之前有个非业务需求，提供一个在web端手动清缓存的功能。这个功能乍听很简单，但是！我司系统的缓存结构有六成是这种模式{prefix}__XX_，并且我司用的是MemCached!!
> 
> 曾经我不觉得MemCached和Redis的差距有多大，现在我感受到了（捂脸哭
> 
> MemCached不支持对key的模糊查询，但Redis支持啊！！！
> 哎，应该早点换了它的。

## 2021年7月6日
> 昨晚试了一下SpringBoot的RabbitTemplate，跟.Net版本的rabbitmq.client非常不一样。  
> 
> 由于项目中用的是.Net Core，先记录一下在项目中是如何解决RabbitMQ连接断开的问题。  
>> 代码一开始复用了ConnectionFactory，但上测试服频频出现无端断开连接导致mq发送失败的情况，因此我决定升级这部分前人写的代码。  
>> 
>> 看了看官方文档（真的太难了），官方建议复用一个Connection，在Connection中创建多个Channel，降低创建连接的频率，减少不必要的开支。  
>> 
>> 改的第一版复用了Connection，针对不同的Exchange都开了一个Channel并保持连接。我以为这就是终点。  
>> 
>> 结果上测试服后只是比原来断开的频率降低一半，还是会出现断开导致mq发送失败的情况，并且没有相关的错误日志可以用来排查问题。（当时裂开，我还是太稚嫩了  
>> 
>> 所以，是不是Channel保持连接的方式不对？能不能用类似数据库连接池的方式？  
>> 调整思考方向后，在StackOverFlow上发现了一个大神！他提到.Net Core提供了一个叫IObjectPool的接口，配合IPooledObjectPolicy、ObjectPoolProvider能够很方便地实现自定义对象池~  
>> 关于这部分的实现，引用一下另一位大神的的文章 _https://blog.csdn.net/qinyuanpei/article/details/108225448_
>
> .Net Core版本的Channel池告一段落，再看看RabbitTemplate。  
> 看了一篇对RabbitTemplate池化的博客，这里池化的是RabbitTemplate，针对不同的Exchange，创建一个RabbitTemplate，用ConcurrentHashMap保存信息。  
> 看完这个博客，我有些疑惑，RabbitTemplate是实现了Channel的池化吗？  
> RabbitTemplate源码 https://github.com/spring-projects/spring-amqp/blob/e3af31bf9a4be561fb3f8f642a3093e8add07d92/spring-rabbit/src/main/java/org/springframework/amqp/rabbit/core/RabbitTemplate.java#L1006
