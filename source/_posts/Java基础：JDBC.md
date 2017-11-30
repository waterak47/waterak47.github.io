---
title: Java基础：JDBC
date: 2017-11-28 15:16:57
tags: Java基础
categories: Java
---
## 什么是JDBC

JDBC 指 Java 数据库连接，是一种标准Java应用编程接口（ JAVA API），用来连接 Java 编程语言和广泛的数据库。
JDBC API 库包含下面提到的每个任务，都是与数据库相关的常用用法。

> 
- 制作到数据库的连接。
- 创建 SQL 或 MySQL 语句。
- 执行 SQL 或 MySQL 查询数据库。
- 查看和修改所产生的记录。

从根本上来说，JDBC 是一种规范，它提供了一套完整的接口，允许便携式访问到底层数据库，因此可以用 Java 编写不同类型的可执行文件，例如：

> 
- Java 应用程序
- Java Applets
- Java Servlets
- Java ServerPages (JSPs)
- Enterprise JavaBeans (EJBs)

所有这些不同的可执行文件就可以使用 JDBC 驱动程序来访问数据库，这样可以方便的访问数据。
JDBC 具有 ODBC 一样的性能，允许 Java 程序包含与数据库无关的代码。

<!--more-->

## 数据库相关代码

请导入：
```sql
/*
 Navicat Premium Data Transfer

 Source Server         : JavaDev
 Source Server Type    : MySQL
 Source Server Version : 50720
 Source Host           : 127.0.0.1:33060
 Source Schema         : mydb

 Target Server Type    : MySQL
 Target Server Version : 50720
 File Encoding         : 65001

 Date: 30/11/2017 17:35:10
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for customer
-- ----------------------------
DROP TABLE IF EXISTS `customer`;
CREATE TABLE `customer` (
  `mid` char(5) NOT NULL,
  `nam` varchar(20) DEFAULT NULL,
  `birh` datetime DEFAULT NULL,
  `sex` char(1) DEFAULT '0',
  PRIMARY KEY (`mid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- ----------------------------
-- Records of customer
-- ----------------------------
BEGIN;
INSERT INTO `customer` VALUES ('D0001', 'Jack1', '1990-06-01 00:00:00', '1');
INSERT INTO `customer` VALUES ('D0002', 'Jack2', '1990-06-01 00:00:00', '1');
INSERT INTO `customer` VALUES ('D0003', 'Jack3', '1993-06-01 00:00:00', '1');
INSERT INTO `customer` VALUES ('D0004', 'Jack4', '1994-06-01 00:00:00', '1');
INSERT INTO `customer` VALUES ('D0005', 'Jack5', '1995-06-01 00:00:00', '1');
INSERT INTO `customer` VALUES ('D0006', 'Jack6', '1995-06-01 00:00:00', '1');
INSERT INTO `customer` VALUES ('D0007', 'Jack7', '1996-06-01 00:00:00', '0');
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;

```

## JDBC编程步骤

> 
1.加载驱动程序
通过DriverManager加载驱动程序driver
2.建立连接(提供JDBC连接的URL)
通过DriverManager类获得表示数据库连接的Connection类对象
3.操作数据
通过Connection对象绑定要执行的语句，生成Statement类对象
执行SQL语句，接收执行结果集ResultSet
可选的对结果集ResultSet类对象的处理
4.释放资源
必要的关闭ResultSet、Statement和Connection

## JDBC代码

### 连接数据库
```java
package JDBC;

import java.sql.*;

public class JDBCDemo1 {

    private static String user = "vagrant";
    private static String password = "vagrant";

    public static void main(String[] args) throws Exception {
        try {
            //加载驱动程序
            Class.forName("com.mysql.jdbc.Driver");
            System.out.println("加载驱动成功");
        } catch (ClassNotFoundException e) {
            System.out.println("加载驱动失败");
            e.printStackTrace();
        }

        //JDBC的URL
        String url = "jdbc:mysql://127.0.0.1:33060/";
        Connection connection = null;

        try {
            //调用DriverManager对象的getConnection()方法，获得一个Connection对象
            connection = DriverManager.getConnection(url, user, password);
            Statement state = connection.createStatement();
            System.out.println("连接成功");
            state.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }
}

```

### 新增数据

```java
package JDBC;

import java.sql.*;

public class JDBCDemo2 {

    private static String user = "vagrant";
    private static String password = "vagrant";

    public static void main(String[] args) throws Exception {
        try {
            //加载驱动程序
            Class.forName("com.mysql.jdbc.Driver");
            System.out.println("加载驱动成功");
        } catch (ClassNotFoundException e) {
            System.out.println("加载驱动失败");
            e.printStackTrace();
        }

        //JDBC的URL
        String url = "jdbc:mysql://127.0.0.1:33060/mydb";
        Connection connection = null;

        try {
            //调用DriverManager对象的getConnection()方法，获得一个Connection对象
            connection = DriverManager.getConnection(url, user, password);
            Statement state = connection.createStatement();
            System.out.println("连接成功");

            String sql = "INSERT customer(mid, nam, birh, sex) VALUES (?, ?, ?, ?)";
            PreparedStatement prest = connection.prepareStatement(sql, ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet
                    .CONCUR_READ_ONLY);

            prest.setString(1, "D0006");
            prest.setString(2, "Jack6");
            prest.setString(3, "1996-06-01 00:00:00");
            prest.setString(4, "0");

            prest.executeUpdate();

            state.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

### 修改数据

```java
package JDBC;

import java.sql.*;

public class JDBCDemo3 {

    private static String user = "vagrant";
    private static String password = "vagrant";

    public static void main(String[] args) throws Exception {
        try {
            //加载驱动程序
            Class.forName("com.mysql.jdbc.Driver");
            System.out.println("加载驱动成功");
        } catch (ClassNotFoundException e) {
            System.out.println("加载驱动失败");
            e.printStackTrace();
        }

        //JDBC的URL
        String url = "jdbc:mysql://127.0.0.1:33060/mydb";
        Connection connection = null;

        try {
            //调用DriverManager对象的getConnection()方法，获得一个Connection对象
            connection = DriverManager.getConnection(url, user, password);
            Statement state = connection.createStatement();
            System.out.println("连接成功");

            String sql = "UPDATE customer SET nam = ? WHERE mid=?";
            PreparedStatement prest = connection.prepareStatement(sql, ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet
                    .CONCUR_READ_ONLY);

            prest.setString(1, "Jack666");
            prest.setString(2, "D0001");

            prest.executeUpdate();

            state.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

### 删除数据

```java
package JDBC;

import java.sql.*;

public class JDBCDemo4 {

    private static String user = "vagrant";
    private static String password = "vagrant";

    public static void main(String[] args) throws Exception {
        try {
            //加载驱动程序
            Class.forName("com.mysql.jdbc.Driver");
            System.out.println("加载驱动成功");
        } catch (ClassNotFoundException e) {
            System.out.println("加载驱动失败");
            e.printStackTrace();
        }

        //JDBC的URL
        String url = "jdbc:mysql://127.0.0.1:33060/mydb";
        Connection connection = null;

        try {
            //调用DriverManager对象的getConnection()方法，获得一个Connection对象
            connection = DriverManager.getConnection(url, user, password);
            Statement state = connection.createStatement();
            System.out.println("连接成功");

            String sql = "DELETE FROM customer WHERE mid=?";
            PreparedStatement prest = connection.prepareStatement(sql, ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet
                    .CONCUR_READ_ONLY);

            prest.setString(1, "D0002");

            prest.executeUpdate();

            state.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```

## JDBC: execute、executeQuery和executeUpdate之间的区别

execute、executeQuery和executeUpdate都是由JDBC的Statement的接口提供的,具体使用哪个方法是根据SQL语句所产生的内容决定。

### executeQuery方法

用于产生单个结果集（ResultSet）的语句，例如:被执行最多的SELECT 语句。 
这个方法被用来执行 SELECT 语句，但也只能执行查询语句，执行后返回代表查询结果的ResultSet对象。

```java
package JDBC;

import java.sql.*;

public class JDBCDemo5 {

    private static String user = "vagrant";
    private static String password = "vagrant";

    public static void main(String[] args) throws Exception {
        try {
            //加载驱动程序
            Class.forName("com.mysql.jdbc.Driver");
            System.out.println("加载驱动成功");
        } catch (ClassNotFoundException e) {
            System.out.println("加载驱动失败");
            e.printStackTrace();
        }

        //JDBC的URL
        String url = "jdbc:mysql://127.0.0.1:33060/mydb";
        Connection connection = null;

        try {
            //调用DriverManager对象的getConnection()方法，获得一个Connection对象
            connection = DriverManager.getConnection(url, user, password);
            Statement state = connection.createStatement();
            System.out.println("连接成功");
            String sql = "select * from customer";
            ResultSet results = state.executeQuery(sql);
            while (results.next()) {
                System.out.println("name: " + results.getString("nam"));
            }
            state.close();
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}

```  

### executeUpdate方法

用于执行`INSERT`、`UPDATE`或`DELETE`语句以及`SQL DDL(数据定义语言)`语句，例如 `CREATE TABLE` 和 `DROP TABLE`。
`INSERT`、`UPDATE` 或 `DELETE` 语句的效果是修改表中零行或多行中的一列或多列。
executeUpdate的返回值是`一个整数（int）`，指示受影响的行数（即更新计数）。
`对于 CREATE TABLE 或 DROP TABLE 等不操作行的语句，executeUpdate 的返回值总为0`。

使用executeUpdate方法是因为在`createTableCoffees`中的`SQL语句是DDL(数据定义语言)`语句。创建表，改变表，删除表都是 DDL 语句的例子，要用executeUpdate方法来执行。
你也可以从它的名字里看出，方法 executeUpdate 也被用于执行更新表 SQL 语句。实际上，相对于创建表来说，executeUpdate 用于更新表的时间更多，因为表只需要创建一次，但经常被更新。 

### execute方法

Statement的execute()方法`几乎可以执行任何原生SQL语句`，但它执行SQL语句时比较麻烦(它无法直接返回一个ResultSet，而是需要我们手工再次获取)，通常情况下，使用executeQuery()、或executeUpdate()方法更加简单。但如果程序员不清楚SQL语句的类型，则只能使用execute()方法来执行该SQL语句了。

>  
注意，使用execute()方法执行SQL语句的返回值"只是"boolean值，它表明了执行该SQL语句是否返回了ResultSet对象，而我们要得到具体的ResultSet对象还需要额外的方法
- getResult(): 获取该Statement执行查询语句所返回的ResultSet对象
- getUpdateCount(): 获取该Statement执行DML语句所影响的记录行数

#### 结果集
{% gist 8f06bb093779707e6397798f30640bba JDBCDemo5.java %}

#### 影响记录行数
{% gist d0c951ee0ceb5fd93992a3ee426c7f69 JDBCDemo6.java %}



参考：
http://wiki.jikexueyuan.com/project/jdbc/introduction.html
http://www.jianshu.com/p/c0acbd18794c
http://www.jianshu.com/p/54d69abe76ed
http://blog.csdn.net/u012830807/article/details/17333331
https://www.cnblogs.com/LittleHann/p/3695332.html