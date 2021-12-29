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

## 2021年12月28日
> 生产服出了个神奇的bug，其中一个consumer一直没办法正确ack/nack消息。
> 在开发服模拟的时候，发现从第二条消息开始，无论ack还是nack都会触发这个错误。
> ```
> RabbitMQ.Client.Exceptions.AlreadyClosedException: 'Already closed: The AMQP operation was interrupted: AMQP close-reason, initiated by Peer, code=406, text='PRECONDITION_FAILED - unknown delivery tag 1', classId=60, methodId=80'
> ```
> 根据大佬们给出的solution，如下图:
> ![image](https://user-images.githubusercontent.com/43370259/147526688-9771a499-8240-4bad-a322-1b6bacd1ec93.png)  
> 一一排查Debug，channel number一直是一致，ack/nack的顺序也不是乱序的，啊，那问题在哪里。。  
> 于是，我从类初始化开始，到执行任务的所有关键步骤都打了断点，发现进入两次Init()！！！  
> 两次！！两次！！！原来问题在这里T^T，含泪调整了逻辑，终于修复了。。  
> 再来梳理一下逻辑，逻辑中的消费模式设定为一对一模式，但是是因为两次进了Init()，也就是在同一个channel里加了两个一样的consumer（指业务意义），于是就。。  

## 2021年12月29日
> 对同一个channel添加两个一样的consumer，是work模式。
> 如果要实现其模式，需要在初始化channel时，加一句代码：  
> ```C#
> channel.BasicQos(prefetchSize: 0, prefetchCount: 2, global: false) 
> // prefetchSize：0为长度不限；global：是否将该设置应用到connection下的所有channel
> ```
