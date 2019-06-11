# 1.线程
- 1.概念
    -  线程是进程中执行代码的一个分支，每个执行分支（线程）要想工作执行代码需要cpu进行调度 
    -  也就是说线程是cpu调度的基本单位，每个进程至少都有一个线程，而这个线程就是我们通常说的主线程。
 
- 2.多线程的使用
    - 2.1 导入线程模块
        - import threading
    - 2.2 格式
        ```
        1.导入线程模块
        import threading
        2.创建子线程
        sub_thread = threading.Thread(target = 任务名)
        3.线程启动
        sub_thread.start()
        ```
    - 2.3 线程类Thread 的参数说明
        - Thread([group [, target [, name [, args [, kwargs]]]]])
            - 和进程类相同
            - group: 线程组，目前只能使用None
            - target: 执行的目标任务名
            - args: 以元组的方式给执行任务传参
            - kwargs: 以字典方式给执行任务传参
            - name: 线程名，一般不用设置
- 3.线程执行带有参数的任务
    - 元组方式传参
        -  sub_thread = multiprocessing.Process(target=task, args=(5,))
        -  需要和任务的参数名顺序一一对应,或者使用关键字参数
    - 字典方式传参 
        - sub_thread = multiprocessing.Process(target=task, kwargs={"count": 3})
        - key值必须和任务的参数名相同

# 2.线程的注意点
- 1.线程之间的执行是无序的
    - 由cpu调度决定,即多个线程执行的先后顺序是不确定的,这一点同样适用于进程
- 2.主线程会等待所有的子线程执行结束后再结束
    - 和进程相同
    - 可以用守护主线程
        -  sub_thread = threading.Thread(target=show_info, daemon=True)
        -  sub_thread.setDaemon(True)
        -  以上两种效果等同
    - 线程没有进程里的子进程销毁命令
- 3.线程之间共享全局变量
    - 线程都在进程当中,所以能共享全局变量
    - 而子进程是主进程的复制,在子进程里修改它的全局变量不会影响到其他子进程以及主进程的全局变量
- 4.线程之间共享全局变量数据出现错误问题
    
```
    import threading

    # 定义全局变量
    g_num = 0
    
    
    # 循环1000000次每次给全局变量加1
    def sum_num1():
        for i in range(1000000):
            global g_num
            g_num += 1
    
        print("sum1:", g_num)
    
    
    # 循环1000000次每次给全局变量加1
    def sum_num2():
        for i in range(1000000):
            global g_num
            g_num += 1
        print("sum2:", g_num)
    
    
    if __name__ == '__main__':
        # 创建两个线程
        first_thread = threading.Thread(target=sum_num1)
        second_thread = threading.Thread(target=sum_num2)
    
        # 启动线程
        first_thread.start()

        # 启动线程
        second_thread.start()
    ```
    ```
    sum1: 1210949
    sum2: 1496035
    ```
    - 因为在两个子线程对全局变量取值并运算的同时,可能存在重复的情况
        - 即A线程取值时,B线程同时取值,导致两个线程完成单次任务时,其实只对全局变量进行了一次修改
    - 为了避免此类情况,可以使用线程同步:
        - 线程等待和互斥锁都能保证数据的安全性，数据不会出错
        - 使用线程同步让多任务改成了单任务执行,会降低执行效率
        - 线程等待
            - join
            - 在进程中也同样适用
            - 在有多个线程时,如果希望同时只有一个线程工作, 可以依次使用join 来让线程单独有序的运行
        - 互斥锁
            - 互斥锁的作用就是保证同一时刻只能有一个线程去操作共享数据，保证共享数据不会出现错误问题
            - 使用互斥锁的好处确保某段关键代码只能由一个线程从头到尾完整地去执行
            
            - 加上互斥锁那个线程先执行我们决定不了，但是能够保证数据不会出错
            - threading模块中定义了Lock 变量,其实质是一个函数.通过该函数可以创建锁
            - 格式
                ```
                # 1.创建锁
                变量名 = threading.Lock()
                # 2.上锁
                变量名.acquire()
                ...这里编写代码能保证同一时刻只能有一个线程去操作, 对共享数据进行锁定...
                
                # 3.解锁
                变量名.release()
                
                ```
                - acquire , release没有自动提示
                - 如果在调用acquire方法的时候 其他线程已经使用了这个互斥锁，那么此时acquire方法会堵塞，直到这个互斥锁释放后才能再次上锁。
            - 死锁
                - 当互斥锁没有合理的释放时,会导致死锁的情况
                - 即此时程序仍在运行,但是停止了相应,不能处理其他任务(后续代码)
                - 在实际运用和开发中,使用互斥锁时,要尽量避免死锁的情况
                    - 死锁不同于死循环,没有可以正面使用的方式

# 3.进程与线程对比
    - 1.关系对比
        - 线程时依附在进程里面的,没有进程就没有线程
        - 一个进程默认提供一条线程,进程可以创建多个线程
    
    - 2.区别对比
        - 全局变量
            - 进程之间不共享全局变量
            - 线程之间共享全局变量,但是注意资源竞争的问题
                - 可以通过线程同步的方式解决
                - 即线程等待或者互斥锁
        - 资源分配
            - 创建进程的资源开销要比创建线程的资源开销大
            - 进程是 操作系统 资源分配的基本单位
            - 线程时 CPU 调度的基本单位
        - 关系
            - 线程不能够够独立执行,必须一寸在进程中
            - 一个进程中至少有一个线程
        - 稳定性
            - 多进程开发比单进程多线程开发稳定性要强
            - 某个进程挂掉不影响其他进程,但是其所有线程会同样挂掉
    - 3.优缺点对比
        - 进程
            - 优点:可以利用cpu多核
            - 缺点:资源开销大
            - 计算相关的程序需要利用多核 进程
        - 线程
            - 优点:资源开销小
            - 缺点:只能使用单核
            - 文件读写,下载,网络编程一般都会用单核 线程
    - 4.Summary
        - 进程和线程都是完成多任务的一种方式
        - 根据对资源,稳定性,是否需要共享全局变量
            - 来决定使用什么方式完成多任务
        - 线程把指定任务执行完成以后,会自动销毁

# 4.Extra
    - 关于全局变量
        - 对于一个可变类型的全局变量
            - 如果是增,删,改
                - 不需要使用global
            - 如果是重新赋值
                - 需要使用global 
        - 对于一个不可变类型的全局变量
            - 进行任意更改操作
                - 都需要使用global
    - global 实际是声明 更改全局变量的内存地址
        - 即 如果内存地址没有更改,就不需要使用
    - 递归锁(RLock)
        - 很少使用
        - 即锁上加锁,打开了第一层锁才能打开第二层并执行其内部代码
        
    ```
        from threading import RLock

        lock = RLock()
        lock.acquire()
        lock.acquire()
        print(123)
        lock.release()
        print(456)
        lock.release()

        ```
    - 线程池
        - 创建一个线程池并在其中创建多个子线程
        - 在需要执行任务的时候,调用线程池的子线程执行,若此时线程池没有空闲子线程则等待
        - 子线程在执行完任务后,不会销毁,而是回到线程池
       
         ```
    import time
    from concurrent.futures import ThreadPoolExecutor,ProcessPoolExecutor
    def func(num):
            print(num)
            time.sleep(1)
        print(num)
    if __name__ == '__main__':
            t=ThreadPoolExecutor(20) #20个线程，每次处理20个
            for i in range(50):
            t.submit(func,i) #异步提交命令
        t.shutdown()#同join整个线程池
        print('done')

         ```