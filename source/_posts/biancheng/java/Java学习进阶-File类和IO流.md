---
title: Java学习进阶-File类和IO流
date: 2019-02-24 23:10:34
tags: Java
categories: 编程
---
Java进阶学习-File类和IO流

<!-- more -->
- File类 `java.io.File` 类是文件和目录路径名的抽象表示，只要用于文件和目录的创建，查找和删除等操作
    - 静态方法
        - `static String pathSeparator`  与系统有关的路径分隔符，为了方便，它被表示为一个字符串。
            - Windows 分隔符 ; 
            - Linux 分隔符 :
        - `static char pathSeparatorChar`  与系统有关的路径分隔符。 
        - `static String separator` 与系统有关的默认名称分隔符，为了方便，它被表示为一个字符串。
            - Windows \
            - Linux /
        - `static char separatorChar ` 与系统有关的默认名称分隔符。 
    -  路径：绝对路径和相对路径
        - 路径不区分大小写
        - 路径中的文件名称分隔符Windows中使用反斜杠,由于反斜杠是转义字符，两个反斜杠代表一个普通的反斜杠
    - 构造方法
        - `File(String pathname)`  通过将给定路径名字符串转换为抽象路径名来创建一个新 File 实例。
            - String pathname 字符串的路径名称
            - 路径可以是以文件结尾，也可以是以文件夹结尾
            - 路径可以是相对路径，也可以是绝对路径
            - 路径可以是存在的，也可以是不存在的
        - `File(String parent, String child)` 根据 parent 路径名字符串和 child 路径名字符串创建一个新 File 实例。
        - `File(File parent, String child)` 根据 parent 抽象路径名和 child 路径名字符串创建一个新 File 实例。
    - 成员方法
        - 获取方法
            - `public String getAbsolutePath()` 返回此File的绝对路径名字符串
            - `public String getPath()` 将File转换为路径名字符串
            - `public String getName()` 返回此File表示的文件或目录的名称
            - `public long length()` 返回此File表示的文件的长度
                - 获取构造方法指定的文件的大写，以字节为单位
                    - 注意：1、文件夹是没有大小概念的，不能获取文件夹的大小， 2、如果构造方法中给出的路径不存在，那么length()返回值是一个0
        - 判断方法：
            - `public boolean exists()` 此File表示的文件或目录是否实际存在；
            - `public boolean isDirectory()` 此File表示的是否为目录；
            - `public boolean isFile()` 此File表示的是否为文件；
        - 创建删除功能的方法
            - `public boolean createNewFile()` 当且仅当具有该名称的文件不存在的时候，创建一个新的空文件
                - 文件不存在，创建文件返回true，若存在不会创建，返回false
                - 此方法只能创建文件，不可以创建文件夹
                - 创建文件的路径必须存在，否则会抛出异常
            - `public boolean delete()` 删除由此Fiel表示的文件或目录(文件夹)
                - 文件或者文件夹删除成功返回true
                - 文件夹中有内容不会删除或者构造方法中的目录不存在，返回false
                - 注意：1、delete方法是直接在硬盘删除文件/文件夹，不走回收站，删除要谨慎
            - `public boolean mkdir()` 创建由此Fiel表示的目录
            - `public boolean mkdirs()` 创建由此Fiel表示的目录,包括任何必须但不存在的父目录(单级目录和多级目录都可以创建)
        - 目录的遍历
            - `public String[] list()` 返回一个String数组，表示该File目录中的所有子文件或目录
            - `public String[] listFiles()` 返回一个File数组，表示该File目录中的所有子文件或目录
            - 注意
                - list方法或listFiles方法遍历的是构造方法中给出的目录
                - 如果构造方法中给出的目录不存，就会抛出空指针
                - 如果构造方法中给出的路径不是一个目录，也会抛出空指针
- 递归：指当前方法内调用自己的现象
    - 注意：
        - 递归一定要有条件限定，保证递归能够停下来，否则会发生栈内存溢出
        - 在递归中虽然有条件限定，但是递归次数不能太多，否则也会发生栈内存溢出
        - 构造方法，禁止递归
- 过滤器
    - `java.io.FileFilter`接口：用于抽象路径名(File对象)的过滤器，用来过来文件(File对象)
        - `boolean accept(File pathname)` 测试指定抽象路径名是否应该包含在某个路径名列表中。
            - File pathname：使用ListFiles方法遍历目录得到的每一个文件对象
            
    - `java.io.FilenameFilter filter` 接口:实现此接口的类实例可用于过滤器文件名，用来过来文件夹 
        -  `boolean accept(File dir, String name) ` 测试指定文件是否应该包含在某一文件列表中。 
            - File dir:构造方法中传递的被遍历的目录
            - String name：使用ListFiles方法遍历目录，获取的每一个文件/文件夹的名称
    - 过滤器接口是没有实现类的，需要自己书写实现类，重写过滤器的accept，在方法中自己定义过滤规则
- IO流
    - 字节流：字节输入流(InputStream) 字节输出流(OutputStream)
    - 字符流：字符输入流(Reader) 字符输出流(Writer)
    - 字节输出流(OutputStream):此抽象类是表示输出字节流的所有类的超类
        - `void close()` 关闭此输出流并释放与此流有关的所有系统资源。
        - `void flush()` 刷新此输出流并强制写出所有缓冲的输出字节。
        - `void write(byte[] b)` 将 b.length 个字节从指定的 byte 数组写入此输出流。
        - `void write(byte[] b, int off, int len)` 将指定 byte 数组中从偏移量 off 开始的 len 个字节写入此输出流。
        - `void write(int b)` 将指定的字节写入此输出流。
        - 子类：
            - `java.io.FilterOutputStream` 文件字节输出流 把内存中的数据写到硬盘的文件中
                - 构造方法：
                    - `FilterOutputStream(String name)`:创建一个向具有指定名称的文件中写入数据的输出文件流
                    - `FilterOutputStream(File file)`：创建一个向指定File对象表示的文件中写入数据的文件输出流
                        - 参数：1、String name 文件的路径 2、File file：文件
                        - 作用：创建一个FileOutputStream对象，根据构造方法中传递的文件/文件路径，创建一个空的文件，会把FileOutputStream对象指向创建好的文件
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            
            