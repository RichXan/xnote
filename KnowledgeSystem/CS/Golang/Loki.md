![loki架构](https://pic1.zhimg.com/v2-6c6c7434fe8103e41a7401aaf8e7c7f8_b.png)
![ingester存储不同数据类型](https://pic1.zhimg.com/v2-866ddedf1c0b108e029091939b6d7390_b.png)
- Distributor：
	- promtail收集日志然后发给loki，Distributor就是第一个接收到日志的组件
	- Loki通过构建压缩数据块来实现批处理和压缩数据
- ingester：
	- 有状态的组件、负责构建和刷新chunck，当chunck达到一定数量或时间后刷新到内存中
	- 每个流的日志对应一个ingester
	- 当日志到达Distributor后，根据元数据和hash算法计算出应该到哪个ingester上面
	- 对块和索引使用单独的数据库，因为它们存储的数据类型不同
- Querier：
	- 由Querier负责给一个时间范围和标签选择器
	- Querier查看索引以确定哪些块匹配，并通过greps将结果显示出来，它还从Ingester获取尚未刷新的最新数据
	- 对于每个查询，一个查询器将为您显示所有相关日志。实现了查询并行化，提供分布式grep，即使是大型查询也是足够的
- Loki的索引存储可以是cassandra/bigtable/dynamodb
- chuncks可以是各种对象存储
- Querier和Distributor都是无状态的组件
- 对于ingester他虽然是有状态的 但当新的节点加入或者减少整节点间的chunk会重新分配，以适应新的散列环




- Compactor：存储index实体