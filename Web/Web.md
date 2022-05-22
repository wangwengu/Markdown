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

## `HTML`基础标签

### `HTML`文件结构

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

### 文本标签

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

### 图片标签

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

### 音频

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

### 视频

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

### 超链接

>   1.   用法
>
>        ```html
>        <!-- 跳转的地址 -->
>        <!-- 新标签页打开 -->
>        <a href="https://www.acwing.com" target="_blank">
>            <img width="30" src="/images/logo.png" alt="logo">
>        </a>
>        ```

### 表单

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

### 列表

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

### 表格

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

### 语义标签

>   

# `CSS`

# `JS`

# `React`

# `Vue`