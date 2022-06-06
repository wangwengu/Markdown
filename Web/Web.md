# 配置及其他

## `VS code`

### 安装插件

>   1.   `Live Server`
>   2.   `Auto Rename Tag`

### 设置相关

>   1.   自动格式化, 勾选 `Editor: Format On Save`
>
>        ![4300917](img/4300917.png)

## [MDN官方文档](https://developer.mozilla.org/zh-CN/plus)

# `HTML`

## `HTML`文件结构

1.   文档结构

     >   `html`的所有标签为树形结构, 例如:
     >
     >   ```html
     >   <!DOCTYPE html>
     >   <html lang="zh-CN">
     >   
     >   <head>
     >       <meta charset="UTF-8">
     >       <meta name="viewport" content="width = device-width, initial-scale = 1.0">
     >       <title>Web应用课</title>
     >   </head>
     >   
     >   <body>
     >       <h1>第一讲</h1>
     >   </body>
     >   
     >   </html>
     >   ```
     >
     >   其树形图如下所示:
     >
     >   ![4300951](img/4300951.png)

2.   `meta` 标签

     >   常用属性: `description`, `keywords`
     >
     >   +   `description` 作用: 搜索引擎中关于网站的描述
     >
     >       ![4301030](img/4301030.png)
     >
     >   +   `keywords` 作用: 搜索引擎收录的网站关键字
     >
     >       ![4301037](img/4301037.png)
     >
     >   用法如下:
     >
     >   ```html
     >   <meta name="keywords" content="算法,计算机,人工智能">
     >   <meta name="description" content="组队刷怪">
     >   ```

3.   `link` 标签

     >   常用属性: `icon`
     >
     >   +   `icon` 作用: 网站的 `logo`
     >
     >       ![4301044](img/4301044.png)
     >
     >   用法如下:
     >
     >   ```html
     >   <link rel="icon" href="/images/logo.png">
     >   ```

4.   多行注释

     >   用法如下:
     >
     >   ```html
     >   <body>
     >       <!-- 这是第一条注释 -->
     >       <h1>第一讲</h1>
     >   </body>
     >   ```

## 文本标签

1.   绝大部分文本标签可以看成带有 `css` 样式的 `div` 或者 `span` 标签

2.   `div` 标签

     >   定位: 块状标签(默认带回车符 `\n` )
     >
     >   ![4301117](img/4301117.png)
     >
     >   用法如下:
     >
     >   ```html
     >   <body>
     >       <div>Hello</div>
     >       <div>world</div>
     >   </body>
     >   ```
     >
     >   优点:
     >
     >   +   便于调整 `css` 样式, `div` 可以看做一个整体, 可用于对整体进行样式调整
     >   +   方便 `js` 操作, `js` 可以方便的对于 `div` 进行各种操作

3.   `span` 标签

     >   定位: 行内标签(默认无回车符 `\n`)
     >
     >   ![4301122](img/4301122.png)
     >
     >   用法如下:
     >
     >   ```html
     >   <body>
     >       <span>Hello</span>
     >       <span>world</span>
     >   </body>
     >   ```

4.   `h1` 标签

     >   定位: 带有 `css` 样式的 `div` 标签
     >
     >   ![4301127](img/4301127.png)
     >
     >   内置了预先设置好的样式, 其他块状元素或者行内元素同理
     >
     >   用法如下:
     >
     >   ```html
     >   <body>
     >       <h1>Hello world</h1>
     >   </body>
     >   ```

5.   `pre` 标签

     >   作用: 保留原格式中的空格 `space` 和回车 `\n`
     >
     >   ![4301138](img/4301138.png)
     >
     >   内置了预先设置好的样式, 其他块状元素或者行内元素同理
     >
     >   用法如下:
     >
     >   ```html
     >   <body>
     >       <pre>
     >   #include &lt;iostream&gt;
     >   using namespace std;
     >   int main() {
     >       int a, b;
     >       cin &gt;&gt; a &gt;&gt; b;
     >       cout &lt;&lt; a + b &lt;&lt; endl;
     >       return 0;
     >   }
     >       </pre>
     >   </body>
     >   ```

## 图片标签

>   常用属性: `width`、`height`、`src`、`alt`
>
>   +   `width` 作用: 调整宽度, 默认单位: `px`, 只写宽度不写高度, 会自动将高度等比例缩放
>   +   `height` 作用: 调整高度, 默认单位: `px`, 只写高度不写宽度, 会自动将宽度等比例缩放
>   +   `src` 作用: 图片路径
>   +   `alt` 作用: 当图片加载不出来时, 进行信息的显示
>
>   定位: 行内标签 `<img>`, 当一行放不下的时候, 自动换行
>
>   ![4301201](img/4301201.png)
>
>   用法如下:
>
>   ```html
>   <body>
>       <h1>Hello World</h1>
>       <img width="300px;" src="/images/mountain.jpeg" alt="山">
>       <img width="300px;" src="/images/mountain.jpeg" alt="山">
>       <img width="300px;" src="/images/mountain.jpeg" alt="山">
>       <img width="300px;" src="/images/mountain3.jpeg" alt="山">
>   </body>
>   ```

## 音频

>   1.   第一种方式
>
>        ```html
>        <!-- controls控制音频出现 -->
>        <audio controls src="/audios/bgm1.mp3">Audios</audio>
>        ```
>
>   2.   第二种方式
>
>        ```html
>        <audio controls>
>            <!-- 如果第一个显示不出来, 则显示第二个, 依次类推 -->
>            <source src="/audios/bgm2.mp3" type="audio/mpeg">
>            <source src="/audios/bgm3.mp3" type="audio/mpeg">
>        </audio>
>        ```

## 视频

>   1.   第一种方式
>
>        ```html
>        <!-- 显示控制栏、宽度、地址 -->
>        <video controls width="300" src="/videos/video1.mp4">视频</video>
>        ```
>
>   2.   第二种方式
>
>        ```html
>        <!-- 第一个不能播放, 则播放第二个, 以此类推 -->
>        <video controls width="300">
>            <source src="/videos/video1.mp4" type="video/mp4">
>            <source src="/videos/video2.mp4" type="video/mp4">
>        </video>
>        ```

## 超链接

>   1.   用法
>
>        ```html
>        <!-- 跳转的地址 -->
>        <!-- 新标签页打开 -->
>        <a href="https://www.acwing.com" target="_blank">
>            <img width="30" src="/images/logo.png" alt="logo">
>        </a>
>        ```

## 表单

>   1.   用法
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="zh-CN">
>        <head>
>            <meta charset="UTF-8">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <meta name="keywords" content="算法,计算机,人工智能">
>            <meta name="description" content="组队刷怪">
>            <link rel="icon" href="/images/logo.png">
>            <title>Web应用课</title>
>        </head>
>        <body>
>            <!-- action表示点击提交按钮之后, 跳转到某个表单 -->
>            <!-- 注意: 参数也会进行传递 -->
>            <form action="/about.html">
>                <!-- label用于绑定input标签 -->
>                <!-- 绑定的是ID -->
>                <label for="username">用户名</label>
>                <!-- type填写的是类型 -->
>                <!-- required表示当前字段必须填写 -->
>                <!-- placeholder提示符 -->
>                <!-- name在url中会做名称, 看下图 -->
>                <input type="text" required placeholder="用户名" maxlength="10" minlength="3" name="username" id="username">
>                <br>
>                <label for="age">年龄</label>
>                <input type="number" placeholder="年龄" name="age" id="age">
>                <br>
>                <label for="email">邮箱</label>
>                <input type="email" placeholder="邮箱" name="email" id="email">
>                <br>
>                <label for="">密码</label>
>                <input type="password" required placeholder="密码" name="password" id="password">
>                <br>
>                <!-- 当类型是选框的时候, 所有name一样的值都必须归为同一组, 即同一组只能选择一个 -->
>                <label for="cpp">c++</label>
>                <input type="radio" name="lang" value="c++" id="cpp">
>                <br>
>                <label for="java">java</label>
>                <input type="radio" name="lang" id="java">
>                <br>
>                <label for="python">python</label>
>                <input type="radio" name="lang" id="python">
>                <br>
>                <!-- 文本域 -->
>                <textarea type="textarea" rows="10" cols="30" name="comment" id="comment"></textarea>
>                <br>
>                <select value="lang">
>                    <!-- 默认选择第一个 -->
>                    <option value="java">java</option>
>                    <option value="python">python</option>
>                    <option value="c++">c++</option>
>                </select>
>                <button type="submit">提交</button>
>            </form>
>        </body>
>        </html>
>        ```
>
>   2.   图片 `1`
>
>        ![5172110](img/5172110.png)
>
>        图片 `2`
>
>        ![5172115](img/5172115.png)

## 列表

>   1.   用法
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="zh-CN">
>                                           
>        <head>
>            <meta charset="UTF-8">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <meta name="keywords" content="算法,计算机,人工智能">
>            <meta name="description" content="组队刷怪">
>            <link rel="icon" href="/images/logo.png">
>            <title>Web应用课</title>
>        </head>
>                                           
>        <body>
>            <!-- 每个ol或者ul里面都需要有li标签 -->
>            <ol>
>                <li>
>                    chinese
>                    <ul>
>                        <li>Beijing</li>
>                    </ul>
>                    <ul>
>                        <li>Shanghai</li>
>                    </ul>
>                    <ul>
>                        <li>Guangzhou</li>
>                    </ul>
>                </li>
>                <li>
>                    english
>                    <ul>
>                        <li>New York</li>
>                    </ul>
>                    <ul>
>                        <li>London</li>
>                    </ul>
>                    <ul>
>                        <li>Pari</li>
>                    </ul>
>                </li>
>            </ol>
>        </body>
>                                           
>        </html>
>        ```

## 表格

>   1.   用法
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <title>Document</title>
>        </head>
>        <body>
>            <table>
>                <!-- 表格标题 -->
>                <caption>成绩单</caption>
>                <!-- 标题栏 -->
>                <thead>
>                    <!-- 每一行使用tr包裹起来 -->
>                    <tr>
>                        <th>姓名</th>
>                        <th>语文</th>
>                        <th>数学</th>
>                        <th>英语</th>
>                    </tr>
>                </thead>
>                <!-- 正文 -->
>                <tbody>
>                    <tr>
>                        <td>wyp</td>
>                        <td>100</td>
>                        <td>100</td>
>                        <td>100</td>
>                    </tr>
>                    <tr>
>                        <td>sarah</td>
>                        <td>101</td>
>                        <td>101</td>
>                        <td>101</td>
>                    </tr>
>                </tbody>
>            </table>
>        </body>
>        </html>
>        ```

## 语义标签

>   1.   用法
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <title>Document</title>
>        </head>
>        <body>
>            <!-- 头部内容 -->
>            <header>
>                <h3>我的收藏</h3>
>                <!-- 导航栏 -->
>                <nav>
>                    <ul>
>                        <li><a href="">编辑</a></li>
>                        <li><a href="">联系我</a></li>
>                        <li><a href="">关于我</a></li>
>                    </ul>
>                </nav>
>            </header>
>            <hr>
>            <!-- 正文 -->
>            <section>
>                <h4>图片</h4>
>                <!-- 代表一段独立内容 -->
>                <!-- 通常与<figcaption>配合使用 -->
>                <figure>
>                    <img width="100" src="/images/logo.png" alt="logo">
>                    <!-- 通常作为figure的标注 -->
>                    <figcaption>logo</figcaption>
>                </figure>
>                <figure>
>                    <img width="100" src="/images/mountain.jpeg" alt="mountain">
>                    <figcaption>mountain</figcaption>
>                </figure>
>            </section>
>            <section>
>                <h4>文章</h4>
>                <article>
>                    <h5>背影</h5>
>                    <p>背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影</p>
>                    <p>背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影背影</p>
>                </article>
>            </section>
>            <hr>
>            <!-- 页脚 -->
>            <footer>
>                &copy;2000-2022 wyp 版权所有
>            </footer>
>        </body>
>        </html>
>        ```

## 特殊符号

>   | HTML源代码 | 显示结果 |          描述          |
>   | :--------: | :------: | :--------------------: |
>   |   `&lt;`   |    <     |   小于号或者显示标记   |
>   |   `&gt;`   |    >     |   大于号或者显示标记   |
>   |  `&amp;`   |    &     | 可用于显示其他特殊字符 |
>   |  `&quot;`  |    “     |          引号          |
>   |  `&reg;`   |    ®     |         已注册         |
>   |  `&copy;`  |    ©     |          版权          |
>   | `&trade;`  |    ™     |          商标          |
>   |  `&nbsp;`  |          |      不断行的空白      |

# `CSS`

## 样式定义方式

### 行内样式表

>   1.   用法:
>
>        ```html
>        <body>
>            <!-- 注意点: 必须带单位, 因为width可能有多个单位, 例如: vh, em等等 -->
>            <div style="width: 100px; height: 100px; background-color: red">侬好</div>
>        </body>
>        ```

### 内部样式表

>   1.   用法
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <title>Document</title>
>            <!-- 定义内部样式表 -->
>            <style type="text/css">
>                /* 修改所有的div类 */
>                div {
>                    width: 50px;
>                    height: 50px;
>                    background-color: lightblue;
>                }
>                            
>                /* 所有所有类名为div_color的 */
>                .div_color {
>                    background-color: lightcoral;
>                }
>                            
>                /* 修饰所有类名为div_size的 */
>                .div_size {
>                    width: 100px;
>                    height: 100px;
>                }
>            </style>
>        </head>
>        <body>
>            <div class="div_size">你好</div>
>            <br>
>            <div>侬好</div>
>            <br>
>            <!-- 可以定义多个类名, 使用空格隔开即可 -->
>            <div class="div_color div_size">Hello</div>
>        </body>
>        </html>
>        ```

### 外部样式表

>   1.   用法
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <!-- 可以链接多个css文件 -->
>            <!-- 后面的css会覆盖掉前面的css, 原因: 按照顺序执行 -->
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div class="div_size">你好</div>
>            <br>
>            <div>侬好</div>
>            <br>
>            <!-- 可以定义多个类名, 使用空格隔开即可 -->
>            <div class="div_color div_size">Hello</div>
>        </body>
>        </html>
>        ```

## 选择器

### 标签选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>div 1</div>
>            <div>div 2</div>
>            <div>div 3</div>
>            <p>p 1</p>
>            <p>p 2</p>
>            <p>p 3</p>
>        </body>
>        </html>
>        ```
>
>   2.    `css`
>
>        ```css
>        div {
>            width: 100px;
>            height: 100px;
>            margin-bottom: 10px;
>            background-color: lightblue;
>        }
>                            
>        p {
>            width: 100px;
>            height: 100px;
>            background-color: lightcoral;
>        }
>        ```

### `ID` 选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>div 1</div>
>            <div>div 2</div>
>            <div id="mydiv">div 3</div>
>            <p>p 1</p>
>            <p id="myp">p 2</p>
>            <p>p 3</p>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        #mydiv {
>            background-color: brown;
>        }
>                            
>        #myp {
>            background-color: purple;
>        }
>        ```

### 类选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <!-- 具有两个类: 既具有颜色类又具有尺寸类 -->
>            <!-- 
>                注意: 
>                    1. 其有两个执行顺序
>                        -> div标签内嵌的顺序, 即 color_tag ===> size_tag
>                        -> 外部样式表定义的顺序, 即 size_tag ===> color_tag
>                    2. 应该以哪个作为标准??
>                        答: 以外部样式表的顺序作为标准, 即 size_tag的优先级 > color_tag的优先级
>            -->
>            <div class="color_tag size_tag">div 1</div>
>            <div>div 2</div>
>            <div>div 3</div>
>            <!-- 具有颜色类 -->
>            <p class="color_tag">p 1</p>
>            <p>p 2</p>
>            <!-- 具有尺寸类 -->
>            <p class="size_tag">p 3</p>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        .size_tag {
>            width: 200px;
>            height: 200px;
>        }
>                            
>        .color_tag {
>            background-color: lightblue;
>        }
>        ```

### 伪类选择器

#### 链接伪类选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <a href="/about.html">about</a>
>            <br>
>            <button>按钮</button>
>            <br>
>            <input type="text">
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            width: 100px;
>            height: 100px;
>            margin-bottom: 10px;
>            background-color: lightblue;
>        }
>                            
>        p {
>            width: 100px;
>            height: 100px;
>            background-color: lightcoral;
>        }
>                            
>        /* 链接访问前的样式 */
>        a:link {
>            /* 设置字体的颜色 */
>            color: aquamarine;
>        }
>                            
>        /* 链接访问过后的样式 */
>        a:visited {
>            /* 设置字体的颜色 */
>            color: purple;
>        }
>                            
>        /* 鼠标悬停的样式 */
>        a:hover {
>            /* 设置字体的颜色 */
>            color: blue;
>        }
>                            
>        /* 鼠标长按的样式 */
>        a:active {
>            /* 设置字体的颜色 */
>            color: coral;
>        }
>                            
>        /* 鼠标悬停的样式 */
>        button:hover {
>            /* 设置字体的颜色 */
>            color: white;
>            /* 设置背景颜色 */
>            background-color: deepskyblue;
>        }
>                            
>        /* 鼠标悬停的样式 */
>        input:hover {
>            /* 改变宽度 */
>            width: 250px;
>        }
>                            
>        /* 聚焦后的样式 */
>        input:focus {
>            /* 放大1.2倍 */
>            transform: scale(1.2);
>            /* 过渡时间: 200毫秒 */
>            transition: 200ms;
>        }
>        ```

#### 位置伪类选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>div 1</div>
>            <p>p 1</p>
>            <p>p 2</p>
>            <p>p 3</p>
>            <p>p 4</p>
>            <p>p 5</p>
>            <p>p 6</p>
>            <p>p 7</p>
>            <p>p 8</p>
>            <p>p 9</p>
>            <p>p 10</p>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```c++
>        div {
>            width: 50px;
>            height: 50px;
>            margin-bottom: 10px;
>        }
>                            
>        /*
>            nth:child()求的是其父标签的第i个孩子是否满足条件
>            如: p:nth-child(even)求的是所有偶数标签
>            第一个标签是div
>            第二个标签是p, 其是父标签body的第二个标签了, 是偶数, 故应用此样式
>            第三个标签是p, 其是父标签body的第三个标签了, 是奇数, 故不应用此样式
>            ...
>            最终, 应用此样式对应的是p中的1, 3, 5, ..., 是奇数, 而不是偶数
>            原因: 不是以p为基准, 而是以其父节点body为标准, 从上往下开始数, 1, 2, ...
>        */
>        p:nth-child(even) {
>            width: 50px;
>            height: 50px;
>            background-color: lightcoral;
>        }
>        ```

#### 目标伪类选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <!-- 链接指向对应的id标签 -->
>            <a href="#btn">链接</a>
>            <div>div 1</div>
>            <div>div 2</div>
>            <div>div 3</div>
>            <div>div 4</div>
>            <div>div 5</div>
>            <div>div 6</div>
>            <div id="btn">div 7</div>
>            <div>div 8</div>
>            <div>div 9</div>
>            <div>div 10</div>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```c++
>        div {
>            width: 150px;
>            height: 150px;
>            margin-bottom: 10px;
>            background-color: lightblue;
>        }
>                            
>        /* 当页面的url指向该元素时, 触发此样式 */
>        /* 页面地址: http://127.0.0.1:5500/index.html#btn */
>        /* 地址含有#btn即为指向该元素, 否则, 没有 */
>        #btn:target {
>            transform: scale(1.5);
>            transition: 2s;
>            background-color: purple;
>        }
>        ```

### 复合选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css" type="text/css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div class="big">div 1</div>
>            <div>div 2</div>
>            <div class="big color">div 3</div>
>            <p>p 1</p>
>            <p class="big">p 2</p>
>            <p>p 3</p>
>            <p>p 4</p>
>            <ul>
>                <li>
>                    1
>                    <ul>
>                        <li>1.1</li>
>                        <li>1.2</li>
>                        <li>1.3</li>
>                    </ul>
>                </li>
>                <li>
>                    2
>                </li>
>                <li>
>                    3
>                </li>
>            </ul>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        /* 同时选中div标签和p标签 */
>        div,
>        p {
>            width: 100px;
>            height: 100px;
>            margin-bottom: 10px;
>            background-color: lightblue;
>        }
>                       
>        /* 选中p标签中类名为big的元素 */
>        p.big {
>            width: 200px;
>            height: 200px;
>        }
>                       
>        /* 选中div标签中类名同时为big和color的元素 */
>        div.big.color {
>            background-color: purple;
>        }
>                       
>        /*  
>            选择紧紧跟着p标签的p标签, 必须是直系父子关系
>            注意: 第一个p标签不会产生效果, 因此, 第一个p标签的父亲并不是p标签
>        */
>        p + p {
>            background-color: red;
>        }
>                       
>        /* 选择ul下的所有li标签, 不管是否是直系血亲 */
>        ul li {
>            color: red;
>        }
>                       
>        /* 选择li下的ul下的li, 注意: 必须是直系血亲, 即: 只能是父子关系, 不可越辈分 */
>        li > ul > li {
>            color: lightblue;
>        }
>        ```

### 通配符选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <input type="text" id="">
>            <input type="number" name="" id="">
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        /* 给所有标签设置样式 */
>        * {
>            background-color: lightblue;
>        }
>                       
>        /* 给具有id属性的input标签设置背景颜色 */
>        input[id] {
>            background-color: blue;
>        }
>                       
>        /* 给具有type属性并且type属性值为number的标签设置背景颜色 */
>        input[type=number] {
>            background-color: purple;
>        }
>        ```

### 伪元素选择器

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <h2>悯农</h2>
>            <h2>咏柳</h2>
>            <h2>滕王阁序</h2>
>            <h2>静夜思</h2>
>            <p>锄禾日当午,</p>
>            <p>汗滴禾下土。</p>
>            <p>谁知盘中餐,</p>
>            <p>粒粒皆辛苦。</p>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        /* 给每一行的第一个字母添加颜色 */
>        p::first-letter {
>            color: red;
>        }
>                       
>        /* 给第一行的文字添加颜色 */
>        p::first-line {
>            color: purple;
>        }
>                       
>        /* 鼠标框选的时候, 字体颜色为红色, 背景颜色为浅绿色 */
>        p::selection {
>            color: red;
>            background-color: lightblue;
>        }
>                       
>        /* 在h2标签的前面添加《 */
>        h2::before {
>            content: "《";
>        }
>                       
>        /* 在h2标签的后面添加》 */
>        h2::after {
>            content: "》";
>        }
>        ```

### 样式渲染优先级

>   1.   先来先到
>
>   2.   范围越小, 权值越高, 具体而言:
>
>        `!important` `>` 行内样式 `>` ID选择器 `>` 类与伪类选择器 `>` 标签选择器 `>` 通用选择器

## 颜色

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <p>p 1</p>
>            <p>p 2</p>
>            <p>p 3</p>
>            <p>p 4</p>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        /* 使用系统预设的颜色 */
>        p:first-child {
>            color: red;
>        }
>                       
>        /* 使用十六进制设置颜色 */
>        p:nth-child(2) {
>            /* 第一种方式 */
>            color: #ADD8E6;
>            /* 第二种方式, 其等价于#AABBCC */
>            color: #ABC;
>        }
>                       
>        /* 使用rgb方式设置颜色, (red, green, blue) */
>        p:nth-child(3) {
>            color: rgb(173, 216, 230);
>        }
>                       
>        /* 使用rgba方式设置颜色, 最后的参数是透明度 */
>        p:nth-child(4) {
>            color: rgba(0, 255, 255, 1);
>        }
>        ```

## 文本

### `text-align`

>   1.   简介
>
>        `text-align` `CSS` 属性定义行内内容（例如文字）如何相对它的块父元素对齐。`text-align` 并不控制块元素自己的对齐，只控制它的行内内容的对齐
>
>   2.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <h3>悯农</h3>
>            <div>
>                锄禾日当午，<br>
>                汗滴禾下土。<br>
>                谁知盘中餐，<br>
>                粒粒皆辛苦。
>            </div>
>        </body>
>        </html>
>        ```
>
>   3.   `css`
>
>        ```c++
>        h3 {
>            /* 文字居中 */
>            text-align: center;
>        }
>        
>        div {
>            /* 文字居中 */
>            text-align: center;
>        }
>        ```
>
>   4.   注意事项
>
>        +   如果父节点定义了文字的排版, 子节点会继承格式

### `line-height`

>   1.   简介
>
>        `line-height` `CSS` 属性用于设置多行元素的空间量，如多行文本的间距。对于块级元素，它指定元素行盒（`line boxes`）的最小高度。对于非替代的 `inline` 元素，它用于计算行盒（`line box`）的高度
>
>   2.   知识点补充:长度单位
>
>        | 单位  | 描述                                                     |
>        | ----- | -------------------------------------------------------- |
>        | `px`  | 设备上的像素点                                           |
>        | `%`   | 相对于<font style="color: red">父元素</font>的百分比     |
>        | `em`  | 相对于<font style="color: red">当前元素</font>的字体大小 |
>        | `rem` | 相对于<font style="color: red">根元素</font>的字体大小   |
>        | `vw`  | 相对于<font style="color: red">视窗宽度</font>的百分比   |
>        | `vh`  | 相对于<font style="color: red">视窗高度</font>的百分比   |
>
>   3.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>
>                锄禾日当午
>            </div>
>        </body>
>        </html>
>        ```
>
>   4.   `css`
>
>        ```css
>        div {
>            text-align: center;
>            /* 窗口宽度的50% */
>            width: 50vw;
>            /* 窗口高度的10% */
>            height: 10vh;
>            /* 设置背景颜色 */
>            background-color: lightblue;
>            /* 行高设置为宽度, 则可以让文字水平居中 */
>            line-height: 10vh;
>        }
>        ```

### `letter-spacing`

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>
>                锄禾日当午
>            </div>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            /* 设置字与字之间的距离, 为1.5倍的字符 */
>            letter-spacing: 1.5em;
>        }
>        ```

### `text-indent`

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>
>                锄禾日当午
>            </div>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            /* 缩进2个字符 */
>            text-indent: 2em;
>        }
>        ```

### `text-decoration`

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <a href="">联系我们</a>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        a {
>            /* 取消链接的下划线 */
>            text-decoration: none;
>        }
>        ```

### `text-shadow`

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>锄禾日当午</div>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            /* 给文字设置阴影 */
>            /* 参数: x坐标, y坐标, 模糊效果, 颜色 */
>            /* 注意: 阴影可以设置多个, 不一定非要一个 */
>            text-shadow: 
>            5px 5px 2px red,
>            -5px -5px 2px  gray
>            ;
>        }
>        ```

## 字体

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>锄禾日当午</div>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            /* 字体大小 */
>            font-size: 24px;
>            /* 字体样式: 斜体 */
>            font-style: italic;
>            /* 字体粗细: 范围(1~1000), 注意: 无单位 */
>            font-weight: 600;
>            /* 设置字体 */
>            font-family: Courier;
>        }
>        ```

## 背景

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div></div>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            /* 宽度 */
>            width: 200px;
>            /* 高度 */
>            height: 200px;
>            /* 设置背景颜色 */
>            background-color: lightblue;
>            /* 背景图片 */
>            background-image: url('/static/images/mountain.jpeg');
>            /* 是否拉伸 */
>            background-repeat: no-repeat;
>            /* 背景图片大小 */
>            /* cover表示全部覆盖 */
>            background-size: cover;
>            /* 设置背景初始的位置:水平位置, 垂直位置 */
>            background-position: 10px 50px;
>            /* 固定在屏幕的绝对位置 */
>            background-attachment: fixed;
>        }
>        ```

## 边框

>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div></div><br>
>            <div></div><br>
>            <div></div>
>            <table>
>                <tbody>
>                    <tr>
>                        <td></td>
>                        <td></td>
>                        <td></td>
>                    </tr>
>                    <tr>
>                        <td></td>
>                        <td></td>
>                        <td></td>
>                    </tr>
>                    <tr>
>                        <td></td>
>                        <td></td>
>                        <td></td>
>                    </tr>
>                </tbody>
>            </table>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            /* 宽度 */
>            width: 100px;
>            /* 高度 */
>            height: 100px;
>            /* 背景颜色 */
>            background-color: lightblue;
>            /* 边框的样式, 实线 */
>            border-style: solid;
>            /* 边框的宽度 */
>            border-width: 5px;
>            /* 边框的颜色 */
>            border-color: black gray green blue;
>            /* 边框的圆角半径 */
>            border-radius: 50%;
>        }
>        
>        td {
>            /* 宽度 */
>            width: 50px;
>            /* 高度 */
>            height: 50px;
>            /* 边框的样式 */
>            border-style: solid;
>        }
>        
>        table {
>            /* 单元格之间是否进行合并还是分隔 */
>            border-collapse: collapse;
>        }
>        ```

## 元素展示格式

>   +   `display`
>       +   `block`
>           +   独占一行
>           +   `width`、`height`、`margin`、`padding` 均可控制
>               +   内边距 `padding` 会更改元素的大小
>           +   `width` 默认 `100%`
>       +   `inline`
>           +   可以共占一行
>           +   `width`与`height`无效，水平方向的`margin`与`padding`有效，竖直方向的`margin`与`padding`无效
>           +   `width`默认为本身内容宽度
>       +   `inline-block`
>           +   可以共占一行
>           +   `width`、`height`、`margin`、`padding`均可控制
>           +   `width`默认为本身内容宽度
>   +   `white-space`
>       +   `CSS` 属性是用来设置如何处理元素中的 空白 (`en-US`)
>   +   `text-overflow`
>       +   `CSS` 属性确定如何向用户发出未显示的溢出内容信号。它可以被剪切，显示一个省略号或显示一个自定义字符串
>   +   `overflow`
>       +   `CSS`属性 `overflow` 定义当一个元素的内容太大而无法适应 块级格式化上下文 时候该做什么。它是 `overflow-x` 和 `overflow-y` 的 简写属性
>
>   1.   `html` 小白鼠
>
>        ```html
>        <!DOCTYPE html>
>        <html lang="en">
>        <head>
>            <meta charset="UTF-8">
>            <meta http-equiv="X-UA-Compatible" content="IE=edge">
>            <meta name="viewport" content="width=device-width, initial-scale=1.0">
>            <link rel="stylesheet" href="/static/css/style.css">
>            <title>Document</title>
>        </head>
>        <body>
>            <div>离离原上草</div>
>            <span>一岁一枯荣一岁一枯荣一岁一枯荣一岁一枯荣一岁一枯荣一岁一枯荣一岁一枯荣一岁一枯荣一岁一枯荣一岁一枯荣</span>
>            <img src="/static/images/logo.png" alt="">
>            <div>
>        int main() {
>            int a, b;
>            cin >> a >> b;
>            return 0;
>        }
>            </div>
>        </body>
>        </html>
>        ```
>
>   2.   `css`
>
>        ```css
>        div {
>            /* 宽度 */
>            width: 800px;
>            /* 高度 */
>            height: 500px;
>            /* 外边距 */
>            margin: 10px;
>            /* 内边距 */
>            padding: 20px;
>            /* 背景颜色 */
>            background-color: lightblue;
>            /* 强行修改成行内元素 */
>            display: inline;
>            /* 设置空白 */
>            white-space: pre;
>        }
>        
>        span {
>            /* 设置宽度 */
>            width: 50px;
>            /* 设置高度 */
>            height: 100px;
>            /* 设置左外边距 */
>            margin-left: 50px;
>            /* 设置左内边距 */
>            padding-left: 20px;
>            /* 设置背景颜色 */
>            background-color: gray;
>            /* 强行修改成块状元素 */
>            display: block;
>            /* 当元素的内容超出元素的大小 */
>            overflow: hidden;
>            /* 如何处理超出的内容元素 */
>            text-overflow: ellipsis;
>        }
>        
>        img {
>            /* 宽度 */
>            width: 50px;
>            /* 高度 */
>            height: 50px;
>            /* 外边距 */
>            margin: 5px;
>            /* 内边距 */
>            padding: 20px;
>        }
>        ```





# `JS`

# `React`

# `Vue`