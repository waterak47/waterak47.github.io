---
title: Java基础：I/O操作
date: 2017-11-09 10:39:41
comments: true
tags: Java基础
categories: Java
---

![](https://ws4.sinaimg.cn/large/006tKfTcgy1flwlz8tcfog31kw24i7aq.gif)


## I/O简介
I/O就是输入和输出，核心是I/O流，流用于读写设备上的数据，包括硬盘文件、内存、键盘、网络...

> 根据数据的走向分为：输入流、输出流
> 根据处理的数据类型分为：字节流、字符流

字节流能处理所有类型数据，对应的类以`Stream`结尾。
字符流只能处理文本数据，对应的类以`Reader`或`Writer`结尾。

字符流与字节流的区别：
> 1.字节流操作的基本单元为字节；
  2.字符流操作的基本单元为Unicode码元。
  3.字节流默认不使用缓冲区；
  4.字符流使用缓冲区。
  5.字节流通常用于处理二进制数据，实际上它可以处理任意类型的数据，但它不支持直接写入或读取Unicode码元；
  6.字符流通常处理文本数据，它支持写入及读取Unicode码元。

<!--more-->

各种流的继承关系:

![](https://ws1.sinaimg.cn/large/006tKfTcgy1flbn1ooov2j30k40laq56.jpg)

## File

### File类别：
> File类，文件和目录路径名的抽象表示形式，总的来说就是java创建删除文件目录的一个类库

### File的构造函数：

> `File(File parent, String child)`, 根据 parent 抽象路径名和 child 路径名字符串创建一个新 File 实例
  `File(String pathname)`, 通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例
  `File(String parent, String child)`, 根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例
  `File(URI uri)`, 通过将给定的 file: URI 转换为一个抽象路径名来创建一个新的 File 实例

### File类常用方法：

> `boolean isFile()`, 该方法的作用是判断当前File对象是否是文件
  `boolean isDirectory()`, 判断该路径指示的是否是文件夹
  `String[] list()`, 返回文件和目录清单
  `File[] listFiles()`, 该方法的作用是返回当前文件夹下所有的文件对象, 包含其属性
  `boolean mkdir()`, 该方法的作用是创建当前文件文件夹，而不创建该路径中的其它文件夹。假设d盘下只有一个test文件夹，则创建d： estabc文件夹则成功，如果创建d：a文件夹则创建失败，因为该路径中d：a文件夹不存在。如果创建成功则返回true，否则返回false.
  `boolean mkdirs()`, 该方法的作用是创建文件夹，如果当前路径中包含的父目录不存在时，也会自动根据需要创建
  `long length()`, 该方法的作用是返回文件存储时占用的字节数;该数值获得的是文件的实际大小，而不是文件在存储时占用的空间数
  `boolean exists()`, 该方法的作用是判断当前文件或文件夹是否存在

### File类常用属性：
> `static String pathSeparator`, 与系统有关的路径分隔符字符，出于方便考虑，它被表示为一个字符串。 此字段被初始化为包含系统属性 file.separator 的值的第一个字符。在 UNIX 系统上，此字段的值为 `/`；在 Microsoft Windows 系统上，它为 `\\`

### File类使用：
``` java
package IO;

import java.io.File;

public class FileDemo {
    public static void main(String[] args) throws Exception {
        demo1();
        demo2();
    }

    //读取文件
    public static void demo1() {
        File file1 = new File("./src/IO/file.java");
        //打印文件名称
        System.out.println(file1.getName());
        //获取文件大小
        System.out.println(file1.length());
    }

    //文件列表
    public static void demo2() {
        File file2 = new File("./src/temp/");

        if (file2.exists() && file2.isDirectory()) {
            String[] lists = file2.list();
            //打印文件列表
            for (int i = 0, len = lists.length; i < len; i++) {
                System.out.println(lists[i]);
            }
        }
    }
}

```

## 字节流
`Java中的字节流处理的最基本单位为单个字节，它通常用来处理二进制数据`。Java中最基本的两个字节流类是`InputStream`和`OutputStream`，它们分别代表了组基本的输入字节流和输出字节流。

### InputStream和OutputStream
InputStream是一个输入流，也就是用来读取文件的流，抽象方法read读取下一个字节，当读取到文件的末尾时候返回 -1。如果流中没有数据read就会阻塞直至数据到来或者异常出现或者流关闭。这是一个受查异常，具体的调用者必须处理异常。

OutputStream是一种输出流，具体的方法和InputStream差不多，只是，一个读一个写。但是，他们都是抽象类，想要实现具体的功能还是需要依赖他们的子类来实现。

### FileInputStream 函数接口
``` java
FileInputStream(File file)         // 构造函数1：创建“File对象”对应的“文件输入流”
FileInputStream(FileDescriptor fd) // 构造函数2：创建“文件描述符”对应的“文件输入流”
FileInputStream(String path)       // 构造函数3：创建“文件(路径为path)”对应的“文件输入流”

int      available()             // 返回“剩余的可读取的字节数”或者“skip的字节数”
void     close()                 // 关闭“文件输入流”
FileChannel      getChannel()    // 返回“FileChannel”
final FileDescriptor     getFD() // 返回“文件描述符”
int      read()                  // 返回“文件输入流”的下一个字节
int      read(byte[] buffer, int byteOffset, int byteCount) //读取“文件输入流”的数据并存在到buffer，从byteOffset开始存储，存储长度是byteCount。
long     skip(long byteCount)    // 跳过byteCount个字节
```

### FileInputStream代码
``` java
package IO;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FileDemo2 {
    public static void main(String[] args) throws Exception {
        demo2();
    }

    public static void demo1() {
        File file = new File("./src/IO/file.java");
        FileInputStream inputStream = null;

        try {
            inputStream = new FileInputStream(file);
            int data;
            byte[] bytes = new byte[1024];

            while ((data = inputStream.read(bytes)) != -1) {
                System.out.println(new String(bytes, 0, data));
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                inputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static void demo2() {
        File file = new File("./src/IO/file.java");
        FileInputStream inputStream = null;

        try {
            inputStream = new FileInputStream(file);
            byte[] bytes = new byte[1024];
            int data = inputStream.read(bytes);
            
            for(int i =0, len = bytes.length; i < len; i++) {
                System.out.println((char)bytes[i]);
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                inputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

```

### FileOutputStream 函数接口
``` java
FileOutputStream(File file)                   // 构造函数1：创建“File对象”对应的“文件输入流”；默认“追加模式”是false，即“写到输出的流内容”不是以追加的方式添加到文件中。
FileOutputStream(File file, boolean append)   // 构造函数2：创建“File对象”对应的“文件输入流”；指定“追加模式”。
FileOutputStream(FileDescriptor fd)           // 构造函数3：创建“文件描述符”对应的“文件输入流”；默认“追加模式”是false，即“写到输出的流内容”不是以追加的方式添加到文件中。
FileOutputStream(String path)                 // 构造函数4：创建“文件(路径为path)”对应的“文件输入流”；默认“追加模式”是false，即“写到输出的流内容”不是以追加的方式添加到文件中。
FileOutputStream(String path, boolean append) // 构造函数5：创建“文件(路径为path)”对应的“文件输入流”；指定“追加模式”。

void                    close()      // 关闭“输出流”
FileChannel             getChannel() // 返回“FileChannel”
final FileDescriptor    getFD()      // 返回“文件描述符”
void                    write(byte[] buffer, int byteOffset, int byteCount) // 将buffer写入到“文件输出流”中，从buffer的byteOffset开始写，写入长度是byteCount。
void                    write(int oneByte)  // 写入字节oneByte到“文件输出流”中 
```

### FileOutputStream代码
``` java
package IO;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileDemo3 {
    public static void main(String[] args) throws Exception {
        demo1();
    }

    public static void demo1() {
        File file = new File("./src/IO/file.java");
        File outFile = new File("./src/IO/out.txt");
        FileInputStream inputStream = null;
        FileOutputStream outputStream = null;
        try {
            inputStream = new FileInputStream(file);
            outputStream = new FileOutputStream(outFile);
            byte[] bytes = new byte[1024];
            int data;

            while((data = inputStream.read(bytes)) != -1) {
                outputStream.write(bytes);
            }

        } catch (IOException e1) {
            e1.printStackTrace();
            try {
                outputStream.close();
                inputStream.close();
            } catch (IOException e2) {
                e2.printStackTrace();
            }
        }
    }
}
```

### ObjectOutputStream和ObjectInputStream
Java平台允许我们在内存中创建可复用的Java对象，但一般情况下，只有当JVM处于运行时，这些对象才可能存在，即，这些对象的生命周期不会 比JVM的生命周期更长。但在现实应用中，就可能要求在JVM停止运行之后能够保存(持久化)指定的对象，并在将来重新读取被保存的对象。Java对象序 列化就能够帮助我们实现该功能。

使用Java对象序列化，在保存对象时，会把其状态保存为一组字节，在未来，再将这些字节组装成对象。必须注意地是，对象序列化保存的是对象的"状态"，即它的成员变量。由此可知，对象序列化不会关注类中的静态变量。

我们可以使用java的IO流中的对象流ObjectOutputStream和ObjectInputStream来实现序列化和反序列化的操作。

Java对象序列化就是将对象转换为字节流，然后可以通过这些值再生成相同状态的对象。对象序列化是对象持久化的一种实现方法,它是将一个对象的属性和方法转化为一种序列化的格式以用于存储和传输,反序列化就是根据这些保存的信息重建对象的过程。

序列化的实现：将需要被序列化的类实现Serializable接口，然后使用一个输出(如:FileOutputStream)来构造一个ObjectOutputStream(对象流)对象，接着，使用ObjectOutputStream对象的writeObject(Object obj)方法就可以将参数为obj的对象写出(即保存其状态)，要恢复的话则用输入流。所以使用ObjectOutputStream类实现序列化，ObjectInputStream类实现反序列化。


``` java
package IO;

import java.io.*;

public class FileDemo4 {
    public static void main(String[] args) throws Exception {
        writeObject();
        readObject();
    }

    //写入
    public static void writeObject() throws IOException {

        File file = new File("./src/IO/obj.txt");

        //输出文件
        OutputStream outFile = new FileOutputStream(file);

        ObjectOutputStream objOut = new ObjectOutputStream(outFile);

        Shop shop = new Shop("MacBookPro");

        //文件写入，序列化
        objOut.writeObject(shop);
    }

    //读取
    public static void readObject() throws ClassNotFoundException, IOException {

        File file = new File("./src/IO/obj.txt");

        //读取文件
        InputStream inputFile = new FileInputStream(file);

        ObjectInputStream objInput = new ObjectInputStream(inputFile);

        //读取之前存储的文件，反序列化，将文件还原
        Object obj = objInput.readObject();
        Shop p = (Shop)obj;
        System.out.println(p.getProductName());
    }
}

```


## 参考
http://longpo.iteye.com/blog/2203627
http://www.jianshu.com/p/21ed71b26a5d
https://chenjiabing666.github.io/2017/05/23/Java-IO%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%E4%B8%80/