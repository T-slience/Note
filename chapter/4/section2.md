# 1.搭建Python自带静态Web服务器
- 1.静态Web服务器
     - 可以为发出请求的浏览器提供静态文档的程序
     - 静态: 数据不会发生变化
- 2.搭建Python自带的静态Web服务器
     - 1.进入自己指定静态文件的目录
     - 2.在目录内打开终端
     - 3.执行指令
          - python -m http.server 端口号
          - -m 表示运行包里面的模块
     - 4.在浏览器输入
          - http://127.0.0.1:端口号/文件名
          - http://localhost:端口号/文件名
          - 可以访问对应的html文件,也可以对压缩文件进行下载
          - 端口号如果不指定,则默认是8000

# 2.静态Web服务器 -- 返回固定页面数据

- 1.步骤分析
     - HTTP协议是基于TCP协议的,可以通过socket 来建立服务器
     - 获取浏览器发送的HTTP 请求报文数据
     - 读取固定页面数据,把数据拼接成HTTP响应报文数据,再发送给浏览器
     - 发送完成以后 ,关闭客户端的套接字

- 2.示例
        ```
     import socket
   
     if __name__ == '__main__':
         # 创建tcp服务端套接字
         tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
         # 设置端口号复用, 程序退出端口立即释放
         tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
         # 绑定端口号
         tcp_server_socket.bind(("", 9000))
         # 设置监听
         tcp_server_socket.listen(128)
         while True:
             # 等待接受客户端的连接请求
             new_socket, ip_port = tcp_server_socket.accept()
             # 代码执行到此，说明连接建立成功
             recv_client_data = new_socket.recv(4096)
             # 对二进制数据进行解码
             recv_client_content = recv_client_data.decode("utf-8")
             print(recv_client_content)
     
             with open("static/index.html", "rb") as file:
                 # 读取文件数据
                 file_data = file.read()
        
             # 响应行
             response_line = "HTTP/1.1 200 OK\r\n"
             # 响应头
             response_header = "Server: PWS1.0\r\n"
     
             # 响应体
             response_body = file_data
     
             # 拼接响应报文
             response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
             # 发送数据
             new_socket.send(response_data)
     
             # 关闭服务与客户端的套接字
             new_socket.close()
          ```
          
# 3.静态Web服务器 -- 返回指定页面数据
- 1,步骤分析
     - 获取用户请求资源的路径
          - 根据浏览器发送的请求报文,可以知道在请求资源路径在请求行
          - 拿到路径比较好的方式:可以通过对请求报文分割,通过生成后的列表的下标得到路径
     - 根据路径,读取指定文件的数据
     - 拼接指定文件数据的响应报文,发送给浏览器
     - 如果没有指定文件,则默认输出固定页面
     - 判断请求的文件是否存在于服务端,不存在则组装404状态的响应报文发送给浏览器
     - 如果接收数据长度为0 ,可以认为服务端已断开连接,则可以关闭套接字


- 2.示例

```
import socket

def main():
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用, 程序退出端口立即释放
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
    # 绑定端口号
    tcp_server_socket.bind(("", 9000))
    # 设置监听
    tcp_server_socket.listen(128)
    while True:
        # 等待接受客户端的连接请求
        new_socket, ip_port = tcp_server_socket.accept()
        # 代码执行到此，说明连接建立成功
        recv_client_data = new_socket.recv(4096)
        if len(recv_client_data) == 0:
            print("关闭浏览器了")
            new_socket.close()
            return

        # 对二进制数据进行解码
        recv_client_content = recv_client_data.decode("utf-8")
        print(recv_client_content)
        # 根据指定字符串进行分割， 最大分割次数指定2
        request_list = recv_client_content.split(" ", maxsplit=2)

        # 获取请求资源路径
        request_path = request_list[1]
        print(request_path)

        # 判断请求的是否是根目录，如果条件成立，指定首页数据返回
        if request_path == "/":
            request_path = "/index.html"

        try:
            # 动态打开指定文件
            with open("static" + request_path, "rb") as file:
                # 读取文件数据
                file_data = file.read()
        except Exception as e:
            # 请求资源不存在，返回404数据
            # 响应行
            response_line = "HTTP/1.1 404 Not Found\r\n"
            # 响应头
            response_header = "Server: PWS1.0\r\n"
            with open("static/error.html", "rb") as file:
                file_data = file.read()
            # 响应体
            response_body = file_data

            # 拼接响应报文
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            # 发送数据
            new_socket.send(response_data)
        else:
            # 响应行
            response_line = "HTTP/1.1 200 OK\r\n"
            # 响应头
            response_header = "Server: PWS1.0\r\n"

            # 响应体
            response_body = file_data

            # 拼接响应报文
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            # 发送数据
            new_socket.send(response_data)
        finally:
            # 关闭服务与客户端的套接字
            new_socket.close()
    
    if __name__ == '__main__':
        main()

```

- 对于以上代码,还有可以改进的地方
    - maxsplit= 可以不写,直接写数字即可代表最大分割次数
    - return 会直接终止TCP服务端,可以使用continue ,这样即跳过了本次循环,又可以继续等待连接
    - 可以将拼接响应报文放到 finally 之后,这样可以简化代码
    - 除了可以使用try,except以外,还可以导入OS模块,使用os.path.exists(文件路径) 判断文件是否存在


# 4.静态Web服务器 -- 多任务版

- 1.步骤分析
    - 之前搭建的静态Web服务器不支持多用户同时访问,实际开发应用中不切实际
    - 使用多线程比多进程更加节省内存资源
    - 当客户端和服务端成功建立连接时,创建子线程,让子线程专门处理客户端的请求,这样主线程就可以继续接受连接请求
    - 设置守护主线程,防止主线程无法退出


- 2.示例:

```
import socket
import threading

# 处理客户端的请求
def handle_client_request(new_socket):
    # 代码执行到此，说明连接建立成功
    recv_client_data = new_socket.recv(4096)
    if len(recv_client_data) == 0:
        print("关闭浏览器了")
        new_socket.close()
        return

    # 对二进制数据进行解码
    recv_client_content = recv_client_data.decode("utf-8")
    print(recv_client_content)
    # 根据指定字符串进行分割， 最大分割次数指定2
    request_list = recv_client_content.split(" ", maxsplit=2)

    # 获取请求资源路径
    request_path = request_list[1]
    print(request_path)

    # 判断请求的是否是根目录，如果条件成立，指定首页数据返回
    if request_path == "/":
        request_path = "/index.html"

    try:
        # 动态打开指定文件
        with open("static" + request_path, "rb") as file:
            # 读取文件数据
            file_data = file.read()
    except Exception as e:
        # 请求资源不存在，返回404数据
        # 响应行
        response_line = "HTTP/1.1 404 Not Found\r\n"
        # 响应头
        response_header = "Server: PWS1.0\r\n"
        with open("static/error.html", "rb") as file:
            file_data = file.read()
        # 响应体
        response_body = file_data

        # 拼接响应报文
        response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
        # 发送数据
        new_socket.send(response_data)
    else:
        # 响应行
        response_line = "HTTP/1.1 200 OK\r\n"
        # 响应头
        response_header = "Server: PWS1.0\r\n"

        # 响应体
        response_body = file_data

        # 拼接响应报文
        response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
        # 发送数据
        new_socket.send(response_data)
    finally:
        # 关闭服务与客户端的套接字
        new_socket.close()

 # 程序入口函数
def main():
    # 创建tcp服务端套接字
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # 设置端口号复用, 程序退出端口立即释放
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
    # 绑定端口号
    tcp_server_socket.bind(("", 9000))
    # 设置监听
    tcp_server_socket.listen(128)

    while True:
        # 等待接受客户端的连接请求
        new_socket, ip_port = tcp_server_socket.accept()
        print(ip_port)
        # 当客户端和服务器建立连接程，创建子线程
        sub_thread = threading.Thread(target=handle_client_request, args=(new_socket,))
        # 设置守护主线程
        sub_thread.setDaemon(True)
        # 启动子线程执行对应的任务
        sub_thread.start()


if __name__ == '__main__':
    main()
```

- 相较于之前的返回指定页面,只是多了子线程的创建,守护主线程及调用.


# 5.静态Web服务器 -- 面向对象开发

- 1.步骤分析
    - 把提供服务的Web服务器抽象成一个类(HTTPWebServer)
    - 提供Web服务器的初始化方法(init),在里面创建socket对象
    - 提供一个开启Web服务器的方法,让Web服务器处理客户端请求操作
    - 将新套接字后的接收写成静态方法,因为它不需要用到self 和 class 
    


- 2.示例
    ```
    import socket
    import threading
    
    
    # 定义web服务器类
    class HttpWebServer(object):
        def __init__(self):
            # 创建tcp服务端套接字
            tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            # 设置端口号复用, 程序退出端口立即释放
            tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
            # 绑定端口号
            tcp_server_socket.bind(("", 9000))
            # 设置监听
            tcp_server_socket.listen(128)
            # 保存创建成功的服务器套接字
            self.tcp_server_socket = tcp_server_socket
    
        # 处理客户端的请求
        @staticmethod
        def handle_client_request(new_socket):
            # 代码执行到此，说明连接建立成功
            recv_client_data = new_socket.recv(4096)
            if len(recv_client_data) == 0:
                print("关闭浏览器了")
                new_socket.close()
                return
    
            # 对二进制数据进行解码
            recv_client_content = recv_client_data.decode("utf-8")
            print(recv_client_content)
            # 根据指定字符串进行分割， 最大分割次数指定2
            request_list = recv_client_content.split(" ", maxsplit=2)
    
            # 获取请求资源路径
            request_path = request_list[1]
            print(request_path)
    
            # 判断请求的是否是根目录，如果条件成立，指定首页数据返回
            if request_path == "/":
                request_path = "/index.html"
    
            try:
                # 动态打开指定文件
                with open("static" + request_path, "rb") as file:
                    # 读取文件数据
                    file_data = file.read()
            except Exception as e:
                # 请求资源不存在，返回404数据
                # 响应行
                response_line = "HTTP/1.1 404 Not Found\r\n"
                # 响应头
                response_header = "Server: PWS1.0\r\n"
                with open("static/error.html", "rb") as file:
                    file_data = file.read()
                # 响应体
                response_body = file_data
    
                # 拼接响应报文
                response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
                # 发送数据
                new_socket.send(response_data)
            else:
                # 响应行
                response_line = "HTTP/1.1 200 OK\r\n"
                # 响应头
                response_header = "Server: PWS1.0\r\n"
    
                # 响应体
                response_body = file_data
    
                # 拼接响应报文
                response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
                # 发送数据
                new_socket.send(response_data)
            finally:
                # 关闭服务与客户端的套接字
                new_socket.close()
    
        # 启动web服务器进行工作
        def start(self):
            while True:
                # 等待接受客户端的连接请求
                new_socket, ip_port = self.tcp_server_socket.accept()
                # 当客户端和服务器建立连接程，创建子线程
                sub_thread = threading.Thread(target=self.handle_client_request, args=(new_socket,))
                # 设置守护主线程
                sub_thread.setDaemon(True)
                # 启动子线程执行对应的任务
                sub_thread.start()
    
    
    # 程序入口函数
    def main():
        # 创建web服务器对象
        web_server = HttpWebServer()
        # 启动web服务器进行工作
        web_server.start()
    
    
    if __name__ == '__main__':
        main()
    
    ```





































































