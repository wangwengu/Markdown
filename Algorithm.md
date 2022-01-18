#### 动态规划专题

##### [Acwing 1064. 小国王](https://www.acwing.com/problem/content/1066/)

**题目描述**

> 在 `n×n` 的棋盘上放 `k` 个国王，国王可攻击相邻的 `8` 个格子，求使它们无法互相攻击的方案总数。

**输入格式**

> 共一行，包含两个整数 `n` 和 `k`。

**输出格式**

> 共一行，表示方案总数，若不能够放置则输出 `0`。

**数据范围**

> $1\;\leq\;n\;\leq\;10$
>
> $0\;\leq\;k\;\leq{n^2}$

**输入样例**

> 3 2

**输出样例**

> 16

**手写稿**

![小国王](https://gitee.com/peter95535/image-bed/raw/master/img/%E5%B0%8F%E5%9B%BD%E7%8E%8B.png)

**代码**

```c++
#include <iostream>
#include <vector>
using namespace std;
typedef long long LL;
const int N = 15, M = 110, K = 1 << 10;
int n, m;
int cnt[K];
vector<int> state;
vector<int> convert[K];
LL f[N][M][K];
// 判断是否含有连续的 1
bool check(int u) {
    for (int i = 0; i < n; i ++ )
        if (u >> i & 1 && u >> i + 1 & 1) return false;
    return true;
}
// 统计 1 的数量
int count(int u) {
    int res = 0;
    for (int i = 0; i < n; i ++ )
        if (u >> i & 1) res ++;
    return res;
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 0; i < 1 << n; i ++ )
        if (check(i)) {
            state.push_back(i);
            cnt[i] = count(i);
        }
    for (int i = 0; i < state.size(); i ++ )
        for (int j = 0; j < state.size(); j ++ ) {
            int a = state[i], b = state[j];
            if ((a & b) == 0 && check(a | b)) convert[i].push_back(j);
        }
    f[0][0][0] = 1;
    for (int i = 1; i <= n + 1; i ++ )
        for (int j = 0; j <= m; j ++ )
            for (int a = 0; a < state.size(); a ++ )
                for (auto& b : convert[a])
                    if (j >= cnt[state[a]]) f[i][j][a] += f[i - 1][j - cnt[state[a]]][b];
    cout << f[n + 1][m][0] << endl;
    return 0;
}
```

##### [Acwing 327. 玉米田](https://www.acwing.com/problem/content/329/)

**题目描述**

> 农夫约翰的土地由 `M×N` 个小方格组成，现在他要在土地里种植玉米。
>
> 非常遗憾，部分土地是不育的，无法种植。
>
> 而且，相邻的土地不能同时种植玉米，也就是说种植玉米的所有方格之间都不会有公共边缘。
>
> 现在给定土地的大小，请你求出共有多少种种植方法。
>
> 土地上什么都不种也算一种方法。

**输入格式**

> 第 `1` 行包含两个整数 `M` 和 `N`。
>
> 第 `2..M+1`行：每行包含 `N` 个整数 `0` 或 `1`，用来描述整个土地的状况，`1` 表示该块土地肥沃，`0` 表示该块土地不育。

**输出格式**

> 输出总种植方法对 $10^8$ 取模后的值。

**数据范围**

> $1\;\leq{M},\;N\leq12$

**输入样例**

> 2 3
> 1 1 1
> 0 1 0

**输出样例**

> 9

**手写稿**

![玉米田](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%8E%89%E7%B1%B3%E7%94%B0.jpg)

**代码**

```c++
#include <iostream>
#include <vector>
using namespace std;
const int N = 15, M = 1 << 12, mod = 1e8;
int n, m;
int g[N];
int f[N][M];
vector<int> state;
vector<int> convert[M];
bool check(int u) {
    for (int i = 0; i < m; i ++ )
        if (u >> i & 1 && u >> i + 1 & 1) return false;
    return true;
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ )
        for (int j = 0, s; j < m; j ++ ) {
            scanf("%d", &s);
            // 通常情况下 1 代表有障碍 0 代表无障碍
            g[i] += !s << j;
        }
    for (int i = 0; i < 1 << m; i ++ )
        if (check(i))
            state.push_back(i);
    for (int i = 0; i < state.size(); i ++ )
        for (int j = 0; j < state.size(); j ++ ) {
            int a = state[i], b = state[j];
            if ((a & b) == 0 && check(a))
                convert[i].push_back(j);
        }
    f[0][0] = 1;
    // i 循环到 n + 1 小技巧 不需要额外在计算
    for (int i = 1; i <= n + 1; i ++ )
        for (int j = 0; j < state.size(); j ++ )
            for (auto& k : convert[j]) {
                // 如果同列的相邻两行之间都是 1 则放弃本次循环
                if (g[i] & state[j]) continue;
                f[i][j] = (f[i][j] + f[i - 1][k]) % mod;
            }
    cout << f[n + 1][0] << endl;
    return 0;
}
```

##### [LeetCode 213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

**题目描述**

> 你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 **围成一圈** ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。
>
> 给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。 

**示例 1**

> 输入：nums = [2,3,2]
> 输出：3
> 解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。

**示例 2**

> 输入：nums = [1,2,3,1]
> 输出：4
> 解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3），偷窃到的最高金额 = 1 + 3 = 4 。

**示例 3**

> 输入：nums = [0]
> 输出：0

**提示**

> `1 <= nums.length <= 100`
> `0 <= nums[i] <= 1000`

**思路**

> 由于第一家和最后一家是相连的，故此分为两组：
>
> 第一组：`1～n - 1`
>
> 第二组：`2 ～n`
>
> 分别进行动态规划即可

**手写稿**

![打家劫舍 II](https://gitee.com/peter95535/image-bed/raw/master/img/%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D%20II.png)

**代码**

```c++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) return nums[0];
        int l;
        vector<vector<int>> f(n + 5, vector<int>(2));
        for (int i = 2; i <= n; i ++ ) {
            f[i][0] = max(f[i - 1][0], f[i - 1][1]);
            f[i][1] = f[i - 1][0] + nums[i - 1];
        }
        l = max(f[n][1], f[n][0]);
        f = vector<vector<int>>(n + 5, vector<int>(2));
        for (int i = 1; i <= n - 1; i ++ ) {
            f[i][0] = max(f[i - 1][0], f[i - 1][1]);
            f[i][1] = f[i - 1][0] + nums[i - 1];
        }
        return max(l, max(f[n - 1][0], f[n - 1][1]));
    }
};
```

##### [LeetCode 221. 最大正方形](https://leetcode-cn.com/problems/maximal-square/)

**题目描述**

> 在一个由 `'0'` 和 `'1'` 组成的二维矩阵内，找到只包含 `'1'` 的最大正方形，并返回其面积。

**示例 1**

![最大正方形 图1](https://gitee.com/peter95535/image-bed/raw/master/img/%E6%9C%80%E5%A4%A7%E6%AD%A3%E6%96%B9%E5%BD%A2%20%E5%9B%BE1.jpeg)

> 输入：`matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]`
> 输出：`4`

**示例 2**

![最大正方形 图2](https://gitee.com/peter95535/image-bed/raw/master/img/%E6%9C%80%E5%A4%A7%E6%AD%A3%E6%96%B9%E5%BD%A2%20%E5%9B%BE2.jpeg)

> 输入：`matrix = [["0","1"],["1","0"]]`
> 输出：`1`

**示例 3**

> 输入：`matrix = [["0"]]`
> 输出：`0`



**提示**

> `m == matrix.length`
> `n == matrix[i].length`
> `1 <= m, n <= 300`
> `matrix[i][j] 为 '0' 或 '1'`

**法一：动态规划**

![最大正方形 动态规划](https://gitee.com/peter95535/image-bed/raw/master/img/%E6%9C%80%E5%A4%A7%E6%AD%A3%E6%96%B9%E5%BD%A2%20%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92.png)

**代码一：二维数组**

```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& g) {
        int n = g.size(), m = g[0].size();
        vector<vector<int>> f(n + 5, vector<int>(m + 5));
        int res = -1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                if (g[i - 1][j - 1] == '0') f[i][j] = 0;
                else f[i][j] = min(f[i - 1][j], min(f[i][j - 1], f[i - 1][j - 1])) + 1;
                res = max(res, f[i][j] * f[i][j]);
            }
        return res;
    }
};
```

**代码二：滚动数组优化**

```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& g) {
        int n = g.size(), m = g[0].size();
        vector<vector<int>> f(2, vector<int>(m + 5));
        int res = -1;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= m; j ++ ) {
                if (g[i - 1][j - 1] == '0') f[i & 1][j] = 0;
                else f[i & 1][j] = min(f[i - 1 & 1][j], min(f[i & 1][j - 1], f[i - 1 & 1][j - 1])) + 1;
                res = max(res, f[i & 1][j] * f[i & 1][j]);
            }
        return res;
    }
};
```

**法二：单调栈**

![最大正方形 单调栈](https://gitee.com/peter95535/image-bed/raw/master/img/%E6%9C%80%E5%A4%A7%E6%AD%A3%E6%96%B9%E5%BD%A2%20%E5%8D%95%E8%B0%83%E6%A0%88.png)

**代码**

```c++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& g) {
        int n = g.size(), m = g[0].size();
        vector<vector<int>> f(n + 5, vector<int>(m + 5));
        for (int i = 0; i < m; i ++ ) f[0][i] = g[0][i] - '0';
        for (int i = 1; i < n; i ++ )
            for (int j = 0; j < m; j ++ )
                if (g[i][j] == '1') f[i][j] = f[i - 1][j] + 1;
        int res = -1;
        for (int i = 0; i < n; i ++ ) {
            stack<int> sk;
            vector<int> left(m + 5), right(m + 5);
            for (int j = 0; j < m; j ++ ) {
                while (sk.size() && f[i][j] <= f[i][sk.top()]) sk.pop();
                if (sk.empty()) left[j] = -1;
                else left[j] = sk.top();
                sk.push(j);
            }
            sk = stack<int>();
            for (int j = m - 1; j >= 0; j -- ) {
                while (sk.size() && f[i][j] <= f[i][sk.top()]) sk.pop();
                if (sk.empty()) right[j] = m;
                else right[j] = sk.top();
                sk.push(j);
            }
            for (int j = 0; j < m; j ++ )
                if (right[j] - left[j] - 1 >= f[i][j])
                    res = max(res, f[i][j] * f[i][j]);
        }
        return res;
    }
};
```

##### [LeetCode 223. 矩形面积](https://leetcode-cn.com/problems/rectangle-area/)

**题目描述**

> 给你 **二维** 平面上两个 **由直线构成且边与坐标轴平行/垂直** 的矩形，请你计算并返回两个矩形覆盖的总面积。
>
> 每个矩形由其 **左下** 顶点和 **右上** 顶点坐标表示：
>
> 第一个矩形由其左下顶点 `(ax1, ay1)` 和右上顶点 `(ax2, ay2)` 定义。
> 第二个矩形由其左下顶点 `(bx1, by1)` 和右上顶点 `(bx2, by2)` 定义。

**示例 1**

![矩形面积 图1](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%9F%A9%E5%BD%A2%E9%9D%A2%E7%A7%AF%20%E5%9B%BE1.png)

> 输入：`ax1 = -3, ay1 = 0, ax2 = 3, ay2 = 4, bx1 = 0, by1 = -1, bx2 = 9, by2 = 2`
> 输出：`45`

**示例 2**

> 输入：`ax1 = -2, ay1 = -2, ax2 = 2, ay2 = 2, bx1 = -2, by1 = -2, bx2 = 2, by2 = 2`
> 输出：`16`

**提示**

> $-10^4\;<=\;ax1,\;ay1,\;ax2,\;ay2,\;bx1,\;by1,\;bx2,\;by2\;<=\;10^4$

**手写稿**

![矩形面积](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%9F%A9%E5%BD%A2%E9%9D%A2%E7%A7%AF.png)

**代码**

```c++
class Solution {
public:
    int computeArea(int ax1, int ay1, int ax2, int ay2, int bx1, int by1, int bx2, int by2) {
        int X = max(0, min(ax2, bx2) - max(ax1, bx1));
        int Y = max(0, min(ay2, by2) - max(ay1, by1));
        return (ax2 - ax1) * (ay2 - ay1) + (bx2 - bx1) * (by2 - by1) - X * Y;
    }
};
```

##### [LeetCode 300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

**题目描述**

> 给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。
>
> 子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

**示例 1**

> 输入：`nums = [10,9,2,5,3,7,101,18]`
> 输出：`4`
> 解释：最长递增子序列是 `[2,3,7,101]`，因此长度为 `4` 。

**示例 2**

> 输入：`nums = [0,1,0,3,2,3]`
> 输出：`4`

**示例 3**

> 输入：`nums = [7,7,7,7,7,7,7]`
> 输出：`1`

**提示**

> $1 <= nums.length <= 2500$
> $-10^4 <= nums[i] <= 10^4$

**进阶**

> 你可以设计时间复杂度为 $O(n^2)$ 的解决方案吗？
> 你能将算法的时间复杂度降低到 $O(n log(n))$ 吗?

**方法一：动态规划**

**手写稿**

![最长递增子序列](https://gitee.com/peter95535/image-bed/raw/master/img/%E6%9C%80%E9%95%BF%E9%80%92%E5%A2%9E%E5%AD%90%E5%BA%8F%E5%88%97.png)

**代码**

```c++
// 时间复杂度 O(n ^ 2)
class Solution {
public:
    int lengthOfLIS(vector<int>& g) {
        int n = g.size();
        vector<int> f(n + 5, 1);
        int res = 0;
        for (int i = 0; i < n; i ++) {
            for (int j = 0; j < i; j ++ )
                if (g[i] > g[j]) f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }
        return res;
    }
};
```

**方法二：二分查找**

**思路一：**

**步骤**

> 1. 使用 `vector` 存储长度为 `i` 的递增子序列的最小值，其下标 `i` 表示递增子序列的长度，其值 `q[i]` 表示长度为 `i` 的递增子序列的最小值
> 2. 在 `q` 数组中二分查找小于 `x` 的最大值，则下一个值一定就是大于等于 `x` 的最小值
>    + 注意点：如果当前值比 `q` 中的所有值都小，则将当前值设置为最小值；不过，不进行特判，按照算法步骤，寻找下一个值的话，就会造成下一个值比当前值小
>    + 例子：`4, 10, 4`
> 3. `q` 是一个递增严格子序列

**代码**

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> q;
        for (auto& num : nums) {
            if (q.empty() || num > q.back()) q.push_back(num);
            else {
                // 需要特判，如果 num 比最小值都小，则将最小值设置为 num
                if (q[0] >= num) q[0] = num;
                else {
                    // 二分查找小于当前数的最大值
                    int l = 0, r = q.size() - 1;
                    while (l < r) {
                        int mid = l + r + 1 >> 1;
                        if (q[mid] < num) l = mid;
                        else r = mid - 1;
                    }
                    q[l + 1] = num;
                }
            }
        }
        return q.size();
    }
};
```

**思路二：**

**步骤**

> 1. 同思路一，唯一不同的是，我们这次寻找的是大于等于 `x` 的最小值

**代码**

```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> q;
        for (auto& num : nums) {
            if (q.empty() || num > q.back()) q.push_back(num);
            else {
                // 二分查找大于等于当前数的最小值
                int l = 0, r = q.size() - 1;
                while (l < r) {
                    int mid = l + r >> 1;
                    if (nums[mid] >= num) r = mid;
                    else l = mid + 1;
                }
                q[r] = num;
            }
        }
        return q.size();
    }
};
```

##### end

#### 位运算专题

##### [LeetCode 260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

**题目描述**

> 给定一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。你可以按 任意顺序 返回答案。
>
> 进阶：你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

**示例 1**

> 输入：`nums = [1,2,1,3,2,5]`
> 输出：`[3,5]`
> 解释：`[5, 3]` 也是有效的答案。

**示例 2**

> 输入：`nums = [-1,0]`
> 输出：`[-1,0]`

**示例 3**

> 输入：`nums = [0,1]`
> 输出：`[1,0]`

**提示**

> + $2\;\leq\;nums.length\;\leq\;3\;*\;10^4$
> + $-2^{31}\;\leq\;nums[i]\;\leq\;2^{31}\;-\;1$
> + 除两个只出现一次的整数外，nums 中的其他数字都出现两次

**题解**

> 1. 将所有数字进行异或之后得到的值 `ab` 即为只出现一次的那两个元素 `a` 和 `b` 的异或值
> 2. 由异或的性质可知 `ab` 的二进制表示中某一位 `k`上的值为 `1` ，说明元素 `a` 和 元素 `b` 在 `k` 上的二进制是不同的。因此，可以根据第 `k` 位进行划分，将整个数组划分成为两部分，一部分表示第 `k` 位的值为 `1` ，另一部分表示第 `k` 位的值为 `0`，此时问题变成在一个数组中，只有一个数出现一次，其他数出现两次，可解

**代码**

```c++
class Solution {
public:
    int get(vector<int>& nums, int k, int u) {
        int res = 0;
        for (auto& num : nums)
            if ((num >> k & 1) == u) res ^= num;
        return res;
    }
    vector<int> singleNumber(vector<int>& nums) {
        int ab = 0;
        for (auto& num : nums) ab ^= num;
        int k = 0;
        while ((ab >> k & 1) == 0) k ++;
        return {get(nums, k, 1), get(nums, k, 0)};
    }
};
```

##### end

#### 数学相关

##### [LeetCode 264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)

**题目描述**

> 给你一个整数 `n` ，请你找出并返回第 `n` 个丑数 。
>
> 丑数 就是只包含质因数 `2、3` 和/或 `5` 的正整数。

**示例 1**

> 输入：`n = 10`
> 输出：`12`
> 解释：`[1, 2, 3, 4, 5, 6, 8, 9, 10, 12]` 是由前 `10` 个丑数组成的序列。

**示例 2**

> 输入：`n = 1`
> 输出：`1`
> 解释：`1` 通常被视为丑数。

**提示**

> $1\;\leq\;n\;\leq\;1690$

**题解**

> 1. 若 `a` 是丑数，则 `a * 2` 也是丑数
>
>    + $$
>      若a是丑数，则a可以表示成 \\
>      a = p_1^{n}\;*\;p_2^{m}\;*\;p_3^{k}，\;其中p_1为2，p_2为3，p_3为5 \\
>      现在来看\;a\;*\;p = p_1^{n}\;*\;p_2^{m}\;*\;p_3^{k}\;*\;p
>      $$
>
> 2. 三指针做法

**手写稿**

![丑数 II](https://gitee.com/peter95535/image-bed/raw/master/img/%E4%B8%91%E6%95%B0%20II.png)

**代码**

```c++
class Solution {
public:
    int nthUglyNumber(int n) {
        vector<int> q(1, 1);
        for (int i = 0, j = 0, k = 0; q.size() < n; ) {
            int t = min(q[i] * 2, min(q[j] * 3, q[k] * 5));
            q.push_back(t);
            // 不能使用 if else 因为会有相同值
            if (q[i] * 2 == t) i ++;
            if (q[j] * 3 == t) j ++;
            if (q[k] * 5 == t) k ++;
        }
        return q.back();
    }
};
```

##### [LeetCode 279. 完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

**题目描述**

> 给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。
>
> 给你一个整数 n ，返回和为 n 的完全平方数的 最少数量 。
>
> 完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。

 **示例 1**

> 输入：`n = 12`
> 输出：`3` 
> 解释：`12 = 4 + 4 + 4`

**示例 2**

> 输入：`n = 13`
> 输出：`2`
> 解释：`13 = 4 + 9`

**提示**

> $1 <= n <= 10^4$

**法一:动态规划**

**手写稿**

![完全平方数](https://gitee.com/peter95535/image-bed/raw/master/img/%E5%AE%8C%E5%85%A8%E5%B9%B3%E6%96%B9%E6%95%B0.png)

**代码**

```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> f(n + 5, 1e9);
        f[0] = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 1; j <= i / j; j ++ )
                f[i] = min(f[i], f[i - j * j] + 1);
        return f[n];
    }
};
```

**方法二:数学**

**步骤**

> 1. [四平方和定理](https://baike.baidu.com/item/%E5%9B%9B%E5%B9%B3%E6%96%B9%E5%92%8C%E5%AE%9A%E7%90%86):每个正整数均可表示为4个整数的平方和,定义此题的上限,答案只能为 `1, 2, 3, 4`
> 2. [勒让德三平方和定理](https://en.wikipedia.org/wiki/Legendre%27s_three-square_theorem):当 $n=4^a(8b+7)$时，`n` 不能写成 `3` 个数的平方和
> 3. 枚举答案是否为 `2`

**代码**

```c++
class Solution {
public:
    bool check(int n) {
        int r = sqrt(n);
        return r * r == n;
    }
    int numSquares(int n) {
        // 判断是否是一个数字
        if (check(n)) return 1;
        // 判断是否是两个数字
        for (int i = 1; i <= n / i; i ++ )
            if (check(n - i * i)) return 2;
        // 判断是否是四个数字
        while (n % 4 == 0) n /= 4;
        if (n % 8 == 7) return 4;
        return 3;
    }
};
```



##### end

#### 滑动窗口专题

##### [LeetCode 5977. 最少交换次数来组合所有的 1 II](https://leetcode-cn.com/problems/minimum-swaps-to-group-all-1s-together-ii/)

**题目描述**

> 交换 定义为选中一个数组中的两个 互不相同 的位置并交换二者的值。
>
> 环形 数组是一个数组，可以认为 第一个 元素和 最后一个 元素 相邻 。
>
> 给你一个 二进制环形 数组 `nums` ，返回在 任意位置 将数组中的所有 `1` 聚集在一起需要的最少交换次数。

**示例 1**

> 输入：`nums = [0,1,0,1,1,0,0]`
> 输出：`1`
> 解释：这里列出一些能够将所有 1 聚集在一起的方案：
> `[0,0,1,1,1,0,0]` 交换 `1` 次。
> `[0,1,1,1,0,0,0]` 交换 `1` 次。
> `[1,1,0,0,0,0,1]` 交换 `2` 次（利用数组的环形特性）。
> 无法在交换 `0` 次的情况下将数组中的所有 `1` 聚集在一起。
> 因此，需要的最少交换次数为 `1` 。

**示例 2**

> 输入：`nums = [0,1,1,1,0,0,1,1,0]`
> 输出：`2`
> 解释：这里列出一些能够将所有 `1` 聚集在一起的方案：
> `[1,1,1,0,0,0,0,1,1]` 交换 `2` 次（利用数组的环形特性）。
> `[1,1,1,1,1,0,0,0,0]` 交换 `2` 次。
> 无法在交换 `0` 次或 `1` 次的情况下将数组中的所有 `1` 聚集在一起。
> 因此，需要的最少交换次数为 `2` 。

**示例 3**

> 输入：`nums = [1,1,0,0,1]`
> 输出：`0`
> 解释：得益于数组的环形特性，所有的 `1` 已经聚集在一起。
> 因此，需要的最少交换次数为 `0` 。

**提示**

> $1\;\leq\;nums.length\;\leq\;10^5$
> `nums[i]` 为 `0` 或者 `1`

**题解**

> 1. 统计出 `1` 的个数，作为滑动窗口的大小
> 2. 将数组复制一遍，目的是断环成链
> 3. 答案即为窗口内 `0` 的最小个数

**代码**

```c++
class Solution {
public:
    int minSwaps(vector<int>& nums) {
        // 滑动窗口
        // 第一步：统计 1 的个数 作为窗口的大小
        int cnt = 0;
        for (auto& num : nums) cnt += num;
        // 第二步：断环成链
        nums.insert(nums.begin(), nums.begin(), nums.end());
        int res = INT_MAX, tmp = 0;
        for (int i = 0, j = 0; i < nums.size(); i ++ ) {
            if (i + 1 > cnt) tmp -= nums[j ++];
            // tmp 统计的是窗口内 1 的个数
            tmp += nums[i];
            res = min(res, cnt - tmp);
        }
        return res;
    }
};
```

##### end

#### 阅读理解题

##### [LeetCode 275. H 指数 II](https://leetcode-cn.com/problems/h-index-ii/)

**题目描述**

> 给你一个整数数组 `citations` ，其中 `citations[i]` 表示研究者的第 `i` 篇论文被引用的次数，`citations` 已经按照 升序排列 。计算并返回该研究者的 `h` 指数。
>
> `h` 指数的定义：`h` 代表“高引用次数”（`high citations`），一名科研人员的 `h` 指数是指他（她）的 （`n` 篇论文中）总共有 `h` 篇论文分别被引用了至少 `h` 次。且其余的 `n - h` 篇论文每篇被引用次数 不超过 `h` 次。
>
> 提示：如果 `h` 有多种可能的值，h 指数 是其中最大的那个。
>
> 请你设计并实现对数时间复杂度的算法解决此问题。

**示例 1**

> 输入：`citations = [0,1,3,5,6]`
> 输出：`3`
> 解释：给定数组表示研究者总共有 `5` 篇论文，每篇论文相应的被引用了 `0, 1, 3, 5, 6` 次。
>      由于研究者有 `3` 篇论文每篇 至少 被引用了 `3` 次，其余两篇论文每篇被引用 不多于 `3` 次，所以她的 `h` 指数是 `3` 。

**示例 2**

> 输入：`citations = [1,2,100]`
> 输出：`2`

**提示**

> + $n == citations.length$
> + $1 <= n <= 10^5$
> + $0 <= citations[i] <= 1000$
> + $citations 按 升序排列$

**分析**

> 1. 将数组从大到小排序，然后从前往后一次枚举每一个数字，如果当前数字 `g[i] >= n - i`，则找到答案
> 2. 本题中由于按照从小到大排序的，所以做的时候需要进行坐标变换 `g[n - mid]` 表示的是从大到小的第 `mid` 个数字
> 3. 本题中二分的是答案也就是 `H` 的值，最小是 `0`，最大是 `n`

**代码**

```c++
class Solution {
public:
    int hIndex(vector<int>& g) {
        int n = g.size(), l = 0, r = n;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (g[n - mid] >= mid) l = mid;
            else r = mid - 1;
        }
        return l;
    }
};
```

##### end

#### 双指针专题

##### 快慢指针

###### [LeetCode 141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

**题目描述**

> 给你一个链表的头节点 `head` ，判断链表中是否有环。
>
> 如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 `0` 开始）。如果 `pos` 是 `-1`，则在该链表中没有环。注意：`pos` 不作为参数进行传递，仅仅是为了标识链表的实际情况。
>
> 如果链表中存在环，则返回 `true` 。 否则，返回 `false` 。 

**示例 1**

![环状链表 图1](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%8E%AF%E7%8A%B6%E9%93%BE%E8%A1%A8%20%E5%9B%BE1.png)

> 输入：`head = [3,2,0,-4], pos = 1`
> 输出：`true`
> 解释：链表中有一个环，其尾部连接到第二个节点。

**示例 2**

![环形链表 图2](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8%20%E5%9B%BE2.png)

> 输入：`head = [1,2], pos = 0`
> 输出：`true`
> 解释：链表中有一个环，其尾部连接到第一个节点。

**示例 3**

![环形链表 图3](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8%20%E5%9B%BE3.png)

> 输入：`head = [1], pos = -1`
> 输出：`false`
> 解释：链表中没有环。

**提示**

> 链表中节点的数目范围是 $[0, 10^4]$
> $-10^5 <= Node.val <= 10^5$
> `pos` 为 `-1` 或者链表中的一个 有效索引 。
>
>
> 进阶：你能用 O(1)（即，常量）内存解决此问题吗？

**题解**

> 1. 设置 `fast` 和 `slow` 两个指针
> 2. `fast` 一次走两步，`slow` 一次走一步，分两种情况：
>    + `fast` 走到空指针，说明无环
>    + `fast` 和 `slow` 相等，说明有环

**代码**

```c++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head) return false;
        auto fast = head, slow = head;
        do {
            fast = fast -> next;
            slow = slow -> next;
            if (fast) fast = fast -> next;
            else return false;
            if (fast == slow) return true;
        } while (fast);
        return false;
    }
};
```

###### [LeetCode 142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

**题目描述**

> 给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。
>
> 如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 `0` 开始）。如果 `pos` 是 `-1`，则在该链表中没有环。注意：`pos` 不作为参数进行传递，仅仅是为了标识链表的实际情况。
>
> 不允许修改 链表。

**示例 1**

![环形链表 II 图1](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8%20II%20%E5%9B%BE1.png)

> 输入：`head = [3,2,0,-4], pos = 1`
> 输出：返回索引为 `1` 的链表节点
> 解释：链表中有一个环，其尾部连接到第二个节点。

**示例 2**

![环形链表 II 图 2](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8%20II%20%E5%9B%BE%202.png)

> 输入：`head = [1,2], pos = 0`
> 输出：返回索引为 `0` 的链表节点
> 解释：链表中有一个环，其尾部连接到第一个节点。

**示例 3**

![环形链表 II 图3](https://gitee.com/peter95535/image-bed/raw/master/img/%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8%20II%20%E5%9B%BE3.png)

> 输入：`head = [1], pos = -1`
> 输出：返回 `null`
> 解释：链表中没有环。

**提示**

> + 链表中节点的数目范围在范围 $[0, 10^4]$ 内
> + $-10^5 <= Node.val <= 10^5$
> + `pos` 的值为 `-1` 或者链表中的一个有效索引

**进阶**

> 你是否可以使用 `O(1)` 空间解决此题？

**题解**

> 1. 设置 `fast` 和 `slow` 指针
> 2. `fast` 指针一次走两步，`slow` 指针一次走一步，分两种情况讨论
>    + 如果 `fast` 指针为空，说明无环
>    + 如果 `fast == slow`，则让 `fast == head`，然后让 `fast` 和 `slow` 指针同时走，当两者相遇时，即为环的入口

**正确性证明**

> 
>
> 证明如下
> $$
> 当fast指针和slow指针两者相遇的时候，满足l_{fast}\;=\;2\;*\;l_{slow}\;\;(1) \\
> l_{fast}\;=\;l_{ab}\;+\;n\;*\;l_{圆}\;+\;l_{bc}\;\;(2) \\
> l_{slow}\;=\;l_{ab}\;+\;l_{bc}\;\;(3)，结合(1)(2)(3)式，有 \\
> \begin{align*}
>   l_{ab}\;+\;l_{bc}&=\;n\;*\;l_{圆} \\
>    &= (n\;-\;1)\;*\;l_{圆}\;+\;l_{bc}\;+\;l_{cb}，从而有
>    \end{align*}
> \\
> l_{ab} = (n\;-\;1)\;*\;l_{圆}\;+\;l_{cb}
> $$

**代码**

 ```c++
 class Solution {
 public:
     ListNode *detectCycle(ListNode *head) {
         if (!head) return NULL;
         auto fast = head, slow = head;
         do {
             fast = fast -> next;
             slow = slow -> next;
             if (fast) fast = fast -> next;
             if (!fast) return NULL;
         } while (fast != slow);
         fast = head;
         while (fast != slow) {
             fast = fast -> next;
             slow = slow -> next;
         }
         return fast;
     }
 };
 ```

###### [LeetCode 287. 寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

**题目描述**

> 给定一个包含 `n + 1` 个整数的数组 `nums` ，其数字都在 `1` 到 `n` 之间（包括 `1` 和 `n`），可知至少存在一个重复的整数。
>
> 假设 `nums` 只有 一个重复的整数 ，找出 这个重复的数 。
>
> 你设计的解决方案必须不修改数组 `nums` 且只用常量级 `O(1)` 的额外空间。

**示例 1**

> 输入：`nums = [1,3,4,2,2]`
> 输出：`2`

**示例 2**

> 输入：`nums = [3,1,3,4,2]`
> 输出：`3`

**示例 3**

> 输入：`nums = [1,1]`
> 输出：`1`

**示例 4**

> 输入：`nums = [1,1,2]`
> 输出：`1`

**提示**

> + $1 <= n <= 10^5$
> + $nums.length == n + 1$
> + $1 <= nums[i] <= n$
> + `nums` 中 只有一个整数 出现 两次或多次 ，其余整数均只出现 一次

**进阶**

> 如何证明 `nums` 中至少存在一个重复的数字?
> 你可以设计一个线性级时间复杂度 `O(n)` 的解决方案吗？

**题解**

> 1. `n + 1` 个整数，且数字都在 `1 ～ n` 之间，因此，一定有一个数字被指向两次，以下标和下标对应的数字连接一条有向边的话，则问题转化为寻找环的入口的问题，快慢指针做即可

**代码**

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int fast = 0, slow = 0;
        do {
            fast = nums[nums[fast]];
            slow = nums[slow];
        } while (fast != slow);
        fast = 0;
        while (fast != slow) {
            fast = nums[fast];
            slow = nums[slow];
        }
        return fast;
    }
};
```

###### end

#### 模拟题专题

##### [LeetCode 299. 猜数字游戏](https://leetcode-cn.com/problems/bulls-and-cows/)

**题目描述**

> 你在和朋友一起玩 猜数字`（Bulls and Cows）`游戏，该游戏规则如下：
>
> 写出一个秘密数字，并请朋友猜这个数字是多少。朋友每猜测一次，你就会给他一个包含下述信息的提示：
>
> 猜测数字中有多少位属于数字和确切位置都猜对了（称为 "`Bulls`"，公牛），
> 有多少位属于数字猜对了但是位置不对（称为 "`Cows`"，奶牛）。也就是说，这次猜测中有多少位非公牛数字可以通过重新排列转换成公牛数字。
> 给你一个秘密数字 `secret` 和朋友猜测的数字 `guess` ，请你返回对朋友这次猜测的提示。
>
> 提示的格式为 "`xAyB`" ，`x` 是公牛个数， `y` 是奶牛个数，`A` 表示公牛，`B` 表示奶牛。
>
> 请注意秘密数字和朋友猜测的数字都可能含有重复数字。

**示例 1**

> 输入：`secret = "1807", guess = "7810"`
> 输出：`"1A3B"`
> 解释：数字和位置都对（公牛）用 `'|'` 连接，数字猜对位置不对（奶牛）的采用斜体加粗标识。
> `"1807"
>   |
> "7810"`

**示例 2**

> 输入：`secret = "1123", guess = "0111"`
> 输出：`"1A1B"`
> 解释：数字和位置都对（公牛）用 `'|'` 连接，数字猜对位置不对（奶牛）的采用斜体加粗标识。
> `"1123"        "1123"
>   |      or     |
> "0111"        "0111"`
> 注意，两个不匹配的 `1` 中，只有一个会算作奶牛（数字猜对位置不对）。通过重新排列非公牛数字，其中仅有一个 `1` 可以成为公牛数字。

**示例 3**

> 输入：`secret = "1", guess = "0"`
> 输出：`"0A0B"`

**示例 4**

> 输入：`secret = "1", guess = "1"`
> 输出：`"1A0B"`

**提示**

> + $1 <= secret.length, guess.length <= 1000$
> + $secret.length == guess.length$
> + `secret` 和 `guess` 仅由数字组成

**题解**

> 1. 两次遍历；
> 2. 第一次遍历数组并且统计公牛的次数，同时将不满足公牛的字符使用多重集合记录下来
> 3. 第二次遍历数组并且利用多重集合统计奶牛的次数，注意一个数字只能对应一个数字，因此，如果比对成功需要将成功的字符从多重集合里面删除

**代码**

```c++
class Solution {
public:
    string getHint(string s, string t) {
        int a = 0, b = 0;
        multiset<char> hash;
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == t[i]) a ++;
            else hash.insert(s[i]);
        }
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == t[i]) continue;
            if (hash.count(t[i])) {
                b ++;
                hash.erase(hash.find(t[i]));
            }
        }
        return to_string(a) + "A" + to_string(b) + "B";
    }
};
```

###### end

#### 字典树

##### [LeetCode 212. 单词搜索 II](https://leetcode-cn.com/problems/word-search-ii/)

**题目描述**

> 给定一个 `m x n` 二维字符网格 `board` 和一个单词（字符串）列表 `words`，找出所有同时在二维网格和字典中出现的单词。
>
> 单词必须按照字母顺序，通过 相邻的单元格 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

**示例 1**

> 输入：`board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]`
> 输出：`["eat","oath"]`

**示例 2**

> 输入：`board = [["a","b"],["c","d"]], words = ["abcb"]`
> 输出：`[]`

**提示**

> + $m == board.length$
> + $n == board[i].length$
> + $1 <= m, n <= 12$
> + $board[i][j] 是一个小写英文字母$
> + $1 <= words.length <= 3 * 10^4$
> + $1 <= words[i].length <= 10$
> + `words[i]` 由小写英文字母组成
> + `words` 中的所有字符串互不相同

**题解**

> 1. 使用字典树存储每个单词
> 2. 从每一个起点，深度优先搜索每个单词即可

**代码**

```c++
class Solution {
public:
    // 定义字典树结构体
    struct Node {
        int id;
        Node* son[26];
        Node() {
            id = -1;
            for (int i = 0; i < 26; i ++ ) son[i] = NULL;
        }
    }*root;
    int n, m;
    int dx[4] = {1, -1, 0, 0};
    int dy[4] = {0, 0, -1, 1};
    unordered_set<int> ids;
    void insert(string word, int id) {
        auto p = root;
        for (int i = 0; i < word.size(); i ++ ) {
            int u = word[i] - 'a';
            if (!p -> son[u]) p -> son[u] = new Node();
            p = p -> son[u];
        }
        p -> id = id;
        return;
    }
    void dfs(vector<vector<char>>& g, int x, int y, Node* p) {
        if (p -> id != -1) ids.insert(p -> id);
        char t = g[x][y];
        g[x][y] = '.';
        for (int i = 0; i < 4; i ++ ) {
            int tx = x + dx[i];
            int ty = y + dy[i];
            if (tx < 0 || tx >= n || ty < 0 || ty >= m || g[tx][ty] == '.') continue;
            int u = g[tx][ty] - 'a';
            if (p -> son[u]) dfs(g, tx, ty, p -> son[u]);
        }
        g[x][y] = t;
        return;
    }
    vector<string> findWords(vector<vector<char>>& g, vector<string>& words) {
        n = g.size(), m = g[0].size();
        root = new Node();
        for (int i = 0; i < words.size(); i ++ ) insert(words[i], i);
        vector<string> res;
        for (int i = 0; i < n; i ++ )
            for (int j = 0; j < m; j ++ ) {
                int u = g[i][j] - 'a';
                if (root -> son[u]) dfs(g, i, j, root -> son[u]);
            }
        for (auto& id : ids) res.push_back(words[id]);
        return res;
    }
};
```

#### 字符串专题

##### KMP

###### [LeetCode 214. 最短回文串](https://leetcode-cn.com/problems/shortest-palindrome/)

**题目描述**

> 给定一个字符串 s，你可以通过在字符串前面添加字符将其转换为回文串。找到并返回可以用这种方式转换的最短回文串。

**示例 1**

> 输入：`s = "aacecaaa"`
> 输出：`"aaacecaaa"`

**示例 2**

> 输入：`s = "abcd"`
> 输出：`"dcbabcd"`

**提示**

> + $0 <= s.length <= 5 * 10^4$
> + $s$ 仅由小写英文字母组成

**手写稿**

![WechatIMG95](https://gitee.com/peter95535/image-bed/raw/master/img/WechatIMG95.jpeg)

**思路**

> 1. 要想让 `c + a + b` 最短，则只要 `c` 和 `b` 最短即可，换句话说，让 `a` 最长即可
>
> 2. 通过上述分析，问题转化为在原串`（a + b）`中寻找最长回文子串即可解决问题
>
> 3. 通过以下步骤，寻找最长回文子串即可
>
>    + 将原串逆序复制一遍，用 `#` 连接，放在原串的后面
>    
>      ![image-20220118072649711](https://gitee.com/peter95535/image-bed/raw/master/img/image-20220118072649711.png)
>    
>    + 寻找新串 `a + b + b + a` 的最长回文前缀即可，即 `KMP` 的 `next` 数组的含义
>    
> 4. 找出 `a` 之后，将 `a` 之后的部分，即 `b` 截取出来，逆序放到最前方
>

**代码**

```c++
class Solution {
public:
    string shortestPalindrome(string s) {
        string t = s;
        t = ' ' + t + '#' + string(t.rbegin(), t.rend());
        int n = t.size();
        vector<int> ne(n + 5);
        for (int i = 2, j = 0; i < n; i ++ ) {
            while (j && t[i] != t[j + 1]) j = ne[j];
            if (t[i] == t[j + 1]) j ++;
            ne[i] = j;
        }
        t = s.substr(ne[n - 1]);
        return string(t.rbegin(), t.rend()) + s;
    }
};
```

#### 扫描线问题

##### [LeetCode 218. 天际线问题](https://leetcode-cn.com/problems/the-skyline-problem/)

**问题描述**

> 城市的天际线是从远处观看该城市中所有建筑物形成的轮廓的外部轮廓。给你所有建筑物的位置和高度，请返回由这些建筑物形成的 天际线 。
>
> 每个建筑物的几何信息由数组 `buildings` 表示，其中三元组 `buildings[i] = [lefti, righti, heighti]` 表示：
>
> + `lefti` 是第 `i` 座建筑物左边缘的 `x` 坐标。
> + `righti` 是第 `i` 座建筑物右边缘的 `x` 坐标。
> + `heighti` 是第 `i` 座建筑物的高度。
>
> 天际线 应该表示为由 “关键点” 组成的列表，格式 `[[x1,y1],[x2,y2],...]` ，并按 `x` 坐标 进行 排序 。关键点是水平线段的左端点。列表中最后一个点是最右侧建筑物的终点，`y` 坐标始终为 `0` ，仅用于标记天际线的终点。此外，任何两个相邻建筑物之间的地面都应被视为天际线轮廓的一部分。
>
> 注意：输出天际线中不得有连续的相同高度的水平线。例如 `[...[2 3], [4 5], [7 5], [11 5], [12 7]...]` 是不正确的答案；三条高度为 `5` 的线应该在最终输出中合并为一个：`[...[2 3], [4 5], [12 7], ...]`

**示例 1**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/merged.jpg)

> 输入：`buildings = [[2,9,10],[3,7,15],[5,12,12],[15,20,10],[19,24,8]]`
> 输出：`[[2,10],[3,15],[7,12],[12,0],[15,10],[20,8],[24,0]]`
> 解释：
> 图 `A` 显示输入的所有建筑物的位置和高度，
> 图 `B` 显示由这些建筑物形成的天际线。图 `B` 中的红点表示输出列表中的关键点。

**示例 2**

> 输入：`buildings = [[0,2,3],[2,5,3]]`
> 输出：`[[0,3],[5,0]]`

**提示**

> + $1 <= buildings.length <= 10^4$
> + $0 <= left_i < right_i <= 2^{31} - 1$
> + $1 <= height_i <= 2^{31} - 1$
> + $buildings 按 left_i 非递减排序$

**题解**

> 1. 将横坐标按照从小到大排序，如果横坐标相同，则分下列三种情况：
>
>    + 如果是左端点，则按照从大到小排序
>
>      ![1](https://gitee.com/peter95535/image-bed/raw/master/img/1.jpg)
>
>    + 如果是右端点，则按照从小到大排序
>
>      ![2](https://gitee.com/peter95535/image-bed/raw/master/img/2.jpg)
>
>    + 如果是左右端点重合，则按照先处理左端点，再处理右端点
>
>      ![3](https://gitee.com/peter95535/image-bed/raw/master/img/3.jpg)
>
> 2. 使用 `pair` （默认升序）数组 `point` 来存储每一个点，排序，由于第三种情况的存在，所以，将左端点的值存储为负数，作用有二：
>
>    + 当左右端点重合的时候，可以保证的是先处理左端点，再处理右端点、
>    + 可以根据正负号，判断当前处理的是左端点还是右端点
>
> 3. 遍历 `point` 数组，使用 `multiset` 存储高度，分为两种情况：
>
>    + 如果当前遍历的点是左端点并且当前点大于 `multiset` 中的最大值，说明高度升高，一定产生关键点，记录答案，并将当前点插入 `multiset` 中
>    + 如果当前遍历的点是右端点 `h` ，先将右端点 `h` 删除，查看最大值，若 `h` 大于 `multiset` 的最大值，则说明 `h` 就是最大值，那么高度也会发生变化，即高度变低，记录答案即可
>
> 4. 问题解答
>
>    + 为啥要分左端点和右端点处理，题目不是说水平线段的左端点？
>
>      ![image-20220118134627011](https://gitee.com/peter95535/image-bed/raw/master/img/image-20220118134627011.png)
>
>      + 如图，答案并不都是在左端点，其中一个答案在红色矩形的右端点

**代码**

```c++
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& g) {
        vector<vector<int>> res;
        vector<pair<int, int>> point;
        multiset<int> height;
        for (auto& item : g) {
            point.push_back({item[0], -item[2]});
            point.push_back({item[1], item[2]});
        }
        // 先将x轴加入答案
        height.insert(0);
        sort(point.begin(), point.end());
        for (auto& p : point) {
            int x = p.first, h = abs(p.second);
            if (p.second < 0) { // 左端点
                if (h > *height.rbegin()) res.push_back({x, h});
                height.insert(h);
            }
            else { // 右端点
                height.erase(height.find(h));
                if (h > *height.rbegin()) res.push_back({x, *height.rbegin()});
            }
        }
        return res;
    }
};
```

#### end
