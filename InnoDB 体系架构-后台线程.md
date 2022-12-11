## InnoDB 体系架构：后台线程

### Master Thread

> 负责将缓冲池中的数据一步刷新到磁盘



### IO Thread

> InnoDB引擎中大量使用了 AIO（Asyn IO）来处理写IO请求，这样可以极大提高数据库性能。而IO Thread 的工作主要是负责这些IO请求的回调处理。

共有4类IO线程:

1. write

   数量： 1（InnoDB 1.0）4（InnoDB 1.0.x）

   参数：innodb_write_io_threads

2. read

   数量：1 （InnoDB 1.0）4（InnoDB 1.0.x）

   参数：innodb_read_io_threads

3. Insert buffer

   数量：1

4. log

   数量：1

### Purge Thread

> 主要作用是处理回滚日志（undolog），没当事务被提交后，该事务所使用的undolog可能不再需要，这个时候需要Purge Thread 来回收已经使用并分配的undo 页。

数量：1（InnoDB 1.1）支持多个（InnoDB 1.2 开始）

参数：innodb_purge_threads

### Page Cleaner Thread

> 主要作用是把脏页刷新到磁盘。