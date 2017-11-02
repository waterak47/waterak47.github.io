---
title: 'Java:复制文件和文件夹'
date: 2017-11-02 10:45:10
tags: Java基础
categories: Java
---
### Java基础练习
Java代码实现文件夹拷贝,文件夹可能包含文件夹和文件
<!--more-->
```java
import java.io.*;

public class CopyUtils {

    public static boolean copyFile(String oldPath, String newPath) {

        File oldFile = new File(oldPath);
        File newFile = new File(newPath);

        //判断源文件是否存在
        if (!oldFile.exists()) {
            System.out.println("源文件不存在");
            return false;
        }
        //源文件存在，判断是否为目录
        if (oldFile.isDirectory()) {
            System.out.println("仅拷贝文件，不拷贝文件夹");
            return false;
        }
        //新文件存在
        if (newFile.exists()) {
            System.out.println("文件已存在");
            return true;
        }

        String newFilePath = newFile.getAbsolutePath() + File.separator + oldFile.getName();
        boolean isCreateDirs = newFile.mkdirs();

        //新文件目录无法创建
        if (!isCreateDirs) {
            System.out.println("无法创建目标文件目录");
            return false;
        }

        int data;
        byte[] bytes = new byte[1024];

        FileInputStream inputStream = null;
        FileOutputStream outPutStream = null;

        try {

            inputStream = new FileInputStream(oldFile);
            outPutStream = new FileOutputStream(newFilePath);

            while ((data = inputStream.read(bytes)) != -1) {
                outPutStream.write(bytes, 0, data);
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (outPutStream != null) {
                    outPutStream.close();
                }

                if (inputStream != null) {
                    inputStream.close();
                }

            } catch (IOException ex) {
                ex.printStackTrace();
            }
        }

        return true;
    }

    public static boolean copyFolder(String oldPath, String newPath) {
        boolean result = false;

        try {
            File oldFile = new File(oldPath);
            File newFile = new File(newPath);

            //检查目标目录
            if (!newFile.exists()) {
                newFile.mkdirs();
            }

            //读取源目录文件;
            String[] fileNames = oldFile.list();

            //检查目录文件数量
            if (fileNames.length == 0) {
                return false;
            }

            File temp = null;

            for (int i = 0, len = fileNames.length; i < len; i++) {
                if (oldPath.endsWith(File.separator)) {
                    temp = new File(oldPath + fileNames[i]);
                } else {
                    temp = new File(oldPath + File.separator + fileNames[i]);
                }

                if (temp.isFile()) {
                    FileInputStream input = new FileInputStream(temp);
                    FileOutputStream output = new FileOutputStream(newPath + File.separator + (temp.getName()).toString());
                    byte[] bytes = new byte[1024];
                    int data;

                    while((data = input.read(bytes)) != -1) {
                        output.write(bytes, 0, data);
                    }

                    output.flush();
                    output.close();
                    input.close();
                }

                if(temp.isDirectory()) {
                    copyFolder(oldPath + File.separator + fileNames[i], newPath + File.separator + fileNames[i]);
                }
            }

            if(new File(newPath).exists()) {
                result = true;
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

        return result;
    }
}
```

```java

//测试

class TestCopyUtils {
    public static void main(String[] args) throws Exception {
        test1();
    }

    public static void test1() throws Exception {
        CopyUtils.copyFolder("./src/day1/", "./src/test_folder/");
    }
}
```