- 服务之间可以通过http调用
	- http://t.zoukankan.com/dengbingbing-p-10390531.html
![](https://images2017.cnblogs.com/blog/1216496/201710/1216496-20171019104632287-239294179.png)


1. 在自己的服务里调用DaprSDK使用Dapr
	1. DaprSDK去使用Dapr是用protobuff这种数据格式（速度会比json快十倍）
2. 在compoments中配置要使用的数据库或者要用的别的东西
3. configuration有tarcking链路、logging日志、metric监控、consul服务发送
4. 然后调用invoke、invokebind调用要使用的数据库，对数据进行操作

各个服务可以通过dapr的consul相互调用