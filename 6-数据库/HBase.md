笔记来源 https://juejin.im/post/6844903753271754759#heading-4

# 逻辑存储结构
![image1](https://github.com/interviewBATTMD/interviewBATTMD/blob/master/6-%E6%95%B0%E6%8D%AE%E5%BA%93/HBase.png)

# HBase 中的 KEY 组成
KEY 的组成是以 Row key 、CF(Column Family) 、Column 和 TimeStamp 组成的。

TimeStamp 在 HBase 中充当的作用就是版本号

![image2](https://github.com/interviewBATTMD/interviewBATTMD/blob/master/6-%E6%95%B0%E6%8D%AE%E5%BA%93/data.png)

# Region Server 和 Region 的关系
* 一个 Region Server 就是一个机器节点(服务器)

* 一个 Region Server 包含着多个 Region

* 一个 Region 包含着多个列簇 (CF)

* 一个 Region Server 中可以有多张 Table，一张 Table 可以有多个 Region

![image3](https://github.com/interviewBATTMD/interviewBATTMD/blob/master/6-%E6%95%B0%E6%8D%AE%E5%BA%93/Region%20Server.png)

# Hbase读取数据的过程
Client 请求读取数据时，先转发到 ZK 集群，在 ZK 集群中寻找到相对应的 Region Server，再找到对应的 Region，

* 先查 MemStore，如果在 MemStore 中获取到数据，则直接返回，

* 否则再由 Region 找到对应的 Store File，查到具体的数据。

在整个架构中，HMaster 和 HRegion Server 可以是同一个节点上，可以有多个 HMaster 存在，但是只有一个 HMaster 在活跃。

**在 Client 端会进行 rowkey-> HRegion 映射关系的缓存** ，降低下次寻址的压力。

# HBase 写入数据的过程
先是 Client 进行发起数据的插入请求，

* 如果 Client 本身存储了关于 Rowkey 和 Region 的映射关系的话，那么就会先查找到具体的对应关系，

* 如果没有的话，就会在ZK中进行查找到对应 Region server，然后再转发到具体的 Region 上。

所有的数据在写入的时候先是记录在 WAL 中，同时检查关于 MemStore 是否满了，

* 如果是满了，那么就会进行刷盘，输出到一个 Hfile 中，

* 如果没有满的话，那么就是先写进 Memstore 中，然后再刷到 WAL 中。

