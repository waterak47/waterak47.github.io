---
title: Java基础：commons-dbutils使用
date: 2017-12-05 19:48:27
comments: true
tags: Java基础
categories: Java
---

## 介绍commons-dbutils

commons-dbutils是Apache组织提供的一个开源 JDBC工具类库，能让我们更简单的使用JDBC。

<!--more-->

## commons-dbutils常用类

### QueryRunner类

```txt
QueryRunner中提供对sql语句操作的API
它主要有三个方法：
    * query()用于执行select
        该方法会自行处理 PreparedStatement 和 ResultSet 的创建和关闭。

    * update()用于执行 insert update delete
    * batch() 批处理
```

### ResultSetHandler接口

```txt
定义select操作后，怎样封装结果集。
以下实现ResultSetHandler接口类
```

> 
ArrayHandler： 把结果集中的第一行数据转成对象数组。
ArrayListHandler： 把结果集中的每一行数据都转成一个数组，再存放在list中
BeanHandler: 把结果集中的第一行数据封装到一个对应的JavaBean实例中
BeanListHandler: 将结果集中的每一行数据都封装到一个对应的JavaBean实例中，存放到List里
ColumnListHandler: 将结果集中某一列的数据存放到List中。
KeyedHandler(name)：将结果集中的每一行数据都封装到一个Map<列名,列值>里，再把这些map再存到一个map里，其key为指定的key。
MapHandler: 将结果集中的第一样数据封装到一个Map里，key是列明，value就是对应的值
MapListHandler：将结果集中的每一行数据都封装到一个Map里，然后再存放到List


实例代码:
```java
package JDBC;

import DBTools.DemlClass1;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.*;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;
import java.util.Map;

public class JDBCDemo7 {

    public static void main(String[] args) throws Exception {
//        arrayHandlerDemo();
//        arrayListHandlerDemo();
//        beanHandlerDemo();
//        beanListHandlerDemo();
//        columnListHandlerDemo();
//        mapHandlerDemo();
//        mapListHandlerDemo();
        keyedHandlerDemo();
    }

    public static void temp() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        ArrayHandler arrayHandler = new ArrayHandler();
        ArrayListHandler arrayListHandler = new ArrayListHandler();

        try {
            String selectSql = "select * from customer";
            DemlClass1 result = queryRunner.query(conn, selectSql, new BeanHandler<DemlClass1>(DemlClass1.class));

            System.out.println(result.toString());

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    //ArrayHandler
    public static void arrayHandlerDemo() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        ArrayHandler arrayHandler = new ArrayHandler();

        try {
            String selectSql = "select * from customer";
            Object[] result = queryRunner.query(conn, selectSql, arrayHandler);

            for (Object obj : result) {
                System.out.print(obj + "\t");
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    //ArrayListHandler
    public static void arrayListHandlerDemo() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        ArrayListHandler arrayListHandler = new ArrayListHandler();

        try {
            String selectSql = "select * from customer";
            List<Object[]> result = queryRunner.query(conn, selectSql, arrayListHandler);

            for (Object[] obj : result) {
                for (Object o : obj) {
                    System.out.print(o + "\t");
                }
                System.out.println();
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    //BeanHandler
    public static void beanHandlerDemo() {

        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        BeanHandler<Customer> beanHandler = new BeanHandler<Customer>(Customer.class);

        try {
            String selectSql = "select * from customer";
            Customer customer = queryRunner.query(conn, selectSql, beanHandler);
            System.out.println(customer);

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    //BeanListHandler
    public static void beanListHandlerDemo() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        BeanListHandler<Customer> beanListHandler = new BeanListHandler<Customer>(Customer.class);

        try {
            String selectSql = "select * from customer";
            List<Customer> customers = queryRunner.query(conn, selectSql, beanListHandler);

            for (Customer customer : customers) {
                System.out.println(customer);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

    }

    //ColumnListHandler
    public static void columnListHandlerDemo() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        ColumnListHandler columnListHandler = new ColumnListHandler("nam");

        try {
            String selectSql = "select * from customer";
            Object result = queryRunner.query(conn, selectSql, columnListHandler);

            System.out.println(result);

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    //MapHandler
    public static void mapHandlerDemo() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        MapHandler mapHandler = new MapHandler();

        try {
            String selectSql = "select * from customer";
            Map<String, Object> result = queryRunner.query(conn, selectSql, mapHandler);

            for (Map.Entry<String, Object> item : result.entrySet()) {
                System.out.println(item.getKey() + ": " + item.getValue());
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    //MapListHandler
    public static void mapListHandlerDemo() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        MapListHandler mapListHandler = new MapListHandler();

        try {
            String selectSql = "select * from customer";
            List<Map<String, Object>> result = queryRunner.query(conn, selectSql, mapListHandler);

            for (Map<String, Object> item : result) {
                for (Map.Entry<String, Object> data : item.entrySet()) {
                    System.out.print("Key: " + data.getKey() + " Value: " + data.getValue());
                }
                System.out.println();
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    //KeyedHandler
    public static void keyedHandlerDemo() {
        Connection conn = DBUtilsConfig.getConnection();
        QueryRunner queryRunner = new QueryRunner();
        KeyedHandler keyedHandler = new KeyedHandler("nam");

        try {
            String selectSql = "select * from customer";
            Object result = queryRunner.query(conn, selectSql, keyedHandler);
            Map<Object, Map<String, Object>> data = (Map<Object, Map<String, Object>>)result;
            System.out.println(result);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```



参考：
http://www.jianshu.com/p/23987c675450

