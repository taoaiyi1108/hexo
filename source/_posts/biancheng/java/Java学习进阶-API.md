---
title: Java学习进阶-API
date: 2019-01-07 20:11:43
tags: Java
categories: 编程
---
Java进阶学习笔记-API

<!-- more -->
- 使用Scanner进行键盘输入
```java
    package cn.itcast.study;
    
    import java.util.Scanner; /*1:导包*/
    
    public class ScannerDemo {
        public static void main(String[] args) {
            /*2:创建*/
            Scanner sc = new Scanner(System.in); /*System.in 代表从键盘输入*/
    
            /*3:使用*/
            /*获取键盘输入的一个int数字*/
            int num = sc.nextInt();
            System.out.println("输入的键盘数字是"+ num);
            /*获取键盘输入的一个字符串*/
            String str = sc.next();
            System.out.println("输入的键盘字符是"+ str);
        }
    }
```

- 匿名对象：只有右边的对象，没有左边的名字和赋值运算符
    - 格式：`new 类名称();`
    - 使用： `new Person().name = "张三" `
    - 注意事项：匿名对象只能使用唯一的一次，下次使用不得不在创建一个新的对象
    - 使用建议：如果一个对象只是用唯一的一次，就可以使用匿名对象

- Random 
```java
package cn.itcast.study;

import java.util.Random;

public class ScannerDemo {
    public static void main(String[] args) {
        Random r = new Random();
        int num = r.nextInt(); /*不传参数 int的整个区间随机*/
        System.out.println("输出的随机数是：" + num);

        int nun = r.nextInt(10); /*传入参数单边随机的区间 左闭右开 [0~10) 也就是0~9*/
        System.out.println("输出的随机数是：" + nun);
    }
}
```

- 对象数组 
     - 创建 `Person[] array = new Person[3]`
     - 注意事项：数组一旦创建，程序运行期间数组长度不可以改变
     
- ArrayList集合类
    - 概述：是大小可变得数组的实现，存储在内的数据称之为元素，此类提供一些方法来操作内部存储的元素，ArrayList中不断添加元素，其大小可自动增长；
    - 和数组的区别
        - 数组的长度不可以发生改变
        - ArrayList集合的长度是可以随意变化的
    - 对于ArrayList来说，有一个尖括号<E>代表泛型：也就是装在集合当中的元素，全都是统一的什么类型
        - 注意：泛型只能是引用类型，不能是基本类型    
    - 注意事项：
        - 对于ArrayList集合来说，直接打印得到不是地址值，而是内容，如果为空，得到的是：[]
    - 基本使用
    ```java
      package cn.itcast.study;
      
      import java.util.ArrayList;
      
      public class ScannerDemo {
          public static void main(String[] args) {
              ArrayList<String> list = new ArrayList<>();
              /*创建了一个ArrayList集合，集合的名称是list，里面装的都是String字符串类型的数据*/
              /*从 JDK1.7+ 开始，右侧的尖括号中可以不写任何内容，但是<>本身还是要写上的*/
              System.out.println(list); /* [] */
      
              /*向集合中添加数据*/
              list.add("张三");
              list.add("李四");
              System.out.println(list);
      
              /*list.add(100) 错误写法 创建的时候尖括号的泛型已经说了是字符串，添加进去的元素必须是字符串，否则就会报错*/
          }
      }
    ```
    - ArrayList常用方法
        ```
          public boolean add(E e): 向集合中添加元素，参数类型和泛型一致
          public E get(int index): 从集合中获取元素，参数是索引编号，返回值就是对于位置的元素
          public E remove(int index): 从集合中删除元素，参数是索引编号，返回值是被删除掉的元素
          public int size(): 获取集合的尺寸长度，返回值是集合中包含的元素个数 
          
          对于ArrayList集合来说add添加元素是一定成功的，其他集合add就不一定成功，所以返回值代表的就是集合添加元素是否成功
        ```
        ```
          list.add("张三");
          list.get(0);
          list.remove(0);
          list.size();
        ```
    - ArrayList集合存储基本数据
    ```
        ArrayList<int> list = new ArrayList<>(); // 错误写法 泛型只能是引用类型，int是基本类型
        
        如果希望向ArrayList集合中存储基本类型，必须使用基本类型对应的包装类
        - 基本类型 包装类(引用类型，包装类都位于java.lang包下)
        - byte    Byte
        - short   Short
        - int     Integer
        - long    Long
        - float   Float
        - double  Double
        - char    Character
        - boolean Boolean
        
        ArrayList<Integer> list = ArrayList<>();
        
        从 JDK1.5+ 开始，支持自动装箱，自动拆箱
        - 自动装箱：基本类型 --> 包装类型
        - 自动拆箱：保证类型 --> 基本类型
    ```
    ```java
      package cn.itcast.study;
      
      import java.util.ArrayList;
      
      public class ScannerDemo {
          public static void main(String[] args) {
              ArrayList<Integer> list = new ArrayList<>();
              list.add(10);
              int num = list.get(0);
              System.out.println("ArrayList中的第一个元素是：" + num); /*ArrayList中的第一个元素是：10*/
          }
      }

    ```
- String类
    - 程序中所有双引号字符串都是String类的对象（就算没有new 也是）
    - 特点：
        - 字符串是常量，一旦被创建，不可改变；字符串的内容永不可变【重点】
        - 正是因为字符串不可改变，所以字符串是可以共享使用的
        - 字符串效果是相当是char[]字符数组但是底层原理是byte[]字节数组
    - 创建字符串的方式(3构造方法+1直接创建)
        - `public String()` 创建一个空白字符串，不含有任何内容
        - `public String(char[] array)` 根据字符数组的内容来创建对应的字符串
        - `public String(byte[] array)` 根据字节数组的内容来创建对应的字符串
        ```java
          String str1 = new String();

          char[] charArray = {'a','b'};
          String str2 = new String(charArray);
    
          byte[] byteArray = {87,98,99};
          String str3 = new String(byteArray);
        ```
        - 直接创建 `String str = "HelloWorld" `
    - 注意事项：不管有没有new，直接写上双引号，就是字符串对象
    - 字符串的常量池
        - 程序中直接写上的双引号字符串，就在常量池中。new的字符串不在常量池中
            - 对应引用类型 == 比较的是地址值
    - 字符串的使用（常用方法）
        - 字符串内容比较
            - ` public boolean equals(Object obj) ` 字符串内容进行比较 参数是如何对象，只有参数是一个字符串并且内容相同才会给true，否则返回的是false
                - 任何对象都可以用Object来接收
            ```java
              String str = "Hello";
              String str1 = "Hello";
              char[] charArray = {'H','e','l','l','o'};
              String str2 = new String(charArray);
        
              System.out.println(str.equals(str1)); /*true*/
              System.out.println(str1.equals(str2)); /*true*/
              System.out.println(str2.equals("Hello")); /*true*/
              System.out.println("Hello".equals(str)); /*true*/
              System.out.println("hello".equals(str)); /*false*/
            ```
                - 注意事项：equals方法具有对称性，也就是a.equals(b)和b.equals(a)效果一样，如果比较双方一个常量一个变量，推荐把常量写在前边，推荐 `"abc".equals(str)` 不推荐 `str.equals("abc")`
                
            - `public boolean equalsIgnoreCase(String str)` 忽略大小写进行内容的比较字符串内容
        - 字符串与获取相关的方法
            ```java
               public int length() /*获取字符串长度*/
               public String concat(String str) /*将当前字符串和参数字符串，拼接串返回值新的字符串*/
               public char charAt(int index) /*获取指定索引位置的单个字符*/
               public int indexOf(String str) /*查找参数字符在本字符串中首次出现的位置，如果没有返回-1值*/
            ```
        - 字符串截取方法
            ```java
              public String substring(int index) /*从参数位置一直到字符串末尾，返回一个新的字符串*/
              public String substring(int begin, int end) 
              /*截取begin开始到end结束中间的字符串，左闭右开区间，包含左边不包含右边[0,2)*/
            ```
        - 字符串转换相关的方法
            ```java
              public char[] toCharArray() /*将当前字符串拆分出字符数组作为返回值*/
              public byte[] getBytes /*获得当前字符串底层的字数组*/
              public String replice(CharSequence oldString, CharSequence newString) 
              /*将所有出现的旧的字符串替换成新的字符串，返回替换之后的字符串*/
            ```
        - 字符串的分割方法
            - 注意事项：split的参数其实是一个这表达式，如果要是用英文句点"."进行切分，必须写成"\\."
        ```java
          public String[] split(String regex) /*按照参数的规则，把字符串切分成若干个部分*/
        ```
- Arrays数组工具类
    - 只有lang包不需要导包
    - Arrays是一个和数组相关的工具类，里面提供了大量的静态方法，用来实现数组常见的操作
    ```java
      public static String tostring(数组) /*将参数数组变成字符串(按照默认格式[元素1，元素2，元素3, ...])*/
      public static void sort(数组) /*按照默认升序(从小到大)对数组元素进行排序*/
      /*备注：如果是数值，sort默认按照升序从大到小，如果说字符串，sort默认按照字母升序，
             如果是自定义类型，那么这个自定义类需要有Comparable或者Comparator接口支持*/
    ```
- Math数学工具类
    - Math是一个数学相关的工具类，里面提供了大量的静态方法，用来数学运算常见的操作
    ```java
      public static double abs(double num) /*获取绝对值*/
      public static double ceil(double num) /*向上取整*/
      public static double floor(double num) /*向下取整*/
      public static long round(double num) /*四舍五入*/
    
      Math.PI /*代表近似的圆周率*/
    ```
    
- Object类
    - toString()
        - 将一个对象返回为字符串形式，它会返回一个String实例
        - 判断一个类是否重写了toString，直接打印这个类的对象即可，没有重写toString，打印的是对象的地址值，重写了打印的就不是地址值
    - equals()
        - 比较两个对象是否 "相等"；
            - 基本类型比较的是值
            - 引用类型比较的是两个对象的地址值
        - 重写equals()
- Objects类
    - 在JDK7添加了一个Objects工具类，它提供了一些方法类操作对象，他有一些静态的使用方法组成，这些方法是 `null-save` (空指针安全的)或 `null-tolerant` (容忍空指针的)，用来计算对象的 `hascode` ,返回对象的字符串表示形式，比较两个对象，在比较两个类的时候，Object的equals方法任意抛出空指针异常，而Objects的equals方法就化解了这个问题
    ```java
      public class Demo {
          public static void main(String[] args){
              String str = null;
              String str1 = "abc";
            
              str.equals(str1); /*回报 空指针异常错误*/
            
              Objects.equals(str,str1);
          }
      }
    ```
    
- Date类
    - java.util.Date(不在java.lang包下使用需要导包),表示特定的瞬间，精确到毫秒
    ```java
      public class TimeDemo{
          public static void main(String[] args){
              System.out.println(System.currentTimeMillis());
              /*当前系统时间到1970年1月1日 00:00:00共经历的毫秒值*/
              /*毫秒是一个long类型的值*/
              /*中国是东八区 会把时间增加8个小时*/
            
              demo1();
          }
        
          public static void demo1(){
            
              /*构造方法*/
              Date date = new Date(); /*获取当前系统时间和日期*/
              Date date1 = new Date(11236816238126L);  /*把毫秒转成时间和日期*/
              
              /*成员方法*/
              Date date2 = new Date();
              long time = date2.getTime(); /*把日期转换为毫秒*/
          }
      }
    ```
    - DateFormat类
        - 是时间，日期格式化子类的抽象类
        - 作用：格式化(日子 --> 文本), 解析(文本 --> 日期)
        - 成员方法
            - format：按照指定的模式，把Date日期格式化为符合模式的字符串 `string forma(Date date)`
            - parse：把符合模式的字符串，解析为Date日期 `Date parse(String source)`
        - 由于DateFormat类是一个抽象类，不能直接new来创建实例对象，我们可以它的子类SimpleDateFormat类创建对象
        ```java
          public class DataFormatDemo {
              public static void main(String[] args) throws ParseException {
                  method();
                  method1();
              }
          
              private static void method1() throws ParseException {
                  SimpleDateFormat sdf = new SimpleDateFormat("yyyy年mm月dd日");
                  Date parse = sdf.parse("2019年10月20日");
                  System.out.println(parse); /*Sun Jan 20 00:10:00 GMT+08:00 2019*/
              }
          
              public static void method(){
                  SimpleDateFormat sdf = new SimpleDateFormat("yyyy年mm月dd日");
                  Date date = new Date();
                  String format = sdf.format(date);
                  System.out.println(format); /*2019年10月20日*/
              }
          }
        ```
        