---
title: Java基础：网络编程
date: 2017-11-17 10:16:12
tags:
- Java基础
- Java网络编程
categories: Java
---
## 网络编程中两个主要的问题

一个是如何准确的定位网络上一台或多台主机，另一个就是找到主机后如何可靠高效的进行数据传输。

在TCP/IP协议中IP层主要负责网络主机的定位，数据传输的路由，由IP地址可以唯一地确定Internet上的一台主机。

而TCP层则提供面向应用的可靠（tcp）的或非可靠（UDP）的数据传输机制，这是网络编程的主要对象，一般不需要关心IP层是如何处理数据的。

目前较为流行的网络编程模型是客户机/服务器（C/S）结构。即通信双方一方作为服务器等待客户提出请求并予以响应。客户则在需要服务时向服务器提 出申请。服务器一般作为守护进程始终运行，监听网络端口，一旦有客户请求，就会启动一个服务进程来响应该客户，同时自己继续监听服务端口，使后来的客户也 能及时得到服务。

<!--more-->

## 获取IP

```java
package temp.com.networkdemo;

import java.net.InetAddress;
import java.net.UnknownHostException;

public class GetIPAddress {
    public static void main(String[] args) throws Exception {
        getAddressDemo1();
        getAddressDemo2();
    }

    public static void getAddressDemo1() {

        try {
            //获取网址IP
            InetAddress ip = InetAddress.getByName("www.baidu.com");
            System.out.println(ip.getHostAddress());
        } catch (UnknownHostException e) {
            e.printStackTrace();
        }

    }

    public static void getAddressDemo2() {
        try {
            InetAddress ip = InetAddress.getLocalHost();
            String myIP = ip.getHostAddress();
            String myName = ip.getHostName();
            System.out.println("ip: "+ip + " name: " + myName);
        } catch (UnknownHostException e) {
            e.printStackTrace();
        }
    }
}
```

## UDP接收与发送数据

### UDP接收

实现UDP接收端:

```
实现封装数据包 java.net.DatagramPacket 将数据接收
实现输出传输     java.net.DatagramSocket 接收数据包
```

实现步骤:

```
1.创建DatagramSocket对象,绑定端口号要和发送端端口号一致
2.创建字节数组,接收发来的数据
3.创建数据包对象DatagramPacket
4.调用DatagramSocket对象方法, receive(DatagramPacket dp)接收数据,
  数据放在数据包中
5.拆包
	发送的IP地址
		数据包对象DatagramPacket方法getAddress()获取的是发送端的IP地址对象,
		返回值是InetAddress对象
	接收到的字节个数
		数据包对象DatagramPacket方法 getLength()
	发送方的端口号
		数据包对象DatagramPacket方法 getPort()发送端口
```

代码实现:


```java

package temp.com.networkdemo;

import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPReceive {
    public static void main(String[] args) throws Exception {
        //创建数据包传输对象DatagramSocket, 绑定端口号8888
        DatagramSocket ds = new DatagramSocket(8888);
        //创建字节数组
        byte[] data = new byte[1024];
        //创建数据包 传递直接数组
        DatagramPacket dp = new DatagramPacket(data, data.length);
        //传送数据包
        ds.receive(dp);

        //获取发送端IP
        String ip = dp.getAddress().getHostAddress();
        //获取端口号
        int port = dp.getPort();
        //获取收到字节数
        int len = dp.getLength();
        System.out.println("IP: " + ip + " port:" + port);
        System.out.println("Content: " + new String(data, 0, len));

        //关闭资源
        ds.close();
    }
}
```

### UDP发送

实现UDP发送端:

```
实现封装数据包 java.net.DatagramPacket 封装数据包
实现输出传输     java.net.DatagramSocket 发送数据包
```

实现步骤：

```
创建DatagramPacket对象,封装数据, 接收的地址和端口
创建DatagramSocket
调用DatagramSocket类方法send,发送数据包
关闭资源
```

代码实现：

```java
package temp.com.networkdemo;

import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPClient {
    public static void main(String[] args) throws Exception {
        //要发送的数据
        byte[] date = "我在测试发数据".getBytes();
        //封装IP地址
        InetAddress ip = InetAddress.getByName("127.0.0.1");
        //封装数据、IP、端口号
        DatagramPacket dp = new DatagramPacket(date, date.length, ip, 8888);
        //发送数据包
        DatagramSocket ds = new DatagramSocket();
        ds.send(dp);
        //关闭资源
        ds.close();
    }
}
```

## TCP接收与发送数据

TCP传输
```
Socket和ServerSocket
建立客户端和服务器端
建立连接后，通过Socket中的IO流进行数据的传输
关闭socket
同样，客户端与服务器端是两个独立的应用程序。
```

TCP发送端步骤:
```
建立客户端的Socket服务,并明确要连接的服务器
如果连接建立成功,就表明,已经建立了数据传输的通道.就可以在该通道通过IO进行数据的读取和写入.该通道称为Socket流,Socket流中既有读取流,也有写入流.
通过Socket对象的方法,可以获取这两个流
通过流的对象可以对数据进行传输
如果传输数据完毕,关闭资源
```

代码实现：

```java

package temp.com.networkdemo;

import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

public class TCPClient {
    public static void main(String[] args) throws Exception {
        //创建socket对象，连接服务器
        Socket socket = new Socket("127.0.0.1", 8888);
        //获取socket套接字对象的字节输出流，将数据写入服务器
        OutputStream out = socket.getOutputStream();
        out.write("将数据写入服务器".getBytes());

        //接收服务器返回数据，使用socket套接字对象的字节输入流
        InputStream input = socket.getInputStream();
        byte[] data = new byte[1024];
        int len = input.read(data);
        System.out.println("Content: " + new String(data, 0, len));

        //关闭资源
        socket.close();
    }
}

```

TCP接收端步骤:

```
建立服务器端的socket服务，需要一个端口
服务端没有直接流的操作,而是通过accept方法获取客户端对象，在通过获取到的客户端对象的流和客户端进行通信
通过客户端的获取流对象的方法,读取数据或者写入数据
如果服务完成,需要关闭客户端,然后关闭服务器，但是,一般会关闭客户端,不会关闭服务器,因为服务端是一直提供服务的
```
