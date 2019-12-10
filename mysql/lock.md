### 全局锁
flush tables with read lock (ftwrl)
执行此命令后，该数据库的所有写操作将被阻塞
典型使用场景：全库逻辑备份

## 使用优点：
1. 替代事务隔离，防止数据库引擎不支持事务隔离（mylsam不支持事务）
   可重复读开启后，可拿到一致性试图
   官方自带逻辑备份工具mysqldump使用参数 -single-transaction即可开启事务拿到一致性试图
2. 替代set global readonly = true 
   在客户端异常断开的时候，全局锁可以释放，而readonly不会
   readonly会被用来做其他逻辑，比如判断主从，故修改global变量的方式影响更大

### 表锁
lock table read
本会话和其他会话均可读，不可写
lock table write
持有锁的会话可以读写表，其他会话不可读写

## 特点：
1. 会话断开，锁解除
2. 不建议使用，锁表影响较大，建议使用更细粒度锁

### MDL锁
mysql5.5后自带，无需显示使用

## 特点：
1. 读锁不互斥
2. 读写锁互斥，写锁互斥

举例如下：
![](image/MDL锁示例.jpg)
我们可以看到 session A 先启动，这时候会对表 t 加一个 MDL 读锁。由于 session B 需要的也是 MDL 读锁，因此可以正常执行。之后 session C 会被 blocked，是因为 session A 的 MDL 读锁还没有释放，而 session C 需要 MDL 写锁，因此只能被阻塞。如果只有 session C 自己被阻塞还没什么关系，但是之后所有要在表 t 上新申请 MDL 读锁的请求也会被 session C 阻塞。前面我们说了，所有对表的增删改查操作都需要先申请 MDL 读锁，就都被锁住，等于这个表现在完全不可读写了。

如果某个表上的查询语句频繁，而且客户端有重试机制，也就是说超时后会再起一个新 session 再请求的话，这个库的线程很快就会爆满。





