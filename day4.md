# day4

​																																																				时间：2022年4月9日22:02:56

## 目录

- Spring中数据库的传播特性
- Spring中数据库的隔离级别

## 一、Spring支持的常用数据库事务传播属性

### 1.事务的属性:

#### a.propagation:用来设置事务的传播行为

​	事务的传播行为:一个方法运行在了一个开启了事务的方法中时，当前方法是使用原来的事务还是开启一个新的事务

- Propagation.REQUIRED:默认值，使用原来的事务，**失败的时候会导致所有的子事务全部回滚**

- Propagation.REQUIRES_NEW:将原来的事务挂起，开启一个新的事务，**失败的时候仅仅回滚失败的子事务**

#### b.isolation:用来设置事务的隔离级别

- Isolation. REPEATABLE_READ:可重复读，MySQL默认的隔离级别
- Isolation.READ_COMMITTED:读已提交，Oracle默认的隔离级别，**开发时通常使用的隔离级别**

#### c.隔离值(7个)



|   传播属性    |                             描述                             |
| :-----------: | :----------------------------------------------------------: |
|   REQUIRED    | 如果有事务在运行，当前的方法就在这个事务内运行，否则，就启<br/>动-一个新的事务，并在自己的事务内运行 |
| REQUIRES_NEW  | 当前的方法必须启动新事务，并在它自己的事务内运行．如果有事
务正在运行，应该将它挂起 |
|   SUPPORTS    | 如果有事务在运行，当前的方法就在这个事务内运行.否则它可以不运行在事务中. |
| NOT_SUPPERTED |   当前的方法不应该运行在事务中．如果有运行的事务，将它挂起   |
|   MANDATORY   | 当前的方法必须运行在事务内部，如果没有正在运行的事务，就抛出异常 |
|     NEVER     |  当前的方法不应该运行在事务中．如果有运行的事务，就抛出异常  |
|    NESTED     | 如果有事务在运行，当前的方法就应该在这个事务的嵌套事务内运行．否则，就启动-个新的事务，并在它自己的事务内运行. |

#### d.使用

**@Transactional(propagation=Propagation.ReQUIRED)**

<img src="D:\桌面\面试题\pic\day4\1649514757432.png" alt="1649514757432" style="zoom:67%;" />

<img src="D:\桌面\面试题\pic\day4\1649514805787.png" alt="1649514805787" style="zoom: 67%;" />

```
结束即提交
```



## 二、数据库的隔离级别（数据库事务并发问题）

### 1.概念

#### a.脏读

- Transaction01将某条记录的AGE值从20修改为30。

- Transaction02读取了Transaction01更新后的值:30。

- Transaction01回滚，AGE 值恢复到了20。

- Transaction02读取到的30就是一个无效的值。

#### b.幻读

- Transaction01读取了AGE 值为20。

- Transaction02将AGE值修改为30。‘

- Transaction01再次读取AGE值为30，和第一次读取不一致。“

#### c.不可重复读

- Transaction01读取了STUDENT 表中的一部分数据

- Transaction02向STUDENT表中插入了新的行。

- Transaction01读取了STUDENT表时，多出了一些行。

### 2.隔离级别

数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题。一个事务与其他事务隔离的程度称为**隔离级别**。SQL标准中规定了多种事务隔离级别,不同隔离级别对应不同的干扰程度,隔离级别越高，数据一致性就越好，但并发性越弱。

#### a.读未提交:READ UNCOMMITTED

允许Transaction01读取Transaction02未提交的修改。

#### b.读已提交:READ COMMITTED（常用）

要求Transaction01只能读取Transaction02已提交的修改。**(避免脏读)**

#### c.可重复读:REPEATABLE READ

确保Transaction01可以多次从一个字段中读取到相同的值，即 Transaction01执行期间禁止其它事务对这个字段进行更新**（脏读、不可重复度）**

#### d.序列化:SERIALIZABLE

确保Transaction01可以多次从一个表中读取到相同的行在 Transaction01执行期间，禁止其它事务对这个表进行添加、更新、删除操作。可以避免任何并发问题，但性能十分低下。**（全部避免）**

##### 	隔离表

|                              | 脏读 | 幻读 | 不可重复读 |
| :--------------------------: | :--: | :--: | :--------: |
| READ UNCOMMITTED（读未提交） |  有  |  有  |     有     |
|  READ COMMITTED（读已提交）  |  无  |  有  |     有     |
| REPEATABLE READ（可重复读）  |  无  |  无  |     有     |
|    SERIALIZABLE（序列化）    |  无  |  无  |     无     |

```
mysql支持全部的隔离级别，默认是可重复读
oracle只支持READ COMMITTED和SERIALIZABLE，默认是读已提交
```

##### 使用

```
@Transactional(isolation=Isolation.DEFAULT)
```

