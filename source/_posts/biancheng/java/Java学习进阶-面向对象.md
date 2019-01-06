---
title: Java学习进阶-面向对象
date: 2019-01-06 13:21:38
tags: java
categories: 编程
---
Java进阶学习笔记-面向对象

<!-- more -->

- 面向对象的特征：封装性，继承性，多态性
- 类和对象
    - 类的定义
        - 注意事项：成员变量直接定义在类方法中，在方法的外面；成员方法不要写static关键字
    ```java
       /*创建类*/
       package cd.itcast.study;
       public class Student {
           /*成员变量（属性）*/
           String name;
           int age;
       
           /*成员方法（行为）*/
           public void eat() {
               System.out.println("吃");
           }
       }
    ```
    ```java
       /*使用类*/
       package cd.itcast.study;
       public class Teacher {
           public static void main(String[] args) {
               /*导包 同一包cd.itcast.study下直接使用，不需要导入*/
               /*创建
               * 类名称 对象名 = new 类名称();*/
               Student stu = new Student();
               /*使用*/
               System.out.println(stu.name); /*null*/
               System.out.println(stu.age); /*0*/
               stu.name = "张三";
               stu.age = 18;
               stu.eat();/*吃*/
           }
       }

    ```
    - 成员变量和局部变量
        - 定义的位置不一样【重点】
            - 局部变量：在方法的内部
            - 成员变量：在方法的外部，直接写咋类中
        - 作用范围不一样【重点】
            - 局部变量：在方法中能使用，出了方法就不可以使用了
            - 成员变量：整个类都可以使用
        - 默认值不一样【重点】
            - 局部变量：没有默认值，如果要使用，必须手动赋值
            - 成员变量：如果没有赋值，会有默认值，规则和数组一样
        - 内存位置不一样（了解）
            - 局部变量：位于栈内存
            - 成员变量：位于堆内存
        - 生命周期不一样（了解）
            - 局部变量：随之方法进栈而诞生，随着方法出栈而消失
            - 成员变量：随着对象创建而诞生，随着对象被垃圾回收而消失
- 面向对象-封装性
    - 封装性在Java中的体现
        - 方法就是一种封装
        - 关键字private也是一直封装
            - 为题描述：定义Person的年龄时，无法阻止不合理的数值被设置进来
            - 解决方案：用private进行修饰，那么本类中可以随意访问，一旦超出了本类外围之外就不可以直接访问了
            ```java
              public class Person {
                  private int age;
                  private boolean man; /*是否男生 Boolean 类型*/
                  /*只有在Person这个类中才可以使用age属性，其他类中引用了Person，但不可以Person.age 直接使用了 */
                  /*但是可以间接访问age：就是定义一组setter和getter方法*/
                  /*setter不能有返回值参数类型和成员变量对应，getter不能有参数，返回值类型和成员变量对应*/
                  public void setAge(int num){
                      if(num < 100 && age >= 0){
                          age = num;
                      }else {
                          System.out.println("数据不合理");              
                      }
                  }
                  public int getAge(){
                      return age;           
                  }
                  /*****************************************/
                  public void setMan(boolean b){
                      man = b;
                  }
                  /*boolean的getter方法名要写上is+属性名*/
                  public boolean isMan(){
                      return man;
                  }
              }
            ```
            - 设置age `Person.setAge(12)`
    - this关键字的作用
        - 当方法的局部变量和成员变量的重名的时候，根据‘就近原则’，优先使用局部变量，如果必须使用成员变量，就是用this.成员变量名称就可以使用了
        ```java
          public class Person {
              String name;
              public void sayHello(String name){
                  System.out.println(name+ "你好，我是" + name);    /*张三你好，我是张三*/
                  System.out.println(name+ "你好，我是" + this.name);    /*张三你好，我是李四*/
              }
          }
        ```
        - 调用 `Person.name = "李四"; Person.sayHello("张三")` `谁调用了方法this就是谁`
    - 构造方法：专门用来创建对象的方法，当我们用关键字new来创建方法时，其实就是在构造方法；
        - 格式：public 类名称(参数类 参数名称){ 方法体 }
        - 注意事项：
            - 构造方法的名称必须和所在的类名称完全一样，大小写也要一样
            - 构造方法不要写返回值类型，连void都不写
            - 构造方法不能return一个返回值
            - 如果没有编写构造方法，那么编译器就会默认赠送一个构造方法，没有参数，方法体什么事情都不做 `public Student(){}`
            - 一旦编写了至少一个构造方法，那么编译器将不再怎送；
            - 构造方法也是可以进行重载的
            ```java
              /*创建*/
              public class Student {
                  private String name;
                  private int age;
                  public Student() {
                      System.out.println("无参构造方法执行了");                
                  }
                  public Student(String name, int age){
                      this.name = name;
                      this.age = age;
                      System.out.println("全参构造方法执行了");                
                  }
                  public void setName(String name){
                      this.name = name;
                  }
                  public String getName(){
                      return name;
                  }
              }
            ```
            ```java
              /*使用*/
              public class Teacher {
                  public static void main(String[] args){
                      String stu1 = new Student();     /*无参构造方法执行了*/    
                      String stu2 = new Student("张三",20); /*全参构造方法执行了*/
                  }
              }
            ```
    - 标准的类：
        - 一个标准的类必须满足如下四个组成部分：
            - 所有的成员变量都要使用private关键字进行私有化修饰
            - 为每一个成员变量编写一对Getter/Setter方法
            - 编写一个无参数的构造方法
            - 编写一个全参数的构造方法
        - 一个标准的类也叫作 `Java Bean`
        - 编译器自动编写Getter/Setter
            - `编译器Code -- Generate -- Getter and Setter -- shift+鼠标左键选中要编写的成员变量 -- ok`
            - `快捷键 alt+insert -- Getter and Setter -- shift+鼠标左键选中要编写的成员变量 -- ok`
        - 编译器自动编写无参数和有参数的构造方法
            - `编译器Code -- Generate -- Construct -- SelectNone`
            - `编译器Code -- Generate -- Construct -- shift+鼠标左键选中要编写的成员变量 -- ok`
            - `快捷键 alt+insert -- Construct -- SelectNone`
            - `快捷键 alt+insert -- Construct -- shift+鼠标左键选中要编写的成员变量 -- ok`
        