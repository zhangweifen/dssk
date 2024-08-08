# dssk
! 拿着ods层的数据，做（维度表DIM跟明细表DWD），接着做DWS（由明细表DWD跟维度表DIM关联汇总而成）跟ADS
1.建立表的命名规则：
（1）维度表（DIM）命名规则：dim_用户_存储策略（增量表\拉链表）
(2)明细表（DWD）命名规则：
（3）汇总表（DWS）命名规则

1.（4）Flume生产环境采集数据如何选择（临时储存环境）
	如果下一级是Kafka，优先选择Kafka Channel。
	如果是金融、对钱要求准确的公司，选择File Channel。（不会丢数据）
	如果就是普通的日志，通常可以选择Memory Channel。
2.Kafka生产端分区分配策略：分区可以让数据均匀分布在不同的brokee(节点)上
（1）轮询
（2）随机：有代码可以随机
（3）自定义分区
**Kafka丢不丢数据**
1）Producer角度
acks=0，生产者发送过来数据就不管了，可靠性差，效率高；
acks=1，生产者发送过来数据Leader应答，可靠性中等，效率中等；
acks=-1，生产者发送过来数据Leader和ISR队列里面所有Follwer应答，可靠性高，效率低；
在生产环境中，acks=0很少使用；acks=1，一般用于传输普通日志，允许丢个别数据；acks=-1，一般用于传输和钱相关的数据，对可靠性要求比较高的场景。
2）Broker角度
	副本数大于等于2。
	min.insync.replicas大于等于2。
**Kafka数据重复**
去重= 幂等性+事务
4）总结
（1）生产者角度
	acks设置为-1 （acks=-1）。
	幂等性（enable.idempotence = true） + 事务 。
（2）broker服务端角度
	分区副本大于等于2 （--replication-factor 2）。
	ISR里应答的最小副本数量大于等于2 （min.insync.replicas = 2）。
（3）消费者
	事务 + 手动提交offset （enable.auto.commit = false）。
	消费者输出的目的地必须支持事务（MySQL、Kafka）。



