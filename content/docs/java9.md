---
title: javase 多线程
categories:
  - javase
abbrlink: 2701
date: 2024.8.23
tags: 
   - java
---

# 简介

多线程是同时有多条执行流程

thread是线程的代表

# 构造方法

一：

- 继承Thread
- 重写run()方法

```
public class BigStar {
    public static void main(String[] args) {
        Thread t = new Test();
        t.start();
        for(int i = 0;i<5;i++){
            System.out.println("big"+i);
        }
    }
}
```

```
public class Test extends Thread{
    @Override
    public void run(){
        for(int i = 0;i<5;i++){
            System.out.println("test"+i);
        }
    }
}
```

这样就是两个线程来跑，但是谁先谁后是不确定的

这种方式编码比较简单，不利于功能的扩展

不能再继承其他的类了

- **必须使用start方法 注册单独的执行流程**
- **不要把主线程的任务放到子线程之前**

二：

- 声明实现Runnable接口的类

- 然后重写run方法
- 创建任务类对象
- 然后创建线程对象

```
public class BigStar {
    public static void main(String[] args) {
        Runnable target = new Test();
        new Thread(target).start();
        for(int i =0;i<=5;i++){
            System.out.println("bbb"+i);
        }
        }
    }

```

```
public class Test implements Runnable{

    @Override
    public void run() {
        for(int i =0;i<=5;i++){
            System.out.println("aaa"+i);
        }
    }
}
```

这是第二个实现方法

优缺点：

- 可是继承其他的类，实现其他的接口，扩展性强

或者是使用简化形式

```
public class BigStar {
    public static void main(String[] args) {
        // 创建并启动新线程
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i <= 5; i++) {
                    System.out.println("aaa" + i);
                }
            }
        }).start();

        // 主线程继续执行
        for (int i = 0; i <= 5; i++) {
            System.out.println("bbb" + i);
        }
    }
}

```

这样就可以在一个java文件中写出多个线程了

三：

实现Callable接口来解决

**run（）方法不能返回数值类型，而这个方法就能返回结果**

FutureTask：

- 是一个任务对象，实现了Runnable接口

- 可以在线程执行完毕后，可以调用get方法，调取结果

  ```
  import java.util.concurrent.Callable;
  import java.util.concurrent.ExecutionException;
  import java.util.concurrent.Future;
  import java.util.concurrent.FutureTask;
  
  public class BigStar {
      public static void main(String[] args) throws ExecutionException, InterruptedException {
          Callable<String> callable = new Test(100);
          FutureTask<String> futureTask = new FutureTask<>(callable);
          new Thread(futureTask).start();
          String rs = futureTask.get();
          System.out.println(rs);
  
      }
  }
  
  ```

  

```
import java.util.concurrent.Callable;

public class Test implements Callable<String> {
    private int n;

    public Test(int n) {
        this.n = n;
    }

    @Override
    public String call() throws Exception {
        int sum=0;
        for (int i= 0;i<=n;i++){
            sum+=i;
        }
        return "qiu"+sum;

    }
}
```

线程没执行完毕的时候，下面的代会等线程结束之后return的结果

实现多个线程一块跑

```
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Future;
import java.util.concurrent.FutureTask;

public class BigStar {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        Callable<String> callable = new Test(100);
        FutureTask<String> futureTask = new FutureTask<>(callable);
        new Thread(futureTask).start();
        Callable<String> callable2 = new Test(200);
        FutureTask<String> futureTask2 = new FutureTask<>(callable2);
        new Thread(futureTask2).start();
        String rs = futureTask2.get();
        System.out.println(rs);
        String rs2 = futureTask.get();
        System.out.println(rs2);

    }
}

```

求取两个线程一块跑

这个方式的编码比较复杂，但是能够返回一结果

# 常用方法

getName

```

public class BigStar {
    public static void main(String[] args) {
        Thread t1 = new Test();
        System.out.println(t1.getName());
        Thread t2 = Thread.currentThread();
        System.out.println(t2.getName());
        for(int i =0;i<=5;i++){
            System.out.println("bbb"+i);
        }
    }
}

```

获取线程的名字

可以取获取主线程的name，一般就是main

使用setName设置线程的名字，要在线程启动之前

可以为线程添加一个String类型的数据，可以使他添加

```
public class Test extends Thread implements Runnable{
public Test(String name){
    super(name);
}
    @Override
    public void run() {
        for(int i =0;i<=5;i++){
            System.out.println("aaa"+i);
        }
    }
}
```

sleep代码用来休眠

线程按照顺序进行执行

```

public class BigStar {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Test("a");
        t1.start();
        t1.join();
        System.out.println(t1.getName());
        Thread t2 = Thread.currentThread();
        t2.start();
        t2.join();
        System.out.println(t2.getName());
        for(int i =0;i<=5;i++){
            System.out.println("bbb"+i);
        }
    }
}

```

# 线程安全问题

**多个线程在同时操作一个共享资源时，可能出现安全问题**

叫做线程安全问题

- 存在多个线程在同时执行

- 同时访问同一个共享资源

- 都要修改数据

为了解决线程安全问题，我们要使用线程同步方法

## 同步代码块

把访问共享资源的核心代码上锁

每次只允许一个线程加锁后进入

**同步锁必须时同一个对象才可以**

```
public class Test extends Thread implements Runnable{
public Test(String name){
    super(name);
}
    @Override
    public void run() {
        synchronized ("qq") {
            for(int i =0;i<=5;i++){
                System.out.println("aaa"+i);
            }
        }
    }
}
```

ctrl+alt+t

选择第9个就可以放进代码块

锁的范围太大

```
public class Test extends Thread implements Runnable{
public Test(String name){
    super(name);
}
    @Override
    public void run() {
        synchronized (this) {
            for(int i =0;i<=5;i++){
                System.out.println("aaa"+i);
            }
        }
    }
}
```

使用this作为锁，只锁住当前对象

**静态方法使用class文件作为锁**

```
public class Test extends Thread implements Runnable{
public Test(String name){
    super(name);
}
    @Override
    public void run() {
        synchronized (Test.class) {
            for(int i =0;i<=5;i++){
                System.out.println("aaa"+i);
            }
        }
    }
}
```

## 同步方法

在核心方法的前面加上**synchronized**进行修饰

之后这个方法被称为同步方法

其实也是的使用**this**进行锁的，是一个隐式锁对象

**同步代码块锁的范围更小，性能更强一点**

## lock锁

可以直接创建出一个锁对象

更加灵活方便更强大

lock是一个接口，使用**ReentrantLock**来创建锁对象

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Test extends Thread implements Runnable{
    private final Lock lk = new ReentrantLock();
public Test(String name){
    super(name);
}
    @Override
    public void run() {
        lk.lock();
        for(int i =0;i<=5;i++){
                System.out.println("aaa"+i);
        }
        lk.unlock();
    }
}
```

加上final对锁对象进行保护

一旦中间出现了异常，锁却没有解开

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Test extends Thread implements Runnable{
    private final Lock lk = new ReentrantLock();
public Test(String name){
    super(name);
}
    @Override
    public void run() {
        lk.lock();
        try {
            for(int i =0;i<=5;i++){
                    System.out.println("aaa"+i);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            lk.unlock();
        }
    }
}
```

把解锁操作放在finally里面就可以进行更好的实现，保证不出现bug

# 线程通信

线程间通过某种方式互相告知自己的状态

模型：

生产者线程负责生产数据

消费者负责消费生产者生产的数据

ps:生产--消费--生成....

```
public class BigStar {
    public static void main(String[] args) throws InterruptedException {
        // 使用Lambda表达式创建并启动新线程
        new Thread(() -> {
            for (int i = 0; i <= 5; i++) {
                System.out.println("aaa" + i);
            }
        }).start();
        new Thread(()->{
            
        }).start();

        // 主线程继续执行
        for (int i = 0; i <= 5; i++) {
            System.out.println("bbb" + i);
        }
    }
}

```

用Lambda表达式使用更简单的线程创建

```
import java.util.ArrayList;
import java.util.List;

public class testannotation {
private List<String> list =new ArrayList<>();
public synchronized void put() throws InterruptedException {
    String name = Thread.currentThread().getName();
    if(list.size()==0){
        list.add(name+"qqq");
        Thread.sleep(200);
        this.notify();//先唤醒在等待
        this.wait();//要让锁对象进行这个操作才行，在这个里面锁对象是this
    }
    else {
        this.notify();//先唤醒在等待
        this.wait();
        
    }
}
}
```

锁哪个对象，就要用哪个wait等操作

## 线程池

就是一个可以**复用线程**的技术

原理：

分为工作线程和任务队列两部分

任务是一个实现Runnable或者Callable的对象

会控制任务的数量和线程的数量，不会占用过多的资源

```
ExecutorService
```

是一个线程池的接口

```
ExecutorService
```

这是实现类

```
import java.util.concurrent.*;

public class BigStar {
    public static void main(String[] args) {
        // 创建线程池，核心线程数为2，最大线程数为4，保持空闲线程时间为60秒，任务队列容量为10
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2, // 核心线程数
                4, // 最大线程数
                60, // 线程空闲保持时间
                TimeUnit.SECONDS, // 时间单位
                new ArrayBlockingQueue<>(10) // 阻塞队列容量为10
        );

        // 提交任务到线程池
        executor.execute(() -> {
            for (int i = 0; i <= 5; i++) {
                System.out.println("aaa" + i);
            }
        });

        executor.execute(() -> {
            for (int i = 0; i <= 5; i++) {
                System.out.println("bbb" + i);
            }
        });

        executor.execute(() -> {
            for (int i = 0; i <= 5; i++) {
                System.out.println("ccc" + i);
            }
        });

        // 关闭线程池，防止新任务提交，但会执行完已提交的任务
        executor.shutdown();

        // 等待线程池中任务完成
        try {
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }
}

```

例子

参数说明：

- `corePoolSize`: 核心线程数，线程池中始终保留的线程数。

  `maximumPoolSize`: 最大线程数，线程池中允许的最大线程数。

  `keepAliveTime`: 当线程数超过核心线程数时，多余空闲线程的存活时间。

  `unit`: `keepAliveTime` 的时间单位。

  `workQueue`: 存储等待执行任务的阻塞队列。

  `threadFactory`: 线程工厂，提供创建新线程的方式。

  `handler`: 拒绝策略，线程池无法处理任务时的处理策略。

```
ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2, // 核心线程数
                4, // 最大线程数
                60, // 线程空闲保持时间
                TimeUnit.SECONDS, // 时间单位
                new ArrayBlockingQueue<>(10), // 阻塞队列
                Executors.defaultThreadFactory(), // 线程工厂，使用默认的线程工厂
                new ThreadPoolExecutor.AbortPolicy() // 拒绝策略，使用默认的终止策略
        );
```

注意事项：

- **临时线程什么时候创建**？新任务提交的时候核心线程都在忙，任务队列满了，并还可以创建新的临时线程，才会创建临时线程
- **什么时候会开始拒绝新任务**？核心线程和临时线程都在忙，任务队列也满了，新的任务才会开始拒绝新任务

### 任务

```
import java.util.concurrent.*;

public class BigStar {
    public static void main(String[] args) {
        // 创建线程池，核心线程数为2，最大线程数为4，保持空闲线程时间为60秒，任务队列容量为10
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2, // 核心线程数
                4, // 最大线程数
                60, // 线程空闲保持时间
                TimeUnit.SECONDS, // 时间单位
                new ArrayBlockingQueue<>(10), // 队列的树量
                Executors.defaultThreadFactory(), // 线程工厂，使用默认的线程工厂
                new ThreadPoolExecutor.AbortPolicy() // 拒绝策略，使用默认的终止策略
        );

        Runnable target = new Test();
        executor.execute(target);//线程池自动处理
        executor.execute(target);
        executor.execute(target);
        executor.execute(target);//复用前面的核心线程
        // 关闭线程池，防止新任务提交，但会执行完已提交的任务
        executor.shutdown();

        // 等待线程池中任务完成
        try {
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }
}

```

 Runnable在线程池里实现

```
executor.shutdown();
```

柔和的关闭

```****
executor.shutdownNow();
```

强制性的关闭

- new ThreadPoolExecutor.AbortPolicy() 是直接抛异常
- new ThreadPoolExecutor.DiscardOldestPolicy()  丢弃等待最久的任务
- new ThreadPoolExecutor.CallerRunsPolicy() 直接使用主线程调用，绕过线程池
- new ThreadPoolExecutor.DiscardPolicy() 直接丢弃任务

```
Future<String> future3 = executor.submit(new Test);
```

callable的实现，要重写call方法

### Executors

是一个线程池的工具类

提供了很多静态方法来返回不同特点的线程池对象

底层都是ThreadPoolExecutor来实现的

**`newFixedThreadPool(int nThreads)`**: 创建一个固定大小的线程池，线程池中的线程数量是固定的。

**`newSingleThreadExecutor()`**: 创建一个单线程的线程池，只有一个工作线程来执行任务，任务会按照顺序执行。

**`newCachedThreadPool()`**: 创建一个可缓存的线程池，线程池中的线程数根据需要动态调整，适合执行短期任务。

**`newScheduledThreadPool(int corePoolSize)`**: 创建一个线程池，可以在给定延迟后执行任务，或定期执行任务。

```
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class Test {
    public static void main(String[] args) {
        // 创建固定大小的线程池，大小为 3
        ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);

        for (int i = 0; i < 5; i++) {
            fixedThreadPool.execute(() -> {
                System.out.println(Thread.currentThread().getName() + " is executing a task");
            });
        }

        // 关闭线程池
        fixedThreadPool.shutdown();
    }
}

```

核心线程数量应该是多少？

- 计算密集的 cpu+1
- IO密集的 cpu*2

**大型并发系统使用这个会出现系统安全风险**

## 并发并行

进程：正在运行的程序软件就是一个独立的进程

线程属于进程，一个进程中可以运行很多个线程

进程中的多个线程是并发并行进行的

**线程是在cpu上进行的**cpu上能运行的线程是有限的

cpu切换的速度就很快，感觉这些线程在同时进行，这就是并发

**多线程是并发和并行同时进行的**

#### 生命周期（各种状态）

线程从生到死的过程，经历的各种状态

new-->Runnable-->Teminated

或者

堵塞等待等其他的状态
