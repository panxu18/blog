### MySQL日志

主要由3种日志，undolog, redolog binlog下面整理这三种日志的作用。

#### redolog

redoLog保证事务的原子性和持久性，是InnoDB存储引擎特有的一种物理日志，负责把事务对数据库的所有修改都记录下来。redo log存储数据修改之后的值，不管事务是否提交都会记录下来。当电脑数据库掉电，InnoDB存储引擎会使用redo log恢复到掉电前的时刻，以此来保证数据的完整性。

在一条更新语句进行执行的时候，InnoDB引擎会把更新记录写道redo log日志中，然后更新内存，此时算是语句执行完了，然后在空闲的时候或者按照设定的更新策略将redo log中的内容更新到磁盘中。

redo log的大小是固定的，即记录满了以后就从头循环写。

![181121105137361](image\181121105137361.jpg)

#### binlog

binlog属于MySQL Server层，又称为归档日志，属于路积极日志，以二进制的形式记录这个语句的原始逻辑。binlog用于数据复制，在主从复制中，从库利用主库上的binlog进行重播，实现主从同步。还以用于数据库基于时间点的还原。

#### redo log和binlog的区别

- redo log是InnoDB层面的，binlog属于MySQL Server层面的。
- redo log是物理日志，记录该数据更新的内容；binlog是逻辑日志，记录这个更新语句的原始逻辑。
- redo log是循环写，日志空间大小定；binlog是追加写，当一份日志写到一定大小时会更换下一个文件，不会覆盖。

#### undo log

undo log保证事务的原子性，如果事务执行了一半就结束了，但执行过程中已经修改了数据，为了保证事务的原子性，就需要把数据恢复为原来的样子，所以引入了undolog来提供回滚功能。undo log是逻辑日志，当delete一条记录是，undo log中会对应一条insert记录，当update一条记录是，他会记录一条对应相反的updte记录。

undo log还能实现多版本并发控制（MVCC），当用户读取一行记录的时候，如果该记录被其他事务占用，当前事务可以通过undolog读取之前的行信息，以此实现非锁定读。

### InnoDB事务记录日志的流程

例如一个数据A由1修改为2的事务流程如下

1. 事务开启 T1。
2. 写undo日志<T1, A, 1>到log buffer。
3. 将修改A为2，并写redo日志<T1, A, 2>到log buffer。
4. 如果 innodb_flush_log_at_trx_commit=1，则将redo日志写到log file，并刷新落盘。
5. 提交事务。

### InnoDB数据恢复

redo log会记录所有事务的操作，未提交的事务，和回滚的了的事务都会记录在redo log中，所以在恢复时需要先将所有事务的修改全部重做，然后通过undo log回滚那些未提交的事务。因为恢复需要使用到undo日志，所以需要在记录redo log之前需要将undo log持久化，为了简化流程，InnoDB将undo log也看做是数据，对undo log的操作也写入redo log，这样就可以把undo log当成数据缓存在内存，然后在恢复的时候根据redo log恢复undo log。

##### redo log的两阶段提交

1. 两阶段提交
   1. prepare阶段，SQL执行成功，并生成事务ID以及redo和undo内存日志，此阶段审查的redolog只记录事务的操作日志，没记录提交日志。
   2. 写入binlog，先调用write()将binlog写入内存，然后调用fsync()写入磁盘。
   3. commit阶段，在redolog中记录此事务的提交日志，增加commit标签。

2. 恢复过程

   1. 顺序扫描redolog，如果碰到commit的redolog直接提交。

   2. 如果只有prepare，根据事务id去binlog找对应事务的提交记录，如果有记录就提交事务，否则回滚事务。

      

      

参考 https://segmentfault.com/a/1190000023897572





