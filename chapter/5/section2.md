# 1.css
- 1.定义
    - css(Cascading Style Sheet)层叠样式表，它是用来美化页面的一种语言。
    - 一般和html 结合使用
- 2.作用
    - 美化界面, 比如: 设置标签文字大小、颜色、字体加粗等样式。
    - 控制页面布局, 比如: 设置浮动、定位等样式。
- 3.基本语法
    ```
    选择器{
    
    样式规则
    }
    样式规则：
    属性名1：属性值1;
    属性名2：属性值2;
    属性名3：属性值3;
    
    div{
        width:100px;
        height:100px;
        background:gold;
    }
    ```
    - css由两个主要的部分组成: 选择器和一条或多条样式规则.分别在大括号的外面和里面


- 4.引入方式
    - 行内式
        - 仅作用于该行
        - 优点:方便,只管
        - 缺点:缺乏可重用性
        ```
        <div style="width:100px; height:100px; background:red ">hello</div>
        ```
    - 内嵌式
        
        - 使用在html文件的 head标签中,需要加入style标签,在其内部使用
        
        - 作用于当前网页文件
        - 优点:在同一个页面内部便于复用和维护
        - 缺点:在多个页面之间的可重用性不够高(即不能用于其他页面)
        
        ```
        <head>
        <style type="text/css">
            h3{
            color:red;
            }
        </style>
        </head>
        ```
    
    - 外链式
        - 创建一个单独的css文件中,直接编写 选择器{样式规则} 即可
        - 在<head>标签中使用<link>标签直接引入该文件到当前页面中
        - 优点:使得css样式和html页面分离,便于整个页面系统的规划和维护,可重用性高(可被多个页面引用)
        - 缺点:css代码由于分离到单独的css文件中,容易出现css代码过于集中,如果维护不当则极易出现混乱
        - 可以对css代码进行分块,类似于模块,这样不容易出现混乱,需要使用时,可以导入多个
        ```
        <link rel="stylesheet" type="text/css" href="css/main.css">
        ```
    - 如果出现对同标签重复定义某相同样式,则最后一次定义会生效(覆盖,类似于重新赋值)
    - 外链式再看开发的时候使用较多,最能体现html + css 样式分离的思想, 也易于改版维护,代码开起来也是最美观的一种

# 2.css选择器
- 1.作用
    - css选择器是用来选择标签的,选出来后给标签设置样式
- 2.常见的css选择器的种类
    - 标签选择器
    - 类选择器
    - 层级选择器(后代选择器)
    - id 选择器
    - 组选择器
    - 伪类选择器
- 3.标签选择器
    - 根据 标签 来选择标签,以标签开头,此种选择器影响范围大,一般用来做一些通用设置
    - 类似于一个全局变量
    ```
    <style type="text/css">
        p{
        color: red;
        }
    </style>
        <div>hello</div>
        <p>hello</p>
    ```
    
- 4.类选择器
    - 根据 类名 来选择标签, 以".类名"开头,一个类选择器可以应用于多个标签上,一个标签上也可以使用多个类选择器,用空格分隔
    - 应用灵活,可服用,是css中应用最多的一种选择器
    ```
    <style type="text/css">
        .blue{color:blue}
        .big{font-size:20px}
        .box{width:100px;height:100px;background:gold}
    </style>
        <div class="blue">这是一个div</div>
        <h3 class="blue big box">这是一个标题</h3>
        <p class="blue box">这是一个段落</p>
    ```
- 5.层级选择器(后代选择器)
    - 根据 层级关系 选择后代标签,以"选择器1 选择器2"开头,主要应用在标签嵌套的结构中,减少命名
    - 这个层级关系不一定是父子关系，也有可能是祖孙关系，只要有后代关系都适用于这个层级选择器
        - 例如 div p{样式规则},则不仅选择到div标签的p标签,包括p标签内部的标签都会选择到
    ```
    <style type="text/css">
        div p{
        color: red;
        }
        .con{width:300px;height:80px;background:green}
        .con span{color:red}
        .con .pink{color:pink}
        .con .gold{color:gold}
    </style>
        <div>
        <p>hello</p>
        </div>
        <div class="con">
        <span>哈哈</span>
        <a href="#" class="pink">百度</a>
        <a href="#" class="gold">谷歌</a>
        </div>
        <span>你好</span>
        <a href="#" class="pink">新浪</a>
    ```
- 6.id选择器(了解)

    - 根据 id 选择标签 , 以"#id"开头,元素的id名称不能重复,所以id选择器只能对应于页面上一个元素,不能复用
    - id名一般给程序使用,所以不推荐使用id作为选择器
    -  虽然给其它标签设置id=“box”也可以设置样式，但是不推荐这样做，因为id是唯一的，以后js通过id只能获取一个唯一的标签对象。
    ```
    <style type="text/css">
        #box{color:red} 
    </style>
    
    <p id="box">这是一个段落标签</p>   <!-- 对应以上一条样式，其它元素不允许应用此样式 -->
    <p>这是第二个段落标签</p> <!-- 无法应用以上样式，每个标签只能有唯一的id名 -->
    <p>这是第三个段落标签</p> <!-- 无法应用以上样式，每个标签只能有唯一的id名  -->
    ```

- 7.组选择器
    - 根据组合的选择器选择不同的标签，以 , 分割开, 如果有公共的样式设置，可以使用组选择器。
    ```
    <style type="text/css">
    .box1,.box2,.box3{width:100px;height:100px}
        .box1{background:red}
        .box2{background:pink}
        .box2{background:gold}
    </style>
    
    <div class="box1">这是第一个div</div>
    <div class="box2">这是第二个div</div>
    <div class="box3">这是第三个div</div>
    ```

- 8.伪类选择器
    - 用于向选择器添加特殊效果, 以:分割开,当用户和网站交互的时候改变显示效果可以使用伪类选择器
    - 用于交互时的特殊效果
    
    ```
    <style type="text/css">
        .box1{width:100px;height:100px;background:gold;}
        .box1:hover{width:300px;}
    </style>
    
    <div class="box1">这是第一个div</div>
    ```
    - hover : 当鼠标移动到标签位置时,放大标签区域

# 3.css 属性
- 1.常用样式属性 -- 布局
    - width 设置元素(标签)的宽度，如：width:100px;
    - height 设置元素(标签)的高度，如：height:200px;
    - background 设置元素背景色或者背景图片，如：background:gold; 设置元素的背景色, background: url(images/logo.png); 设置元素的背景图片。
    - border 设置元素四周的边框，如：border:1px solid black; 设置元素四周边框是1像素宽的黑色实线
    - 以上也可以拆分成四个边的写法，分别设置四个边的：
    - border-top 设置顶边边框，如：border-top:10px solid red;
    - border-left 设置左边边框，如：border-left:10px solid blue;
    - border-right 设置右边边框，如：border-right:10px solid green;
    - border-bottom 设置底边边框，如：border-bottom:10px solid pink;

- 2.常用样式属性 -- 文本
    - color 设置文字的颜色，如： color:red;
    - font-size 设置文字的大小，如：font-size:12px;
    - font-family 设置文字的字体，如：font-family:'微软雅黑';为了避免中文字不兼容，一般写成：font-family:'Microsoft Yahei';
    - font-weight 设置文字是否加粗，如：font-weight:bold; 设置加粗 font-weight:normal 设置不加粗
    - line-height 设置文字的行高，如：line-height:24px; 表示文字高度加上文字上下的间距是24px，也就是每一行占有的高度是24px
    - text-decoration 设置文字的下划线，如：text-decoration:none; 将文字下划线去掉
    - text-align 设置文字水平对齐方式，如text-align:center 设置文字水平居中
    - text-indent 设置文字首行缩进，如：text-indent:24px; 设置文字首行缩进24px





