---
title: Java学习基础
date: 2018-12-18 22:31:20
tags: Java
categories: 编程
---
Java学习基础-笔记

<!-- more -->
- window命令操作：
    - 切换盘符 D: d:
    - 进入文件夹 cd 文件夹名称
    - 进入多级文件夹 cd 文件夹1\文件夹2
    - 返回 cd ..  
    - 直接返回根目录 cd \
    - 查看当前文件夹中的内容 dir
    - 清空屏幕 cls
    - 退出 exit
- JRE和JDK
    - 运行一个已有的Java程序，只需安装JRE;
    - 运行一个去全新的Java程序，就必须安装JDK；
    - JDK安装目录不要有空格和中文
    - 公共JRE不安装
- Java-HolleWorld
    - 后缀 xx.java
    - javac.exe 编译器
    - jave.exe 解释器
    ```java
        /*第一行第三个单词必须和所在的文件名称完全一样，大小写也要一样*/
        /*public后面代表定义一个类的名称，类是java当中所有源代码的基本组织单位*/
        public class HelloWord {
            /*第二行的内容是万年不变的固定写法，代表main方法*/
            /*这一行代表程序执行的起点*/
            public static void main(String[] args){
                /*第三行代表打印输出语句（其实就是屏幕显示）*/
                /*希望显示什么东西，就在小括号中填写什么东西*/
                System.out.println("Hellow,World!");
            }
        }
    ```
    - 编辑 `javac HelloWorld.java` 编译完成会生成 `HelloWoeld.class`文件
    - 运行 `java HelloWorld`
- Java关键字
    - 特点：完全小写的字母；
    - 在增强版的记事本中（例如Notepad++）有特殊颜色；
- Java标识符
    - 也叫标志符，就是我们程序中自定义的内容；
    - 命名规则：硬性要求（必须要遵守的规则）
        - 标识符可以包含 `英文字母26个，区分大小写` `0-9数字` `$` `_`
        - 标识符不能以数字开头
        - 标识符不能有关键字
    - 命名规则：软性要求
        - 类名桂发：驼峰命名法——首字母大写，后面单词首字母大写
        - 变量命规范：首字母小写——后面单词首字母大写
        - 方法名规范：同变量名
- Java数据类型
    - 基本数据类型
        - 整数型 （关键字，类别） byte（字节型） short（短整型） int（整型 默认） long（长整型）
            - 占用的内存空间不一样 1 2 4 8 个字节
        - 浮点型 float（单精度浮点数） double（双精度浮点数 默认）
            - 4 8 个字节 字节越高越精确
        - 字符型 char 2个字节
        - 布尔型 boolean 1个字节
    - 引用数据类型
        - 字符串，数组，类，接口，Lambda
    - 没有其他的类类型了
    - 注意事项：
        - 字符串不是基本类型而是引用类型
        - 浮点型可能只是一个近似值，并非一个精确值
        - 数据范围和字节数不一定相关，例如float数据范围比long更加广泛，但是float是4字节，long是8个字节
        - 浮点数类型中默认类型是double，如果一定要使用float类型，需要加上一个后缀F（大小写都可以推荐F）
        - 如果是整数默认为int类型 若果一定要使用long类型，需要加上一个后缀L（大小写都可以推荐L）
- Java常量
    - 概念：Java程序运行过程中固定不变的数据
    - 分类：
        - 字符串常量；凡是用双引号引起来的部分 "abc" "123" 内容可以是一个多个或者没有
        - 整数常量：例如直接写上的数字，没有小数点 -100 200
        - 浮点数：直接写上的小数，有小数点 -2.5 2.5
        - 字符常量：凡是用单引号，引起来的的单个字符，就是字符常量 'q' 'A' '9' '中'  引号中间的内容有且只有一个，不能有多个
        - 布尔常量：true， false
        - 空常量：null，代表没有任何数据
        ```java
         public class Dome01Const {
            puclic static void main(String[] args){
                /*字符串常量*/
                Systen.out.println("ABC");
                Systen.out.println("A");
                Systen.out.println("XYZ");
                /*整数常量*/
                System.out.println(30);
                System.out.println(-30);
                /*浮点数常量*/
                System.out.println(3.14);
                System.out.println(-3.14);
                /*字符常量*/
                System.out.println('A');
                System.out.println('0');
                System.out.println(' '); /*中间有空格不会报错，''中间有且必须有一个字符*/
                System.out.println(''); /*不可以这样写会报错*/
                System.out.println(''); /*不可以这样写会报错*/
                /*布尔常量*/
                system.out.println(true);
                system.out.println(false);
                /*空常量。空常量不能用来打印输出*/
                System.out.println(null); /*会报错*/
            }
         } 
        ```
- Java变量
    - 程序运行期间内容可以发生改变的量
    - 创建一个变量并且使用的格式
        - 数据类型 变量名称;  这就是创建变量的格式
        - 变量名称 = 数据值; = 是赋值的意思不是数学中的等于的意思，把右边的数据值放到左边的变量值中
        - 数据类型 变量名称 = 数据值 
		```java
            public class Demo02{
                public static void main(String[] args){
                    /*创建一个的变量*/
                    int num1;
                    num1 = 10;
                    System.out.println(num1);
                    /*改变变量的值*/
                    num1 = 20;
                    System.out.println(num1);
                    /*一步到位*/
                    int num2 = 10;
                    System.out.println(num2);
                    
                    long num3 = 3000000000; /*报错超出数据范围*/
                    long num3 = 3000000000L; /*正确*/
                    
                    float num4 = 2.5F;
                    
                }
            }
		```
		- 使用变量的注意事项
			- 如果创建多个变量，变量之间的名称不可以重复
			- 对于float和long类型，字母后缀F和L不可以丢掉
			- 在使用byte和short类型的变量，那么右侧的数据值不能超过左侧数据范围；
			- 没有赋值的变量不能直接使用，一定要赋值之后才能使用
			- 变量的使用不能超出作用域的范围，先创建后使用
				- 【作用域】：从定义变量的一行开始，一直到所属大括号结束为止
			- 可以通过一个语句创建多个变量（一般不建议）`int a=10,b=20,b=30;`
- Java数据类型转换
	- 当数据类型不一样的时候会发生数据类型转换
	- 自动类型转换（隐式转换）
		- 特点：代码不需要特殊处理自动完成；
		- 规则：数据范围从小到大
	- 强制转换（显式转换）
		- 特点：代码需要特殊的格式处理，不能自动完成
		- 格式：范围小的类型 范围小的变量名 = （范围小的类型）原本范围大的数据
			- `int num = (int)100L`
		- 注意事项
			- 强制类型转换一般不推荐使用，有可能发生精度损失（小数）和数据溢出（整数）
			- byte/short/char 这三种类型都可以发生数学运算，例如加法“+”
				- char类型进行了数学运算，那么字符就会按照一定的规则翻译成一个数字
				```java
                    char num = 'A';
                    System.out.println(num + 1); /*66*/
                ```
			- byte/short/char这三种类型在运算的时候都会被首先提升为int类型，然后再计算
			```java
                byte num = 40;
                byte num1 = 50;
                int result = num + num1; /* 90 */
                
                /*强制接收*/
                short num2 = 60;
                short result1 = (short) (num + num2);
			```
			- boolean类型不能发生数据类型转化，不存在 0 当做false 1当做true
- 数字和字符的对照关系表（编码表）：
	- ASCII码表：American Standard Code for Information Interchange，美国信息交换标准代码；
	- Unicode码表：万国码。也是数字和字符的对照关系，开头0-127部分和ASCII完全一样，但是从128开始包含更多字符。
- Java运算符和表达式
	- 概念：进行特定操作的符号。例如：+
	- 概念： 用运算符来接起来的是式子叫做表达式，例如：20+5，又例如：a+b
	- 算数运算符
		- `+ - * / % ++ --`
	- 对于一个整数的表达式来说，除法用的是整数，结果任然是整数，只看商，不看余数，只有对于整数的除法来说，取模运算符才有余数的意义
	- 注意事项：
		- 一旦运算中有不同的数据类型，那么结果将会是数据类型范围大的那种
		- byte、short、char 在运算是被转成int类型再去计算（ASCII码表）
	- +号的特殊介绍（四则运算当中加号“+”有常见的三种方法）：
		- 对于数值来说，那就是加法；
		- 对于字符char类型来说，在计算之前，char会被提升为int，然后再计算，char类型字符，和int类型数字，之间对照关系表：ASCII、Unicode
		- 对于字符串String（首字母大写，并不是关键字）来说，加好代表字符串链接操作。
		- 任何数据类型和字符串链接的时候，结果都会变成字符串；
	- 自增和自减 `-- ++`
		- 只有变量才能使用自增和自减运算符。常量不可发生改变，所以不能使用。 `30++ 错误写法`
	- 赋值运算符
		- `=  += -= *= /= %=`
	- 比较运算符
		- `== < > <= >= !=`
		- 注意事项：
			- 比较运算符的结果一定是一个Boolean值，成立就是true，不成立就是false
			- 如果进行多次判断，不能连着写 `3 > 4 < 1 错误写法`
	- 逻辑运算符
		- `&& || !`
		- 注意事项：
			- 逻辑运算符只能用于boolean值
			- 与、或需要左右各自有一个Boolean值，但是取反只要有唯一的Boolean值即可
			- 与、或，两种运算符如果有多个条件可以连续写；
	- 三元运算符
		 - 格式： 数据类型 变量名称 = 条件判断 ？ 表达式A ： 表达式B；
		 ```java
			int a = 20;
			int b = 30;
			int max = a > b ? a : b;
		 ```
		 - 注意事项：
			- 必须同时保证表达式A和表达式B都符合左侧数据类型的要求；
				- 错误例子 `int result = 3 > 4 ? 2.5 : 10;`
			- 三元运算符结果必须被使用； `a > b ? a ； b; 错误写法`
- Java方法
	- 定义方法：
		- 格式：`public static void 方法名称(){方法体}`
		- 方法名称的命名规则和变量一样使用小驼峰式（myNameTime）；
		- 方法体：也就是大括号当中包含的任意语句
		- 注意事项：
			- 方法定义的先后顺序无所谓
			- 方法的定义不能有嵌套包含的关系
			- 方法定义好了之后是不会执行的，如果要执行，一定要进行方法的调用
		- 调用方法
			- 方法名称();
		```java
            public calss DemoMethod {
                /*入口方法*/
                public static void main(String[] args){
                    myMethod(); /*方法调用*/
                }
                /*自定义方法*/
                public static void myMethod(){
                    System.out.println('自定义方法')
                }   
            }
		```
	- 定义方法的完整格式：
	    - 格式：修饰符 返回值类型 方法名称(参数类型 参数名称， ...){方法体; return 返回值;};
            - 修饰符：现阶段 `public static`
            - 返回值类型：也就是方法最终返回的结果是什么数据类
            - 方法名称：方法的名字，小驼峰
            - 参数类型：进入方法的数据是什么类型；
            - 参数名称：进入方法的数据对应的名称；    
            - 方法体：方法要做的事
            - return：停止方法，或者将结果还给调用处
	    - 注意事项：
	        - return后面的返回值必须和方法名称前面的返回值类型保持对应；
	- 方法的三种调用格式
	    - 单独调用：方法名称(参数);
	    - 打印调用：System.out.println(方法名称(参数));
	    - 赋值调用：数据类型 变量名称 = 方法名称(参数);
	    
	```java
        public class dome {
            public static void main(String[] args) {
                sum(10,20);/*单独调用*/
                System.out.println(sum(10,20));/*单独调用*/
                int c = sum(10,20);/*赋值调用*/
            }
            /*定义方法*/
            public static int sum(int a, int b){
                int result =    a + b;
                return  result;
            }
        }
     ```
    - 此前学习的方法，返回值类型固定的写为void，这种方法只能够单独调用，不能进行赋值和打印调用；
    - 方法使用的注意事项：
        - 方法应该定义在类中，方法中不能再定义方法
        - 方法有返回值，必须有return
    - 方法重载(Overload)
        - 多个方法的名称一样，但是参数列表不一样
        ```java
          public class dome {
              public static void main(String[] args) {
                  System.out.println(sum(10,20));
                  System.out.println(sum(10,20,30));
                  /*谁的参数对的上就用谁*/
              }
          
              public static int sum(int a, int b){
                  return a + b;
              }
              public static int sum(int a, int b, int c){
                  return a + b +c;
              }
          }
        ```
        - 方法重载和下列因素相关
            - 参数个数不同
            - 参数类型不同
            - 参数多类型顺序不同
        - 方法重载与下列因素无关
            - 参数名称无关
            - 与方法的返回值类型无关
- Java流程
	- 顺序结构
	- 判断语句（选择结构）
		- if语句（单if语句）
		- if...else...
		- if...elseif..else..
	- 选择结构
		- switch语句
		```java
            switch(a){
                case "1" :
                    break;
                default:
                    break;
            }
		```
		- switch语句使用的注意事项：
			- 多个case后面的数值不可以重复
			- switch后面小括号中只能是：
				- 基本数据类型：byte/short/char/int
				- 引用数据类型：String字符串、enum枚举
				- switch语句可以很灵活：前后顺序可以颠倒，而且break语句可以省略（break省略会出现穿透现象）
	- 循环结构（循环语句）
		- for循环
		```java
            for(int i = 1; i <= 100; i++){
                System.out.println("for循环");
            }
		```
		- while循环
			- 保证格式 `while(条件判断){循环体}`
			- 扩展格式 `while(条件判断){循环体;步进语句;}`
			```java
                int i = 0;
                while(i <= 10){
                    System.out.println("while循环");
                    i++;
                }
                /*
                   先判断后循环
                */
			```
		- do...while循环
            - 标准格式
            `do{循环体}while(条件判断)；先循环后判断`
            - 扩展格式
            `初始化表达式;do{循环体,步进表达式}while(布尔表达式)；先循环后判断`
            ```java
                int i = 1;
                do{
                    System.out.println("do...while...循环语句");
                    i++;
                }while(i <= 10);
            ```
        - 三种循环的区别
            - 如果条件出来没有满足过，那么for训话就会执行0次，do-while循环至少执行1次；
            - for循环变量在小括号中定义，只有在循环内部才可以使用；（变量的作用域）
    - break关键字用法有常见的两种：
        - 可以用在switch语句中，一旦执行，整个switch语句立即结束；
        - 还可以用在循环语句中，一旦执行，循环循环立即结束，打断循环；
    - 关于循环的选择建议：凡是次数限定的用for循环，否则多用while循环；
    - 另一种循环控制语句continue关键字：一旦执行立即跳出当前次循环剩余内容，马上开始下一次循环；

- Java数组
    - 概念：数组是一个容器，可以存放多个数据值
    - 特点：
        - 数组是一个引用类型
        - 数组当中的多个数据类型必须统一
        - 数组的长度在程序运行过程中不能改变
    - 数组的初始化：创建
        - 两种创建的初始化：
            - 动态初始化（指定长度）
                - 格式：数据类型[] 数组名称 = new 数据类型[数组长度]; `int[] arrayA = new int[300]`
            - 静态初始化（指定内容）
                - 基本格式(标准格式)：数据类型[] 数组名称 = new 数据类型[]{元素1,元素2, ...}; `int[] arrayB = new int[]{15,25,35}`
                - 省略格式：数据类型[] 数组名称 = {元素1,元素2, ...}; `int[] arrayB = {15,25,35}`
        - 注意事项：静态标准初始化和动态初始化，可以拆封成2个步骤,静态省略初始化是不可以拆成2个步骤的；
        - 使用建议：如果不确定数组中的具体内容，用动态初始化，否则，已经确定了具体的内容，用静态初始化。
        ```java
          int[] arrayA;
          arrayA =  new int[5];
    
          int[] arrayB;
          arrayB = new int[]{1,2,3};
        ```
    - 数组的获取：数组名称[索引值]
    - 如果使用动态初始化数组，其中的元素将会有一个初始值，规则如下：
        - 整数类型 -- 默认 0
        - 浮点类型 -- 默认 0.0
        - 字符类型 -- 默认 '\u0000'
        - 布尔类型 -- false
        - 引用类型 -- null
    - 数组使用常用错误
        - 数组所以越界异常：访问数组元素的时候，索引编号并不存在，就会报次错误 ArrayIndexOutOfBoundsException
            - 原因：数组索引写错了
        - 数组必须进行new初始化才能使用其中的元素，如果赋值了一个null没有new初始化创建，就会发生空指针异常 NullPointException
            - 所有的引用类型都可以赋值一个null，表示没有
            - 原因：忘了new 解决：补上new
    - 获取数组的length `arrayB.length` 数组一旦创建，程序运行期间，长度不能改变
    - 遍历数组：就是对数组中的每一个元素进行逐一挨个处理 `array.fori` 自动生成遍历数组的代码
    - 数组元素翻转
    ```java
      public class dome {
          public static void main(String[] args) {
              int array[] = {10, 20, 30, 40};
              /*遍历======*/
              for (int i = 0; i < array.length; i++) {
                  System.out.println(array[i]);
              }
              /*数组元素反转*/
              for (int min = 0, max = array.length - 1; min < max; min++, max--) {
                  int temp = array[min];
                  array[min] = array[max];
                  array[max] = temp;
              }
          }
      }
    ```

- Java权限修饰符
    - `public > protected > (default) > private`
    ```
                        public > protected > (default) > private
        
        同一个类            YES       YES         YES        YES   (我自己)
        同一个包            YES       YES         YES        NO    (我邻居)
        不同包子类           YES       YES         NO         NO   (我儿子)  
        不同包非子类         YES       NO         NO         NO    (陌生人)
    ```

- Java-内部类
    - 一个类的内部包含一个类
    - 分类
        - 成员内部类
        - 局部内部类(包含匿名内部类)
    - 成员内部类
        - 格式：修饰符 class 外部类名称{ 修饰符 class 内部类名称{ // ... } }
        - 注意：内用外随意访问，外用内，需要有内部对象
        - 使用：
            - 间接使用：在外部类方法中，使用内部类，然后main只是调用外部类的方法
            - 直接使用：
                - 公式：类名称 对象名 = new 外部类名称();
                - 格式：外部类名称.内部类名称 对象名 = new 外部类名称().new 内部类名称();
    - 内部类同名变量访问   
    ```java
      public class Outer {
          int num = 10;
          public class Inter {
              int num = 20;
              public void method(){
                  int num = 30;
                  System.out.println(num); /*方法变量*/
                  System.out.println(this.num); /*内部类变量*/
                  System.out.println(Outer.this.num); /*外部类变量*/
              }
          }
      }
    
      /*访问*/
      public class Demo {
          public static void main(String[] args){
              new Outer().new Inter().method();
          }
      }
    ```
    - 局部内部类：一个类定义在一个方法的内部
    ```java
      /*定义*/
      public class Outer {
          public void method(){
              class Inter {
                  int num = 10;
                  public void inMethod(){
                      System.out.println(num);  
                  }
              }
              Inter inter = new Inter();
              inter.inMethod();
          }
      }
    ```
    ```java
      /*使用*/
    public class Demo {
      public static void main(String[] args){
          Outer outer = new Outer();
          outer.method();
      }        
    }
    ```
    - 局部内部类final问题
        - 局部内部类，如果希望访问坐在方法的局部变量，那么这个局部变量就必须是【有效final的】
        - 从 Java 8+ 开始，只要局部变量事实不变，那么final关键字可以省略
    - 匿名内部类
        - 如果接口的实现类(或者是父类的子类)只需要使用唯一的一次，那么这种情况下，就可以省略掉该类的定义，而改为使用【匿名内部类】
        - 定义格式
            - 接口名称 对象名 = new 接口名称(){ //覆盖重写所有抽象方法  }; 末尾;不可以丢失
            ```java
              MyInterface myInterface = new MyInterface() {
                  @Override
                  public void method() {
                      System.out.println();
                  }
              };
            ```
        - 对格式 `new 接口名称(){...}`进行解析
            - new 代表对象创建动作
            - 接口名称就是匿名内部类需要实现哪个接口
            - {...}这才是匿名内部类的内容
        - 注意事项
            - 匿名内部类在创建对象的时候，只能使用唯一一次