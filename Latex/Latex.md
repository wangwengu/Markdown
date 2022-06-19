# 公式

>   1.   $$
>        问题描述:如果\;d\;\vert\;a\;并且\;d\;\vert\;b,\;证明:d\;\vert\;gcd(a,\;b) \\
>        算术基本定理:任意一个大于1的自然数N,\;可以唯一分解为有限个质数的乘积,\;即有,\\N\;=\;p_1^{\alpha_1}\;*\;p_2^{\alpha_2}\;*\;...\;*\;p_n^{\alpha_n},\;其中,\;p_1,\;p_2,\;...,\;p_n都是质数 \\
>        证明如下: \\
>        由算术基本定理可知: \\
>        a\;=\;p_1^{\alpha_1}\;*\;p_2^{\alpha_2}\;*\;...\;*\;p_n^{\alpha_n} \\
>        b\;=\;p_1^{\beta_1}\;*\;p_2^{\beta_2}\;*\;...\;*\;p_n^{\beta_n} \\
>        d\;=\;p_1^{\gamma_1}\;*\;p_2^{\gamma_2}\;*\;...\;*\;p_n^{\gamma_n} \\
>        由于d\;\vert\;a\;并且\;d\;\vert\;b,\;因此,\;有 \\
>        \left.\begin{array}{col1} {\gamma_1\;\leq\;\alpha_1}\\\gamma_1\;\leq\;\beta_1\end{array}\right\}\Rightarrow\gamma_1\;\leq\;min(\alpha_1,\;\beta_1) \\
>        \left.\begin{array}{col2}
>        {\gamma_2\;\leq\;\alpha_2}\\\gamma_2\;\leq\;\beta_2\end{array}\right\}\Rightarrow\gamma_2\;\leq\;min(\alpha_2,\;\beta_2) \\
>        ... \\
>        \left.\begin{array}{col3}
>        {\gamma_n\;\leq\;\alpha_n}\\\gamma_n\;\leq\;\beta_n\end{array}\right\}\Rightarrow\gamma_n\;\leq\;min(\alpha_n,\;\beta_n) \\\\
>        由于gcd(a,\;b)是a,\;b\;的最大公约数,\;因此,\;有 \\
>        gcd(a,\;b)\;=\;p_1^{min(\alpha_1,\;\beta_1)}\;*\;p_2^{min(\alpha_2,\;\beta_2)}\;*\;...\;*\;p_n^{min(\alpha_n,\;\beta_n)}\\
>        综上所述:d\;\vert\;gcd(a,\;b)
>        $$
>
>   2.   $$
>        D\;=\;\mathop{max\;d_{ij}}\limits_{1\;\leq\;i,\;j\;\leq\;N}\;=\;d_{45}\;=\;d_{56}\;=\;3 \\
>        L\;=\;\frac{2}{6(6\;-\;1)}\sum\limits_{i\;\textgreater\;j}d_{ij}\;=\;(2\;\times\;25)\;/\;(6\;\times\;5)\;=\;1.67 \\
>        \begin{equation}
>        \left(
>          \begin{array}{ccc}
>            R & S \\
>            T & P \\
>          \end{array}
>        \right)
>        \end{equation}
>        $$
>
>   3.   $$
>        d_{ab}\;=\;\sqrt{(x_1\;-\;x_2)^2\;+\;(y_1\;-\;y_2)^2}
>        $$
>
>   4.   $$
>        H(x)\;=\;-\sum_{i\;=\;1}^{n}P(x_i)log_2P(x_i) \\
>        H(x)\;=\;-\sum\limits_{i\;=\;1}^{n}P(x_i)log_2P(x_i) \\
>        H(x)\;=\;-\sum\nolimits_{i\;=\;1}^{n}P(x_i)log_2P(x_i) \\
>        Gain(D,\;a)\;=\;Ent(D)\;-\;\sum\limits_{i\;=\;1}^{n}\frac{\lvert{D_v}\rvert}{\lvert{D}\rvert}Ent(D_v)
>        $$
>
>   5.   $$
>        a\equiv b\pmod p
>        $$
>
>   6.   $$
>        \begin{align*}
>          A &= B + C \\
>            &= C + D + C \\
>            &= 2C + D
>        \end{align*}
>        $$
>
>   7.   $$
>        a^{+\infty} \\
>        a^{-\infty}
>        $$
>
>   8.   $$
>        \vee \\
>        \wedge
>        $$
>
>   9.   $$
>        \sigma=\sqrt{\frac{\sum\nolimits_{i = 1}^{n}(x_i - \bar x)^2}{n}}, 
>        \bar x = \frac{\sum\nolimits_{i = 1}^{n}x_i}{n}
>        $$
>
>   10.   $$
>         \Delta E \geq 0
>         $$
>
>   11.   $$
>         Info_{age}(D)=\frac{5}{14}(-\frac{2}{5}log_2\frac{2}{5} - \frac{3}{5}log_2\frac{3}{5}) \\ \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;+ \frac{4}{14}(-\frac{4}{4}log_2\frac{4}{4} - \frac{0}{4}log_2\frac{0}{4}) \\
>         \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;+ \frac{5}{14}(-\frac{3}{5}log_2\frac{3}{5} - \frac{2}{5}log_2\frac{2}{5}) \\
>         =0.694bits
>         $$
>
>   12.   $$
>         Info_{income}(D)=\frac{4}{14}(-\frac{2}{4}log_2\frac{2}{4} - \frac{2}{4}log_2\frac{2}{4}) \\ \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;+ \frac{6}{14}(-\frac{4}{6}log_2\frac{4}{6} - \frac{2}{6}log_2\frac{2}{6}) \\
>         \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;+ \frac{4}{14}(-\frac{3}{4}log_2\frac{3}{4} - \frac{1}{4}log_2\frac{1}{4}) \\
>         =0.911bits
>         $$
>
>   13.   $$
>         Info_{student}(D)=\frac{7}{14}(-\frac{6}{7}log_2\frac{6}{7} - \frac{1}{7}log_2\frac{1}{7}) \\ \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;+ \frac{7}{14}(-\frac{3}{7}log_2\frac{3}{7} - \frac{4}{7}log_2\frac{4}{7}) \\
>         =0.789bits
>         $$
>
>   14.   $$
>         Info_{Credit\;rating}(D)=\frac{8}{14}(-\frac{6}{8}log_2\frac{6}{8} - \frac{2}{8}log_2\frac{2}{8}) \\ \;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;\;+ \frac{6}{14}(-\frac{3}{6}log_2\frac{3}{6} - \frac{3}{6}log_2\frac{3}{6}) \\
>         =0.856bits
>         $$

# 配置相关

## `PPT` 制作

### 中文支持

>   1.   ![CleanShot 2022-06-19 at 20.37.20@2x](img/CleanShot%202022-06-19%20at%2020.37.20@2x.png)
>
>   2.   修改第一行代码, 如下所示
>
>        ```latex
>        % 设置字体, 编码格式以及支持中文的ppt制作
>        \documentclass[14pt, UTF8]{ctexbeamer}
>        ```
