---
title: Java学习进阶-面向对象
date: 2019-01-06 13:21:38
tags: Java
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
    - static关键字
        - 一旦使用了static关键字，那么这样的内容就不属于自己，而是属于类的，凡是本类的对象都是共享一份
        - 一旦是用static修饰成员方法，那么就成为了静态方法，静态方法不属于对象，而是属于类
        ```java
          public class DemoClass {
          
              private static String room;
        
              String name;
          
              public void goroom(){
                  System.out.println("这是一个成员方法");
              }
              public static  void goRoomm(){
                  System.out.println("这是一个静态方法");
                  System.out.println(name); /*报错 静态不能直接访问非静态*/
                  System.out.println(this); /*报错 静态不能使用this关键字*/
              }
              
          }
        ```
        - 使用
        ```java
          public class Demo {
              public static void main(String[] args) {
                  DemoClass obj = new DemoClass();
                  /*没有static的方法 必须new一个对象才可以点出来*/
                  obj.goroom();
                  /*有static的静态方法*/
                  DemoClass.goRoomm(); /*推荐*/
                  obj.goRoomm(); /*不推荐*/
            
                  /* 静态方法，静态变量 都建议类名称点出来使用*/
            
                  /*对于本类当中的静态方法可以省略类名称*/
                  myMethod();
                  
              }
              public static void myMethod(){
                  System.out.println("本类当中的静态方法");
              }
          }
        ```
        - 注意事项：
            - 静态只能直接方法静态，不能直接访问非静态
                - 原因：【先】有静态方法，【后】有非静态方法
            - 静态方法当中不能有this关键字，因为this代表的是当前对象，通过谁调用，谁就是当前对象
            - 根据类名称访问静态成员变量的时候，全程和对象没有关系，只和类有关系
            
        - 静态代码块
            - 典型用途：用来一次性的对静态成员进行赋值
        ```java
          public class Demo {
              static {
                  /*静态代码块的内容*/
                  System.out.println("静态代码块执行");
                  
                  /*特点：当第一次用到本类时，静态代码块执行唯一的一次
                  * 注意：静态总是优先于非静态，所以静态代码块先执行*/
              }
        
              public Person(){
                  System.out.println("构造方法执行了");
              }
          }
        ```
- 面向对象-继承性
    - 格式：
        - 定义父类的格式：一个普通的类 `public class 父类名称 {  }`
        - 定义子类的格式: `public class childClass extends FatherClass { }`
    - 在父子类继承关系中，如果成员变量出现重名，则创建子类对象时，访问规则：
        - 直接通过子类访问成员变量：等号左边是谁就优先用谁，没有就向上找
        - 间接通过成员方法访问成员变量：该方法属于谁，就优先使用谁，没有则向上找
    - 区分子类方法中重名
    ```java
      public class Fu {
          int num =10;
      }
      public class Zi extends Fu {
          int num = 20;
          public void method(){
             int num = 30;  
             System.out.println(num); /*局部*/
             System.out.println(this.num);/*本类*/
             System.out.println(super.num); /*父类*/
          }
      }
      /*局部变量          直接写成员变量名
       *本类成员变量       this.成员变量名
       *父类成员变量       super.成员变量名 */
    ```
    - 继承中成员方法的访问特性
        - 父子类成员方法重名：创建的对象是谁就优先用谁，如果没有就向上找
    - 注意事项：无论是成员变量还是成员方法，都是向上找父类，绝对不会向下找子类
    - 继承方法中的覆盖重写
        - 重写(Override)：在继承关系中，方法名一样，参数列表一样，也叫作覆盖，复写
        - 重写(Override)和重载(Overload)的区别：
            - 重写发生在继承关系中:方法名一样，参数列表一样
            - 重载：方法名一样，参数列表不一样
        - 方法覆盖重写的特点：创建的是子类对象，优先使用子类方法。
        - 注意事项：
            - 必须保证父子类之间方法名称相同，参数列表也相同，@Override写在方法的前面，用来检测是不是有效的覆盖重写
            ```java
              public class Zi extends Fu {
                  @Override
                  public void method(){}
                      /* @Override 这个注解就算不写，只要满足要求，也是正确的方法覆盖重写*/
                  }
            ```
            - 子类方法的返回值必须小于等于父类方法的返回值范围
            - 子类方法的权限必须大于等于父类方法的权限修饰符；
                - 权限修饰符： public > protected > (default) > private
                - (default)不是关键字default，而是什么都不写，留空   
        - 方法覆盖重写使用环境:已经投入使用的类不要修改，可以new一个新类覆盖重写添加新的功能
    - 继承中构造方法的访问特性
        - 子类构造方法中有一个默认隐含的super()调用，所以一定是先调用了父类构造，后执行了子类构造
        - 子类构造可以通过super关键字来调用父类重载构造
        - super的父类构造调用，必须是子类构造方法的第一个语句，不能一个子类构造调用多次super构造
        - 只有子类构造方法才能调用父类构造方法
        - 子类必须调用父类构造方法，不写默认赠送一个super()，写了则用写的指定的super调用，super只能有一个，而且必须是第一个;
        ```java
            public class Fu {
                public Fu(){
                    System.out.println("父类无参构造");
                }
                public Fu(int num){
                    System.out.println("父类有参构造");
                }
            }
            public class Zi extends Fu {
                public Zi(){
                    /* super(); 在调用父类无参构造方法 默认就有 */
                    super(10); /*在调用父类重载的构造方法*/
                }
            }
        ```
    - super关键字的三种用法
        - 在子类的成员方法中，访问父类的成员变量
        - 在子类的成员方法中，访问父类的成员方法
        - 在子类的构造方法中，访问父类的构造方法
    - this关键字的三种用法(super关键字用来访问父类内容，而this关键字用来访问本类内容)
        - 在本类的成员方法中，访问本类成员变量
        - 在本类的成员方法中，访问本类的另一个成员方法
        - 在本类的构造方法中，访问本类的另一个构造方法：
            - this(...)也必须是构造方法的第一个语句，唯一一个
            - super和this两种构造调用，不能同时使用
        ```java
           public class Zi extends Fu {
               public Zi(){
                   this(10); /*本类的无参构造调用本类的有参构造*/
                   /*this(10,30) 上边已经有了，这里会报错*/
               }
               public Zi(int num){ }
               public Zi(int num, int num1){ }
           }
        ```
- Java语言的继承特点
    - 单继承：一个类的直接父类只有唯一一个
    - 多级继承：c继承的b,b继承的a， a也可以是c的父类，但不是直接父类,Java中最高父类 java.lang.Object
    - 一个子类的直接父类是唯一的但是一个父类可以拥有多个子类
    
- 抽象类
    - 抽象方法：就是加上abstract关键字，然后去掉大括号直接分号结束；
    - 抽象类：抽象方法所在的类必须是抽象方法才可以。在class之前写上abstract即可；
    ```java
      public abstract class Person { /*抽象类*/
          public abstract void eat(); /*抽象方法*/
          public void run(){}; /*普通成员方法*/   
      }   
    ```
    - 使用抽象类和抽象方法
        - 不能直接创建new抽象类方法
        - 必须用一个子类继承抽象父类
        - 子类必须覆盖重写父类当中所有的重写方法(子类去掉抽象方法的abstract关键字，然后补上方法体大括号)
        - 创建子类方法进行使用
        ```java
          public class Student extends Person {
              @Override
              public void eat(){
                  System.out.println("子类重写覆盖父类方法");
              }
          }
        ```
    - 注意事项：
        - 抽象类不能直接创建对象
        - 抽象类中，可以有构造方法，是供子类创建对象时，初始化父类成员使用的
        - 抽象类中，不一定包含抽象方法，但抽象方法的类必须是抽象类
        - 抽象类的子类，必须重新抽象父类中所有的抽象方法，否则，编译无法通过报错，除非该子类也是抽象类；

- 面向对象-多态性
    - 代码当中多态性的体现：父类引用执行子类对象
    - 格式： `父类名称 对象名 = new 子类名称(); 或者 接口名称 对象名 = new 实现类名称()`
    ```java
      public class Demo {
          public static void main(String[] args){
              Fu obj = new Zi();
              obj.method(); /*遵循成员方法使用规范和优先级*/
          }
      }
    ```
    - 对象的向上转型：就是多态写法
        - 含义：右侧创建一个子类，把他看做一个父类来使用
        - 注意：向上转型一定是安全的，从小范围转向了大范围
        - 弊端：向上转型为父类，那么就无法在调用子类特有的内容
    - 对象的向下转型：其实是一个【还原】的动作
        - 格式 `之类名称 对象名 = (子类名称)父类对象`
        - 含义：将父类对象还原为本来的子类对象
        ```java
          Person per = new Student();
          Student stu = (Student)per;
        ```
    - instanceof 关键字
        - 返回以个Boolean值，判断前面的对象能不能作为后面类型的实例

- final关键字
    - 用法
        - 可以用来修饰一个类
            - 格式： `public final class 类名称 { }` 
            - 含义：当前这个类不能有任何子类,也就是说该类的所有成员方法无法进行覆盖重写
        - 可以用来修饰一个方法
            - 格式： `public final int(返回值类型) 方法名称(参数列表){ }`
            - 被final修饰的方法就会成为一个最终方法，不可以被覆盖重写
        - 可以用来修饰修饰一个局部变量
            - 局部变量被final修饰后，就不能进行修改了,(先定义后赋值是可以的，只要保证唯一一次赋值就可以)
        - 可以用来修饰一个成员变量
            - 被final修饰的成员变量，那么这个成员变量也是不可改变了
            - 由于处于变量有默认值，所有用了final之后就必须手动赋值，不会再给默认值
            - 对于final的成员变量，要么直接赋值，要么通过构造方法赋值【二只选其一】
            - 必须保证类当中的所有重载的构造方法，都最终对final的成员变量进行赋值
    - 注意事项：
        - 对于类，方法，abstract和final不能同时使用，因为矛盾；