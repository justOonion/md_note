分布式


消费者 群组
为什么使用群组？ 
消费效率的问题， 生产消息往往很快，消费消息一般很慢
	'.' 
	.'.
	
	1.一个主题的一个分区 只能被一个消费者消费
	2.先来后到。

消费者中的核心概念 
	存在多线程安全问题: 一个线程一个消费者
	群组协调
	分区再均衡
	消费安全问题
	
	
群组协调器 会定期心跳检测消费者的状态

分区再均衡 会触发 stw。 
 重复消费
 数据丢失

提交偏移量的方式
	自动提交 enable.auto.commit = true  默认**5s**钟提交一次偏移量
	手动提交(同步) consumer.commitSync() 阻塞直至提交成功
	手动提交（异步）consumer.commitAsync(new OffsetÇommitCallback(){} //回调函数)
	同步异步结合：正常异步提交，finally 同步提交
	特定提交（指定偏移量）
	
ConsumerRebalanceListener:分区再均衡监听器
	onPartitionsRevoked() 在分区再均衡之前触发 （偏移量写入数据库，提交偏移量）
	onPartitionsAssigned() 分区再均衡之后触发


消费者
	优雅退出： 通过另外一个线程 执行consumer.wakeup()
	独立消费者：
		没有群组的消费者，不需要订阅主题，只需要分配主题中的分区(可以多个分区）即可consumer.assign(partitionList)，没有群组协调器（不推荐）


优先副本： 

ISR: 可能会数据丢失
（why使用）因为全复制机制，会严重影响性能

如何避免数据丢失：ISR + ACKS=-1/all 

副本3 * 分区5
kafka文件分配遵循最小使用原则

普通消息
包装消息（压缩）多条压缩为一条

kafka清理机制 （key 分区）--- 同一个key 清理掉旧值

ACID ：
原子性，一致性，隔离型，永久性

kafka：目标是 做成一个大数据系统
1.消息的顺序
2.不完全leader选取 unclean.leader.election=false  不存在完全复制体，则不选leader
3.同步副本：和leader完全一致 副本数=3， min.insync.replication=2 最小同步副本


可靠的生产者
acks=all/-1()

可靠的消费者(不重复，不丢失)
提交偏移量（自动<时间间隔>，手动）
再均衡监听器（发生了再均衡的时候，通过额外的机制 事务，统一存储）





![kafka-workflow](/Users/chencheng/Doc/MD/kafka-workflow.png)

尽可能将任务分摊，让性能提升
配置：
log.dirs=/tmp/kafka-log_1,/tmp2/kafka-log_2,/tmp3/kafka-log_3

num.recovery.threads.per.data.dir=1
// 监听到消费者消费这个topic， 如果不存在，则创建
auto.create.topics.enable=true

// 新增topic时，创建的分区数
num.partitions=1

// 一个分区最多只能有一个消费者
如果一个topic需要20个消费者，那么至少需要20个分区

// 每一个分区日志保留时间 默认168h，7天
log.retention.hours=168

// 每一个分区的最大日志文件大小
log.retention.bytes=-1 // 无限制   （都设置的以先到的为准）

// 每一饿log文件的大小，超过这个值，就新开一个log文件存储
log.segment.bytes=1073741824

// 默认不开启
log.segment.ms= （都设置的以先到的为准）

// broker 能接受消息的最大字节数
message.max.bytes=10000000

消费端(可获取消息最大字节数 （应该比broker的message.max.bytes 的 大）)
fetch.message.max.bytes=1000000（默认1M）

kafka对cpu要求不高（消息解压缩）

保留多少数据，broker有多少空间可用

生产者需要配置序列化 key.serializer value.serializer
消费者要设置反序列化 key.deserializer value.deserializer












