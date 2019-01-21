---
title: Java学习进阶-接口
date: 2019-01-12 16:35:50
tags: Java
categories: 编程
---
Java进阶学习-接口

<!-- more -->

- 接口就是多个类的公共规范，接口是一种引用类型数据，最重要的内容就是其中：抽象方法
- 接口的定义
    - 格式 `public interface 接口名称{ 接口内容 }`
        - 备注：换成了的关键字interface之后，编译生成的字节码文件任然是：.java --> .class
        - 如果是java 7，接口中包含的内容有；
            - 常量
            - 抽象方法
        - 如果是java 8，还可以额外包含
            - 默认方法
            - 静态方法
        - 如果是java 9，还可以包含
            - 私有方法
- 接口的抽象方法定义
    - 在任何版本的java中，接口都可以定义抽象方法
    - 注意：
        - 接口中抽象方法的修饰符必须是固定个关键字： `public abstract`
        - 这两个关键字可以选择性的省略
        - 方法的三要素可以随意定义
        ```java
          public interface Myinterface {
              public abstract void methosAbs(); /*这是一个抽象方法*/
              abstract void methosAbs1(); /*这是一个抽象方法*/
              public void methosAbs2(); /*这是一个抽象方法*/
              void methosAbs3(); /*这是一个抽象方法*/
          }
        ```
- 接口的抽象方法使用步骤
    - 接口不能直接使用必须有一个实现类来实现接口
        - 格式： `public class 实现类名称 implements 父类`
    - 接口的实现类必须覆盖重写接口类当中的所有重写方法
    - 创建实现类的对象进行使用
    ```java
      public class MyinterfaceImpl implements Myinterface{
      
      
          @Override
          public void methosAbs() {
              System.out.println("这是第一个方法");
          }
      
          @Override
          public void methosAbs1() {
              System.out.println("这是第二个方法");
          }
      
          @Override
          public void methosAbs2() {
              System.out.println("这是第三个方法");
          }
      
          @Override
          public void methosAbs3() {
              System.out.println("这是第四个方法");
          }
      }
      public class Demo {
          public static void main(String[] args) {
              MyinterfaceImpl impl = new MyinterfaceImpl();
              impl.methosAbs();
              impl.methosAbs1();
          }
      }
    ```
    - 注意事项：如果这个实现类并没有完全重写接口当中所有的抽象方法，那么这个实现类自己就必须是以抽象类
- 接口的默认方法
    - 定义
        - 从Java 8开始，接口允许定义默认方法
        - 格式： `public default 返回值类型 方法名称(参数列表){ 方法体 }`
        - 备注：接口当中的默认方法可以解决接口的升级问题
    - 使用
        - 接口的默认方法可以通过接口的实现类直接调用
        - 接口的默认方法也可以被接口的实现类覆盖重写
- 接口的静态方法
    - 定义
        - 从Java 8开始接口中允许定义静静态方法
        - 格式： `public static 返回值类型 方法名称(参数列表){ 方法体 }`
    - 使用
        - 不能通过接口实现类的对象来调用接口当中的静态方法
        - 通过接口名称直接调用接口的静态方法 
- 接口的私有方法
    - 定义
        - 从Java 9开始，接口中允许定义私有方法
            - 普通私有方法：解决多个默认方法中重复代码问题 `private 返回值类型 方法名称(参数列表){ 方法体 }`
            - 静态私有方法：解决多个静态方法中重复代码问题 `private static 返回值类型 方法名称(参数列表){ 方法体 }`
    - 只有接口本身的方法才可以调用私有方法，实现类不可以调用的
- 接口常量
    - 定义：接口中也可以定义 "成员变量"，但必须使用 `public static final` 三个关键字来修饰，这就是接口的常量，不可改变，
    - 格式： `public static final 数量类型 常量名称 = 数据值`  
    - 注意
        - 可以省略 `public static final`
        - 接口中的常量必须进行赋值
        - 接口中常量的名称使用完全大写的字母，用下划线分割
    - 使用：接口名称直接点
    ```java
      public static final int NUM_OR_CLASS = 12;
      MyInterfaceConst.NUM_OR_CLASS;
    ```
    
- 接口使用时注意事项
    - 接口是没有静态代码块和构造方法的
    - 一个类的父类是唯一的，但是一个类可以同时实现多个接口
    ```java
      public class MyInterfaceImp implements MyinterfaceA,MyinterfaceB { /*覆盖重写所有重写方法*/}
    ```
    - 如果实现类实现的多个接口当中，存在重复的抽象方法，秩序覆盖重写一次即可
    - 如果实现类没有覆盖重写所有的抽象类中的所有抽象方法，那么这个实现类必须也是一个的抽象类
    - 如果实现类实现的多个接口中有重复的默认方法，实现类一定要对冲突的默认方法进行覆盖重写
    - 一个类如果直接父类中的方法和接口当中的默认方法产生冲突，优先使用父类当中的方法
- 类和接口之间的关系
    - 类与类之间是单继承的，直接父类只有一个
    - 类与接口之间是多实现的，一类可以实现多个接口
    - 接口与接口之间是多继承的
        - 多个父接口的抽象方法重复，没关系
        - 多个父接口的默认方法重复，那么子接口必须进行默认方法的覆盖重写【而且要带有 `default`关键字】
    ```java
      public interface MyInterface extends MyinterfaceA,MyinterfaceB{}
    ```