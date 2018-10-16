### 概述

+ mysql中索引的存储类型:BTree和Hash
+ 为什么:没有索引就从第一行开始找
+ 索引的类型依赖于存储引擎

### 优劣

+ 优:加快查询速度 唯一索引保证唯一性 分组和排序时可以减少分组排序时间
+ 劣:占用磁盘空间 增改查有性能损耗

### 分类

+ 普通索引和唯一索引
+ 单列索引和组合索引
+ 全文索引

### 设计原则

+ 索引并非多多益善
+ 经常更新的表  少索引
+ 数据量小 尽量不用索引
+ 不同值较少 不宜建立索引 比如性别这一列
+ 经常排序或分组 且是多列 建议组合索引

### Mysql索引使用方法

##### 普通索引

```msyql
CREATE TABLE `customer1` (
`customer_id` bigint(20) NOT NULL COMMENT '客户ID',
`customer_name` varchar(30) DEFAULT NULL COMMENT '客户姓名',
INDEX `idx_customer_id` (`customer_id`) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='客户表';
```

##### 唯一索引

```
CREATE TABLE `customer1` (
    `customer_id` BIGINT (20) NOT NULL COMMENT '客户ID',
    `customer_name` VARCHAR (30) DEFAULT NULL COMMENT '客户姓名',
    UNIQUE INDEX `idx_customer_id` (`customer_id`) USING BTREE
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4 COMMENT = '客户表';
```



##### 组合索引

```
CREATE TABLE `customer1` (
    `customer_id` BIGINT (20) NOT NULL COMMENT '客户ID',
    `customer_name` VARCHAR (30) DEFAULT NULL COMMENT '客户姓名',
     INDEX `idx_group_customer` (`customer_id`,`customer_name`) USING BTREE
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4 COMMENT = '客户表';

SHOW INDEX FROM customer1;
```



##### 全文索引

```
CREATE TABLE `customer1` (
    `customer_id` BIGINT (20) NOT NULL COMMENT '客户ID',
    `customer_name` VARCHAR (255) DEFAULT NULL COMMENT '客户姓名',
     FULLTEXT INDEX `idx_fulltext_customer_name` (`customer_name`)
) ENGINE = MyISAM DEFAULT CHARSET = utf8mb4 COMMENT = '客户表';

SHOW INDEX FROM customer1;
```



注:mysql中只有MyISAM引擎支持全文索引 默认是InnoDB 所以使用全文索引要指定引擎