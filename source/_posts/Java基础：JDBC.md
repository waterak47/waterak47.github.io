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

### 修改数据



参考：
http://wiki.jikexueyuan.com/project/jdbc/introduction.html
http://www.jianshu.com/p/c0acbd18794c
http://www.jianshu.com/p/54d69abe76ed
