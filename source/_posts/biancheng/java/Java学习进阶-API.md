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
        
- calender类 日历类
    - 是一个抽象类，提供了很多操作日历字段的方法(YEAR，MONTH，DAY_OR_MONTH，HOUR)
    - 静态成员方法getInstance()，该方法返回了Calender类的子类对象
    - 常用方法
        - `public int get(int field)` 返回给定日历字段的值
        - `public void set(int field, int value)` 将给定的日历字段设置为给定值
        - `public abstract void add(int field, int amount)` 根据日历的规则，为给定的日历字段添加或删去指定的时间量
        - `public Date getTime()` 返回一个表示此Calender时间值(从历元到现在的毫秒偏移量)的Date对象 
        ```java
          public class CalenderDemo {
              public static void main(String[] args) {
                  Calendar calendar = Calendar.getInstance(); /*对象的多态性*/
                  System.out.println(calendar);
                  demo();
              }
          
              private static void demo() {
                  Calendar c = Calendar.getInstance();
                  c.set(Calendar.YEAR,2019); /*设置字段*/
                  int i = c.get(Calendar.YEAR); /*获取*/
                  int i1 = c.get(Calendar.MONTH);
                  System.out.println(i);
                  System.out.println(i1);
                  c.set(2019,01,15); /*设置年月日*/
                  c.add(Calendar.YEAR,2);/*年份增加2年*/
                  c.getTime();/*把日历转化成日期*/
              }
          }
        ```

- System类
    - 其中提供了大量的静态方法，和系统的操作有关系
    - 常用方法：
        - `public static long currentMills()` 返回以毫秒值为单位的当前时间
        - `public static void arraycopy(Object src, int srcPos, Object dest, int destPos, int length)` 将数组中指定的数据拷贝到另一个数组中
            - 参数：原数组，原数组起始位置，目标数组，目标数组的起始位置，要复制的元素个数
            - 覆盖了数组本身位置上的元素
            
- StringBuilder类(字符串缓冲区)
    - 可以提高字符串的操作效率(可以看出为一个长度可以自由变换的字符串)，底层是一个数组但没有被final修饰，可以改变长度
    - 初始容量是长度是16，超出会自动扩容
    - 构造方法(有参无参构造)
    - 常用方法
        - `append()` 给当前字符串添加
        - `toString()` 将当前的StringBuilder对象转换成String对象
        
- 包装类
    - 基本数据类型，使用起来非常的方便，但是没有对应的方法来操作这些基本类型的数据，可以使用一个类，把基本的数据类型封装起来，在定义一些方法，这些类叫做包装类，我们可以使用包装类的方法来操作基本类型数据
    - 装箱和拆箱
    ```java
      public class IntegerDemo {
          public static void main(String[] args) {
              /*装箱*/
              Integer in1 = new Integer(1);
              System.out.println(in1);
              Integer in2 = new Integer("1");
              System.out.println(in2);
              Integer in3 = Integer.valueOf(1);
              System.out.println(in3);
              Integer in4 = Integer.valueOf("1");
              System.out.println(in4);
              /*拆箱*/
              int in5 = in1.intValue();
              System.out.println(in5);
          }
      }
    ```
    - 自动装箱和拆箱：基本类型数据和包装类之间可以自动的相互转换
    ```java
      public static void interDemo(){
          Integer i = 1; /*自动装箱 直接把int类型数据赋值给包装类 --> Integer i = new Integer(1);*/
          i = i + 2; /*自动拆箱 i是包装类，无法直接参与运算，可以自动转化为基本数据类型，在进行计算，i+1 --> i.intValue() + 2*/
      }
    ```
    - 基本类型和字符串类型相互转换
    ```java
      public class IntegerChangeString {
          public static void main(String[] args) {
              /*基本类型 --> 字符串类型*/
              int i = 1;
              String s1 = i + "";
              System.out.println(s1 + 200); /*1200*/
              String s2 = Integer.toString(123);
              System.out.println(s2+123);/*123123*/
              String s3 = String.valueOf(123);
              System.out.println(s3+123); /*123123*/
      
              /*字符串类型 --> 基本类型*/
              int i1 = Integer.parseInt(s1);
              System.out.println(i1-10); /*-9*/
          }
      }
    ```
    
- 集合Collection
    - 集合只能存储对象引用，常用集合有List，Set，Map，其中List和Map继承了Collection接口
    - 常用功能
        - add() 给定对象添加到当前集合中
        - clear() 清空集合中所有元素
        - remove() 把给定的对象在集合中删除
        - contains() 判断集合是否包含给定的对象 
        - isEmpty() 判断当前集合是否为空
        - size() 返回集合元素个数
        - toArray() 把集合中的元素储存到数组中
- Collections：
        - addAll() 往集合中添加一些元素
        - shuffle() 打乱集合的元素顺序
        - sort() 将集合元素按照默认规则排序
            - 注意：被排序的集合里面存储的元素必须实现Comparable，重写接口中的方法compareTo定义排序规则
            - 排序规则：自己(this) - 参数(升序)反之降序
        - sort() 参数:集合，比较器
- Iterator迭代器
    - Iterator接口(迭代器)对集合进行遍历
    - 常用方法：
        - hasNext() 判断集合是否有下一个元素，有返回true 没有返回false
        - next() 取出集合中下一个元素
    - 使用：由于Iterator迭代器是一个接口，使用时需要使用其实现类，Iterator接口的实现类获取比较特殊，在Collection接口中，有一个iterator方法，返回的就是Iterator接口的实现类
        - 先使用集合中的iterator方法获取Iterator接口的实现类；
        ```java
          public class IteratorDemo {
              public static void main(String[] args) {
                  Collection<String > coll = new ArrayList<>();
                  coll.add("李四");
                  coll.add("王五");
                  coll.add("赵柳");
          
                  Iterator<String> it = coll.iterator();/*多态*/
                  System.out.println(it.hasNext()); /*true*/
                  System.out.println(it.next()); /*李四*/
          
                  /*while 循环取出集合所有元素*/
                  while (it.hasNext()){
                      System.out.println(it.next());
                  }
                  /*for 循环*/
                  for (Iterator<String> it2 = coll.iterator(); it2.hasNext();){
                      System.out.println(it2.next());
                  }
              }
          }
        ```
        
- forEach 
    - JDK1.5以后出现，专门用来遍历数组和集合的，内部原理就是要给Iterator迭代器，在便利过程中不可以对元素进行增删操作
    ```java
      public class ForEach {
          public static void main(String[] args) {
              demo1();
              demo2();
          }
      
          private static void demo2() {
              ArrayList<String> list = new ArrayList<>();
              list.add("王五");
              list.add("李四");
              for(String i:list){
                  System.out.println(i);
              }
          }
      
          private static void demo1() {
              int[] arr = {1,2,3,4};
              for (int i: arr) {
                  System.out.println(i);
              }
          }
      }
    ```
- 泛型：是一种未知的数据类型，不知道使用何种数据类型的时候，可以使用泛型，可以看做为一个变量，来接收数据类型
    - 创建集合使用泛型的优势：避免数据类型的转换，存储什么类型取出就是什么类型，把运行期的异常提升到编译期；弊端：泛型觉得集合存储的数据类型
    - 创建集合不使用泛型的优势：不用泛型规定集合存储的数据类型，集合默认存储object类型，也就是什么数据都可以存储；弊端：不安全，会引发异常
    - 自定义含有泛型的类
        ```java
          public class GenericClass<E> {
              /*定义一个含有泛型的类*/
              private E name;
          
              public E getName() {
                  return name;
              }
          
              public void setName(E name) {
                  this.name = name;
              }
          }
          public class GenericClassDemo {
              public static void main(String[] args) {
                  /*不写泛型默认是一个object类型*/
                  GenericClass gc = new GenericClass();
                  gc.setName("我叫王大锤");
                  Object gcName = gc.getName();
                  /*泛型使用Integer*/
                  GenericClass<Integer> gc2 = new GenericClass<>();
                  gc2.setName(1);
                  Integer gc2Name = gc2.getName();
                  int gc2NameInt = gc2.getName();
                  /*泛型使用String*/
                  GenericClass<String> gc3 = new GenericClass<>();
                  gc3.setName("我也叫王大锤");
                  String gc3Name = gc3.getName();
              }
          }
        ```
    - 自定义含有泛型的方法：定义在方法的修饰符和返回值之间
    ```java
      public class GenericMethod {
          /*定义一个含有泛型的方法*/
          public <M> void method01(M m){
              System.out.println(m);
          }
          /*静态方法*/
          public static <M> void method02(M m){
              System.out.println(m);
          }
      }
      public class GenericMethodDemo {
          public static void main(String[] args) {
              GenericMethod gm = new GenericMethod();
              gm.method01(10);
              gm.method01("我是王大锤");
              /*静态方法*/
              GenericMethod.method02("静态方法使用不建议创建对象使用，直接类名点方法名使用");
          }
      }
    ```
    - 自定义含有泛型的接口
    ```java
      public interface GenericInterface<I> {
          /*定义含有泛型的接口*/
          public abstract void method(I i);
      }
      public class GenericInterfaceImp1 implements GenericInterface<String> {
          /*含有泛型接口第1种使用：定义接口的实现类，实现接口，并且指定接口的泛型*/
          @Override
          public void method(String s) {
              System.out.println(s);
          }
      }
      public class GenericInterfaceImp2<I> implements GenericInterface<I> {
          /*含有泛型接口第2种使用：接口使用什么泛型，实现类就用什么泛型，类跟着接口走*/
          @Override
          public void method(I i) {
              System.out.println(i);
          }
      }
      public class GenericInterfaceDemo {
          public static void main(String[] args) {
              GenericInterfaceImp1 gmI1 = new GenericInterfaceImp1();
              gmI1.method("String"); /*String*/
      
              GenericInterfaceImp2<Integer> gmI2 = new GenericInterfaceImp2<>();
              gmI2.method(123);/*123*/
      
              GenericInterfaceImp2<Double> gmI3 = new GenericInterfaceImp2<>();
              gmI3.method(8.8);/*8.8*/
          }
      }
    ```
    - 泛型的通配符<?>
    ```java
      public class GenericInterface1 {
          /**
           * 泛型的通配符
           * ？：代表任意的数据类型
           * 使用方法：不能创建对象使用，只能作为方法的参数使用*/
          public static void main(String[] args) {
              ArrayList<Integer> list = new ArrayList<>();
              list.add(1);
              list.add(2);
              ArrayList<String> list01 = new ArrayList<>();
              list01.add("王五");
              list01.add("李四");
              /*定义一个可以遍历所有类型的ArrayList集合的方法，此时不知道集合的数据类型，就可以使用泛型的通配符来接收数据类型*/
      
              printList(list);
              printList(list01);
          }
      
          public static void printList(ArrayList<?> list){
              /*使用迭代器遍历*/
              Iterator<?> it = list.iterator();
              while (it.hasNext()){
                  Object o = it.next();
                  System.out.println(o);
              }
          }
      }
    ```
    - 泛型通配符的高级使用(受限泛型)
        - 上限限定： `? extends E` 使用的泛型类型只能是E类型的子类或者是本身
        - 下限限定： `? super E` 使用的泛型只能是E的父类或者本身

- List接口
    - 特点：
        - 有序集合：存储元素和取出元素顺序一致
        - 有索引：包含了一些带索引的方法
        - 与set集合不同，允许存储重复元素
    - 方法：
        - add(index,elemnt); 将元素添加到指定的位置
        - get(index); 获取指定位置的元素
        - remove(index); 删除指定位置的元素
        - set(index,element); 替换指定的元素，返回值被替换的元素
    - 遍历：for,iterator(迭代器),增强for
    - 子类(实现类)
        - ArrayList：底层是一个数组结构(查询快，增删慢)，多查询建议使用
        - LinkedList：
            - 底层是一个链表集合(查询慢，增删块)，多操作建议使用
            - 方法：
                - addFirst(e);元素添加到列表开头
                - addLast(e); 元素添加到列表末尾
                - getFirst(e); 
                - getLast(e);
                - romoveFirst(e);
                - romoveLast(e);
                - pop(); --> removeFirst();
                - push(); --> addFirst();
                - isEmpty();判断列表是否为空(没有元素返回true);
                
- Set接口
    - 特点：
        - 不包含重复元素的集合
        - 没有索引，不能够使用普通的for循环遍历
    - 子类，实现类：
        - HashSet:
            - 特点：
                - 无序集合(元素取出存入顺序不一致)
                - 底层是一个哈希表结构(查询的速度特别快);
            - 哈希值：
                - hashCode():返回对象的哈希码值
            - HashSet集合存储自定义类型元素
                -必须重写  equals 和 hasCode 方法
        - LinkedHashSet：
            - 特点：有序，不允许元素重复
- 可变参数：方法的参数数据类型确定个数不确定
    - 注意事项
        - 一个方法的可变参数只有一个
        - 方法的参数有多个时，可变参数必须写在参数列表的末尾
    ```java
      public class Demo{
          public static void main(String[] args){
              int i = method(1,1,1);
              System.out.println(i); /*3*/
          }
          public static void method(int...arr){
              int sum = 0;
              for (int i : arr) {
                  sum += i;
              }
              return sum;
          }
      }
    ```

- Map<K,V>集合
    - 特点：
        - 是一个双列集合，一个元素包含两个值(一个Key，一个Value)
        - key和value的数据类可以相同也可以不同
        - key是不允许重复的，value是可以重复的
        - key和value是一一对应的
    - 常用的实现类(子类)
        - HashMap<k,v> 无序集合，多线程，底层是hash表(查询快)
            - 子类：LinkedHashMap 底层是一个hash表+链表，有序集合
    - 常用方法：
        - put(k,v) 返回被替换的值
        - get(k)
        - remove(k)
        - containsKey(k)集合中是否包含key
        - keySet() 获取Map集合中的所有key，存储到Set集合中
        - entrySet() 获取Map集合中所有的键值对对象的集合(Set集合)
        ```java
          public class Demo {
              public static void main(String[] args){
                  Map<String,Integer> map = new HashMap<>();
                  map.put("张三",19);
                  map.put("李四",20);
                  Integer age = map.remove("张三");
                  map.get("李四"); /*20*/
                  map.containsKey("李四");/*true*/
                  /*Map集合遍历的方式一*/
                  Set<String> set = map.keySet();
                  Iterator<String> it = set.iterator();
                  while (it.hasNext()){
                      String key = it.next();
                      Integer value = map.get(key);
                  }
                  
                  for(String key : set){
                      Integer value = map.get(key);
                  }
            
                  /*Map集合遍历的方式二*/
                  Set<Map.Entry<String,Integer>> set = map.entrySet();
                  Iterator<Map.Entry<String,Integer>> it = set.iterator();
                  while (it.hasNext()){
                      Map.Entry<String,Integer> entry = it.next();
                      String key = entry.getKey();
                      Integer value = entry.getValue();
                  }
            
                  for(Map.Entry<String,Integer> entry : set){
                      String key = entry.getKey();
                      Integer value = entry.getValue();
                  }
              }
          }
        ```
    - Entry<K,V>
        - 在Map接口中有一个内部接口Entry(Map.Entry<K,V>)
        - 作用: 用来记录键与值(键值对对象，键与值的映射关系)
    - Hashtable<K,V>
        - 不允许存储null值、null建，单线程(速度慢)
        - JDK 1.2 之后被取代了
        - 子类Properties仍在使用，Properties集合是一个唯一和IO流相结合的集合
        
- JDK 1.9 对集合添加的优化 of方法(静态方法) (List Set Map)
    - 给集合一次性添加多个元素
    - 使用前提：集合中存储的元素个数已经确定，不在改变时使用
    - 注意事项：
        - of方法只适用于List Set Map,不适用于接口的实现类
        - of方法的返回值是一个不可以改变的集合，不能在使用add，或者put添加元素了
        - Map和Set在调用of方法的时候，不有重复元素，否则会抛出异常
        ```java
          public class Demo {
              public static void main(String[] args){
                 List<String> list = List.of("a","b","a");
                 Set<String> set = Set.of("a","b");
                 Map<String,Integer> map = Map.of("张三",19,"李四",20);
              }
          }
        ```