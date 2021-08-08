## 2021年8月8日
> 记录一些在使用ES的过程中零散的点。
>- 项目中的高频静态数据, 例如进入财务阶段的订单数据或凭证等.
>- 项目中使用单机es提供服务，在部署启动时，如果出现service_unavailable/1/state错误时，查配置文件/etc/elasticsearch/elasticsearch.yml(ubuntu环境, es版本：7.9)
>```
> 修改discovery.type: single-node
>```
>- ES 7.0以前的层级结构：index → type → doc → attr; 7.0以后: [type被移除](https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html)
>- Luecene中没有update操作，所以ES提供的update方法，实际上是将原记录标记为“待删除”，然后插入新doc，重建索引。   
>- keyword与text: keyword会自动建立索引且可用于排序,聚合; text需要进行分词再建立索引, 不支持排序,聚合;
>- 如果数据结构是两层或两层以上, 不建议用nest, 因为其update操作的代价很大, 需要reindexing root obj和其他的nested obj;  
>- 接上一点, 可以使用parent-child, 缺点是必须在同一个分片(shard)中;
>- term不适用于text字段中, 因为它是精确匹配, 类似match_phrase, 但会导致ES做analysis, 建议用match或match_phrase代替;

> 关于Routing
>- 指路由, 用于定位到index partition;
>- Routing只在多节点的场景下有效;
>- 在join查询中无效;
>- 在parent-child中, child的Routing是必须的, parent如果没有指定Routing, 则会默认用_id, shrad_num=hash(routing)%num-primary_shards;
>
> Routing to an index partition
>- 默认的routing是id, 当用了custom routing时(在一些需要使routing值一样的数据放在同一个shard中以减少查询分片数的场景), 很容易导致数据倾斜, 所以可用index.routing_partition_size避免一定程度的数据倾斜; 其计算公式为
>```
> shrad_num=(hash(routing)+hash(_id)%routing_partition_size)%num_primary_shards
>```



