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

### 音频和视频



# `CSS`

# `JS`

# `React`

# `Vue`