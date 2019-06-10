# 1.html
- 1.html定义
    - 使用标记标签来描述网页的一种标记语言(不是一种编程语言)
    - html(HyperText Mark-up Language),指 超文本标记语言
    - 标记语言是一套标记标签(markup tag)
    - 标记,就是标签,元素 如:<html></html>等
    - 超文本有两层含义
        - 因为网页中还可以有图片,视频,音频等内容
        - 还可以在网页中跳转到另一个网页(超链接文本)

- 2.html基本结构
    - 网页文件的后缀是 .htm 或者 .html
    - 可以用文本的方式编辑,但是没有专业工具便捷
    - 用浏览器打开,浏览器会按照相应标签将文件渲染成网页
    ```
    <!DOCTYPE html>
    <html>
    <head>            
        <meta charset="UTF-8">
        <title>网页标题</title>
    </head>
    <body>
          网页显示内容
    </body>
    </html>
    ```
    - 第一行<!DOCTYPE html>是文档声明, 用来指定页面所使用的html的版本, 这里声明的是一个html5的文档。
    - html.../html标签是开发人员在告诉浏览器，整个网页是从html这里开始的，到<html>结束,也就是html文档的开始和结束标签。
    - head.../head标签用于定义文档的头部,是负责对网页进行设置标题、编码格式以及引入css和js文件的。
    - body.../body标签是编写网页上显示的内容。
 
- 3.vscode 的基本使用
 - 1.介绍
         - vscode( Visual Studio Code) ,是由微软研发的一款免费的,开源的跨平台代码编辑器
         - 目前是前端(网页)开发使用最多的一款软件开发工具
 - 2.安装
        - https://code.visualstudio.com/Download       
        - vscode插件:
            - Chinese (Simplified) Language Pack for VS Code
            - open in browser
 - 3.快捷键
        - 注释: ctrl + /
        - 快速创建html文档基本结构: shift + 1 +  tab/enter
        - 插入多个光标: alt + 点击
        - 插入光标: ctrl + alt + up/down
        - 放大/缩小 :  ctrl + +/-
        - 显示/隐藏侧边栏 : ctrl + B

- 4.常用的html标签
    - 学习 html 语言就是学习标签的用法,常用的标签有20多个 
        - 标签不区分大小写,但推荐使用小写
        - 根据书写形式,分为双标签(闭合标签)和单标签(空标签)
            - 双标签:
                - 指由开始标签和结束标签组成的一对标签,这种标签允许嵌套和承载内容,如:div ,p 
            - 单标签:
                - 指由一个标签组成,没有标签内容,如:br ,hr ,img

    
            ```
            <!-- 1、成对出现的标签：-->
            
            <h1>h1标题</h1>
            # 从h1到h6一次变小,但是超出6也不会报错,会变成普通文字
            
            <div>这是一个div标签</div>
            
            <p>这个一个段落标签</p>
            # 段落标签
            
            <!-- 2、单个出现的标签： -->
            <br>
            # 换行符
            
            <img src="images/pic.jpg" alt="图片">
            # 如果图片存在则打开图片,不存在则输出alt的信息
           
             <hr>
            # 一条分隔线
            
            <!-- 3、带属性的标签，如src、alt 和 href等都是属性 -->
            <img src="images/pic.jpg" alt="图片">
            <a href="http://www.baidu.com">百度网</a>
            
            
            <!-- 4、标签的嵌套 -->
            <div>
                <img src="images/pic.jpg" alt="图片">
                <a href="http://www.baidu.com">百度网</a>
            </div>
                ```
    
            

- 5.资源路径
    - 当使用img标签,以及后面css 文件的调用时,都需要指定资源路径
    - 资源路径可以分为相对路径和绝对路径
        - 相对路径
            - 相对于当前操作html 文档所在目录算起的路径
        - 绝对路径
            - 从根目录或者盘符 算起的路径
    - 在html中一般都会使用相对路径,绝对路径的操作在其他电脑上打开会有可能出现资源文件找不到的问题
        

- 6.列表标签
    - 有序列表标签(ol标签)
        - 有序标签,代表在数据前面会有序号显示
    ```
    <!-- ol标签定义有序列表 -->
    <ol>
        <!-- li标签定义列表项目 -->
        <li><a href="#">列表标题一</a></li>
        <li><a href="#">列表标题二</a></li>
        <li><a href="#">列表标题三</a></li>
    </ol>
    ```
    
    - 无序列表标签(ul标签)
        - 无序标签,代表在数据前面有黑点显示
    ```
    <!-- ul标签定义无序列表 -->
    <ul>
        <!-- li标签定义列表项目 -->
        <li>列表标题一</li>
        <li>列表标题二</li>
        <li>列表标题三</li>
    </ul>
    ```

- 7.表格标签
    - 表格由行和列组成,如同一个excel 文件
    - table标签:双标签,表示一个表格
    - table标签内部:
        - tr标签: 双标签,表示表格中的一行
            - td标签:双标签,表示表格中的普通列
            - th标签:双标签,表示表格中的表头

    ```
    <table>
        <tr>
            <th>姓名</th>
            <th>年龄</th>
        </tr>
        <tr>
            <td>张三</td>
            <td>18</td> 
        </tr>
    </table>
    ```
    - border-collapse:collapse;可以设置表格的边线合并(CSS)
 
- 8.表单标签
    - 表单用于收集不通类型的用户输入数据,然后可以将用户数据提交到Web服务器
    - form标签:单标签,表示表单 标签, 定义整体的表单区域
    - label标签: 双标签 ,表示表单元素的文字标注标签,定义文字标注
    - input标签: 单标签, 表示表单元素的用户输入标签,可以定义不同类型的用户输入数据方式
        - type属性
            - type="text" 定义单行文本输入框
            - type="password" 定义密码输入框
            - type="radio" 定义单选框
            - type="checkbox" 定义复选框
            - type="file" 定义上传文件
            - type="submit" 定义提交按钮
            - type="reset" 定义重置按钮
            - type="button" 定义一个普通按钮
    - textarea标签:双标签,表示表单元素的多行文本输入框标签,定义多行文本输入框
    - select标签:双标签,表示表单元素的下拉列表标签,定义下拉列表
        - option标签于select配合使用,定义下拉列表的选项

    ```
    <form>
        <p>
            <label>姓名：</label><input type="text">
        </p>
        <p>
            <label>密码：</label><input type="password">
        </p>
        <p>
            <label>性别：</label>
            <input type="radio"> 男
            <input type="radio"> 女
        </p>
        <p>
            <label>爱好：</label>
            <input type="checkbox"> 唱歌
            <input type="checkbox"> 跑步
            <input type="checkbox"> 游泳
        </p>
        <p>
            <label>照片：</label>
            <input type="file">
        </p>
        <p>
            <label>个人描述：</label>
            <textarea></textarea>
        </p>
        <p>
            <label>籍贯：</label>
            <select>
                <option>北京</option>
                <option>上海</option>
                <option>广州</option>
                <option>深圳</option>
            </select>
        </p>
        <p>
            <input type="submit" value="提交">
            <input type="reset" value="重置">
        </p>
    </form>
    ```                

- 9.表单提交
    - form 表单属性设置
        - action属性 设置表单数据提交地址(网址)
        - method属性 设置表单提交的方式，一般有“GET”方式和“POST”方式, 不区分大小写
        - 提交方式就是等同于HTTP的GET 和 POST
        
    ```
    <form action="https://www.baidu.com" method="GET">
        <p>
            <label>姓名：</label><input type="text" name="username" value="11" />
        </p>
        <p>
            <label>密码：</label><input type="password" name="password" />
        </p>
        <p>
            <label>性别：</label>
            <input type="radio" name="gender" value="0" /> 男
            <input type="radio" name="gender" value="1" /> 女
        </p>
        <p>
            <label>爱好：</label>
            <input type="checkbox" name="like" value="sing" /> 唱歌
            <input type="checkbox" name="like" value="run" /> 跑步
            <input type="checkbox" name="like" value="swiming" /> 游泳
        </p>
        <p>
            <label>照片：</label>
            <input type="file" name="person_pic">
        </p>
        <p>
            <label>个人描述：</label>
            <textarea name="about"></textarea>
        </p>
        <p>
            <label>籍贯：</label>
            <select name="site">
                <option value="0">北京</option>
                <option value="1">上海</option>
                <option value="2">广州</option>
                <option value="3">深圳</option>
            </select>
        </p>
        <p>
            <input type="submit" name="" value="提交">
            <input type="reset" name="" value="重置">
        </p>
    </form>
    ```
    - 表单元素属设置
        - name: 表单元素的名称，用于作为提交表单数据时的参数名
        - value: 表单元素的值，用于作为提交表单数据时参数名所对应的值


    
    
    
    
    






