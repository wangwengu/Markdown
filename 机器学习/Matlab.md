# 常用函数

## `repmat` 拓展矩阵

>   1.   用法: `repmat(a, n, m)`
>
>   2.   含义: 将 `a` 变成 `n` 行 `m` 列的矩阵
>
>   3.   示例:
>
>        ```matlab
>        a = [1, 1, 1]
>        A = repmat(a, 2, 3)
>        ```
>
>        结果:
>
>        ```matlab
>        1 1 1 1 1 1 1 1 1
>        1 1 1 1 1 1 1 1 1
>        ```
>
>        说明:
>
>        将 `a` 矩阵变成 `2` 行 `3` 列的新矩阵, 将 `a` 复制 `2` 行并且复制 `3` 列

## `rand` 函数

>   1.   用法: `rand(n, m)`
>
>   2.   含义: 随机生成 `n` 行 `m` 列的范围 `(0, 1)` 之间的矩阵
>
>   3.   示例:
>
>        ```matlab
>        rand(2, 3)
>        ```
>
>        结果:
>
>        ```matlab
>        0.6443    0.8116    0.3507
>        0.3786    0.5328    0.9390
>        ```

## round 函数

>   1.   用法: `round(a)`
>
>   2.   含义: 将 `a` 矩阵进行四舍五入
>
>   3.   示例:
>
>        ```matlab
>        ans = [
>        	0.8759    0.6225    0.2077
>            0.5502    0.5870    0.3012
>        ]
>        round(ans)
>        ```
>
>        结果:
>
>        ```matlab
>        1     1     0
>        1     1     0
>        ```

## `sum` 函数

>   1.   用法: `sum(a)`
>
>   2.   含义: 将 `a` 矩阵进行求和
>
>   3.   示例:
>
>        ```matlab
>        a = [1, 2, 3, 4, 1]
>        x = [1, 1, 0, 1, 1]
>        sum(a.*x)
>        ```
>
>        结果:
>
>        ```matlab
>        12
>        ```
>
>        说明:
>
>        `x[i] = 1`, 即表示 `+ a[i]`
>
>        `x[i] = 0`, 即表示 `+ 0`

## `max` 函数

>   1.   用法: `max(a)`
>
>   2.   含义: 求解 `a` 矩阵的最大值
>
>   3.   示例:
>
>        ```matlab
>        a = [4, 5, 2, 3, 1];
>        mx = 0, idx = 0;
>        [mx, idx] = max(a)
>        ```
>
>        结果:
>
>        ```matlab
>        mx = 5 % 最大值
>        idx = 2 % 最大值对应的下标
>        ```

## `transpose` 函数

>   1.   用法: `transpose(a)`
>
>   2.   含义: 将 `a` 矩阵进行转置
>
>   3.   示例:
>
>        ```matlab
>        a = [1, 3, 5]
>        b = transpose(a);
>        b
>        ```
>
>        结果:
>
>        ```matlab
>        ans = 
>        	1
>        	3
>        	5
>        ```

## `disp` 函数

>   1.   用法: `disp(a)`
>
>   2.   含义: 同 `c++` `printf` 用于输出内容
>
>   3.   示例:
>
>        ```matlab
>        a = [1, 3, 5]
>        disp(a)
>        ```
>
>        结果:
>
>        ```matlab
>        1 3 5
>        ```

```matlab
% 清屏
clear;
clc;
close all;

% 初始化变量
v = [95, 4, 60, 32, 23, 72, 80, 62, 65, 46]; % 物品体积
w = [55, 10, 47, 5, 4, 50, 8, 61, 85, 87]; % 物品价值
m = 269; % 背包总容量

% 初始化种群
n = 10; % 物品数量
birds = 20; % 鸟(粒子)的数量
gen = 30; % 迭代30次
c1 = 0.5; % 加速因子(权重)
c2 = 0.5; % 加速因子(权重)
k = 0.8; % 惯性因子

V = repmat(v, birds, 1); % 将v拓展成20 * 10的矩阵
W = repmat(w, birds, 1); % 将w拓展成20 * 10的矩阵
x = round(rand(birds, n)); % 初始化每一维的位置 20 * 10, m维空间, m维的坐标
v_0 = rand(birds, n); % 初始化速度矩阵, 每个粒子的速度
xbest = zeros(birds, n); % 初始化20 * 10的0矩阵, 代表初始化的时候,一件物品都不选
fxbest = zeros(birds, 1); % 每个粒子的适应度 20 * 1
gbest = zeros(1, n); % 全局最优解, 即为答案
fgbest = 0; % 全局最优解的适应度
iter = 0; % 迭代次数
while iter < gen % 迭代gen次
    fx = sum(transpose((W.*x))); % 计算背包内物品的价格
    sx = sum(transpose((V.*x))); % 计算背包内物品的体积
    for i = 1 : birds % 遍历每一个粒子
        if sx(i) > m % 如果超出背包总容量, 则将第i个粒子(方案)价格设置为0
            fx(i) = 0;
        end
    end
    for i = 1 : birds % 遍历每一个粒子
        if fxbest(i) < fx(i) % 如果鸟i当前所搜索到的方案比其历史最佳方案还要好, 则更新
            fxbest(i) = fx(i); % 记录鸟i的历史值
            xbest(i, :) = x(i, :); % 记录鸟i的历史最佳方案
        end
    end
    if fgbest < max(fxbest) % 如果当前某个鸟i的局部最优解比全局最优解更好, 则更新
        [fgbest, idx] = max(fxbest); % 记录所有鸟的局部最优解的最大值, 以及其对应的下标
        gbest = xbest(idx, :); % 记录目前为止的全局最优解方案
    end
    for i = 1 : birds % 遍历每一只鸟
        if x(i, :) == gbest % 如果当前鸟已经是当前搜索到的全局最优解
            x(i, :) = round(rand(1, n)); % 则重新赋值, 防止陷入局部最优解
        end
    end
    v1 = rand(birds, n); % 当前鸟i的最优解的初速度
    v2 = rand(birds, n); % 所有鸟的最优解的初速度
    % 产生新速度, 由三部分组成
    % 第一部分: 鸟i初速度
    % 第二部分: 鸟i的历史最佳位置对其的一个影响速度
    % 第三部分: 所有鸟的历史最佳位置(全局最优解)对其的一个影响速度
    % 三部分的合力即为新的初始速度
    v_0 = v_0 * k + c1 * v1.*(xbest - x) + c2 * v2.*(repmat(gbest, birds, 1) - x);
    disp(x);
    x = x + v_0;
    disp(v_0);
    for i = 1 : birds % 遍历每只鸟
        for j = 1 : n % 遍历每一维
            if x(i, j) < 0.5
                x(i, j) = 0;
            else
                x(i, j) = 1;
            end
        end
    end
    iter = iter + 1;
end 
disp(fgbest);
sgbest = sum(transpose(v.*gbest));
disp(gbest);
```

295
