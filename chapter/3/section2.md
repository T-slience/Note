# 1.TCP客户端开发
```
import socket

if __name__ == '__main__':
    # 1.创建tcp客户端套接字
    # 1.1. AF_INET：表示ipv4
    # 1.2. SOCK_STREAM: tcp传输协议
    tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 2.和服务端应用程序建立连接
    tcp_client_socket.connect(("192.168.131.62", 8080))
    # 代码执行到此，说明连接建立成功
    # 3.1准备发送的数据,并进行转码
    send_data = "你好服务端，我是客户端小黑!".encode("gbk")
    # 3.2 发送数据
    tcp_client_socket.send(send_data)
    # 4.1接收数据, 这次接收的数据最大字节数是1024
    recv_data = tcp_client_socket.recv(1024)
    # 返回的直接是服务端程序发送的二进制数据
    print(recv_data)
    # 4.2对数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收服务端的数据为:", recv_content)
    # 5.关闭套接字
    tcp_client_socket.close()
```
- 导入socket模块
    - import socket
- 创建客户端socket对象
    - socket.socket(AddressFamily,Type)
    - AddressFamily 表示IP地址类型,
        - 分为IPv4 AF_INET  和 IPv6  AF_INET6
    - Type 表示传输协议类型
        - SOCK_STREAM TCP传输协议
        - SOCK_DGRAM  UDP传输协议
- 方法说明
    - connect((host,port)) 表示和服务端套接字简历连接
        - host 是服务器ip地址,port 是应用程序的端口号 

# 2.TCP服务端开发



```
import socket

if __name__ == '__main__':
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用，让程序退出端口号立即释放
        # 1.SOL_SOCKET 选择该套接字的选项  
        # 2.SO_REUSEADDR 选择端口复用功能
        # 3.True 启动
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True) 
    # 给程序绑定端口号 
    tcp_server_socket.bind(("", 8989))
    # 设置监听
    # 128:最大等待建立连接的个数， 提示： 目前是单任务的服务端，同一时刻只能服务与一个客户端，后续使用多任务能够让服务端同时服务与多个客户端，
    # 不需要让客户端进行等待建立连接
    # listen后的这个套接字只负责接收客户端连接请求，不能收发消息，收发消息使用返回的这个新套接字来完成
    tcp_server_socket.listen(128)
    
    # 等待客户端建立连接的请求, 只有客户端和服务端建立连接成功代码才会解阻塞，代码才能继续往下执行
    # 1. 专门和客户端通信的套接字： service_client_socket
    # 2. 客户端的ip地址和端口号： ip_port
    service_client_socket, ip_port = tcp_server_socket.accept()
    # 代码执行到此说明连接建立成功
    print("客户端的ip地址和端口号:", ip_port)
    # 接收客户端发送的数据, 这次接收数据的最大字节数是1024
    recv_data = service_client_socket.recv(1024)
    # 获取数据的长度
    recv_data_length = len(recv_data)
    print("接收数据的长度为:", recv_data_length)
    # 对二进制数据进行解码
    recv_content = recv_data.decode("gbk")
    print("接收客户端的数据为:", recv_content)
    # 准备发送的数据
    send_data = "ok, 问题正在处理中...".encode("gbk")
    # 发送数据给客户端
    service_client_socket.send(send_data)
    # 关闭服务与客户端的套接字， 终止和客户端通信的服务
    service_client_socket.close()
    # 关闭服务端的套接字, 终止和客户端提供建立连接请求的服务
    tcp_server_socket.close()
```

- 为什么要设置端口复用
    - 当客户端和服务端建立连接后，服务端程序退出后端口号不会立即释放，需要等待大概1-2分钟。
    - 修改服务器端口也可以让客户端重新连接,但是不推荐(服务器端口一般不会改动)
    - 端口复用格式:
        ```
        # 参数1: 表示当前套接字
        # 参数2: 设置端口号复用选项
        # 参数3: 设置端口号复用选项对应的值
        tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
       
         ```
         
         
- 说明
         
    - 1.绑定端口号 "bind"
        - 参数:元组类型, (ip地址,端口号)
        - ip地址一般不指定,表示本服务端任意一个IP均可(针对多网卡的服务器)
    - 2.设置监听 "listen"
        - 参数: 最大等待建立连接数
        - 即 已经建立了连接,后面的客户端还可以建立,不过需要等待,直至之前的连接断开
    - 3.等待接受客户端的连接请求--"accept"
        - accept 会产生一个新的套接字,针对客户端和服务端的收发消息
        - 需要用套接字和ip的元组对其进行拆包
    - 4.发送数据 "send"
        - 参数: 要发送的二进制数据， 注意: 字符串需要使用encode()方法进行编码
    - 5.接收数据  ‘recv’
        - 参数: 表示每次接收数据的大小，单位是字节，注意: 解码成字符串使用decode()方法
    - 6.关闭新的套接字表示通信完成
        - 服务器套接字一般不关闭,表示还可以让客户端再次连接
    - 7.编码格式
        - Linux 系统 的编码格式为 "utf-8"(国际使用,优先推荐,不行再更换其他)
        - windows 编码格式为 "gbk"(国内使用)
        - java 相关的编码格式  "gb2312"


# 3.多任务版TCP服务端程序开发
- 1.需求
    - 可以同时连接多个客户端,并且多次进行收发数据通信
- 2.解析
    - 可以选择使用多线程或者多进程来完成连接多个客户端的需求
    - 此时需要在accept前建立一个死循环,表示可以不断接受新的连接需求
    - 因为要完成多任务,可以把收发数据单独写成一个任务函数,然后用子进程或子线程来处理,并且以死循环的方式完成多次收发的需求
    - 可以通过返回数据长度看客户端是否断开连接
    - 因为多线程更加节约资源,此次采用多线程.
   
```
        import socket
        import threading
    
        
        # 处理客户端的请求操作
        def handle_client_request(service_client_socket, ip_port):
            # 循环接收客户端发送的数据
            while True:
                # 接收客户端发送的数据
                recv_data = service_client_socket.recv(1024)
                # 容器类型判断是否有数据可以直接使用if语句进行判断，如果容器类型里面有数据表示条件成立，否则条件失败
                # 容器类型: 列表、字典、元组、字符串、set、range、二进制数据
                if recv_data:
                    print(recv_data.decode("gbk"), ip_port)
                    # 回复
                    service_client_socket.send("ok，问题正在处理中...".encode("gbk"))
        
                else:
                    print("客户端下线了:", ip_port)
                    break
            # 终止和客户端进行通信
            service_client_socket.close()
        
        
        if __name__ == '__main__':
            # 创建tcp服务端套接字
            tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            # 设置端口号复用，让程序退出端口号立即释放
            tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
            # 绑定端口号
            tcp_server_socket.bind(("", 9090))
            # 设置监听, listen后的套接字是被动套接字，只负责接收客户端的连接请求
            tcp_server_socket.listen(128)
            # 循环等待接收客户端的连接请求
            while True:
                # 等待接收客户端的连接请求
                service_client_socket, ip_port = tcp_server_socket.accept()
                print("客户端连接成功:", ip_port)
                # 当客户端和服务端建立连接成功以后，需要创建一个子线程，不同子线程负责接收不同客户端的消息
                sub_thread = threading.Thread(target=handle_client_request, args=(service_client_socket, ip_port))
                # 设置守护主线程
                sub_thread.setDaemon(True)
                # 启动子线程
                sub_thread.start()
        
        
            # tcp服务端套接字可以不需要关闭，因为服务端程序需要一直运行
            # tcp_server_socket.close()
            ```




# 4.TCP网络应用程序的注意点
- 1.当 TCP 客户端程序想要和 TCP 服务端程序进行通信的时候必须要先建立连接
- 2.TCP 客户端程序一般不需要绑定端口号，因为客户端是主动发起建立连接的。
- 3.TCP 服务端程序必须绑定端口号，否则客户端找不到这个 TCP 服务端程序。
- 4.listen 后的套接字是被动套接字，只负责接收新的客户端的连接请求，不能收发消息。
当 TCP 客户端程序和 TCP 服务端程序连接成功后， TCP 服务器端程序会产生一个新的套接字，收发客户端消息使用该套接字。
- 5.关闭 accept 返回的套接字意味着和这个客前户端已经通信完毕。
- 6.关闭 listen 的套接字意味着服务端的套接字关闭了，会导致新的客户端不能连接服务端，但是之前已经接成功的客户端还能正常通信。
- 7.当客户端的套接字调用 close 后，服务器端的 recv 会解阻塞，返回的数据长度为0，服务端可以通过返回数据的长度来判断客户端是否已经下线，反之服务端关闭套接字，客户端的 recv 也会解阻塞，返回的数据长度也为0。
    















