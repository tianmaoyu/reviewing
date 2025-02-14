# Java

下载 oracle openjdk1.7 / oracle-openjdk-23



![image-20250213091731204](./img/image-20250213091731204-9409457-9409461.png)



![image-20241206133211397](./img/image-20241206133211397.png)

把安装的 1.7 本地加载上来

![image-20241206133554030](./img/image-20241206133554030.png)

## 一，数据结构

> 数组：array，链表：linkedList； 树：tree； 队列：queue； 栈：stact；； 字典：dict（map）

Openjdk  / OracleJdk 稍微有点不一样

![image-20241205093000202](./img/image-20241205093000202.png)

![image-20241205094655336](/Users/eric/Desktop/笔记/复习日记/img/image-20241205094655336.png)



#### List： 

1. 元素可以重复
2. 元素顺序存储
3. 可以索引访问

#### Set：

1. 不可重复
2. 元素不保证顺序
3. 不可索引访问

### Map：

1.  key， value 形式
2. 底层 采用 数组+链表或者tree 实现



### 1,`HashMap`

> 待手写一遍

> https://www.bilibili.com/video/BV1b84y1G7o5/?spm_id_from=333.337.search-card.all.click&vd_source=90371f8b7d79625f67fcdcfff3dee01f

1. node结构： hash，key，value，next

2. 底层实现： 数组+单链表/树 ； 链表长度超过 一个值

3. index：数组寻址： key -  hash值- mod（右移动） 上数组长度 得到index `newTab[e.hash & (newCap - 1)] = e;`  

4. hash：hash 函数 哈希扰动

   > ```
   > 两个结果一样，只不过 & 运算更快
   > newTab[e.hash & (newCap - 1)] = e;` 和 `newTab[e.hash % newCap] = e;
   > ```

5. 扩容机制：超过一个yuzhi 负载因； `newCap = oldCap << 1`2两倍。* 2 效果一样；

6. Put添加元素：计算index，如果没有碰撞直接赋值；如果碰撞：插入到链表的尾部； 如果链表超过  tree 预值；进行数化； 当如有值的时候还要判断是否相等；如果相同的 后续还要判断是否替换， 尾插法

7. resize：遍历整个数组，如果数组中的元素的 next 为空 就直接重新计算哈希 赋值到新的位置； 如果元素是树，那么就进树的相应操作，如果next不为空 这里有个小技巧 计算每一个

8. remove：

9. ==为什么不使用头插法==：

10. ==手写一遍==：

### 2,`CurrentHashMap`

oracle openjdk1.7 / Jdk1.8 



### hashtable

1. 对于 对外提供的 接口方法上 统统 加了 synchronized； 锁住整个实列-整个hashtable
1. 线程安全，简单粗暴，并发性能低

### 1.7 currentHashMap

1. 16 个segments； 分段锁-提高一定的性能
2. 一个segment 继承 ReentrantLock，里面有 事数组+链表形式

==注意== ReentrantLock ，push 是所在单给segment， 

==缺点==  锁主每个桶，每个桶中一个数组+链表； 如果很多线程同时往一个桶写，即使他们没有哈希碰撞，也被锁住； 1.8 干脆直接锁住数组的第一个每个元素，也不需要分段了

#### 2.1 CAS  比较并且交换

> 通过比较内存中的值 是否与提供的期望值是否相等，相等就更新，否者失败； 具体实现一般由cpu 来具体实现

**优点**：

- 是一种原子操作，
- 是一种高效的多线程同步，无锁算法，也叫乐观锁
- 适合**高并发低竞争**场景，

**缺点：**

- ABA 问题 ；解决办法同步**锁**，添加**版本号**
- 添加字段来表示锁的标识位
- 失败从新试：也叫自旋； 高竞争下会导致大量失败，然后尝试 对cpu 资源是一种浪费，多也是对性能的有影响

**应用：**

- 常用于`AtomicInteger`、`AtomicReference`等原子类。

  

#### 2.2  synchroinzed

- 它是java语言中封装的锁，锁存储在对象头中
- JVM内置的**监视器锁（Monitor）**通过`monitorenter`和`monitorexit`字节码实现
- 同步锁，通过线程阻塞，线程等待，线程通知 来控制线程的执行顺序；
- 线程的上下文切换，阻塞，资源消耗大；
- 隐式加锁/释放，自动管理。

**优点：**

- **简单易用**：语法简洁，无需手动释放锁。
- **JVM优化**：锁升级机制适应不同场景。（太偏知道就行）
- **可重入**：同一线程可重复获取锁。

**缺点：**

- 功能简单： 不支持中断，超时（需要配合线程中的一些功能），不支持公平，非公平
- **非公平锁**：默认抢占式，可能导致线程饥饿。
- 重量级：线程的上下文切换，阻塞，资源消耗大；

**应用：**

- 常常配合 object.notify(), object.wait() 等
- 代块，方法，静态方法，可以锁类，对象



#### 2.2, **`ReentrantLock`** api

基于**AQS（AbstractQueuedSynchronizer）**框架，内部通过`CAS`和双向队列（CLH）管理线程阻塞。内部使用 LockSport 本地方法来阻塞线程，和唤醒线程，提供更灵活的控制，适合复杂并发需求。

优点：

- **功能丰富**：支持超时（`tryLock`）、可中断（`lockInterruptibly`）、公平性选择。
- **条件变量灵活**：可绑定多个`Condition`，分组控制线程唤醒。
- 高竞争下表现优秀。

缺点：

- **使用复杂**：需手动释放锁，易遗漏（推荐在`finally`中释放）

**应用：**

- 线程池、资源池



#### 2.3 ，Volatile，告诉jvm，此变量直接从内存读取，保证可见性  cpu 的行为

内存中的数据被称为主内存，然后每个线程都有自己的一小块内存区域，称为工作内存； 一个变量都会 在线程中会被考呗一个副本， cpu 一级高速缓存，二级高速缓存；工作内存拿到的某个数据不一定是最新； Volatile 高速cpu 此值必须冲主内存中取，不要优化，也不要缓存它 

#### CAS API：

```java
U = sun.misc.Unsafe.getUnsafe();
Class<?> k = ConcurrentHashMap.class;
//获取 sizeCtl 内存便宜量
SIZECTL = U.objectFieldOffset(k.getDeclaredField("sizeCtl"));
# 
public native boolean compareAndSwapInt(Object obj, long offset, int expected, int update);
```



一，hash 值的计算

> & 01111,1111,1111,1111 防止为负数

```java
 static final int spread(int h) {
        return (h ^ (h >>> 16)) & HASH_BITS;
    }
```



#### 一，initTable  使用cas

使用一个字段作为标识位，-1:是否有线程正在初始化

```java
if ((sc = sizeCtl) < 0)
     Thread.yield(); // lost initialization race; just spin
U.compareAndSwapInt(this, SIZECTL, sc, -1)
```

#### 二，没有哈希碰撞的插入： hashtable 数组中index 位置 赋值 cas 

```java
casTabAt(tab, i, null, new Node<K,V>(hash, key, value, null))
```

#### 三，有hash碰撞的插入  ，尾部插入，或者树插入

1. 使用 synchorized 对 链表的头，或者树的root 节点 加锁  （比较费时的用）

   ![image-20241206190449851](./img/image-20241206190449851.png)

#### 四，扩容

```java
    /** Secondary seed isolated from public ThreadLocalRandom sequence */
    @sun.misc.Contended("tlr")
    int threadLocalRandomSecondarySeed;


    UNSAFE = sun.misc.Unsafe.getUnsafe();
    Class<?> tk = Thread.class;
    SEED = UNSAFE.objectFieldOffset
            (tk.getDeclaredField("threadLocalRandomSeed"));
    PROBE = UNSAFE.objectFieldOffset
            (tk.getDeclaredField("threadLocalRandomProbe"));
    SECONDARY = UNSAFE.objectFieldOffset
            (tk.getDeclaredField("threadLocalRandomSecondarySeed"));


    public native int getInt(Object var1, long var2);

    public native void putInt(Object var1, long var2, int var4);
       
```

#### 五，扩容& 迁移 （==核心难点==） -多线程扩容，扩容时增删改？

https://blog.csdn.net/m0_37550986/article/details/125230667

> 对添加，删除  锁住的这个 数组上当元素，后序操作很好理解； 如果对整个 数组扩容 又该如何做呢？

- 多线程环境下触发扩容条件之后，如何保证只有一个线程去新建新数组？
- 在数据迁移过程中如果有数据的添加，删除，查询该怎么处理？
- 在**ConcurrentHashMap**中是多个线程同时扩容的，那么如何协调多个线程同时扩容；
- 如何确保数据全部迁移完成？

1. 每个线程负责 一部分？ 如何分配的。 每个线程 竞争 区间； 使用标识位来表示当前是否扩容完成；
2. 数据迁移时，难道就不怕多个  线程 的数据都迁移到一个 节点上；  扩容 n的二次方，也就是扩容后，要么在原理的位置，要么在 原理位置+原来的长度位置；
3. 如果这个节点正在迁移，当前又来一个插入到当前节点上，move 标签来表示当前正在迁移，插入的线程 进入协助扩容；扩容完成，继续回到 之前的 for 寻还
4. 删除一样，也是进入死循环，如果遇到在扩容就 辅助扩容；
5. 获取，没有锁 则使用 锁
6. 对于元素的赋值，

==死循环==

```java
for (int[] tab = table;;)
```

![image-20241207120525134](./img/image-20241207120525134.png)

```java
    public static void main(String[] args){
        int count=0;
        int[] table={1,2,3,4,5};
        //这是一个死循环
        for (int[] tab = table;;) {
            System.out.println(  );
            count++;
            if(count>100){
                break;
            }
        }
    }
```



### Unsafe 的使用 设计到 classloader

```java
public static Unsafe getUnsafe() {    
Class<?> caller = Reflection.getCallerClass();    
if (!VM.isSystemDomainLoader(caller.getClassLoader()))      
      throw new SecurityException("Unsafe");   
return theUnsafe;}
```



> 直接使用 会报错

```java
package org.example;

import sun.misc.Unsafe;

public class ClassloaderTest {
    public static void main(String[] args) {
        Unsafe unsafe = Unsafe.getUnsafe();
        System.out.println(unsafe);
    }
}

```

```java
  Field field = Unsafe.class.getDeclaredField("theUnsafe");
  field.setAccessible(true);
  Unsafe unsafe = (Unsafe) field.get(null);
  System.out.println(unsafe);
```



### 3, `TreeMap`

> avl 树 的平衡差1， 红黑树 的平衡最大差 1倍； 平衡条件相对宽松

1. Tree，平衡二叉树，红黑树
2. key value，key 排序，不能重复
3. 每个 节点 要么红，要么黑；
4. root 必须黑节点，两条路径上黑点相同 （最大差一倍内-  ）
5. 叶子几点 黑点，新插入的是红几点
6. 连续两个节点不能为红，黑色可以

==avl 树== 手写

==红黑树== 手写



### 3,`ArrayList`

1. 底层实现：数组
2. 扩容机制 ： 默认 1.5倍，创建新的内存，复制原来的
3. 增删改查的时间复杂度  get ，set O(1);  delete，add 尾部O(1) 其他为 n； 查找 n； 
4. 线程安全：不安全
5. ==随机性能==：遍列查询，由于连续内存，往往会被高速缓存 一级缓存二级缓存，命中率高；
6. 内存：使用率高

### 4.`LinkedArrayList` 、`LinkedList`

1. 底层实现：双向链表， 存储 head，tail 节点
2. 扩容：动态的数据结构，理论无限大
3. 时间复杂度：添加 头尾：1； index： n； get by index :n; 搜索：n； remove ：n
4. 线程不安全： 不安全
5. 随机访问：慢， 插入删除头尾 块；
6. 内存：使用率差

### 5,`HashSet`  

1. 底层使用率hashmap 实现，所有的value 都是同一一个 object； 

   ```java
   private static final Object PRESENT = new Object();
   
   public boolean add(E e) {
           return map.put(e, PRESENT)==null;
   }
   public boolean remove(Object o) {
           return map.remove(o)==PRESENT;
   }
   ```

### 6, `TreeSet`

1. 底层使用 TreeMap, 只用了 key ， value 是一个固定的值

   ```java
   private static final Object PRESENT = new Object();
   
   public boolean add(E e) {
           return m.put(e, PRESENT)==null;
       }
   public boolean remove(Object o) {
           return m.remove(o)==PRESENT;
       }    
   ```

   

## 二，线程并发

> 并发问题，线程死锁，力度太大

### 1,`Thread`

https://juejin.cn/post/7379921204754300962?searchId=2024120809391080CB9CE89CBCEFA67BF2

##### 1.1 线程状态

> 1. New  ：new Thread 时的状态，被创建但是没被执行
> 2. Runnable/Running ： 调用start 方法时，准备好执行，但是还未获得cpu 时间； 已经在执行
> 3. Bolcked：线程阻塞状态，线程未获取 synchorized 同步代码的锁； 未获得锁叫阻塞
> 4. watting： 等待， object.wait()  当前获得锁的等待;  threadA.join() 当前线程等待 threadA； lockSupport.part(); 
> 5. wating_time:超时等待. obj.wait(timeout). threadA.join(timeout); lockSuport(), thread.sleep(timeout)
> 6. 终止状态：正常退出，或者没有捕获异常而退出



1.2 线程之间的通信

1. 注意 object.wait()/ object.notify()  只能配合   synchionzed 同步块中使用
2. interrupt： 也可以唤醒正常 等待的线程+超时等待

```java
  Thread threadA = new Thread("threadA"){
            @Override
            public void run() {

                   System.out.println("threadA is running ");
                    try {
                        Thread.sleep(1000000);
                    } catch (InterruptedException e) {

                    }
                    System.out.println( "threadA state:" + Thread.currentThread().getState());
            }
        };

        threadA.start();
        System.out.println( "threadA state:" + threadA.getState());
        Thread.sleep(500);
        System.out.println( "threadA state:" + threadA.getState());
        Thread.sleep(100);
        threadA.interrupt();
```

```java
package org.example.thread;
import lombok.SneakyThrows;
public class  TwoThreadDemo{
    public static volatile  int sum=0;
    public static final Object lock=new Object();
    @SneakyThrows
    public static void main(String[] args){

        Thread threadA= new Thread(TwoThreadDemo::add);
        Thread threadB= new Thread(TwoThreadDemo::add);
        threadA.start();
        threadB.start();
        Thread.sleep(5000);
    }
    @SneakyThrows
    public static void add(){
        for(int i=0;i<10; i++){

            synchronized (lock){
                TwoThreadDemo.sum++; System.out.println("thread:"+Thread.currentThread().getName()+" sum:"+sum);
                lock.notify();
                lock.wait(50);
            }
        }
    }
}
```

1. 实现 Runnable 接口，继承 Thread类，使用线程池
2. ==interrupt 难点==

### 2,`ThreadLocal`

1. 实现原理
2. 使用场景

### 3,`ExecutorService`

https://juejin.cn/post/6844904078728757261

```java
//c 初始的值= -536870912
int wc = workerCountOf(c);
     if (wc >= CAPACITY || wc >= (core ? corePoolSize : maximumPoolSize))
                    return false;
// CAPACITY 是一个 64位 全部为1的，值= 2^29-1； 也是最大线程数量
// 如果是核心线程，不能超过 核心，如果不是核心，不能超过 最大线程  
  


```

![image-20241208164027840](./img/image-20241208164027840.png)

- [x] 如何区分核心线程，非核心线程？ ：没有区分

- [x] 阻塞队列满了后的？各种拒绝策略？：自定义拒绝策略

- [x] 阻塞队列？： 使用  ReentrantLock  实现， 如果集合为null ，要么等待，要么==阻塞？aqs相关代码==

- [x] 任务 是如何交给 worker 的？ ：创建线程时直接给，要么队列中等待

- [x] 如何判断线程执行还是空闲的？ 如何体现 ==超时时间==呢？：已经解决

  1. 每个线程执行任务都会做一些后续工作
  2. 比如：检测当前线程池是否可以终止，剪掉正在worker 的个数
  3. 把当前线程从 工作线程中移除，判断是否异常结束
  4. 如果中间有 线程失败 挂掉，还会补 新创建线程-维持核心线程数

  ```java
  private void interruptIdleWorkers(boolean onlyOne) {
          final ReentrantLock mainLock = this.mainLock;
          mainLock.lock();
          try {
              for (Worker w : workers) {
                  Thread t = w.thread;
                  if (!t.isInterrupted() && w.tryLock()) {
                      try {
                         // 只会中断一个等待中的线程，也就是没有运行的  
                          t.interrupt();
                      } catch (SecurityException ignore) {
                      } finally {
                          w.unlock();
                      }
                  }
                  if (onlyOne)
                      break;
              }
          } finally {
              mainLock.unlock();
          }
      }
  
  
  boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;
  ```

  ![image-20241208182303417](./img/image-20241208182303417.png)

  

  ![image-20241208180615037](./img/image-20241208180615037.png)

- [x] 几个常用参数： corePoolSize，maximumPoolSize；keepAliveTime；workQueue，threadFactory；RejectedExecutionHandler

- [x] 内部实现原理：

- [x] > 1. 创建的时候不会创建 线程
  > 2. 提交任务时，判断核心线程，小于核心就开启新的线程，并没有去==判断有没有空闲==线程，直接把当前任务给新创建的线程
  > 3. 如果核心线程已经满，那就把任务添加到队列；如果核心线程数量 设置0 那么就创建 线程执行
  > 4. 如果核心线程满：队列也满，就拒绝
  > 5. 如果线程处于空闲状态，通过超时获取任务，没有获取到超时，返回null 结束空闲线程

- [x] 使用注意事项

  1. 防止大量任务排队，做好拒绝策略
  2. 在多线程 上下线程切换时，对需要把线程私有的变量 转交给下一个线程
  3. 使用完毕手动关闭 线程池
  4. 可以设置 核心线程，也会超时，保障没有任务不必占用资源
  5. 也可以提交一些待返回值的 任务

- [x] 使用返回值的 callable<E>. 返回类型 e

  ```java
  Future<Integer> submit = threadPooll.submit(() -> 100);
  ```

  

- [x] ==考察点，值得借鉴的地方==



### **一、线程池核心参数与工作流程**

Java 线程池通过 `ThreadPoolExecutor` 实现，其核心参数如下：

```java
ThreadPoolExecutor(
    int corePoolSize,      // 核心线程数
    int maximumPoolSize,   // 最大线程数
    long keepAliveTime,    // 非核心线程空闲存活时间
    TimeUnit unit,         // 时间单位
    BlockingQueue<Runnable> workQueue, // 任务队列
    RejectedExecutionHandler handler   // 拒绝策略
)
```

**工作流程**：

1. 提交任务时，优先使用核心线程处理。
2. 核心线程满后，任务进入队列等待。
3. 队列满后，创建非核心线程（直到达到最大线程数）。
4. 所有线程和队列均满时，触发拒绝策略。

------

### **二、核心线程数（corePoolSize）设置原则**

#### **1. 任务类型决定线程数**

- CPU 密集型任务（如计算、逻辑处理）：
  - 线程数 ≈ CPU 核心数 + 1（防止线程阻塞浪费资源）。
  - 示例：4 核 CPU → `corePoolSize = 5`。
- IO 密集型任务（如网络请求、文件读写）：
  - 线程数 ≈ 2 * CPU 核心数（利用线程等待 IO 的空闲时间）。
  - 示例：4 核 CPU → `corePoolSize = 8`。

#### **2. 动态调整**

- 使用 `setCorePoolSize()` 方法动态调整核心线程数（如根据业务高峰/低谷）。
- 默认情况下，核心线程即使空闲也不会被回收（需设置 `allowCoreThreadTimeOut(true)` 改变行为）。

------

### **三、最大线程数（maximumPoolSize）设置原则**

- **公式**：`maximumPoolSize = corePoolSize + 弹性扩展余量`。
- 场景示例：
  - **突发流量**：若任务量可能突然激增，可设置较大的 `maximumPoolSize`（如 `corePoolSize * 2`）。
  - **资源敏感型系统**：若服务器资源有限，需严格控制最大线程数，避免 OOM。
- **注意**：线程数过多会导致频繁上下文切换，反而降低性能。

------

### **四、任务队列（workQueue）选择**

| **队列类型**                              | **特点**                                                     | **适用场景**                       |
| ----------------------------------------- | ------------------------------------------------------------ | ---------------------------------- |
| **无界队列**（如 `LinkedBlockingQueue`）  | 队列无限增长，可能导致 OOM                                   | 任务量可控且拒绝策略要求不高的场景 |
| **有界队列**（如 `ArrayBlockingQueue`）   | 队列满后触发创建非核心线程                                   | 需要限制资源使用的场景             |
| **同步移交队列**（`SynchronousQueue`）    | 不存储任务，直接移交线程执行（需配合较大的 `maximumPoolSize`） | 高吞吐、短任务场景                 |
| **优先级队列**（`PriorityBlockingQueue`） | 按优先级处理任务                                             | 需要任务优先级的场景               |
| **DelayedWorkQueue** 延迟周期             |                                                              |                                    |

------

### **五、拒绝策略（RejectedExecutionHandler）**

| **策略**                | **行为**                                               |
| ----------------------- | ------------------------------------------------------ |
| **AbortPolicy**（默认） | 直接抛出 `RejectedExecutionException`，                |
| **CallerRunsPolicy**    | 由提交任务的线程直接执行任务（降低提交速度，避免雪崩） |
| **DiscardPolicy**       | 静默丢弃新任务                                         |
| **DiscardOldestPolicy** | 丢弃队列中最旧的任务，重新提交新任务                   |

------

### **六、最佳实践与示例**

#### **1. 通用配置模板**

```java
int cpuCores = Runtime.getRuntime().availableProcessors();

ThreadPoolExecutor executor = new ThreadPoolExecutor(
    cpuCores + 1,                   // corePoolSize
    cpuCores * 2,                   // maximumPoolSize
    60, TimeUnit.SECONDS,           // keepAliveTime
    new LinkedBlockingQueue<>(1000),// 有界队列（容量1000）
    new ThreadFactoryBuilder().setNameFormat("task-pool-%d").build(), // 自定义线程名
    new CallerRunsPolicy()          // 拒绝策略
);
```

#### **2. 场景优化**

- Web 服务器请求处理（IO 密集型）：

  ```java
  corePoolSize = 2 * cpuCores;
  maximumPoolSize = 4 * cpuCores;
  workQueue = new LinkedBlockingQueue<>(2000);
  ```

- 批量数据处理（CPU 密集型）：

  ```java
  corePoolSize = cpuCores + 1;
  maximumPoolSize = cpuCores + 1; // 严格限制线程数
  workQueue = new SynchronousQueue<>(); // 无缓冲，直接执行
  ```

------

### **七、注意事项**

1. **避免无界队列**：防止任务堆积导致内存溢出（OOM）。

2. 监控线程池状态：

   ```java
   // 获取活跃线程数
   executor.getActiveCount();
   // 获取队列积压任务数
   executor.getQueue().size();
   ```

3. 优雅关闭线程池：

   ```java
   executor.shutdown();          // 停止接收新任务，等待已有任务完成
   executor.shutdownNow();       // 尝试立即终止所有任务（慎用）
   ```

4. **线程池隔离**：不同业务使用独立线程池，避免互相影响。

------

### 八，补充常用功能

在web应用中，通常还要出来，线程池中，上下两个线程任务交接情况；： 一个请求，过来使用线程池中的线程 来处理任务，请求线程返回情况； ==threadlocal== 数据获取和释放问题。



### **八、总结**

- **核心线程数**：由任务类型（CPU/IO 密集型）和 CPU 核心数决定。
- **最大线程数**：根据系统负载和资源限制动态调整。
- **队列选择**：优先使用有界队列，结合拒绝策略保护系统稳定性。
- **拒绝策略**：推荐 `CallerRunsPolicy` 或自定义策略，避免直接丢弃任务。

通过合理配置线程池参数，可以在高并发场景下实现资源高效利用与系统稳定性的平衡。





==备注==：在IO密集型任务中，推荐线程数约为CPU核心数的两倍，主要基于以下原因：

1. **利用IO等待时间**：
   - **阻塞期间CPU空闲**：当线程执行IO操作（如读写文件、网络请求）时，会进入阻塞状态，此时CPU可执行其他线程的任务。
   - **提高吞吐量**：通过增加线程数，CPU在等待一个线程的IO完成时，可切换到其他线程执行计算任务，减少空闲时间。
2. **经验公式与平衡点**：
   - **2倍核心数的由来**：假设线程有约50%的时间在等待IO，则双倍线程可让CPU在等待期间处理另一线程的任务，保持高利用率。
   - **避免过度切换**：过多的线程会增加上下文切换开销，两倍核心数是平衡利用率和切换成本的经验值。
3. **实际场景调整**：
   - **IO等待时间差异**：若IO延迟更高（如远程API调用），可适当增加线程数；若IO较快（如本地SSD），可减少线程数。
   - **系统资源限制**：需考虑内存、文件描述符等资源，避免因线程过多导致OOM或资源竞争。

**示例**：

- 4核CPU处理网络请求：
  - 设置8个线程，每个线程在等待响应时，CPU处理其他请求，提升并发能力。
- 调整依据：
  - 监控CPU利用率：若长期低于70%，可增加线程；若过高（>90%）或上下文切换频繁，则减少线程。

**总结**：两倍核心数是平衡IO等待和CPU利用的经验起点，实际需根据任务特性和系统监控动态调整。

-----

Redis 出现卡顿（延迟高、响应慢）通常由多种原因引起，需要结合具体场景排查。以下是常见原因及对应的解决方案：







### 3,同步：`Synchronied ` vs `CAS`

1. 使用场景，同步代码块，对可以锁在 类型上，也可以锁在对象上，可以在方法上，也可以在方法内部；一种悲观锁，没有获得锁的线程处于 blocked 状态，如果在线程多线程通信配合objec.wat()/object.notify()； 一般情况暂用的资源较多，不支持 ==中断操作==
2. 对其比其他 锁，cas:  对比和交换，通过对比对期望值，如果相等，就更新，无锁，也是一种乐观锁，在高并发下 提供跟好的一种性能，； 如果更新比较频繁 大量重试也会是一种浪费； 会出现 aba 问题； 采用多版本号记录； 底层具体一般有 cpu 来实现。
3. 对比 lock 接口： 对锁的范围力度更细， 它实现了 Synchronied全部功能，添加添加，类似 object.wait(),object.notify() 相关功能； 并且支持 ==中断==。`lock.lockInterruptibly()`

### 4, `valitale` 可见性，防重排序

1. 作用原理，又cpu 实现，每个线程执行时会拷贝一份数据到自己线程空间中去，比如高速缓存，和主内存的拷贝，不允许拷贝。

### 5, `AQS` QueuedSynchronizer

： 构建锁和同步器的基础框架，基于队列的同步器，时 juc  包的核心，它使使用 先进先出的队列 实现，实现队列使用的是 双向链表，而不是 数组； 线程通过队列来等待资源；



> 对于线程的 阻塞，唤醒 通过 LockSupport 来实现

```java
//唤醒某个线程
LockSupport.unpark(node.thread);
 //挂起当前的线程，会响应中断
 LockSupport.park(this);

// 返回是否被中断，并且把中断标识位设置为 false
return Thread.interrupted();
```



tryAcqurie 在具体子内中使用

![image-20241208225056432](./img/image-20241208225056432.png)

![image-20241208224703165](./img/image-20241208224703165.png)

未抢到锁的添加到尾部

```java
  private Node addWaiter(Node mode) {
        Node node = new Node(Thread.currentThread(), mode);
        // Try the fast path of enq; backup to full enq on failure
        Node pred = tail;
        if (pred != null) {
            node.prev = pred;
            if (compareAndSetTail(pred, node)) {
                pred.next = node;
                return node;
            }
        }
        enq(node);
        return node;
    }
```

以 ReentrantLock：为列：

3. 如何被阻塞？

   cas 更加 state 值 来抢占锁，如果成功继续执行，没有就把当前线程 加入到等待队列 等待尾不；

4. 公平和不公平： 当前线程执行完毕后 去排队还是不回去排队问题。

5. 如何被唤醒？：正常唤醒，又前一个已经节点唤醒后一个节点；1. 第二种通过中断唤醒：

6. 如何不能被响应中断？ 通过中断标识位

7. 如何能被响应中断？：



### 6， final ： 

> 表示应用的地址初始后地址不能再变动，但是内部的值可以变动

```java
private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
```



1. 内部实现原理

### 6, 同步：`Lock`  ：`ReentrantReadWriteLock`；`ReentrantLock`

1. 锁的可重入性

2. 公平锁与非公平锁, Synchronied 不能够控制 随机的获取

3. 锁的尝试获取（`tryLock()`）

4. ==中断响应== 

   ```java
   lock.lock();  // 无法响应中断
   lock.lockInterruptibly();  // 支持中断的锁请求
   ```

   

5. 条件变量 (`Condition`)

6. 性能与吞吐量

对读共享，对写排斥：多并发下多读 少写炒作有性能上的提



### 7,Atomic***

1. 并发的一些原则操作类，底层多采用 cas 实现

### 8，了解 **`CopyOnWriteArrayList`**、**`BlockingQueue`**

1. 内部原理

==待手写aqs==



## 三，JVM



> https://github.com/openjdk/jdk
>
> https://blog.csdn.net/twotwo22222/article/details/127890270
>
> https://www.cnblogs.com/hjcenry/p/17937034
>
> 官方文档：
>
> pdf：https://docs.oracle.com/javase/specs/
>
> https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.6

Javac. 如何编译 .java 的？

Java 如何运行 .class 的？



```java
public class Hello{
    public static void main(String[] args){
        System.out.println("Hello World");
    }
}
```

```shell
javac Hello.java
java Hello
// java 启动jvm 实列 java.exe 调用 jvm.dll
// jvm 调用。sun.launcher.LauncherHelper 获取 main 类和 方法 其中就 初始化了 加载器
```

jdk 代码入口 / github 下载源代码

> Java.c  中的 JavaMain 方法

![image-20241209194633554](./img/image-20241209194633554.png)

其中有个变量 ：mainClass，执行main 方法

```c
mainClass = LoadMainClass(env, mode, what);

 /* Invoke main method. */
(*env)->CallStaticVoidMethod(env, mainClass, mainID, mainArgs);
```

```c

static jclass LoadMainClass(JNIEnv *env, int mode, char *name)
{
    jmethodID mid;
    jstring str;
    jobject result;
    jlong start, end;
    //获取java 中的  sun.launcher.LauncherHelper
    jclass cls = GetLauncherHelperClass(env);
    NULL_CHECK0(cls);
    if (JLI_IsTraceLauncher()) {
        start = CounterGet();
    }
    //checkAndLoadMain
    NULL_CHECK0(mid = (*env)->GetStaticMethodID(env, cls,
                "checkAndLoadMain",
                "(ZILjava/lang/String;)Ljava/lang/Class;"));

    str = NewPlatformString(env, name);
    result = (*env)->CallStaticObjectMethod(env, cls, mid, USE_STDERR, mode, str);

    if (JLI_IsTraceLauncher()) {
        end   = CounterGet();
        printf("%ld micro seconds to load main class\n",
               (long)(jint)Counter2Micros(end-start));
        printf("----%s----\n", JLDEBUG_ENV_ENTRY);
    }

    return (jclass)result;
}
```

![image-20241209200102836](./img/image-20241209200102836.png)

```
private static final ClassLoader scloader = ClassLoader.getSystemClassLoader();
```

==不是很确定==helperClass是否已经实列化，并且  scloader已经赋值

```c
jclass
GetLauncherHelperClass(JNIEnv *env)
{
    if (helperClass == NULL) {
        NULL_CHECK0(helperClass = FindBootStrapClass(env,
                "sun/launcher/LauncherHelper"));
    }
    return helperClass;
}
```



Java 可以运行 是如何加载 类的？

```java
//由jvm 启动时调用，首先初始化， boot， ext 两个 ClassLoader
sun.misc.Launcher launcher = sun.misc.Launcher.getLauncher();
```

### 1, 类的加载

把二进制变成一个类型

```java
 protected final Class<?> defineClass(String name, byte[] b, int off, int len,
                                         ProtectionDomain protectionDomain)
```



```java
myClassLoader.getParent().getParent().getParent()
```

**Bootstrap ClassLoader**

1. 由虚拟机实现，c++代码编写

2. 核心包的加载 

3. ```shell
   String boot = System.getProperty("sun.boot.class.path");
   // 可以 指定参数 替换 核心路径 -Xbootclasspath:your_path
   ```

**Extention ClassLoader**

1. 由 java 代码编写，但是由jvm 来实列化

2. 加载 扩展包

3. ```java
    String property = System.getProperty("java.ext.dirs");
   ```

**AppClassLoader**:应用类加载器

1. 由java代码编写，但是由jvm来实列化

2. 加载自己写的代码，或者一些第三方引用包，指定：classpath

3. ```
   java.class.path
   ```

```java
String boot = System.getProperty("sun.boot.class.path");
for(String item:boot.split(":")){
    System.out.println(item);
}
String ext = System.getProperty("java.ext.dirs");
for(String item:ext.split(":")){
    System.out.println(item);
}
String app = System.getProperty("java.class.path");
for(String item:app.split(":")){
    System.out.println(item);
}
```



```java
package org.example;

public class ClassLoaderSimple2 {

    public static void main(String[] args) {
        // 创建一个自定义的类加载器
        MyClassLoaderA myClassLoader1 = new MyClassLoaderA();
        MyClassLoaderB myClassLoader2 = new MyClassLoaderB();
        // 使用自定义加载器加载类
        String basePath = "/Users/eric/Users/eric/code/simple/target/classes/"; // 本地类文件的路径
        MyClassLoaderC myClassLoader3 = new MyClassLoaderC(basePath);

        try {
            // 使用自定义的类加载器加载MyClass类
            Class<?> clazz1 = myClassLoader1.loadClass("com.example.MyClass");
            Class<?> clazz2 = myClassLoader2.loadClass("com.example.MyClass");
            Class<?> clazz3 = myClassLoader3.loadClass("com.example.MyClass");
//
            System.out.println(clazz2==clazz1 );
            // 两个是不一样
            System.out.println(clazz2==clazz3 );

            // 创建 MyClass 实例并调用方法
            Object instance1 = null;
            Object instance2 = null;
            try {
                instance1 = clazz1.getDeclaredConstructor().newInstance();
                clazz1.getMethod("hello").invoke(instance1);
                instance2 = clazz1.getDeclaredConstructor().newInstance();
                clazz1.getMethod("hello").invoke(instance2);
                System.out.println(instance1.equals(instance2));

            }  catch (Exception e) {
                throw new RuntimeException(e);
            }
//            System.out.println("Class loaded: " + clazz1.getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```

```java
package org.example;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class MyClassLoaderC extends ClassLoader {
    private String basePath;
    public MyClassLoaderC(String basePath) {
        this.basePath = basePath;
    }

    @Override
    public Class<?> loadClass(String name) throws ClassNotFoundException {
//        使用当前的加载
        if(name.equals("com.example.MyClass")){
            return this.findClass(name);
        }
//       父类加载
        return super.loadClass(name);
    }
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        // 转换类名为文件路径
        String filePath = basePath + name.replace('.', '/') + ".class";
        File classFile = new File(filePath);
        // 如果文件不存在，抛出 ClassNotFoundException
        if (!classFile.exists()) {
            throw new ClassNotFoundException("Class not found: " + name);
        }
        // 读取文件中的字节
        try (FileInputStream fis = new FileInputStream(classFile)) {
            byte[] classData = new byte[(int) classFile.length()];
            fis.read(classData);
            // 使用 defineClass 将字节数组转换为 Class 对象
            return defineClass(name, classData, 0, classData.length);
        } catch (IOException e) {
            e.printStackTrace();
            throw new ClassNotFoundException("Error loading class: " + name, e);
        }
    }

    public static void main(String[] args) {
        try {
            // 使用自定义加载器加载类
            String basePath = "/Users/eric/code/simple/target/classes"; // 本地类文件的路径
            MyClassLoaderC loaderA = new MyClassLoaderC(basePath);
            // 加载类
            Class<?> clazz = loaderA.loadClass("com.example.MyClass");
            // 输出类的全名
            System.out.println("Class loaded: " + clazz.getName());
            // 创建类的实例
            Object instance = clazz.getDeclaredConstructor().newInstance();
            // 调用方法（假设 MyClass 有一个 hello() 方法）
            clazz.getMethod("hello").invoke(instance);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



### 2,Java 类加载器 `ClassLoader` 双亲委派

- 从 appclassloader 的缓存中找有没有 blue.class 文件

- 没有找到，交给extclassloader去缓存查询

  ```c
  //findLoadedClass0  ：缓存是一个 本地方法，
  ```

- 还没有找到，交给bootstrapclassloader去缓存查询。

- 还没有找到，就从boostrap的路径去找，

- 任然没有找到，就交给下一级ext去他的路径找，

- 还没有，就交给appclassloader的路径去找。

![image-1703941410233](./img/image-1703941410233.png)

```c
//JVM中的一种宏定义写法，它用来包裹本地方法（native method）的实现
JVM_ENTRY(jclass, JVM_FindLoadedClass(JNIEnv *env, jobject loader, jstring name))
  JVMWrapper("JVM_FindLoadedClass");
  ResourceMark rm(THREAD);

  Handle h_name (THREAD, JNIHandles::resolve_non_null(name));
  Handle string = java_lang_String::internalize_classname(h_name, CHECK_NULL);

  const char* str   = java_lang_String::as_utf8_string(string());
  // Sanity check, don't expect null
  if (str == NULL) return NULL;

  const int str_len = (int)strlen(str);
  if (str_len > Symbol::max_length()) {
    // It's impossible to create this class;  the name cannot fit
    // into the constant pool.
    return NULL;
  }
  TempNewSymbol klass_name = SymbolTable::new_symbol(str, str_len, CHECK_NULL);

  // Security Note:
  //   The Java level wrapper will perform the necessary security check allowing
  //   us to pass the NULL as the initiating class loader.
  Handle h_loader(THREAD, JNIHandles::resolve(loader));
  if (UsePerfData) {
    is_lock_held_by_thread(h_loader,
                           ClassLoader::sync_JVMFindLoadedClassLockFreeCounter(),
                           THREAD);
  }

  Klass* k = SystemDictionary::find_instance_or_array_klass(klass_name,
                                                              h_loader,
                                                              Handle(),
                                                              CHECK_NULL);

  return (k == NULL) ? NULL :
            (jclass) JNIHandles::make_local(env, k->java_mirror());
JVM_END
```

**双亲委派的意义：**

1. 避免类的重复加载
2. 避免核心api 被篡改

2. jav

### 2，java jvm模型

java 虚拟机规范： https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-5.html

1. 载器，连接，初始化， 
2. 运行时，
3. 执行引擎，
4. 本地方法运行的接口



==基于 hotsport 自己写一个语言==

==如何写一个自己的虚拟机==



### 3,jvm内存模型

官方文档：https://docs.oracle.com/javase/specs/jvms/se8/html/jvms-2.html#jvms-2.5

1. 程序计数器：(program counter）
2. java 虚拟机栈： 存放每个线程执行的栈帧
3. 堆：主要的内存区域存放，由所有 Java 虚拟机线程共享。堆是运行时数据区域，所有类实例和数组的内存都从中分配， 回收管理 垃圾垃圾搜集区， yung （伊甸园，两个幸存区）, old
4. 本地方法栈：存放本地方法 执行的
5. 方法区（一个规范）：存储每个类的结构，例如运行时常量池、字段和方法数据以及方法和构造函数的代码，包括在类和实例初始化以及接口初始化中使用的特殊方法;   它的==实现==：每个版本叫法不一样，==元数据区==，==永久代==





1. 类型如何被初始化
2. 类型 如何被赋值
3. 内存区域划分
4. jvm 代码？

1. class 字节字节码的加载
2. 字节码的连接：验证，准备，连接

### 4,垃圾回收机制

https://www.cnblogs.com/hjcenry/p/17938808

官方文档： https://docs.oracle.com/javase/8/docs/technotes/guides/vm/gctuning/

1. 哪些需要清理？ CG Root： 被栈，本地方法的应用，方法区常量，上的直接或者间接应用

   1. 应用计数器：简单的（python） 无法解决循环应用
   2. Java采用 可达性分析，从 CG Root 出发 遍历对象。

2. 如果从 new 的对象-Eden伊甸园区到，幸存者，再到老年代？

   1.  在Survivor中 每次都在复制，超过 年年龄 6的对象 会迁移到 old 区
   2. 如果一个大对象，直接存在 old 区，如：大数组
   3. old 区满了后 fullgl- 触发stop-the-word； 采用标记整理算法/标记清理

3. 有哪些清理算法： 

   1. 标记，然后删除，标记清理算法：内存碎片
   2. 标记整理：每个整理都要迁移：代价大
   3. 复制清理：直接复制：代价就是两倍内存； young GC （Minor gc）

4. 最常用的，垃圾搜集器，

   java8 默认的垃圾回收器- ParallelGC

   ```java
   eric@mac bin % ./java -XX:+PrintGCDetails -version
   java version "1.8.0_421"
   Java(TM) SE Runtime Environment (build 1.8.0_421-b09)
   Java HotSpot(TM) 64-Bit Server VM (build 25.421-b09, mixed mode)
   Heap
    PSYoungGen total 114688K, used 3932K 
     eden space 98304K, 4% used 
     from space 16384K, 0% used 
     to   space 16384K, 0% used 
    ParOldGen       total 262144K, used 0K 
     object space 262144K, 0% used 
    Metaspace       used 2326K, capacity 4480K, committed 4480K, reserved 1056768K
     class space    used 255K, capacity 384K, committed 384K, reserved 1048576K
   ```

   ```shell
   ./java -XX:+PrintCommandLineFlags -version
   -XX:InitialHeapSize=402653184 
   -XX:MaxHeapSize=6442450944 
   -XX:+PrintCommandLineFlags 
   -XX:+UseCompressedClassPointers 
   -XX:+UseCompressedOops 
   -XX:+UseParallelGC
   java version "1.8.0_421"
   Java(TM) SE Runtime Environment (build 1.8.0_421-b09)
   Java HotSpot(TM) 64-Bit Server VM (build 25.421-b09, mixed mode)
   ```

   

5. GC 性能优化-*自动动态内存管理算法*

   1. 过对象的创建后销毁
   2. 不使用的对象设置null 帮助gc
   3. 合理设置堆的大小，减少 Full gc 的频率
   4. 选择适合的垃圾回收器：

```

```

==源代码查看==。 hotspot 代码

![image-20241210162534893](./img/image-20241210162534893.png)

### G1

1. 划分为 region 小块后，是否还区分 ，yung，old 区域还存在在吗

   > 每一个 region 可以是 yung，old，survier 区； 也有代的概念？

2. 添加了什么，大对象区，怎么做的

3. 具体细节

把整个堆空间 划分位多个小块 region，

> java -XX:+PrintGCDetails -version

```shell
eric@mac ~ % java -XX:+PrintGCDetails -version
[0.006s][warning][gc] -XX:+PrintGCDetails is deprecated. Will use -Xlog:gc* instead.
[0.013s][info   ][gc,init] CardTable entry size: 512
[0.013s][info   ][gc     ] Using G1
[0.014s][info   ][gc,init] Version: 23.0.1+11-39 (release)
[0.014s][info   ][gc,init] CPUs: 8 total, 8 available
[0.014s][info   ][gc,init] Memory: 24576M
[0.014s][info   ][gc,init] Large Page Support: Disabled
[0.014s][info   ][gc,init] NUMA Support: Disabled
[0.014s][info   ][gc,init] Compressed Oops: Enabled (Zero based)
[0.014s][info   ][gc,init] Heap Region Size: 4M
[0.014s][info   ][gc,init] Heap Min Capacity: 8M
[0.014s][info   ][gc,init] Heap Initial Capacity: 384M
[0.014s][info   ][gc,init] Heap Max Capacity: 6G
[0.014s][info   ][gc,init] Pre-touch: Disabled
[0.014s][info   ][gc,init] Parallel Workers: 8
[0.014s][info   ][gc,init] Concurrent Workers: 2
[0.014s][info   ][gc,init] Concurrent Refinement Workers: 8
[0.014s][info   ][gc,init] Periodic GC: Disabled
.....
openjdk version "23.0.1" 2024-10-15
OpenJDK Runtime Environment (build 23.0.1+11-39)
....
```

1. 每个region 的大小，
2. 多少个

### 5, `stop-the-word`，调优参数`jstack` `jmap` 工具等

