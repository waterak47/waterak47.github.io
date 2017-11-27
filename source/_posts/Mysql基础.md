---
title: MySQL基础
date: 2017-11-27 14:09:23
tags: MySQL基础
categories: MySQL
---

一些Mysql的基础内容

<!--more-->

### 登陆MySQL数据库
```
mysql -u your_user_name -p Enter password: your_passwor;
```
![](https://ws3.sinaimg.cn/large/006tKfTcly1flwmifn817j30mb08ita3.jpg)

### 查询数据库
```
show databases;
```
![](https://ws3.sinaimg.cn/large/006tKfTcgy1flwmkj7qmqj30go087q3h.jpg)

### 创建数据库mydb
```
create database mydb;
```
![](https://ws3.sinaimg.cn/large/006tKfTcgy1flwmmeamc2j30go087q3h.jpg)

### 使用指定数据库
```
use mydb;
```
![](https://ws4.sinaimg.cn/large/006tKfTcgy1flwmmzsbuvj30bn02bdfx.jpg)

### 查看当前数据库
```
select database();
```
![](https://ws3.sinaimg.cn/large/006tKfTcgy1flwmnogn05j30bm05ower.jpg)

### 显示数据库
```
show databases;
```
![](https://ws2.sinaimg.cn/large/006tKfTcgy1flwmo84jrsj3091084t95.jpg)

### 创建表格
```
create table customer(
	mid char(5) primary key,
	nam varchar(20),
	birh datetime,
	sex char(1) default'0'
);
```
![](https://ws1.sinaimg.cn/large/006tKfTcly1flwmq9l0wxj30cy05t3z3.jpg)

### 数据库有哪些表
```
show tables;
```
![](https://ws3.sinaimg.cn/large/006tKfTcly1flwmrrtjm7j30a505wmxg.jpg)

### 表结构
```
desc customer;
```
![](https://ws2.sinaimg.cn/large/006tKfTcly1flwmsuqjvuj30fq07jt9h.jpg)

### 删除表
```
drop table customer;
```
![](https://ws3.sinaimg.cn/large/006tKfTcly1flwmu1hu8kj30b902raa8.jpg)

### 插入数据
```
-- 插入一条记录
INSERT INTO customer values("D0001", "Jack", "1990-06-01", "1");
INSERT INTO customer (mid, nam, birh, sex) values("D0002", "Mike", "1991-12-01", "1");

-- 插入多条记录
INSERT INTO customer values
("D0003", "Jack3", "1993-06-01", "1"), 
("D0004", "Jack4", "1994-06-01", "1"), 
("D0005", "Jack5", "1995-06-01", "1");
```
![](https://ws4.sinaimg.cn/large/006tKfTcgy1flwnwyll34j30mf0ezabz.jpg)

### 查询数据
```
select * from customer;
```
![](https://ws4.sinaimg.cn/large/006tKfTcly1flwny6n7bkj30d706gmxm.jpg)

### 自增序列`AUTO_INCREMENT`
```
-- 创建表
create table good (
	id INT AUTO_INCREMENT PRIMARY KEY,
	nam VARCHAR(30)
);

-- 插入数据
INSERT INTO customer (nam) values("jack"), ("mike"), ("franck");
```
![](https://ws1.sinaimg.cn/large/006tKfTcgy1flwo55zwiwj30li099wfg.jpg)

### 更新数据
```
update good set nam="jack123" where id=1;
```
![](https://ws2.sinaimg.cn/large/006tKfTcgy1flwo7cxrqrj30g608bgmf.jpg)

### 删除数据
```
delete from good where id=1;
```
![](https://ws3.sinaimg.cn/large/006tKfTcgy1flwo8gu89xj30dq0efab7.jpg)


### 查询指定

```
select 列名1, 列名2 ... from 表名 [条件表达式];
select nam from good;
```

### 模糊查询

```
select nam from good where nam like 'f%';
```

### 条件组合查询： NOT, AND, OR

```
select * from customer where birh is not null and mid='D0002';
```
![](https://ws2.sinaimg.cn/large/006tKfTcgy1flwoiycbgaj30lf0c7myh.jpg)

### 查询结果排序：ASC升序 DESC降序
```
select * from customer where birh is not null order by birh desc;
select * from customer order by birh desc;
```

![](https://ws3.sinaimg.cn/large/006tKfTcgy1flwon04znhj30md0e6jt5.jpg)

### 分组 GROUP By

| 函数 | 说明 |
| ------| ------ |
| AVG | 平均值 |
| COUNT | 计数 |
| MIN | 最小值 |
| MAX | 最大值 |
| SUM | 求和 |

```
select count(nam), sex from customer group by sex;
```

![](https://ws4.sinaimg.cn/large/006tKfTcgy1flwot18gqrj30ja04xmxg.jpg)




### 删除数据库
```
drop database mydb;
```

