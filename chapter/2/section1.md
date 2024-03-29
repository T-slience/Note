# 1.编辑器 vim
- 1.工作模式
    - 命令模式
        - 开启vim时默认为命令模式, i,o,a,s均可 进入编辑模式 , :冒号进入末行模式 
    - 编辑模式
        - 可以对文本进行编辑 
    - 末行模式
        - 可以输入指令 

- 2.末行模式指令
    - 2.1 保存和退出
        - 若是vim 文件名 则会创建或者打开该文件并进入vim
        - 若是vim 则会直接打开vim ,但是在退出保存时,需要在wq后添加文件名,默认保存在当前路径,可以通过绝对或相对路径更改
            - w 保存
            - wq 保存退出
            - x 保存退出 (等同于 wq)
            - q 退出 (当没有保存时,无法退出)
            - q! 强制退出 (新编辑的信息不会保存)
        
    - 2.2 常用命令
        - 重点:
            - yy  复制光标所在行
            - p     粘贴
            - dd    剪切(变相类似于删除)
            - V     按行选中(可以搭配复制y 进行多行复制)
            - u     撤销
        - 次要:
            - Ctrl+r   (反撤销)
            - 数字+d 粘贴多次
            - .>> (向右缩进)
            - << (向左缩进
            - .    (重复上一次命令)
            - G    (回到最后一行)
            - gg    (回到第一行)
            - 数字+ G   (回到指定行)
            - :/搜索的内容      (搜索指定内容)
            - :%s/要替换的内容/替换后的内容/g   (全局替换)
            - :开始行数,结束行数s/要替换的内容/替换后的内容     (局部替换)
            - shift+6   (回到当前行行首)
            - shift+4   (回到当前行行末)
            - ctr+f     (下一屏)
            - ctr+b     (上一屏)



# 2.软件的安装及卸载
- 1.离线安装及卸载(Ubantu)
    - 命令
        - dpkg
            - 安装和卸载deb安装包
    - 选项
        - -i    安装(install) 
        - -r    卸载(remove)
    - 格式
        -  dpkg -选项  安装包名
        - 注意
            - 需要用绝对或相对路径指定安装包

- 2.在线安装及卸载(Ubantu)
    - 命令
        - sudo apt-get  
    - 选项
        - install   安装
        - remove    卸载
    - 格式
        - sudo apt-get 选项 安装包名
        - 注意
            - 使用该方式安装软件一定要联网 
    - 更换镜像源
        - 在线安装默认是从国外服务器下载安装,会导致下载速度很慢,所以要更换为国内的镜像源服务器
        - 1.可视化方式更改镜像源
            - 打开ubuntu的设置
            - 打开软件和更新
            - 在 "下载自" 选择其他站点
            - 选择相应的国内的镜像源服务器
            - 输入当前用户密码进行授权
            - 重新载入信息(需要联网)
        - 2.手动方式更改镜像源
            - 在网络上搜索进入相应的镜像源服务器网站
            - 选择使用的系统及版本(ubuntu)
            - 将信息复制
            - 打开 /etc/apt/sources.list 
            - 用网站上的信息对其进行覆盖(覆盖前可以将源文件先备份一份,避免出错)
            - 使用 sudo apt-get update 命令 ,保证可以下载最新的软件


# 3.多任务介绍
- 1.概念
    - 指在同一时间内执行多个任务
- 2.方式
    - 并发
        - 在一段时间内交替执行任务
        - 即 任务A执行0.1s 在执行任务B 0.1s,如此反复交替,因为cpu的执行速度快,可以当做任务在同时执行,单核CPU就是这样执行多任务的
    - 并发
        - 真正的同时执行多任务
        - 多核CPU,让每个核心分别执行一个任务,完成多任务的执行
    - 在python中,实现多任务 可以使用多进程or多线程来完成.

# 4.进程

- 1.概念
    - 一个运行的程序或者软件就是一个进程,它是操作系统进行资源分配的基本单位
    - 即每启动一个进程,操作系统都会给其分配一定的运行资源(内存资源)保证进程的运行
        - 比如:现实生活中的公司可以理解成是一个进程，公司提供办公资源(电脑、办公桌椅等)，真正干活的是员工，员工可以理解成线程。
        - 进程只负责索要资源,不负责执行代码,真正执行代码的是线程
- 2.作用 
    - 向系统索要资源,并以线程执行程序
    - 一个程序可以有多个进程
        - 即多进程 完成多任务


- 3.注意
    - 一个程序运行后至少有一个进程，一个进程默认有一个线程
    - 进程里面可以创建多个线程，线程是依附在进程里面的，没有进程就没有线程。
    
# 5.多进程的使用
- 1.导入进程包
    - import multiprocessing
    - 注意:
        - 包和模块的区别
            - ctrl+左键 打开的是 init.py,代表是包
            - 打开的是 名称.py,代表是模块

- 2.Process 进程类说明
    - Process([group [, target [, name [, args [, kwargs]]]]])
    - group：指定进程组，目前只能使用None
    - target：执行的目标任务名
    - name：进程名字(默认为Process-N，N为从1开始递增的整数,可以不用)
    - args：以元组方式给执行任务传参
    - kwargs：以字典方式给执行任务传参
- 3.Process创建的实例对象的常用方法:

    - start()：启动子进程实例（创建子进程）(线程等同)
    - join()：等待子进程执行结束(后面线程等待也是join)
    - terminate()：不管任务是否完成，立即终止子进程

- 4.使用步骤
  
    ```
    # 1. 导入进程包
    import multiprocessing
    # 2.创建子进程并指定执行的任务
    sub_process = multiprocessing.Process (target=任务名)
    # 3.启动进程执行任务
    sub_process.start()
    ```
- 5.获取进程编号
    - 通过获取进程编号,可以看到子进程和父进程的关系 
    - 需导入OS模块 
    - 1.获取当前进程编号
        - os.getpid() 
        - get process id
    - 2.获取当前父进程编号
        - os.getppid()
        - get parent process id

- 6.进程执行带有参数的任务
    - 元组方式传参
        -  sub_process = multiprocessing.Process(target=task, args=(5,))
        -  需要和任务的参数名顺序一一对应,或者使用关键字参数
    - 字典方式传参 
        - sub_process = multiprocessing.Process(target=task, kwargs={"count": 3})
        - key值必须和任务的参数名相同

- 7.进程的注意点
    - 进程之间不共享全局变量
        - 创建子进程会对主进程资源进行拷贝，也就是说子进程是主进程的一个副本，好比是一对双胞胎，之所以进程之间不共享全局变量，是因为操作的不是同一个进程里面的全局变量，只不过不同进程里面的全局变量名字相同而已。
    - 主进程会等待所有的子进程执行结束再结束
        -  如果需要主进程结束 子进程也停止的话可以采用
            - 主进程守护
                - 主进程退出子进程销毁不再执行
                -  sub_process.daemon = True
            - 子进程销毁
                - 子进程执行结束
                -  sub_process.terminate()

- 8.Extra
    - lsof -i:端口号
        - 可以查看使用该端口的是哪个进程,并显示其信息
    - kill pid
        - 通过pid 杀死该进程, 对于比较顽强的进程,可以在后面加上 -9 代表强杀 
        - 在pycharm中,可以使用os模块中的 os.kill(os.getpid(), 9) 进行对该进程强杀
            - 1,9都可以强杀 
            - 对于线程不适用