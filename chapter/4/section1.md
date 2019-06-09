# 1.HTTP协议
- 1.介绍
    - HTTP(HyperText Transfer Protocol), 超文本传输协议
    - 超文本是超级文本的缩写指赵越文本限制或者超链接,如图片,音乐,视频,超链接等,都属于超文本
    - 制作者为 蒂姆·伯纳斯·李,1991年设计出来
    - HTTP协议设计之前目的是传输网页数据的(html),现在可以传输任意类型的数据
    - 传输HTTP协议格式的数据是基于TCP传输协议的,发送数据前需要先建立连接


-  2.HTTP协议的作用
    - 它规定了浏览器和 Web 服务器 通信数据的格式, 也就是还说浏览器和 Web服务器通信需要使用HTTP协议


- 3.浏览器访问 Web 服务器的通信过程

    ![](/assets/访问web服务器的通信过程.png)


# 2.URL的概念
- 1.概念
    - URL(Uniform Resoure Locator), 统一资源定位符,
    可以理解为网络资源地址,也就是常说的网址
- 2.组成
    - 示例:
        - https://news.163.com/hello.html?page=1&count=10

    - 组成:
        - 1.协议部分: https:// ,http:// , ftp://
        - 2.域名部分: news.163.com
        - 3.资源路径部分: hello.html
        - 4.查询参数部分(可选): ?page=1&count=10
            -  ? 后面表示第一个参数,后面的参数用 & 进行连接 
    - 域名:
        - 就是ip地址的别名,它是用点进行分割使用英文字母和数字组成的名字，使用域名目的就是方便的记住某台主机IP地址。
        - 通过DNS服务器,会将域名解析成对应的ip地址,在建立连接
  
     
    

# 3.查看HTTP协议的通信过程
- 1.如何查看
    - 1.安装浏览器(以Google Chrome为例)
    - 2.windows 和Linux 可以按F12 调出开发者工具, mac OS 选择 视图 -> 开发者->开发者工具 或者使用alt + command + i 
    - 3.多平台通用操作 ,网页 上右键选择 检查

- 2.开发者工具介绍
        ![](/assets/通信过程-2.png)

    - 元素（Elements）：用于查看或修改HTML标签
    - 控制台（Console）：执行js代码
    - 源代码（Sources）：查看静态资源文件，断点调试JS代码
    - 网络（Network）：查看http协议的通信过程
    
    ![](/assets/通信过程-3.png)
    
    - 开发者工具的Headers选项总共有三部分组成:
       - General: 主要信息
       - Response Headers: 响应头
       - Request Headers: 请求头
    - Response 选项是查看响应体信息的

# 4.HTTP 请求报文
- ### 1.常见的HTTP请求报文
    - GET  获取web服务器数据
    - POST    向web服务器提交数据(一般用于登录)

- ### 2.示例
    - 1.GET
        ```
        ---- 请求行 ----
        GET / HTTP/1.1  # GET请求方式 请求资源路径 HTTP协议版本
        ---- 请求头 -----
        Host: www.itcast.cn  # 服务器的主机地址和端口号,默认是80
        Connection: keep-alive # 和服务端保持长连接
        Upgrade-Insecure-Requests: 1 # 让浏览器升级不安全请求，使用https请求 即:使用安全的请求
        User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36  # 用户代理，也就是客户端的名称
        Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8 # 可接受的数据类型
        Accept-Encoding: gzip, deflate # 可接受的压缩格式
        Accept-Language: zh-CN,zh;q=0.9 #可接受的语言
        Cookie: pgv_pvi=1246921728; # 登录用户的身份标识
        
        ---- 空行 ----
        ```
        - ### 长连接: 连接建立成功以后，可以发送多次请求和多次响应
        - ### 短连接: 连接建立成功以后，能够发送一次请求和一次响应
    
    - 2.POST
        ```
        ---- 请求行 ----
        POST /xmweb?host=mail.itcast.cn&_t=1542884567319 HTTP/1.1 # POST请求方式 请求资源路径 HTTP协议版本
        ---- 请求头 ----
        Host: mail.itcast.cn # 服务器的主机地址和端口号,默认是80
        Connection: keep-alive # 和服务端保持长连接
        Content-Type: application/x-www-form-urlencoded  # 告诉服务端请求的数据类型
        User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36 # 客户端的名称
        ---- 空行 ----
        ---- 请求体 ----
        username=hello&pass=hello # 请求参数
        ``` 
        - 请求体位于 NETWORK 里的 FORM DATA
        - POST 可以没有请求体, 即等同于GET,很少出现此情况,因为这种情况都会使用GET
- ### 3.小结
    - #### 1.请求行由 请求方式, 请求资源路径 , HTTP协议版本 三部分组成
    - #### 2.在每项数据之间都必须加上: \r\n
    - #### 3.请求报文结构:
    
    ![](/assets/get和post请求报文.png)
        

# 5.HTTP 响应报文
- 1.HTTP 状态码介绍
   
        状态码|说明
        ---|---
        200|请求成功
        307|重定向
        400|错误的请求,请求地址或者参数有误
        404|请求资源在服务器不存在
        500|服务器内部源代码出现错误

- 2.示例
    ```
    --- 响应行/状态行 ---
    HTTP/1.1 200 OK # HTTP协议版本 状态码 状态描述
    --- 响应头 ---
    Server: Tengine # 服务器名称
    Content-Type: text/html; charset=UTF-8 # 内容类型
    Transfer-Encoding: chunked # 发送给客户端内容不确定内容长度，发送结束的标记是0\r\n, Content-Length表示服务端确定发送给客户端的内容大小，但是二者只能用其一。
    Connection: keep-alive # 和客户端保持长连接
    Date: Fri, 23 Nov 2018 02:01:05 GMT # 服务端的响应时间
    --- 空行 ---
    --- 响应体 ---
    <!DOCTYPE html><html lang=“en”> …</html> # 响应给客户端的数据
    ```
    

- 3.小结
    - 1.响应行由 HTTP协议版本 , 状态码, 状态描述,三部分组成 (状态描述可以省略)
    - 2.一个HTTP响应报文 由 响应行,响应头,空行,响应体四个部分组成
    - 3.每项数据也需要用 \r\n 分割
    




































