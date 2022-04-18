### 可重入锁

又名递归锁

指在同一个线程在外层方法获取锁的时候，再进入该线程的内层方法会自动获取锁（前提，锁对象得是同一个对象），不会因为之前已经获取过还没释放锁而阻塞

Java中ReentrantLock和synchronized都是可重入锁，优点是可一定程度避免死锁



可重入锁：可重复可递归调用的锁，在外层使用锁之后，在内层仍然可以使用，并且不发生死锁

在一个synchronized修饰的方法或代码块的内部

调用本类的其他synchronized修饰的方法或代码块时，是永远可以得到锁的



<font color='red'>每个锁对象拥有一个锁计数器和一个指向持有该锁的线程的指针</font>

当执行monitorenter时，如果目标锁对象的计数器为零，那么说明它没有被其他线程所持有，Java虚拟机会将该对象的持有线程设置为当前线程，并且将其计数器加1

在目标锁对象的计数器不为0的情况上，如果锁对象的持有线程是当前线程，那么Java虚拟机可以将其计数器加1，否则需要等待，直至持有线程释放该锁

当执行monitorexit时，Java虚拟机则需将锁对象的计数器减1，计数器为0代表锁已被释放



### LockSupport

线程等待唤醒机制(wait/notify)改良、加强

LockSupport中的park()和unpark()的作用分别是阻塞线程和解除阻塞线程

三种线程等待唤醒的方式

1. 使用Object中的wait()方法让线程等待，使用Object中的notify()方法唤醒线程
2. 使用JUC包中Condition的await()方法让线程等待，使用signal()方法唤醒线程
3. LockSupport的park()和unpark()方法



1方式wait和notify方法必须要放在同步代码块，必须要先wait后notify

2方式类似



LockSupport是用来创建锁和其他同步类的基本线程阻塞原语

使用了一种名为Permit的概念来做到阻塞和唤醒线程地功能，<font color='red'>每个线程都有一个permit</font>

permit只有两个值，1和0，默认是0

可以把许可看成是一种(0,1)信号量，但与信号量不同的是，许可的累加上限是1

![image-20220208204658994](C:\Users\46305\AppData\Roaming\Typora\typora-user-images\image-20220208204658994.png)

![image-20220208204801499](C:\Users\46305\AppData\Roaming\Typora\typora-user-images\image-20220208204801499.png)





### AQS

抽象队列同步器

用来构建锁或者其他同步器组件的<font color='red'>重量级基础框架及整个JUC体系的基石</font>，通过内置的FIFO队列来完成资源获取线程地排队工作，并通过一个int类型的变量表示持有锁的状态



锁：面向锁的<font color='red'>使用者</font>，定义了程序员和锁交互的使用层API，隐藏实现细节，调用即可

同步器：面向锁的<font color='red'>实现者</font>，统一规范并简化了锁的实现，屏蔽了同步状态管理、阻塞线程排队和通知、唤醒机制等

![image-20220208214532669](C:\Users\46305\AppData\Roaming\Typora\typora-user-images\image-20220208214532669.png)

<font color='red'>AQS使用一个volatile的int类型的成员变量来表示同步状态，通过内置的FIFO队列来完成资源获取的排队工作，将每条要去抢占资源的线程封装成一个Node节点来实现锁的分配，通过CAS完成对State值的修改</font>

