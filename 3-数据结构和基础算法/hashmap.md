## 哈希表解决冲突的方式
开放地址法，链地址法，重哈希，开放地址法有线性探测法、二次探测法。  
构造哈希表的方法：  
除数取余法，随机法，平方后取中间某几位  

## STL unordered_map
unordered_map实现是通过bucket vector + bucket list实现，即通过链地址法来实现的一种hashmap，理想状态下每个key都分布在不同的bucket中，这样读写的时间复杂度是O(1)，但实际上会有hash冲突，多个key在同一个bucket下，构成bucket list。

## Google sparse_hash_map和dense_hash_map
sparse_hash_table维护一个groups的vector，每个group下保存实际sparse_hash_map的pair<key, T>, T 里面是个vector来保存存在的value，bitmap来标识value是否存在，所以实际上不管是bucket还是value都是存储在vector中，相对于unordered_map的拉链式实现，会节省不少内存，而且通过bitmap来判断value是否在，find和insert都是O(1)，并且哈希冲突对性能的影响很小。  

dense_hash_map

https://blog.csdn.net/jollyjumper/article/details/23627037?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.add_param_isCf  

# JAVA
## HashMap
拉链法  
HashMap中默认的存储大小就是一个容量为16的数组。当HashMap存储元素达到当前元素的75%时，HashMap的存储空间会扩大2倍。  
## SparseArray稀疏矩阵
SparseArray比HashMap更省内存，某些条件下性能更好，主要是因为它避免了对key的自动装箱。  
SparseArray内存使用两个数组来存储key和value，并且在存储和查找数据时，都使用二分查找法，因此SparseArray内部的key都是有序的。  
