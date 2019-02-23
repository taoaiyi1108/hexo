---
title: Java学习进阶-异常与多线程
date: 2019-02-14 21:49:28
tags: java
categories: 编程
---
Java进阶学习-异常与多线程

<!-- more -->

- 异常
    - 概念：程序在执行过程中出现非正常的情况，最终导致JVM非正常停止；在Java等面向对象的语言中，异常本身是一个类，产生异常就是创建一个异常对象并抛出一个异常对象，Java处理异常的方式是中断处理；
        - 异常不是语法错误，语法错误，编辑都通过不了，程序根本就不会运行；
    - 异常体系(Throwable)
        - Error：严重错误Error，无法通过处理的错误，只能实现避免
        - Exception：表示异常，异常出现后程序员可以通过代码的方式纠正，使程序继续运行，是必须要处理的
    - 异常分类：
        - 编译时期异常：checked异常，在编译时期就会检查，如果没有处理异常，则编译失败(如日期格式化异常)
        - 运行期异常：runtime异常，在运行时期，检查异常，运行异常不会被编译器检测(不报错);(如数学异常)
    - 异常处理 `tyr catch finally throw throws`
        - 抛出异常throw
             - 作用：在指定的方法中抛出指定的异常
             - 使用格式： `throw new xxxException("异常原因")`
             - 注意：
                - throw必须写在方法的内部
                - throw后面new的对象必须是Exception或者Exception的子类对象
                - throw抛出指定的异常对象，我们就必须处理这个异常对象
                    - throw后面创建的是RuntimeException或者RuntimeException的子类对象，我们可以不处理，默认交给JVM处理(打印异常对象，中断程序)
                    - throw后面创建的是编译异常，我们就必须处理这个异常，要么throws，要么try...catch
                    ```java
                      public class Demo{
                          public static int getElement(int[] arr, int index){
                              if(arr == null){
                                  throw new NullPointerException("传递的数组是null");/*空指针异常 NullPointerException 是给运行期异常*/
                              } 
                              if(index < 0 || index > arr.length - 1){
                                  throw new ArrayIndexOutOfBoundsException("索引超出数组使用范围");
                              }
                          }
                      }
                    ```
    - Objects非空判断
        - Objects.requireNonNull(obj) 查看指定对象是不是null
        ```java
          public class Demo {
              public static void methos(Object obj){
                  Objects.requireNonNull(obj);
                  Objects.requireNonNull(obj,"传递的对象时null");
              }
          }
        ```
    - 声明异常 throws 异常处理方式一
        - 作用：处理异常对象，会把异常对象抛出给方法的调用者处理(自己不处理，交给别人处理),最终交给JVM处理-->中断处理
        - 使用格式：在方法声明时使用
        - 注意：
            - throws必须写在方法声明处
            - throws后面new的对象必须是Exception或者Exception的子类对象
            - 方法内部抛出了多个异常，throws就必须声明多个异常，如果抛出的异常有子父类关系，那么直接声明父类异常即可
            - 调用一个声明抛出异常的方法，就必须处理声明的异常，要么继续throws声明抛出，交给方法的调用者处理，最终交给JVM，要么try...catch自己处理异常
            ```java
              public class Demo {
                  /*public static void main(String[] args) throws FileNotFoundException,IOException{*/
                  /*public static void main(String[] args) throws IOException{*/
                  public static void main(String[] args) throws Exception{
                      readFile("c:\\a.txt");
                  }
                  /*FileNotFoundException 继承了 IOException 继承了 Exception 只需要声明父类异常*/
                  /*public static void readFile(String fileName) throws FileNotFoundException,IOException{*/
                  public static void readFile(String fileName) throws IOException{
                      if(!fileName.equals("c:\\\\a.txt")){
                          throw new FileNotFoundException("文件路径不是c:\\a.text");
                      }
                      if(!fileName.endsWith(".txt")){
                          throw new IOException("文件后缀名不对");
                      }
                  }
              }
            ```
    - 捕获异常 try..catch 异常处理方式二
        - 格式：
        ```
            try{
                可能查询异常的代码
            }catch(定义一个异常的变量，用来接收try中抛出的异常对象){
                异常处理逻辑，
                一般会记录到异常日志中
            }
            ...
            catch(异常类名 变量名){}
        ```
        - 注意：
            - try中可能抛出多个异常，那么就可以使用多个catch来处理这些异常对象
            - 如果try中产生了异常，那么就会执行catch中异常逻辑，执行完毕catch中的逻辑，继续执行try...catch之后的代码
            - 如果没有try中没有产生异常，那么就不会执行catch中的异常处理逻辑，只会执行try中的代码，再继续执行try...catch之后的代码
            ```java
               public class Demo {
                    public static void main(String[] args){
                      try{
                          readFile("c:\\a.txt");
                      }catch (IOException e){
                          System.out.println(e);
                          e.getMessage();
                      }
                    }
                    public static void readFile(String fileName) throws IOException{
                        if(!fileName.endsWith(".txt")){
                            throw new IOException("文件后缀名不对");
                        }
                    }
                }
            ```
    - Throwable类中定义了3个异常处理方法：
        - String getMessage() 返回throwable的简短信息
        - String toString() 返回throwable的详细消息信息
        - void printStackTrace() JVM打印异常对象默认调用此方法，打印的异常信息是最全面的
    - finally 代码块
        - 有一些特定的代码无论异常是否发生，都需要执行，另外，因为异常会引发程序跳转，导致有些语句执行不到，而finally就只解决这个问题的，在finally中存放的代码块都是一定执行的 
        - 格式 try...catch(){}finally{}
        - 注意事项
            - finally不能单独使用必须和try一起使用
            - finally一般用于资源释放(资源回收),无论程序是否出现异常，我们都要资源释放(IO)
    - 异常处理注意事项
        - 一个try多个catch：catch里面的异常变量，如果有子父类关系，那么子类的异常就必须写在上面，否则会报错；
        - 如果finally中有return语句，永远返回finally中的结果，避免该情况
        ```java
          public class Demo{
              public static void main(String[] args){
              
              }
              
              public static getArr(){
                  int a = 10;
                  try{
                      return a;
                  }catch (Exception e){
                      System.out.println(e);
                  }finally{
                      /*程序执行完 a = 100 计量不要在finally中写return语句*/
                      a = 100;
                      return a;
                  }
              }
          }
        ```
        - 子父类异常
            - 如果父类抛出多个异常，子类重写父类方法时，抛出和父类相同的异常或者父类异常的子类或者不抛出异常。
            - 父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常。此时子类产生该异常，只能捕获处理，不能声明抛出
            - 注意：父类异常什么样子类异常就是什么样
            ```java
              public class Fu {
                  public void show01() throws NullPointerException,ClassCastException{};
                  public void show02() throws IndexOutOfBoundsException{};
                  public void show03() throws IndexOutOfBoundsException{};
                  public void show04() {};
              }
              class Zi extends Fu {
                  /*子类重写父类方法时，抛出和父类相同的异常*/
                  public void show01() throws NullPointerException,ClassCastException{}; 
                  /*子类重写父类方法时，抛出父类异常的子类*/
                  public void show02() throws ArrayIndexOutOfBoundsException{};
                  /*子类重写父类方法时，不抛出异常*/
                  public void show03(){};
                  /*父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常*/
                  public void show04(){};
                  /*此时子类产生该异常，只能捕获处理，不能声明抛出*/
                  public void show04(){
                      try{
                          throw  new Exception("编译器异常");
                      }catch (Exception e){
                          e.printStackTrace();
                      }
                  };
              }
            ```
    - 自定义异常类
        - 格式：public class XXXException extents Exception | RuntimeException { 添加一个空参的构造方法  添加一个带异常信息的构造方法 }
        - 注意：
            - 自定义异常类一般都是以Exception结尾，说明该类是一个异常类
            - 自定义异常类必须继承Exception或者是RuntimeException
                - 继承Exception：那么自定义的异常类就是一个编译期异常，如果方法内部抛出了编译器异常，就必须处理这个异常，要么throws，要么try...catch
                - 继承RuntimeException：那么自定义的异常就是一个运行期异常，无需处理，交给虚拟机处理(中断处理)
    ```java
      public class RegisterException extends Exception {
          public RegisterException(){
            
          }
          
          public RegisterException(String message){
              super(message);  
          }
      }
      
    ```
    
- 多线程
    - 并发：指两个或者多个事件在同一时间内法师
    - 并行：指两个或者多个事件在同一时刻发生(同时发生)
    - 进程：指一个内存中运行的程序
    - 线程：是进程中的一个执行单元
    - 主线程：执行主方法(main)的线程
    - 创建多线程程序的方式一：创建Thread类的子类
        - 实现步骤
      ```java
          /*
        1.创建一个Thread类的子类
        2.在Thread类的子类中重写Thread类的run方法，设置线程任务
        3.创建Thread类的子类对象
        4.调用Thread类的start方法，开启新的线程，执行run方法
        Java程序属于抢占式调度，那个线程优先级高那个线程优先执行，同一个优先级，随机选择一个线程执行
          */
          public class MyThread extends Thread {
              @Override
              public void run(){
                  for(int i = 0; i < 20; i++){
                      System.out.println(i);
                  }
              }
          }
          
          public class ThreadDemo {
              public static void main(String[] args){
                  MyThread myThread = new MyThread();
                  myThread.start();
              }
          }
      ```
    - Thread类
        - 获取线程的名称：1、使用Thread类中的getName()方法，2、先获取当前执行的线程，使用线程中的方法getName()获取线程名称
            - currentThread() 返回当前正在执行的线程的对象的引用
        - 设置线程的名称：1、使用Thread类中的setName(名字)方法，2、利用带参构造方法传递线程名称，调用父类的方法设置线程名称
        - 成员方法：
            - sleep ： `public void sleep(long millis)` 使当前正在执行的线程以指定的毫秒数暂停
    -创建多线程程序的方式二：声明实现 Runnable 接口的类
    ```java
      /* 
          1.创建一个Runnable接口的实现类
          2.在实现类中重写Runnable中的run方法，设置线程任务
          3.创建一个Runnable接口的实现类对象
          4.创建Thread类对象，构造方法中传递Runnable接口的实现类对象
          5.调用Thread类中的start方法，开启新的线程，执行run方法  
      */
      public class RunnableImpl implements Runnable {
          @Override
          public void run(){
              System.out.println(Thread.currentThread().getName());  
          }
      }
    
      public class RunnableDemo {
          public static void main(String[] args){
              RunnableImpl runnableImpl = new RunnableImpl();
              Thread t = new Thread(runnableImpl);
              t.start();
          }  
      }
    ```
    - Thread和Runnable区别
        - 如果一个类继承Thread，则不适合资源共享，但如果实现了Runnable接口，则很容易实现资源共享
        - 实现Runnable接口创建多线程程序的优势
            - 避免了单继承的局限性(接口可以多继承)
            - 增强程序的扩展性，降低了线程的耦合性
    - 匿名内部类方式实现线程的创建
    ```java
      public class InnerClassThread {
          public static void main(String[] args){
              /*线程的父类是Thread*/
              new Thread(){
                  @Override
                  public void run(){
                      System.out.println(Thread.currentThread().getName());  
                  }  
              }.start();  
              /*线程的节后Runnable*/
              Runnable r = new Runnable(){
                  @Override
                  public void run(){
                      System.out.println(Thread.currentThread().getName());  
                  }  
              };
              new Thread(r).start();
              /*简化版*/
              new Thread(new Runnable(){
                   @Override
                   public void run(){
                       System.out.println(Thread.currentThread().getName());  
                   }  
              }).start();
          }
      }
    ```
    - 线程安全问题：多线程访问共享数据
        - 避免；多线程访问共享数据的时候，只允许同一时间只有一个线程访问共享数据源，其余线程等待
        - 解决线程安全问题方法：
            - 使用同步代码块：
                - 格式： `synchronize(锁对象){ 可能会出现线程安全问题的代码 }`
                - 注意：1、同步代码块中的锁对象可以使任意对象，2、必须保证多个线程使用的锁对象是同一个，3、锁对象作用：可以把同步代码块锁住只让一个线程在同步代码块中执行
                ```java
                  public class RunnableImpl implements Runnable {
                      private int ticket = 100; /*共享数据源*/
                      Object obj = new Object(); /*创建一个锁对象*/
                      @Override
                      public void run(){
                          while (true){
                              /*同步代码块*/
                              synchronized (obj){
                                  if(ticket > 0){
                                      try{
                                          Thread.sleep(10);
                                      }catch (InterruptedException e){
                                          e.printStackTrace();
                                      }
                                      System.out.println(ticket);
                                      ticket--;
                                  }
                              }
                          }
                      }      
                  }
                ```
            - 同步方法：
                - 格式： `public synchronize void method(){ 可能产生线程安全的代码 }`
                ```java
                   public class RunnableImpl implements Runnable {
                        private int ticket = 100; /*共享数据源*/
                        private static int ticketStatic = 100; /*共享数据源*/
                        @Override
                        public void run(){
                            while (true){
                                method();
                            }
                        }
                        /*同步方法*/
                        public synchronized void method(){
                            if(ticket > 0){
                                try{
                                    Thread.sleep(10);
                                }catch (InterruptedException e){
                                    e.printStackTrace();
                                }
                                System.out.println(ticket);
                                ticket--;
                            }
                        } 
          
                        /*静态同步方法*/
                        public static synchronized void methodStatic(){
                            if(ticketStatic > 0){
                                try{
                                    Thread.sleep(10);
                                }catch (InterruptedException e){
                                    e.printStackTrace();
                                }
                                System.out.println(ticketStatic);
                                ticket--;
                            }
                        } 
                   }
                ```
            - Lock锁
                - 方法：lock()获取锁；unlock() 释放锁。
                - 使用步骤：1、在成员位置创建一个ReentrantLock对象；2、有可能会出现安全问题的代码前调用Lock接口中的方法lock获取锁；3、有可能会出现安全问题的代码后调用Lock接口中的方法unlock释放锁；
                ```java
                  public class RunnableImpl implements Runnable {
                      private int ticket = 100; /*共享数据源*/
                      Lock l = new ReentrantLock();
                      @Override
                      public void run(){
                          while (true){
                              l.lock();
                              if(ticket > 0){
                                  try{
                                      Thread.sleep(10);
                                      System.out.println(ticket);
                                      ticket--;
                                  }catch (InterruptedException e){
                                      e.printStackTrace();
                                  }finally{
                                      l.unlock();
                                      /*无论代码释放异常lock都会被释放掉*/
                                  }
                              }
                          }
                      }      
                  }
                ```
    - 线程的等待与唤醒
        - Object.wait(); 使当前线程等待，唤醒线程必须用Object.notify()方法；
        - Object.wait(lang m) 在毫秒值内没有被notify方法唤醒，就会自己醒来
        - notify() 如果有多个线程，随机唤醒一个
        - notifyAll()唤醒所有等待的线程
        - 线程间通信：多个线程处理一个资源，但是处理动作(线程任务)却不相同
        - 注意
            - wait和notify必须由同一个锁对象调用，
            - wait和notify必须要在同步代码块或者同步函数中使用
    - 线程池：容器 -->集合(ArrayList,HashSet,LinkedList<Thread>,HashMap)
        - JDK 1.5 之后提供 
        - `java.util.concurrent.Executors`：线程池的工厂类，用来生产线程池
        - Executors：静态方法
            - `public static ExecutorService newFixedThreadPool(int nThreads)`  创建一个可重用固定线程数的线程池
                - 参数 int nThreads ：创建线程池中包含的线程数量
                - 返回值：ExecutorService接口返回的是ExecutorService接口的实现类，可以使用ExecutorService接口接收(面向接口编程)
        - `java.util.concurrent.ExecutorService`:线程池接口，
            - 用来从线程池中获取线程，调用start方法，执行线程任务
                - submit(Runnable task) 提交一个Runnable 任务用于执行
            - 关闭/销毁线程池的方法 `void shutdown()`
        -  实验步骤：
            - 使用线程池的工程类Executors里面提供的newFixedThreadPool生产一个指定线程数量的线程池
            - 创建一个类，实现Runnable接口，重写run方法，设置线程任务；
            - 调用ExecutorService中的方法submit,传递线程任务，开启线程，执行run方法
            - 调用ExecutorService中的方法shutdown销毁线程池(不建议执行)
            ```java
               public class RunnableImpl implements Runnable {
                   @Override
                   public void run() {
                       System.out.println(Thread.currentThread().getName()+ "创建一个新的线程");
                   }
               }
               public class ThreadPool {
                   public static void main(String[] args) {
                       ExecutorService es = Executors.newFixedThreadPool(2);
                       es.submit(new RunnableImpl());
                       es.submit(new RunnableImpl());
                       es.submit(new RunnableImpl());
               
                       es.shutdown(); /*销毁线程池*/
               
                       es.submit(new RunnableImpl());/*报异常 线程池已经被销毁了*/
                   }
               }
            ```
- Lambda表达式





































































   