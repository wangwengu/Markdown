### 动态规划专题

#### 树形`DP`

##### 树的直径经典求法

**前提条件**

![2091531](https://gitee.com/peter95535/image-bed/raw/master/img/2091531.png)

**定理**

> 任意寻找树中的某一点，寻找树中距离当前节点最远的点 `u`，再寻找距离 `u` 最远的点 `v`，则树的直径即为 `u` 到 `v` 经过的节点数目

**证明**

![2091538](https://gitee.com/peter95535/image-bed/raw/master/img/2091538.png)

##### [AcWing 1072. 树的最长路径](https://www.acwing.com/problem/content/1074/)

**题目描述**

> 给定一棵树，树中包含 `n` 个结点（编号`1~n`）和 `n−1` 条无向边，每条边都有一个权值。
>
> 现在请你找到树中的一条最长路径。
>
> 换句话说，要找到一条路径，使得使得路径两端的点的距离最远。
>
> 注意：路径中可以只包含一个点。

**输入格式**

> 第一行包含整数 `n`。
>
> 接下来 `n−1` 行，每行包含三个整数 $a_i,b_i,c_i$，表示点 $a_i$ 和 $b_i$ 之间存在一条权值为 $c_i$ 的边。

**输出格式**

> 输出一个整数，表示树的最长路径的长度。

**数据范围**

> + $1≤n≤10000,$
> + $1≤a_i,b_i≤n,$
> + $−10^5≤c_i≤10^5$

**输入样例**

```c++
6
5 1 6
1 4 5
6 3 9
2 6 8
6 1 7
```

**输出样例**

```c++
22
```

**手写稿**

![2101347](https://gitee.com/peter95535/image-bed/raw/master/img/2101347.png)

**代码**

```c++
#include <iostream>
#include <cstring>
using namespace std;
const int N = 10010, M = N << 1;
int n, idx, ans;
int h[N], e[M], w[M], ne[M];
void add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = h[a];
    h[a] = idx ++ ;
    return;
}
// 返回以u为根结点的最大权值和
int dfs(int u, int father) {
    // d1表示最大值，d2表示次大值
    int d1 = 0, d2 = 0;
    // 以u为根结点的子树的最大权值和
    int dist = 0;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (j == father) continue; // 原因在题解中已经解释
        int d = dfs(j, u) + w[i];
        dist = max(dist, d);
        if (d >= d1) d2 = d1, d1 = d;
        else if (d >= d2) d2 = d;
    }
    ans = max(ans, d1 + d2);
    return dist;
}
int main() {
    scanf("%d", &n);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ ) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
        add(b, a, c);
    }
    dfs(1, -1);
    cout << ans << endl;
    return 0;
}
```

**标签**

`树形DP`

##### [AcWing 846. 树的重心](https://www.acwing.com/problem/content/description/848/)

**题目描述**

> 给定一颗树，树中包含 `n` 个结点（编号 `1∼n`）和 `n−1` 条无向边。
>
> 请你找到树的重心，并输出将重心删除后，剩余各个连通块中点数的最大值。
>
> 重心定义：重心是指树中的一个结点，如果将这个点删除后，剩余各个连通块中点数的最大值最小，那么这个节点被称为树的重心。

**输入格式**

> 第一行包含整数 `n`，表示树的结点数。
>
> 接下来 `n−1` 行，每行包含两个整数 `a` 和 `b`，表示点 `a` 和点 `b` 之间存在一条边。

**输出格式**

> 输出一个整数 `m`，表示将重心删除后，剩余各个连通块中点数的最大值。

**数据范围**

> + $1≤n≤10^5$

**输入样例**

```c++
9
1 2
1 7
1 4
2 8
2 5
4 3
3 9
4 6
```

**输出样例**

```c++
4
```

**手写稿**

![2092103](https://gitee.com/peter95535/image-bed/raw/master/img/2092103.png)

**注意事项**

> 1. 为啥 `dfs(int u, int father)` 要定义一个 `father` 变量，以防止 `圈2` 操作，从而导致无限循环
>
>    ![2092107](https://gitee.com/peter95535/image-bed/raw/master/img/2092107.png)
>

**代码**

```c++
#include <iostream>
#include <cstring>
using namespace std;
const int N = 100010, M = N << 1;
int n, idx, ans = 1e9;
int h[N], e[M], ne[M];
void add(int a, int b) {
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx ++;
    return;
}
int dfs(int u, int father) { // 以u为根节点的树中的节点个数
    int res = 0; // 以u为根结点的树中的节点个数的最大值
    int num = 1; // 各个子树的节点的个数，至少包含当前的节点，故初始值为1
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        // 如果当前遍历的点是其父亲节点，则返回，防止产生死循环，原因已解释
        if (j == father) continue;
        int d = dfs(j, u);
        num += d;
        res = max(res, d);
    }
    ans = min(ans, max(res, n - num));
    return num;
}
int main() {
    scanf("%d", &n);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ ) {
        int a, b;
        scanf("%d%d", &a, &b);
        // 无向图
        add(a, b);
        add(b, a);
    }
    dfs(1, -1);
    cout << ans << endl;
    return 0;
}
```

**标签**

`动态规划`、`树形DP`、`深度优先搜索`

##### [AcWing 1073. 树的中心](https://www.acwing.com/problem/content/1075/)

**题目描述**

> 给定一棵树，树中包含 `n` 个结点（编号`1~n`）和 `n−1` 条无向边，每条边都有一个权值。
>
> 请你在树中找到一个点，使得该点到树中其他结点的最远距离最近。

**输入格式**

> 第一行包含整数 `n`。
>
> 接下来 `n−1` 行，每行包含三个整数 $a_i,b_i,c_i$，表示点 $a_i$ 和 $b_i$ 之间存在一条权值为 $c_i$ 的边。

**输出格式**

> 输出一个整数，表示所求点到树中其他结点的最远距离。

**数据范围**

> + $1≤n≤10000,$
> + $1≤a_i,b_i≤n,$
> + $1≤c_i≤10^5$

**输入样例**

```c++
5 
2 1 1 
3 2 1 
4 3 1 
5 1 1
```

**输出样例**

```c++
2
```

**手写稿**

![2171435](https://gitee.com/peter95535/image-bed/raw/master/img/2171435.png)

**代码**

```c++
#include <iostream>
#include <cstring>
using namespace std;
const int N = 10010, M = N << 1, INF = 0x3f3f3f3f;
int n, idx;
// d1[u]和d2[u]分别表示根为u的最长路径和次长路径
// up[u]表示向u的上方搜索的最长路径
// p1[u]表示u的最大值来自于父节点还是子节点
int h[N], e[M], w[M], ne[M], d1[N], d2[N], up[N], p1[N];
void add(int a, int b, int c) {
    e[idx] = b;
    w[idx] = c;
    ne[idx] = h[a];
    h[a] = idx ++;
    return;
}
// 向下搜索
int dfs_d(int u, int father) { // dfs_d返回节点u的所有【子树】的最大路径和
    // 初始化节点的最大路径和次大路径为负无穷
    d1[u] = d2[u] = -INF;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        // 如果当前节点j是u的父亲，则返回，原因在楼上的一道题里面已经描述过
        if (j == father) continue;
        // 记得加上当前权值w[i]
        int d = dfs_d(j, u) + w[i];
        // 更新最大值和次大值，同时要记得更新u的最大值来自的路径，只需要记最大值即可
        if (d >= d1[u]) d2[u] = d1[u], d1[u] = d, p1[u] = j;
        else if (d >= d2[u]) d2[u] = d;
    }
    // 如果d1和d2没有被更新，则说明是叶节点，则将其最大值和次大值都初始化为0即可
    if (d1[u] == -INF) d1[u] = d2[u] = 0;
    // 返回节点u最大值
    return d1[u];
}
// 向上搜索
void dfs_u(int u, int father) {
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        if (j == father) continue;
        // 注意是up[j]和up[u]的位置
        // 注意w[i]是加在max的外面的
        if (p1[u] == j) up[j] = max(up[u], d2[u]) + w[i];
        else up[j] = max(up[u], d1[u]) + w[i];
        dfs_u(j, u);
    }
    return;
}
int main() {
    scanf("%d", &n);
    memset(h, -1, sizeof h);
    for (int i = 0; i < n - 1; i ++ ) {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        add(a, b, c);
        add(b, a, c);
    }
    // 树的向下寻找【用子节点更新父节点】
    dfs_d(1, -1);
    // 树的向上寻找【用父节点更新子节点】
    dfs_u(1, -1);
    int res = INF;
    for (int i = 1; i <= n; i ++ ) res = min(res, max(d1[i], up[i]));
    cout << res << endl;
    return 0;
}
```

**标签**

`树形DP`、`换根DP`

##### [AcWing 1075. 数字转换](https://www.acwing.com/problem/content/1077/)

**题目描述**

> 如果一个数 `x` 的约数之和 `y`（不包括他本身）比他本身小，那么 `x` 可以变成 `y`，`y` 也可以变成 `x`。
>
> 例如，`4` 可以变为 `3`，`1` 可以变为 `7`。
>
> 限定所有数字变换在不超过 `n` 的正整数范围内进行，求不断进行数字变换且不出现重复数字的最多变换步数。

**输入格式**

> 输入一个正整数 `n`。

**输出格式**

> 输出不断进行数字变换且不出现重复数字的最多变换步数。

**数据范围**

> + $1≤n≤50000$

**输入样例**

```c++
7
```

**输出样例**

```c++
3
```

**样例解释**

> 一种方案为：`4→3→1→74→3→1→7`。

**手写稿**

![2111129](https://gitee.com/peter95535/image-bed/raw/master/img/2111129.png)

**代码**

```c++
#include <iostream>
#include <cstring>
using namespace std;
const int N = 50010;
int n, idx, ans;
int h[N], e[N], ne[N], st[N], sum[N];
void add(int a, int b) {
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx ++;
    return;
}
int dfs(int u) {
    int d1 = 0, d2 = 0;
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        int d = dfs(j) + 1;
        if (d >= d1) d2 = d1, d1 = d;
        else if (d >= d2) d2 = d;
    }
    ans = max(ans, d1 + d2);
    return d1;
}
int main() {
    scanf("%d", &n);
    memset(h, -1, sizeof h);
    for (int i = 1; i <= n; i ++ )
        // 使用i去更新i*j，其中i就是约数
        for (int j = 2; j <= n / i; j ++ )
            sum[i * j] += i;
    for (int i = 2; i <= n; i ++ )
        // 如果i的约数之和sum[i]符合要求
        if (i > sum[i]) {
            // 注意边的顺序，要求sum[i] -> i，因此，因为add(sum[i], i)而不是add(i, add[i])
            add(sum[i], i);
            // 因为可能会有多棵树，因此，需要记录下树的哪些节点被访问过
            st[i] = true;
        }
    for (int i = 1; i <= n; i ++ )
        // 如果当前节点没有被访问过，则访问，即访问森林中的下一棵树
        if (!st[i]) dfs(i);
    cout << ans << endl;
    return 0;
}
```

**标签**

`树形DP`、`深度优先搜索`

##### [AcWing 285. 没有上司的舞会](https://www.acwing.com/problem/content/287/)

**题目描述**

> `Ural` 大学有 `N` 名职员，编号为 `1∼N`。
>
> 他们的关系就像一棵以校长为根的树，父节点就是子节点的直接上司。
>
> 每个职员有一个快乐指数，用整数 $H_i$ 给出，其中 $1≤i≤N$。
>
> 现在要召开一场周年庆宴会，不过，没有职员愿意和直接上司一起参会。
>
> 在满足这个条件的前提下，主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。

**输入格式**

> 第一行一个整数 `N`。
>
> 接下来 `N` 行，第 `i` 行表示 `i` 号职员的快乐指数 $H_i$。
>
> 接下来 `N−1` 行，每行输入一对整数 `L,K`，表示 `K` 是 `L` 的直接上司。

**输出格式**

> 输出最大的快乐指数。

**数据范围**

> + $1≤N≤6000$,
> + $−128≤Hi≤127$

**输入样例**

```c++
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5
```

**输出样例**

```c++
5
```

**手写稿**

![2150818](https://gitee.com/peter95535/image-bed/raw/master/img/2150818.png)

**代码**

```c++
#include <iostream>
#include <cstring>
using namespace std;
const int N = 6010;
int n, idx;
int happy[N], h[N], e[N], ne[N], is_root[N];
int f[N][2];
void add(int a, int b) {
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx ++;
    return;
}
void dfs(int u) {
    // 如果确定选择当前节点，则先将当前节点的快乐值➕
    f[u][1] = happy[u];
    for (int i = h[u]; i != -1; i = ne[i]) {
        int j = e[i];
        // 先递归子树
        dfs(j);
        // 如果不选择根节点，则子树既可以选择，也可以不选择
        f[u][0] += max(f[j][0], f[j][1]);
        // 如果选择根节点，则子树只能不选择
        f[u][1] += f[j][0];
    }
    return;
}
int main() {
    scanf("%d", &n);
    memset(h, -1, sizeof h);
    memset(is_root, true, sizeof is_root);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &happy[i]);
    for (int i = 1; i <= n - 1; i ++ ) {
        int u, v;
        scanf("%d%d", &u, &v);
        add(v, u);
        is_root[u] = false;
    }
    // 寻找根节点
    int root = 0;
    for (int i = 1; i <= n; i ++ )
        if (is_root[i]) {
            root = i;
            break;
        }
    dfs(root);
    cout << max(f[root][0], f[root][1]) << endl;
    return 0;
}
```

**标签**

`动态规划`、`树形DP`

##### [LeetCode 337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

**题目描述**

>   小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。
>
>   除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 两个直接相连的房子在同一天晚上被打劫 ，房屋将自动报警。
>
>   给定二叉树的 `root` 。返回 在不触动警报的情况下 ，小偷能够盗取的最高金额 。

**示例 1**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/rob1-tree.jpg)

>   输入: `root = [3,2,3,null,3,null,1]`
>   输出: `7` 
>   解释: 小偷一晚能够盗取的最高金额 `3 + 3 + 1 = 7`

**示例 2**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/rob2-tree.jpg)

>   输入: `root = [3,4,5,1,3,null,1]`
>   输出: `9`
>   解释: 小偷一晚能够盗取的最高金额 `4 + 5 = 9`

**提示**

>   +   $树的节点数在 [1, 10^4] 范围内$
>   +   $0 <= Node.val <= 10^4$

**手写稿**

![321848](https://gitee.com/peter95535/image-bed/raw/master/img/321848.png)

**代码**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    // dfs的返回值{选u, 不选u}，即对应着f[u][0]和f[u][1]
    vector<int> dfs(TreeNode* u) {
        if (!u) return {0, 0};
        auto l = dfs(u -> left), r = dfs(u -> right);
        // {不选u, 选u}
        return {max(l[0], l[1]) + max(r[0], r[1]), l[0] + r[0] + u -> val};
    }
    int rob(TreeNode* u) {
        vector<int> f = dfs(u);
        return max(f[0], f[1]);
    }
};
```

**标签**

`树形DP`

##### [AcWing 310. 最小高度树](https://leetcode-cn.com/problems/minimum-height-trees/)

**题目描述**

> 树是一个无向图，其中任何两个顶点只通过一条路径连接。 换句话说，一个任何没有简单环路的连通图都是一棵树。
>
> 给你一棵包含 `n` 个节点的树，标记为 `0` 到 `n - 1` 。给定数字 `n` 和一个有 `n - 1` 条无向边的 `edges` 列表（每一个边都是一对标签），其中 $edges[i] = [a_i, b_i]$ 表示树中节点 $a_i$ 和 $b_i$ 之间存在一条无向边。
>
> 可选择树中任何一个节点作为根。当选择节点 `x` 作为根节点时，设结果树的高度为 `h` 。在所有可能的树中，具有最小高度的树（即，`min(h)`）被称为 最小高度树 。
>
> 请你找到所有的 最小高度树 并按 任意顺序 返回它们的根节点标签列表。
>
> 树的 高度 是指根节点和叶子节点之间最长向下路径上边的数量。

**示例 1**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/e1.jpg)

> 输入：`n = 4, edges = [[1,0],[1,2],[1,3]]`
> 输出：`[1]`
> 解释：如图所示，当根是标签为 `1` 的节点时，树的高度是 `1` ，这是唯一的最小高度树。

**示例 2**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/e2.jpg)

> 输入：`n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]`
> 输出：`[3,4]`

**提示**

> + $1 <= n <= 2 * 10^4$
> + $edges.length == n - 1$
> + $0 <= a_i, b_i < n$
> + $a_i != b_i$
> + $所有 (a_i, b_i) 互不相同$
> + $给定的输入 保证 是一棵树，并且 不会有重复的边$

**解题步骤**

> 1. 对于每一个点 `u` 来说，将其当作根节点，然后计算以 `u` 为根的树的高度，统计出来所有的高度以后，循环求解即可
>
> 2. 计算树的高度，使用树形 `DP` 来计算
>
>    ![2171514](https://gitee.com/peter95535/image-bed/raw/master/img/2171514.png)

**代码**

```c++
class Solution {
public:
    vector<vector<int>> g;
    vector<int> d1, d2, up, p;
    // 向下寻找
    int dfs_d(int u, int father) {
        for (auto& j : g[u]) {
            if (j == father) continue;
            int d = dfs_d(j, u) + 1;
            if (d >= d1[u]) d2[u] = d1[u], d1[u] = d, p[u] = j;
            else if (d >= d2[u]) d2[u] = d;
        }
        return d1[u];
    }
    // 向上寻找
    void dfs_u(int u, int father) {
        for (auto& j : g[u]) {
            if (j == father) continue;
            if (p[u] == j) up[j] = max(up[u], d2[u]) + 1;
            else up[j] = max(up[u], d1[u]) + 1;
            dfs_u(j, u);
        }
        return;
    }
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        g.resize(n + 5);
        d1 = d2 = p = up = vector<int>(n + 5);
        // 建图
        for (auto& edge : edges) {
            int a = edge[0], b = edge[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        // 向下寻找
        dfs_d(0, -1);
        // 向上寻找
        dfs_u(0, -1);
        // 找最小值
        int Min = 1e9;
        for (int i = 0; i < n; i ++ ) Min = min(Min, max(up[i], d1[i]));
        // 找答案
        vector<int> res;
        for (int i = 0; i < n; i ++ ) 
            if (max(up[i], d1[i]) == Min) res.push_back(i);
        return res;
    }
};
```

**标签**

`树形DP`、`树的深度优先搜索`

##### end

#### 状态压缩 `DP`

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

```c++
3 2
```

**输出样例**

```c++
16
```

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

**标签**

`动态规划`

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

```c++
2 3
1 1 1
0 1 0
```

**输出样例**

```c++
9
```

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

**标签**

`动态规划`

#### 状态机

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

**标签**

`动态规划`

##### [AcWing 1058. 股票买卖 V](https://www.acwing.com/problem/content/1060/)

**题目描述**

> 给定一个长度为 `N` 的数组，数组中的第 `i` 个数字表示一个给定股票在第 `i` 天的价格。
>
> 设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:
>
> - 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
> - 卖出股票后，你无法在第二天买入股票 (即冷冻期为 `1` 天)。

**输入格式**

> 第一行包含整数 `N`，表示数组长度。
>
> 第二行包含 `N` 个不超过 `10000` 的正整数，表示完整的数组。

**输出格式**

> 输出一个整数，表示最大利润。

**数据范围**

> + $1≤N≤10^5$

**输入样例**

```c++
5
1 2 3 0 2
```

**输出样例**

```c++
3
```

**样例解释**

> 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]，第一笔交易可得利润 `2-1 = 1`，第二笔交易可得利润 `2-0 = 2`，共得利润 `1+2 = 3`。

**手写稿**

![2091151](https://gitee.com/peter95535/image-bed/raw/master/img/2091151.png)

**代码**

```c++
#include <iostream>
using namespace std;
const int N = 100010;
int n;
int w[N];
int f[N][3];
int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &w[i]);
    // 注意初始化
    f[0][0] = f[0][1] = -1e9, f[0][2] = 0;
    for (int i = 1; i <= n; i ++ ) {
        f[i][0] = max(f[i - 1][0], f[i - 1][2] - w[i]);
        f[i][1] = f[i - 1][0] + w[i];
        f[i][2] = max(f[i - 1][2], f[i - 1][1]);
    }
    cout << max(f[n][1], f[n][2]) << endl;
    return 0;
}
```

**标签**

`动态规划`、`状态机`

#### 简单 `DP`

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

**标签**

`动态规划`

#### `LIS`

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

**标签**

`LIS`、`二分`、`最长上升子序列`

##### [LeetCode 334. 递增的三元子序列](https://leetcode-cn.com/problems/increasing-triplet-subsequence/)

**题目描述**

>   给你一个整数数组 `nums` ，判断这个数组中是否存在长度为 `3` 的递增子序列。
>
>   如果存在这样的三元组下标 `(i, j, k)` 且满足 `i < j < k` ，使得 `nums[i] < nums[j] < nums[k]` ，返回 `true` ；否则，返回 `false` 。

**示例 1**

>   输入：`nums = [1,2,3,4,5]`
>   输出：`true`
>   解释：任何 `i < j < k` 的三元组都满足题意

**示例 2**

>   输入：`nums = [5,4,3,2,1]`
>   输出：`false`
>   解释：不存在满足题意的三元组

**示例 3**

>   输入：`nums = [2,1,5,0,4,6]`
>   输出：`true`
>   解释：三元组 `(3, 4, 5)` 满足题意，因为 `nums[3] == 0 < nums[4] == 4 < nums[5] == 6`

**提示**

>   +   $1 <= nums.length <= 5 * 10^5$
>   +   $-2^{31} <= nums[i] <= 2^{31} - 1$

**进阶**

>   你能实现时间复杂度为 `O(n)` ，空间复杂度为 `O(1)` 的解决方案吗？

**手写稿**

![321734](https://gitee.com/peter95535/image-bed/raw/master/img/321734.png)

**代码**

```c++
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        vector<int> q(2, INT_MAX);
        for (auto& x : nums) {
            // 对应情况一
            if (x <= q[0]) q[0] = x;
            // 对应情况二
            else if (x <= q[1]) q[1] = x;
            // 对应情况三
            else return true;
        }
        return false;
    }
};
```

**标签**

`LIS`、`最长上升子序列`

##### [LeetCode 368. 最大整除子集](https://leetcode-cn.com/problems/largest-divisible-subset/)

**题目描述**

>   给你一个由 无重复 正整数组成的集合 `nums` ，请你找出并返回其中最大的整除子集 `answer` ，子集中每一元素对 `(answer[i], answer[j])` 都应当满足：
>
>   +   `answer[i] % answer[j] == 0` ，或
>   +   `answer[j] % answer[i] == 0`
>       如果存在多个有效解子集，返回其中任何一个均可。

**示例 1**

>   输入：`nums = [1,2,3]`
>   输出：`[1,2]`
>   解释：`[1,3]` 也会被视为正确答案。

**示例 2**

>   输入：`nums = [1,2,4,8]`
>   输出：`[1,2,4,8]`

**提示**

>   +   $1 <= nums.length <= 1000$
>   +   $1 <= nums[i] <= 2 * 10^9$
>   +   $nums 中的所有整数 互不相同$

**手写稿**

![352205](https://gitee.com/peter95535/image-bed/raw/master/img/352205.png)

**代码**

```c++
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& a) {
        int n = a.size();
        // 长度至少为1【其本身一个数字】
        vector<int> f(n + 5, 1);
        // 排序
        sort(a.begin(), a.end());
        // 记录f最大时的下标
        int k = 1;
        for (int i = 1; i <= n; i ++ ) {
            for (int j = 1; j < i;j ++ )
                if (a[i - 1] % a[j - 1] == 0) f[i] = max(f[i], f[j] + 1);
            // 记录f最大时的下标
            if (f[i] > f[k]) k = i;
        }
        // 倒序查找方案，先把最后一个方案加进去
        vector<int> res(1, a[k - 1]);
        while (f[k] > 1)
            // 判断从哪个方案转移过来
            for (int i = 1; i < k; i ++ )
                if (a[k - 1] % a[i - 1] == 0 && f[k] == f[i] + 1) {
                    k = i;
                    res.push_back(a[k - 1]);
                    break;
                }
        return res;
    }
};
```

**标签**

`LIS`、`排序`

#### 背包模型

##### 分组背包

###### [AcWing 10. 有依赖的背包问题](https://www.acwing.com/problem/content/description/10/)

> 有 `N` 个物品和一个容量是 `V` 的背包。
>
> 物品之间具有依赖关系，且依赖关系组成一棵树的形状。如果选择一个物品，则必须选择它的父节点。
>
> 如下图所示：
> ![QQ图片20181018170337.png](https://gitee.com/peter95535/image-bed/raw/master/img/1_bb51ecbcd2-QQ%E5%9B%BE%E7%89%8720181018170337.png)
>
> 如果选择物品 `5`，则必须选择物品 `1` 和 `2`。这是因为 `2` 是 `5` 的父节点，`1` 是 `2` 的父节点。
>
> 每件物品的编号是 `i`，体积是 $v_i$，价值是 $w_i$，依赖的父节点编号是 $p_i$。物品的下标范围是 $1…N$。
>
> 求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。
>
> 输出最大价值。

**输入格式**

> 第一行有两个整数 `N，V`，用空格隔开，分别表示物品个数和背包容量。
>
> 接下来有 `N` 行数据，每行数据表示一个物品。
> 第 `i` 行有三个整数 $v_i,w_i,p_i$，用空格隔开，分别表示物品的体积、价值和依赖的物品编号。
> 如果 $p_i=−1$，表示根节点。 **数据保证所有物品构成一棵树。**

**输出格式**

> 输出一个整数，表示最大价值。

**数据范围**

> + $1≤N,V≤100$
> + $1≤v_i,w_i≤100$
>
> 父节点编号范围：
>
> - 内部结点：$1≤p_i≤N$;
> - 根节点 $p_i=−1$;

**输入样例**

```c++
5 7
2 3 -1
2 2 1
3 5 1
4 7 2
3 6 2
```

**输出样例**

```c++
11
```

**手写稿**

![2161118](https://gitee.com/peter95535/image-bed/raw/master/img/2161118.png)

**代码**

```c++
#include <iostream>
#include <cstring>
using namespace std;
const int N = 110;
int n, m, p, root, idx;
int v[N], w[N], h[N], e[N], ne[N];
int f[N][N];
void add(int a, int b) {
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx ++;
    return;
}
void dfs(int u) {
    for (int i = h[u]; i != -1; i = ne[i]) { // 枚举组
        int son = e[i];
        // 递归子树
        dfs(son);
        // 分组背包
        // 逆序的原因，在图解中
        for (int j = m - v[u]; j >= 0; j -- ) // 枚举组所占用的体积
            for (int k = 0; k <= j; k ++ ) // 枚举组内物品所占体积
                f[u][j] = max(f[u][j], f[u][j - k] + f[son][k]);
    }
    // 添加根节点的权值
    for (int i = m; i >= v[u]; i -- ) f[u][i] = f[u][i - v[u]] + w[u];
    // 如果体积连根节点也不能装下，则重置为0
    // 重置的原因，在图解中
    for (int i = 0; i < v[u]; i ++ ) f[u][i] = 0;
    return;
}
int main() {
    scanf("%d%d", &n, &m);
    memset(h, -1, sizeof h);
    for (int i = 1; i <= n; i ++ ) {
        scanf("%d%d%d", &v[i], &w[i] ,&p);
        if (p == -1) root = i;
        else add(p, i);
    }
    dfs(root);
    cout << f[root][m] << endl;
    return 0;
}
```

**标签**

`动态规划`、`分组背包`、`树形DP`

###### [AcWing 9. 分组背包问题](https://www.acwing.com/problem/content/description/9/)

**题目描述**

> 有 `N` 组物品和一个容量是 `V` 的背包。
>
> 每组物品有若干个，同一组内的物品最多只能选一个。
> 每件物品的体积是 $v_{ij}$，价值是 $w_{ij}$，其中 $i$ 是组号，$j$ 是组内编号。
>
> 求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。
>
> 输出最大价值。

**输入格式**

> 第一行有两个整数 `N，V`，用空格隔开，分别表示物品组数和背包容量。
>
> 接下来有 `N` 组数据：
>
> - 每组数据第一行有一个整数 $S_i$，表示第 $i$ 个物品组的物品数量；
> - 每组数据接下来有 $S_i$ 行，每行有两个整数 $v_{ij},w_{ij}$，用空格隔开，分别表示第 $i$ 个物品组的第 $j$ 个物品的体积和价值；

**输出格式**

> 输出一个整数，表示最大价值。

**数据范围**

> + $0<N,V≤100$
> + $0<S_i≤100$
> + $0<v_{ij},w_{ij}≤100$

**输入样例**

```c++
3 5
2
1 2
2 4
1
3 4
1
4 5
```

**输出样例**

```c++
8
```

**手写稿**

![2142101](https://gitee.com/peter95535/image-bed/raw/master/img/2142101.png)

**代码**

**二维数组**

```c++
#include <iostream>
using namespace std;
const int N = 110;
int n, m;
int s[N], v[N][N], w[N][N], f[N][N];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &s[i]);
        for (int j = 1; j <= s[i]; j ++ ) scanf("%d%d", &v[i][j], &w[i][j]);
    }
    for (int i = 1; i <= n; i ++ ) // 循环每一组
        for (int j = 0; j <= m; j ++ ) { // 循环每一组的体积
            f[i][j] = f[i - 1][j]; // 一件物品都不选
            for (int k = 1; k <= s[i]; k ++) // 循环每一组内的商品
                if (j >= v[i][k]) // 注意：体积需要大于等于当前物品体积才可转移
                    f[i][j] = max(f[i][j], f[i - 1][j - v[i][k]] + w[i][k]);
        }
    cout << f[n][m] << endl;
    return 0;
}
```

**一维数组优化**

```c++
#include <iostream>
using namespace std;
const int N = 110;
int n, m;
int f[N];
int s[N], v[N][N], w[N][N];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &s[i]);
        for (int j = 1; j <= s[i]; j ++ ) scanf("%d%d", &v[i][j], &w[i][j]);
    }
    for (int i = 1; i <= n; i ++ ) // 循环每一组
        for (int j = m; j >= 0; j -- ) { // 循环每一组的体积
            for (int k = 1; k <= s[i]; k ++) // 每一件物品最多选择一件
                if (j >= v[i][k]) // 注意：体积需要大于等于当前物品体积才可转移
                    f[j] = max(f[j], f[j - v[i][k]] + w[i][k]);
        }
    cout << f[m] << endl;
    return 0;
}
```

**标签**

`分组背包`

##### 完全背包

###### [AcWing 3. 完全背包问题](https://www.acwing.com/problem/content/3/)

**题目描述**

> 有 `N` 种物品和一个容量是 `V` 的背包，每种物品都有无限件可用。
>
> 第 `i` 种物品的体积是 $v_i$，价值是 $w_i$。
>
> 求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
> 输出最大价值。

**输入格式**

> 第一行两个整数，`N，V`，用空格隔开，分别表示物品种数和背包容积。
>
> 接下来有 `N` 行，每行两个整数 $v_i,w_i$，用空格隔开，分别表示第 `i` 种物品的体积和价值。

**输出格式**

> 输出一个整数，表示最大价值。

**数据范围**

> + $0<N,V≤1000$
> + $0<v_i,w_i≤1000$

**输入样例**

```c++
4 5
1 2
2 4
3 4
4 5
```

**输出样例**

```c++
10
```

**手写稿**

![2211255](https://gitee.com/peter95535/image-bed/raw/master/img/2211255.png)

**代码**

**代码一：二维数组**

```c++
#include <iostream>
using namespace std;
const int N = 1010;
int n, m;
int v[N], w[N];
int f[N][N];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d%d", &v[i], &w[i]);
    for (int i = 1; i <= n; i ++ )
        for (int j = 0;j <= m; j ++ ) {
            // 不选第i件商品
            f[i][j] = f[i - 1][j];
            if (j >= v[i]) f[i][j] = max(f[i][j], f[i][j - v[i]] + w[i]);
        }
    cout << f[n][m] << endl;
    return 0;
}
```

**代码二：滚动数组**

```c++
#include <iostream>
using namespace std;
const int N = 1010;
int n, m;
int v[N], w[N];
int f[2][N];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d%d", &v[i], &w[i]);
    for (int i = 1; i <= n; i ++ )
        for (int j = 0;j <= m; j ++ ) {
            f[i & 1][j] = f[i - 1 & 1][j];
            if (j >= v[i]) f[i & 1][j] = max(f[i & 1][j], f[i & 1][j - v[i]] + w[i]);
        }
    cout << f[n & 1][m] << endl;
    return 0;
}
```

**代码三：一维数组**

```c++
#include <iostream>
using namespace std;
const int N = 1010;
int n, m;
int v[N], w[N], f[N];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d%d", &v[i], &w[i]);
    for (int i = 1; i <= n; i ++ )
        for (int j = v[i]; j <= m; j ++ )
            f[j] = max(f[j], f[j - v[i]] + w[i]);
    cout << f[m] << endl;
    return 0;
}
```

**标签**

`完全背包`

###### [AcWing 1021. 货币系统](https://www.acwing.com/problem/content/description/1023/)

**题目描述**

> 给你一个 `n` 种面值的货币系统，求组成面值为 `m` 的货币有多少种方案。

**输入格式**

> 第一行，包含两个整数 `n` 和 `m`。
>
> 接下来 `n` 行，每行包含一个整数，表示一种货币的面值。

**输出格式**

> 共一行，包含一个整数，表示方案数。

**数据范围**

> + $n≤15,m≤3000$

**输入样例**

```c++
3 10
1
2
5
```

**输出样例**

```c++
10
```

**手写稿**

![2211418](https://gitee.com/peter95535/image-bed/raw/master/img/2211418.png)

**代码**

**代码一：二维数组**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 20, M = 3010;
int n, m;
int v[N];
// 注意方案数目可能爆int
LL f[N][M];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &v[i]);
    f[0][0] = 1;
    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j <= m; j ++ ) {
            f[i][j] = f[i - 1][j];
            if (j >= v[i]) f[i][j] += f[i][j - v[i]];
        }
    cout << f[n][m] << endl;
    return 0;
}
```

**代码二：滚动数组**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 20, M = 3010;
int n, m;
int v[N];
LL f[2][M];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &v[i]);
    f[0][0] = 1;
    for (int i = 1; i <= n; i ++ )
        for (int j = 0; j <= m; j ++ ) {
            // 注意：不能使用+=，因为f[i & 1][j]可能是别的层的，不一定为0
            f[i & 1][j] = f[i - 1 & 1][j];
            if (j >= v[i]) f[i & 1][j] += f[i & 1][j - v[i]];
        }
    cout << f[n & 1][m] << endl;
    return 0;
}
```

**代码三：一维数组**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 20, M = 3010;
int n, m;
int v[N];
LL f[M];
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &v[i]);
    f[0] = 1;
    for (int i = 1; i <= n; i ++ )
        for (int j = v[i]; j <= m; j ++ ) f[j] += f[j - v[i]];
    cout << f[m] << endl;
    return 0;
}
```

**标签**

`完全背包`

###### [LeetCode 322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

> 给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。
>
> 计算并返回可以凑成总金额所需的 最少的硬币个数 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。
>
> 你可以认为每种硬币的数量是无限的。

**示例 1**

> 输入：`coins = [1, 2, 5], amount = 11`
> 输出：`3`
> 解释：`11 = 5 + 5 + 1`

**示例 2**

> 输入：`coins = [2], amount = 3`
> 输出：`-1`

**示例 3**

> 输入：`coins = [1], amount = 0`
> 输出：`0`

**提示**

> + $1 <= coins.length <= 12$
> + $1 <= coins[i] <= 2^{31} - 1$
> + $0 <= amount <= 10^4$

**手写稿**

![2211833](https://gitee.com/peter95535/image-bed/raw/master/img/2211833.png)

**代码**

**代码一：二维数组**

```c++
class Solution {
public:
    int coinChange(vector<int>& v, int m) {
        int n = v.size();
        // 初始化为正无穷
        vector<vector<int>> f(n + 5, vector<int>(m + 5, 1e9));
        // 前零张纸币，价值总额恰好为零的【纸币数量】为零张
        f[0][0] = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 0; j <= m; j ++ ) {
                f[i][j] = f[i - 1][j];
                if (j >= v[i - 1]) f[i][j] = min(f[i][j], f[i][j - v[i - 1]] + 1);
            }
        // 如果不符合条件，输出-1
        if (f[n][m] == 1e9) f[n][m] = -1;
        return f[n][m];
    }
};
```

**代码二：滚动数组**

```c++
class Solution {
public:
    int coinChange(vector<int>& v, int m) {
        int n = v.size();
        // 初始化为正无穷
        vector<vector<int>> f(2, vector<int>(m + 5, 1e9));
        // 前零张纸币，价值总额恰好为零的【纸币数量】为零张
        f[0][0] = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = 0; j <= m; j ++ ) {
                f[i & 1][j] = f[i - 1 & 1][j];
                if (j >= v[i - 1]) f[i & 1][j] = min(f[i & 1][j], f[i & 1][j - v[i - 1]] + 1);
            }
        // 如果不符合条件，输出-1
        if (f[n & 1][m] == 1e9) f[n & 1][m] = -1;
        return f[n & 1][m];
    }
};
```

**代码三：一维数组**

```c++
class Solution {
public:
    int coinChange(vector<int>& v, int m) {
        int n = v.size();
        // 初始化为正无穷
        vector<int> f(m + 5, 1e9);
        // 前零张纸币，价值总额恰好为零的【纸币数量】为零张
        f[0] = 0;
        for (int i = 1; i <= n; i ++ )
            for (int j = v[i - 1]; j <= m; j ++ )
                f[j] = min(f[j], f[j - v[i - 1]] + 1);
        // 如果不符合条件，输出-1
        if (f[m] == 1e9) f[m] = -1;
        return f[m];
    }
};
```

**标签**

`完全背包`

#### end

### 位运算专题

#### [LeetCode 260. 只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

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

**标签**

`位运算`

#### [LeetCode 338. 比特位计数](https://leetcode-cn.com/problems/counting-bits/)

**题目描述**

> 给你一个整数 `n` ，对于 `0 <= i <= n` 中的每个 `i` ，计算其二进制表示中 `1` 的个数 ，返回一个长度为 `n + 1` 的数组 `ans` 作为答案。

**示例 1**

> 输入：`n = 2`
> 输出：`[0,1,1]`
> 解释：
> `0 --> 0`
> `1 --> 1`
> `2 --> 10`

**示例 2**

> 输入：`n = 5`
> 输出：`[0,1,1,2,1,2]`
> 解释：
> `0 --> 0`
> `1 --> 1`
> `2 --> 10`
> `3 --> 11`
> `4 --> 100`
> `5 --> 101`

**提示**

> + $0 <= n <= 10^5$

**进阶**

> 很容易就能实现时间复杂度为 `O(n log n)` 的解决方案，你可以在线性时间复杂度 `O(n)` 内用一趟扫描解决此问题吗？
> 你能不使用任何内置函数解决此问题吗？（如，`C++` 中的 `__builtin_popcount` ）

**方法一：枚举 + 位运算**

**步骤**

> 1. 遍历每一个数字，检查每一个数字的二进制位中含有的 `1` 的个数即可
> 2. 使用 `lowbit` 函数进行检查 `1` 的数量
>    + `lowbit` 函数 `x & -x` 可以取得 `x` 的最后一位 `1`

**代码**

```c++
class Solution {
public:
    int get(int x) {
        int res = 0;
        while (x) {
            x -= x & -x;
            res ++;
        }
        return res;
    }
    vector<int> countBits(int n) {
        vector<int> res;
        for (int i = 0; i <= n; i ++ ) res.push_back(get(i));
        return res;
    }
};
```

**方法二：动态规划 + 位运算**

**手写稿**

![WX20220122-134731@2x](https://gitee.com/peter95535/image-bed/raw/master/img/WX20220122-134731@2x.png)

**代码**

```c++
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> f(n + 1);
        for (int i = 0; i <= n; i ++ ) f[i] = f[i >> 1] + (i & 1);
        return f;
    }
};
```

**标签**

` 动态规划`、`位运算`

#### [AcWing 318. 最大单词长度乘积](https://leetcode-cn.com/problems/maximum-product-of-word-lengths/)

**题目描述**

> 给你一个字符串数组 `words` ，找出并返回 `length(words[i]) * length(words[j])` 的最大值，并且这两个单词不含有公共字母。如果不存在这样的两个单词，返回 `0` 。

**示例 1**

> 输入：`words = ["abcw","baz","foo","bar","xtfn","abcdef"]`
> 输出：`16`
> 解释：这两个单词为 `"abcw", "xtfn"`。

**示例 2**

> 输入：`words = ["a","ab","abc","d","cd","bcd","abcd"]`
> 输出：`4` 
> 解释：这两个单词为 `"ab", "cd"`。

**示例 3**

> 输入：`words = ["a","aa","aaa","aaaa"]`
> 输出：`0`
> 解释：不存在这样的两个单词。

**提示**

> + $2 <= words.length <= 1000$
> + $1 <= words[i].length <= 1000$
> + $words[i]$ 仅包含小写字母

**手写稿**

![2181145](https://gitee.com/peter95535/image-bed/raw/master/img/2181145.png)

**代码**

```c++
class Solution {
public:
    int maxProduct(vector<string>& words) {
        vector<int> state;
        for (auto& word : words) {
            int s = 0;
            // 26位二进制，也就是说a, aa, aaa实际上映射的都是同一个数字
            // 注意体会
            for (auto& c : word) s |= 1 << (c - 'a');
            state.push_back(s);
        }
        int res = 0;
        for (int i = 0; i < words.size(); i ++ )
            for (int j = i + 1; j < words.size(); j ++ )
                if (!(state[i] & state[j]))
                    res = max(res, (int)(words[i].size() * words[j].size()));
        return res;
    }
};
```

**标签**

`位运算`

#### [LeetCode 371. 两整数之和](https://leetcode-cn.com/problems/sum-of-two-integers/)

**题目描述**

>   给你两个整数 `a` 和 `b` ，不使用 运算符 `+` 和 `-` ，计算并返回两整数之和。

**示例 1**

>   输入：`a = 1, b = 2`
>   输出：`3`

**示例 2**

>   输入：`a = 2, b = 3`
>   输出：`5`

**提示**

>   +   $-1000 <= a, b <= 1000$

**手写稿**



**代码**



**题解**



#### end

### 数学相关

####  普通数学

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

**标签**

`几何`、`数学`

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

> 1. 若 `a` 是<font style ="color: red">**丑数**</font>，则 `a * 2` 也是丑数
>
>    + $$
>      若a是丑数，则a可以表示成 \\
>      a = p_1^{n}\;*\;p_2^{m}\;*\;p_3^{k}，\;其中p_1为2，p_2为3，p_3为5 \\
>      现在来看\;a\;*\;p = p_1^{n}\;*\;p_2^{m}\;*\;p_3^{k}\;*\;p，其中p为2，3，或5 \\
>      证毕
>      $$
>
> 2. 三指针做法

**手写稿**

![2172025](https://gitee.com/peter95535/image-bed/raw/master/img/2172025.png)

**解题步骤**

> 1. 初始丑树数组等于 `q = [1]`，定义三个指针，`i，j，k` 分别对应质因数 `2` 的集合，`3` 的集合，`5` 的集合，初始值为 `i = j = k = 0`
>    + 第一步
>      + `t = min(q[i] * 2, q[j] * 3, q[k] * 5) = min(1 * 2, 1 * 3, 1 * 5) = min(2,  3, 5) = 2`
>      + `t` 取值为 `2`，对应的是 `i` 指针，因此，`i ++`
>      + 此时，`q = [1, 2], i = 1, j = k = 0`
>    + 第二步
>      + `t = min(q[i] * 2, q[j] * 3, q[k] * 5) = min(4, 3, 5) = 3`
>      + `t` 取值为 `3`，对应的是 `j` 指针，因此，`j ++`
>      + 此时，`q = [1, 2, 3], i = 1, j = 1, k = 0`
>    + 第三步
>      + `t = min(q[i] * 2, q[j] * 3, q[k] * 5) = min(4, 6, 5) = 4`
>      + `t` 取值为 `4`，对应的是 `i` 指针，因此，`i ++`
>      + 此时，`q = [1, 2, 3, 4], i = 2, j = 1, k = 0`
>    + 第四步
>      + `t = min(q[i] * 2, q[j] * 3, q[k] * 5) = min(6, 6, 5) = 5`
>      + `t` 取值为 `5`，对应的是 `k` 指针，因此，`k ++`
>      + 此时，`q = [1, 2, 3, 4, 5], i = 2, j = 1, k = 1`
>    + 第五步
>      + `t = min(q[i] * 2, q[j] * 3, q[k] * 5) = min(6, 6, 10) = 6`
>      + `t` 取值为 `6`，对应的是 `i` 和 `j` 指针，因此，`i ++, j ++`
>        + 注意：如果有相等的，指针都需要进行移动，而不能只移动一个指针
>      + 此时，`q = [1, 2, 3, 4, 5, 6], i = 3, j = 2, k = 1`
>    + 依次类推

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

**标签**

`三路归并`、`归并排序`

##### [AcWing 313. 超级丑数](https://leetcode-cn.com/problems/super-ugly-number/)

**题目描述**

> 超级丑数 是一个正整数，并满足其所有质因数都出现在质数数组 `primes` 中。
>
> 给你一个整数 `n` 和一个整数数组 `primes` ，返回第 `n` 个 超级丑数 。
>
> 题目数据保证第 `n` 个 超级丑数 在 `32-bit` 带符号整数范围内。

**示例 1**

> 输入：`n = 12, primes = [2,7,13,19]`
> 输出：`32`
> 解释：给定长度为 `4` 的质数数组 `primes = [2,7,13,19]`，前 `12` 个超级丑数序列为：`[1,2,4,7,8,13,14,16,19,26,28,32]` 。

**示例 2**

> 输入：`n = 1, primes = [2,3,5]`
> 输出：`1`
> 解释：`1` 不含质因数，因此它的所有质因数都在质数数组 `primes = [2,3,5]` 中。

**提示**

> + $1 <= n <= 10^6$
> + $1 <= primes.length <= 100$
> + $2 <= primes[i] <= 1000$
> + $题目数据 保证 primes[i] 是一个质数$
> + $primes 中的所有值都 互不相同 ，且按 递增顺序 排列$

**解题步骤**

> 1. 同 `LeetCode 264 丑树 II`
>
> 2. 唯一不同的是由 `3` 指针变成 `k` 指针
>
> 3. 具体实现的细节
>
>    ![2180807](https://gitee.com/peter95535/image-bed/raw/master/img/2180807.png)
>
>    > 1. 三指针有一个寻找最小值的操作 `min(q[i] * 2, min(q[j] * 3, q[k] * 5))`，在本题中，将寻找 `k` 个指针的最小值操作，换成堆（优先队列来实现）
>    > 2. 优先队列中维护两个变量，分别为 `k` 个指针对应的值以及下标
>    > 3. 考虑一个问题，如何确定某个数 `x` 是由哪个质数迭代而来的
>    >    + 对于丑树 $S_p$ 来说，其下一个数为 $k = S_p * p$ ，因此，当前质数 $p = k / S_p$
>    > 4. 如果还是有些许模糊，手动模拟样例即可

**代码**

```c++
class Solution {
public:
    typedef long long LL;
    typedef pair<LL, LL> PLL;
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        // 定义小根堆
        priority_queue<PLL, vector<PLL>, greater<PLL>> heap;
        // 含义在图示手写稿
        for (auto& x : primes) heap.push({x, 1});
        vector<int> q(n + 5);
        q[1] = 1;
        for (int i = 2; i <= n;) {
            auto tp = heap.top(); heap.pop();
            if (tp.first != q[i - 1]) q[i ++ ] = tp.first;
            int idx = tp.second, p = tp.first / q[idx];
            heap.push({(LL)p * q[idx + 1], idx + 1});
        }
        return q[n];
    }
};
```

**标签**

`优先队列`、`K路归并`

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

**标签**

`动态规划`、`数学`

##### [LeetCode 233. 数字 1 的个数](https://leetcode-cn.com/problems/number-of-digit-one/)

**题目描述**

> 给定一个整数 `n`，计算所有小于等于 `n` 的非负整数中数字 `1` 出现的个数。

**示例 1**

> 输入：`n = 13`
> 输出：`6`

**示例 2**

> 输入：`n = 0`
> 输出：`0`

**提示**

> + $0 <= n <= 10^9$

**题解**

> 1. 将数字分解成每个数字，存放到 vector 里面，注意顺序问题
> 2. 从前往后依次遍历每个数字，判断当前位置上 1 的个数累加即可

**手写稿**

![WX20220119-100611@2x (1)](https://gitee.com/peter95535/image-bed/raw/master/img/WX20220119-100611@2x%20(1).png)

**代码**

```c++
class Solution {
public:
    int countDigitOne(int n) {
        vector<int> num;
        while (n) {
            num.push_back(n % 10);
            n /= 10;
        }
        // 注意顺序问题
        reverse(num.begin(), num.end());
        int res = 0;
        for (int i = 0; i < num.size(); i ++ ) {
            int left = 0, right = 0, p = 1;
            for (int j = 0; j < i; j ++ ) left = left * 10 + num[j];
            for (int j = i + 1; j < num.size(); j ++ )
                right = right * 10 + num[j], p *= 10;
            if (num[i] == 0) res += left * p;
            else if (num[i] == 1) res += left * p + right + 1;
            else res += (left + 1) * p;
        }
        return res;
    }
};
```

**标签**

`数学`

##### [LeetCode 231. 2 的幂](https://leetcode-cn.com/problems/power-of-two/)

**题目描述**

> 给你一个整数 `n`，请你判断该整数是否是 `2` 的幂次方。如果是，返回 `true` ；否则，返回 `false` 。
>
> 如果存在一个整数 `x`  使得 `n == 2x` ，则认为 `n` 是 `2` 的幂次方。

**示例 1**

> 输入：`n = 1`
> 输出：`true`
> 解释：$2^0 = 1$

**示例 2**

> 输入：`n = 16`
> 输出：`true`
> 解释：$2^4 = 16$

**示例 3**

> 输入：`n = 3`
> 输出：`false`

**示例 4**

> 输入：`n = 4`
> 输出：`true`

**示例 5**

> 输入：`n = 5`
> 输出：`false`

**提示**

> + $-2^{31} <= n <= 2^{31} - 1$

**进阶**

> 你能够不使用循环/递归解决此问题吗？

**方法一：对数**

**代码**

```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if (n <= 0) return false;
        return (log2(n) - int(log2(n))) == 0;
    }
};
```

**方法二：位运算**

**解析**

> 1. 一个数字 `x` 如果是 `2` 的幂，则说明 `x` 的二进制表示为 `000001000000`
> 2. `lowbit` 计算最后一位 `1` 的大小，判断是否和 `x` 相等即可

**代码**

```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if (n <= 0) return false;
        return (n & -n) == n;
    }
};
```

**方法三：取巧的位运算**

**解析**

> 1. 由<a href="https://baike.baidu.com/item/%E7%AE%97%E6%9C%AF%E5%9F%BA%E6%9C%AC%E5%AE%9A%E7%90%86/10920095">算术基本定理</a>可知，任意自然数都可以唯一分解为若干个质数的乘积，数 `x` 若是 `2` 的幂，则 `x` 的质因数只有 `2`，除此之外，不存在其他的质因数
>    + 反证法
>      + 如果 `x` 的质因数除了 `2` 之外，还有质因数 `p`，则 `p` 一定不能被分解，因为 `p` 也是质数
> 2. 找出最大的只含有 `2` 的质因子的数 `x`，不难发现，所有满足质因子只有 `2` 的数均是 `x` 的因子

**代码**

```c++
class Solution {
public:
    bool isPowerOfTwo(int n) {
        if (n <= 0) return false;
        return (1 << 30) % n == 0;
    }
};
```

**标签**

`对数`、`位运算`

##### [LeetCode 326. 3 的幂](https://leetcode-cn.com/problems/power-of-three/)

**题目描述**

> 给定一个整数，写一个函数来判断它是否是 `3` 的幂次方。如果是，返回 `true` ；否则，返回 `false` 。
>
> 整数 `n` 是 `3` 的幂次方需满足：存在整数 `x` 使得 `n == 3x`

**示例 1**

> 输入：`n = 27`
> 输出：`true`

**示例 2**

> 输入：`n = 0`
> 输出：`false`

**示例 3**

> 输入：`n = 9`
> 输出：`true`

**示例 4**

> 输入：`n = 45`
> 输出：`false`

**提示**

> + $-2^{31} <= n <= 2^{31} - 1$

**方法一：对数函数**

**公式推导**

> 由于 c++ 没有以任意自然数为底的对数函数，因此，需要使用换底公式进行推导计算，证明如下：
> $$
> 换底公式：log_ab = \frac{log_cb}{log_ca}，其中c为任意自然数 \\
> 当c=10时，有log_ab = \frac{loga}{logb}
> $$

**代码**

```c++
class Solution {
public:
    bool isPowerOfThree(int n) {
        // 不能是负数
        if (n <= 0) return false;
        double x = log10(n) / log10(3);
        return x == int(x);
    }
};
```

**方法二：打表**

**步骤**

> 1. 对于任意一个数 `x` 来说，$3^x$ 可能的取值为 $3^0, 3^1, 3^2, 3^3,..., 3^n,..., 3^{+\infty}$
> 2. 取最大值 $3^n = 3 * 3 ^ {n - 1}$ ，可以看出所有比最大值小的数同时又是 `3` 的幂的数都是最大值的因子，因此，可以找出最大值，然后判断是否是最大值的因子即可
> 3. <font style="color: red">**注意：x必须得是质数，原因：<a href="https://baike.baidu.com/item/%E7%AE%97%E6%9C%AF%E5%9F%BA%E6%9C%AC%E5%AE%9A%E7%90%86/10920095">算术基本定理</a>**</font>，详细解释如下
>    + 由算术基本定理可知，任意<font style="color: red">**自然数**</font>都可以<font style="color: red">**唯一**</font>分解为若干个<font style="color: red">**质数**</font>相乘
>    + 反证法
>      + 如果数 `x` 分解之后的质数除了 `3` 之外，还有别的质数 `p`，则 `x` 一定不是 `3` 的幂，因为 `p` 也是质数，且不能再次分解
>      + 因此，如果数 `x` 是 `3` 的幂，则其质因子有且只有 `3` 才可
> 4. 找出最大的满足质因子只有 `3` 的数 `x`，不难发现，所有满足质因子只有 `3` 的数均是 `x` 的因子

**代码**

```c++
class Solution {
public:
    bool isPowerOfThree(int n) {
        if (n <= 0) return false;
        return 1162261467 % n == 0;
    }
};
```

**标签**

`对数`、`算术基本定理`、`数论`

##### [LeetCode 342. 4的幂](https://leetcode-cn.com/problems/power-of-four/)

**题目描述**

> 给定一个整数，写一个函数来判断它是否是 `4` 的幂次方。如果是，返回 `true` ；否则，返回 `false` 。
>
> 整数 `n` 是 `4` 的幂次方需满足：存在整数 `x` 使得 `n == 4x`

**示例 1**

> 输入：`n = 16`
> 输出：`true`

**示例 2**

> 输入：`n = 5`
> 输出：`false`

**示例 3**

> 输入：`n = 1`
> 输出：`true`

**提示**

> + $-2^{31} <= n <= 2{31} - 1$

**进阶**

> 你能不使用循环或者递归来完成本题吗？

**方法一：对数**

**代码**

```c++
class Solution {
public:
    bool isPowerOfFour(int n) {
        if (n <= 0) return false;
        double x = log10(n) / log10(4);
        return x == int(x);
    }
};
```

**方法二：位运算**

**解析**

> 1. 如果数 `x` 是 `4` 的幂，则 `x` 也一定是 `2` 的幂，即有 $4 ^ x = (2 ^ 2)^x = (2 ^ x) ^ 2$
>    + 先对 `x` 进行开方得到 `r`，判断 $r^2$ 是否等于 `x`
>      + 不等于，返回❌
>      + 等于，判断 `r` 是否是 `2` 的幂即可

**代码**

```c++
class Solution {
public:
    bool isPowerOfFour(int n) {
        if (n <= 0) return false;
        int r = sqrt(n);
        if (r * r != n) return false;
        return (1 << 30) % n == 0;
    }
};
```

**标签**

`对数`、`位运算`

##### [LeetCode 343. 整数拆分](https://leetcode-cn.com/problems/integer-break/)

**题目描述**

>   给定一个正整数 `n` ，将其拆分为 `k` 个 正整数 的和（ `k >= 2` ），并使这些整数的乘积最大化。
>
>   返回 你可以获得的最大乘积 。

**示例 1**

>   输入: `n = 2`
>   输出: `1`
>   解释: `2 = 1 + 1, 1 × 1 = 1`。

**示例 2**

>   输入: `n = 10`
>   输出: `36`
>   解释: `10 = 3 + 3 + 4, 3 × 3 × 4 = 36`。

**提示**

>   +   $2 <= n <= 58$

**手写稿**

![332159](https://gitee.com/peter95535/image-bed/raw/master/img/332159.png)

**代码**

```c++
class Solution {
public:
    int integerBreak(int n) {
        if (n <= 3) return 1 * (n - 1);
        int res = 1;
        while (n >= 5) n -= 3, res *= 3;
        return res * n;
    }
};
```

**标签**

`数学`

##### [LeetCode 357. 计算各个位数不同的数字个数](https://leetcode-cn.com/problems/count-numbers-with-unique-digits/)

**题目描述**

>   给定一个非负整数 `n`，计算各位数字都不同的数字 `x` 的个数，其中 $0 ≤ x < 10^n$ 。

**示例**

>   输入: `2`
>   输出: `91`
>   解释: 答案应为除去 `11,22,33,44,55,66,77,88,99` 外，在 `[0,100)` 区间内的所有数字。

**手写稿**

![350904](https://gitee.com/peter95535/image-bed/raw/master/img/350904.png)

**代码**

```c++
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        // 0 ～ 9 共10个数字，n > 10 无意义
        n = min(n, 10);
        vector<int> f(n + 1);
        // 0 特判
        if (n == 0) return 1;
        f[1] = 9;
        for (int i = 2; i <= n; i ++ ) f[i] = f[i - 1] * (11 - i);
        int res = 0;
        for (int i = 1; i <= n; i ++ ) res += f[i];
        // 加上 0 这个数字
        return res + 1;
    }
};
```

**标签**

`排列组合`、`数学`、`动态规划`

#### 数论相关

##### [AcWing 319. 灯泡开关](https://leetcode-cn.com/problems/bulb-switcher/)

**题目描述**

> 初始时有 `n` 个灯泡处于关闭状态。第一轮，你将会打开所有灯泡。接下来的第二轮，你将会每两个灯泡关闭第二个。
>
> 第三轮，你每三个灯泡就切换第三个灯泡的开关（即，打开变关闭，关闭变打开）。第 `i` 轮，你每 `i` 个灯泡就切换第 `i` 个灯泡的开关。直到第 `n` 轮，你只需要切换最后一个灯泡的开关。
>
> 找出并返回 `n` 轮后有多少个亮着的灯泡。 

**示例 1**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/bulb.jpg)

> 输入：`n = 3`
> 输出：`1 `
> 解释：
> 初始时, 灯泡状态 [关闭, 关闭, 关闭].
> 第一轮后, 灯泡状态 [开启, 开启, 开启].
> 第二轮后, 灯泡状态 [开启, 关闭, 开启].
> 第三轮后, 灯泡状态 [开启, 关闭, 关闭]. 
>
> 你应该返回 `1`，因为只有一个灯泡还亮着。

**示例 2**

> 输入：`n = 0`
> 输出：`0`

**示例 3**

> 输入：`n = 1`
> 输出：`1`

**提示**

> + $0 <= n <= 10^9$

**手写稿**

![2181929](https://gitee.com/peter95535/image-bed/raw/master/img/2181929.png)

**代码**

```c++
class Solution {
public:
    int bulbSwitch(int n) {
        // 寻找1～n中完全平方数的个数
        return sqrt(n);
    }
};
```

**标签**

`完全平方数`、`脑筋急转弯`、`数论`

##### [LeetCode 365. 水壶问题](https://leetcode-cn.com/problems/water-and-jug-problem/)

>   有两个水壶，容量分别为 `jug1Capacity` 和 `jug2Capacity` 升。水的供应是无限的。确定是否有可能使用这两个壶准确得到 `targetCapacity` 升。
>
>   如果可以得到 `targetCapacity` 升水，最后请用以上水壶中的一或两个来盛放取得的 `targetCapacity` 升水。
>
>   你可以：
>
>   装满任意一个水壶
>   清空任意一个水壶
>   从一个水壶向另外一个水壶倒水，直到装满或者倒空

**示例 1**

>   输入: `jug1Capacity = 3, jug2Capacity = 5, targetCapacity = 4`
>   输出: `true`
>   解释：来自著名的 ["Die Hard"](https://www.youtube.com/watch?v=BVtQNK_ZUJg)

**示例 2**

>   输入: `jug1Capacity = 2, jug2Capacity = 6, targetCapacity = 5`
>   输出: `false`

**示例 3**

>   输入: `jug1Capacity = 1, jug2Capacity = 2, targetCapacity = 3`
>   输出: `true`

**提示**

>   +   $1 <= jug1Capacity, jug2Capacity, targetCapacity <= 10^6$

**手写稿**

![351607](https://gitee.com/peter95535/image-bed/raw/master/img/351607.png)

**代码**

```c++
class Solution {
public:
    int gcd(int a, int b) {
        if (!b) return a;
        return gcd(b, a % b);
    }
    bool canMeasureWater(int a, int b, int c) {
        if (a + b < c) return false;
        return c % gcd(a, b) == 0; 
    }
};
```

**标签**

`贝祖定理`、`数论`、`欧几里得算法`

#### end

### 滑动窗口【双端队列】专题

#### [LeetCode 5977. 最少交换次数来组合所有的 1 II](https://leetcode-cn.com/problems/minimum-swaps-to-group-all-1s-together-ii/)

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

 **标签**

`滑动窗口`、`双端队列`、`环`

#### [LeetCode 239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

**题目描述**

> 给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。
>
> 返回滑动窗口中的最大值。

**示例 1**

> 输入：`nums = [1,3,-1,-3,5,3,6,7], k = 3`
> 输出：`[3,3,5,5,6,7]`
> 解释：
> 滑动窗口的位置                            最大值
>
> ---------------               -----
>
> `[1  3  -1] -3  5  3  6  7       3
>  1 [3  -1  -3] 5  3  6  7       3
>  1  3 [-1  -3  5] 3  6  7       5
>  1  3  -1 [-3  5  3] 6  7       5
>  1  3  -1  -3 [5  3  6] 7       6
>  1  3  -1  -3  5 [3  6  7]      7`

**示例 2**

> 输入：`nums = [1], k = 1`
> 输出：`[1]`

**示例 3**

> 输入：`nums = [1,-1], k = 1`
> 输出：`[1,-1]`

**示例 4**

> 输入：`nums = [9,11], k = 2`
> 输出：`[11]`

**示例 5**

> 输入：`nums = [4,-2], k = 2`
> 输出：`[4]`

**提示**

> + $1 <= nums.length <= 10^5$
> + $-10^4 <= nums[i] <= 10^4$
> + $1 <= k <= nums.length$

**题解**

> 1. <font style="color: red">**记住，单调队列是双端队列即可**</font>

**代码**

```c++
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& g, int k) {
        deque<int> dq;
        vector<int> res;
        for (int i = 0; i < g.size(); i ++ ) {
            if (dq.size() && i - dq.front() + 1 > k) dq.pop_front();
            while (dq.size() && g[i] >= g[dq.back()]) dq.pop_back();
            dq.push_back(i);
            if (i >= k - 1) res.push_back(g[dq.front()]);
        }
        return res;
    }
};
```

**标签**

`滑动窗口`

#### end

### 阅读理解题

#### [LeetCode 275. H 指数 II](https://leetcode-cn.com/problems/h-index-ii/)

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

**标签**

`阅读理解`

#### end

### 多指针专题

#### 双指针

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

**标签**

`双指针`、`快慢指针`、`环`

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

**标签**

`双指针`、`快慢指针`、`环`

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

**标签**

`双指针`、`快慢指针`、`环`

#### 三指针

##### [LeetCode 75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

**题目描述**

> 给定一个包含红色、白色和蓝色、共 `n` 个元素的数组 `nums` ，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。
>
> 我们使用整数 `0、 1` 和 `2` 分别表示红色、白色和蓝色。
>
> 必须在不使用库的 `sort` 函数的情况下解决这个问题。

**示例 1**

> 输入：`nums = [2,0,2,1,1,0]`
> 输出：`[0,0,1,1,2,2]`

**示例 2**

> 输入：`nums = [2,0,1]`
> 输出：`[0,1,2]`

**提示**

> + $n == nums.length$
> + $1 <= n <= 300$
> + $nums[i] 为 0、1 或 2$

**进阶**

> 你可以不使用代码库中的排序函数来解决这道题吗？
> 你能想出一个仅使用常数空间的一趟扫描算法吗？

**手写稿**

> ![2260822](https://gitee.com/peter95535/image-bed/raw/master/img/2260822.png)
>
> 1. `0 ~ j - 1` 是红色，`j ~ i - 1` 是白色，`k +1 ~ n` 是蓝色，`i ~ k` 是待分配区域
> 2. 针对 `nums[i]` 的取值分三种情况：
>    + `nums[i]`  取值为 `0`，则交换 `nums[i ++]` 和 `nums[j ++]` 的值
>    +  `nums[i]` 取值为 `1`，则直接 `i ++`
>    + `nums[i]`  取值为 `2`，则交换 `nums[i]` 和 `nums[k -- ]` 的值
>      + 为啥不是 `i ++` ，因为 `i` 到 `k` 都是待分配区域，无法判别 `nums[i]` 是 `0，1` 或者 `2`，所以，不能 `++`

**代码**

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        for (int i = 0, j = 0, k = nums.size() - 1; i <= k;) {
            if (nums[i] == 0) swap(nums[j ++], nums[i ++]);
            else if (nums[i] == 1) i ++;
            else swap(nums[i], nums[k --]);
        }
        return;
    }
};
```

**标签**

`三指针`、`荷兰国旗`

#### end

### 模拟题专题

#### 模拟

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

**标签**

`模拟`

##### [LeetCode 273. 整数转换英文表示](https://leetcode-cn.com/problems/integer-to-english-words/)

**题目描述**

> 将非负整数 num 转换为其对应的英文表示。

**示例 1**

> 输入：`num = 123`
> 输出：`"One Hundred Twenty Three"`

**示例 2**

> 输入：`num = 12345`
> 输出：`"Twelve Thousand Three Hundred Forty Five"`

**示例 3**

> 输入：`num = 1234567`
> 输出：`"One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"`

**示例 4**

> 输入：`num = 1234567891`
> 输出：`"One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"`

**提示**

> + $0 <= num <= 2^{31} - 1$

**手写稿**

![1](https://gitee.com/peter95535/image-bed/raw/master/img/1.png)

**代码**

```c++
class Solution {
public:
    // 预处理数据
    string nums0_19[20] = {
        "Zero", "One", "Two", "Three", "Four", 
        "Five", "Six", "Seven", "Eight", "Nine",
        "Ten", "Eleven", "Twelve", "Thirteen",
        "Fourteen", "Fifteen", "Sixteen", "Seventeen",
        "Eighteen", "Nineteen"
    };
    string nums20_90[8] = {
        "Twenty", "Thirty", "Forty", "Fifty",
        "Sixty", "Seventy", "Eighty", "Ninety"
    };
    string nums1000_1e9[4] = {
        "Billion ", "Million ", "Thousand ", ""
    };
    string get(int x) {
        if (x == 0) return "Zero";
        string res;
        if (x >= 100) {
            res += nums0_19[x / 100] + " Hundred ";
            x %= 100;
        }
        if (x >= 20) {
            res += nums20_90[x / 10 - 2] + " ";
            x %= 10;
        }
        if (x) res += nums0_19[x] + " ";
        return res;
    }
    string numberToWords(int num) {
        if (num == 0) return "Zero";
        string res;
        for (int i = 1e9, j = 0; i >= 1; i /= 1000, j ++ )
            if (num >= i) {
                res += get(num / i) + nums1000_1e9[j];
                num %= i;
            }
        // 注意空格问题
        res.pop_back();
        return res;
    }
};
```

**标签**

`模拟`、`数字转换`、`中英文`

##### end

#### 表达式求值

##### [LeetCode 224. 基本计算器](https://leetcode-cn.com/problems/basic-calculator/)

**题目描述**

> 给你一个字符串表达式 s ，请你实现一个基本计算器来计算并返回它的值。

**示例 1**

> 输入：`s = "1 + 1"`
> 输出：`2`

**示例 2**

> 输入：`s = " 2-1 + 2 "`
> 输出：`3`

**示例 3**

> 输入：`s = "(1+(4+5+2)-3)+(6+8)"`
> 输出：`23`

**提示**

> + $1 <= s.length <= 3 * 10^5$
> + `s` 由数字`、'+'、'-'、'('、')'、`和 `' '` 组成
> + `s` 表示一个有效的表达式

**题解**

> 1. 使用两个栈，一个是数字栈 `num` ，另一个是字符串栈 `oper`
> 2. 遍历每一个字符，分以下五种情况：
>    + 如果遇到空格，直接 `continue`
>    + 如果遇到`(`，直接入栈 `oper`
>    + 如果遇到 `)` ，计算答案，直到 `oper` 栈顶为 `(` 的时候停止
>    + 如果遇到数字，则计算数字的大小，入栈 `num`<font style="color:red">**（注意数字不一定是一位数）**</font>
>    + 如果遇到 `+`，`-`，则继续进行如下判断：
>      + 如果当前值是第一位，例如 `+2`，则为了方便计算，在前面补一个 `0` 即可
>      + 如果当前位不是第一位并且前一位是括号，例如 `(+2)`，则为了方便，补一个 `0` 即可
>      + 除上述情况外，正常计算，直到栈为空或者遇到 `(` 的时候停止
> 3. 如果栈中还有运算符，则继续计算，直到栈为空，答案就是栈顶元素

**代码**

```c++
class Solution {
public:
    stack<int> num;
    stack<char> oper;
    void eval() {
        int b = num.top(); num.pop();
        int a = num.top(); num.pop();
        char op = oper.top(); oper.pop();
        if (op == '+') num.push(a + b);
        else num.push(a - b);
        return;
    }
    int calculate(string s) {
        for (int i = 0; i < s.size(); i ++ ) {
            if (s[i] == ' ') continue;
            else if (s[i] == '(') oper.push(s[i]);
            else if (s[i] == ')') {
                while (oper.top() != '(') eval();
                // 弹出左括号
                oper.pop();
            }
            else if (isdigit(s[i])) {
                int sum = 0;
                while (i < s.size() && isdigit(s[i]))
                    sum = sum * 10 + (s[i ++ ] - '0');
                num.push(sum);
                i --;
            }
            else {
                // 处理特殊情况，如 +2 + a 或者 (-2 + a) 等类似情况
                if (!i || s[i - 1] == '(') num.push(0);
                while (oper.size() && oper.top() != '(') eval();
                // 将当前括号入栈
                oper.push(s[i]);
            }
        }
        while (oper.size()) eval();
        return num.top();
    }
};
```

**标签**

`栈`、`表达式求值`

##### [LeetCode 282. 给表达式添加运算符](https://leetcode-cn.com/problems/expression-add-operators/)

**题目描述**

> 给定一个仅包含数字 `0-9` 的字符串 `num` 和一个目标值整数 `target` ，在 `num` 的数字之间添加 二元 运算符（不是一元）`+`、`-` 或 `*` ，返回所有能够得到目标值的表达式。

**示例 1**

> 输入: `num = "123", target = 6`
> 输出: `["1+2+3", "1*2*3"] `

**示例 2**

> 输入: `num = "232", target = 8`
> 输出: `["2*3+2", "2+3*2"]`

**示例 3**

> 输入: `num = "105", target = 5`
> 输出: `["1*0+5","10-5"]`

**示例 4**

> 输入: `num = "00", target = 0`
> 输出: `["0+0", "0-0", "0*0"]`

**示例 5**

> 输入: `num = "3456237490", target = 9191`
> 输出: `[]`

**提示**

> + $1 <= num.length <= 10$
> + $num 仅含数字$
> + $-2^{31} <= target <= 2^{31} - 1$

**手写稿**

![w](https://gitee.com/peter95535/image-bed/raw/master/img/w.png)

**代码**

```c++
class Solution {
public:
    typedef long long LL;
    string path;
    vector<string> res;
    // 保证答案不越界，但是不保证中间过程不越界
    void dfs(string& num, int u, int len, LL a, LL b, LL target) {
        if (u == num.size())
            if (a == target) {
                res.push_back(path.substr(0, len - 1));
                return;
            }
        LL c = 0;
        for (int i = u; i < num.size(); i ++ ) {
            c = c * 10 + (num[i] - '0');
            path[len ++ ] = num[i];
            // 枚举第len位填写的运算符
            // +
            path[len] = '+';
            dfs(num, i + 1, len + 1, a + b * c, 1, target);
            if (i + 1 < num.size()) {
                // -
                path[len] = '-';
                dfs(num, i + 1, len + 1, a + b * c, -1, target);
                // *
                path[len] = '*';
                dfs(num, i + 1, len + 1, a, b * c, target);
            }
            // 不能有前导0
            if (num[u] == '0') return;
        }
        return;
    }
    vector<string> addOperators(string num, int target) {
        // 扩容
        path.resize(100);
        /*
        	参数1: num字符串
        	参数2: 当前枚举的是num中的哪位字符
        	参数3: 答案字符串的长度
        	参数4: a的值
        	参数5: b的值
        	参数6: 目标值
        */
        dfs(num, 0, 0, 0, 1, target);
        return res;
    }
};
```

**标签**

`dfs`、`表达式求值`

##### end

#### end

### 树专题

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

**标签**

`dfs`、`字典树`

#### 线段树

##### 原理

![1310947](https://gitee.com/peter95535/image-bed/raw/master/img/1310947.png)

##### [AcWing 1275. 最大数](https://www.acwing.com/problem/content/1277/)

**题目描述**

> 给定一个正整数数列 $a_1,a_2,…,a_n$，每一个数都在 $0∼p−1$ 之间。
>
> 可以对这列数进行两种操作：
>
> 1. 添加操作：向序列后添加一个数，序列长度变成 `n+1`；
> 2. 询问操作：询问这个序列中最后 `L` 个数中最大的数是多少。
>
> 程序运行的最开始，整数序列为空。
>
> 一共要对整数序列进行 `m` 次操作。
>
> 写一个程序，读入操作的序列，并输出询问操作的答案。

**输入格式**

> 第一行有两个正整数 `m,p`，意义如题目描述；
>
> 接下来 `m` 行，每一行表示一个操作。
>
> 如果该行的内容是 `Q L`，则表示这个操作是询问序列中最后 `L` 个数的最大数是多少；
>
> 如果是 `A t`，则表示向序列后面加一个数，加入的数是 `(t+a) mod p`。其中，`t` 是输入的参数，`a` 是在这个添加操作之前最后一个询问操作的答案（如果之前没有询问操作，则 `a=0`）。
>
> 第一个操作一定是添加操作。对于询问操作，`L>0` 且不超过当前序列的长度。

**输出格式**

> 对于每一个询问操作，输出一行。该行只有一个数，即序列中最后 `L` 个数的最大数。

**数据范围**

> + $1≤m≤2×10^5$,
> + $1≤p≤2×10^9$,
> + $0≤t<p$

**输入样例**

```c++
10 100
A 97
Q 1
Q 1
A 17
Q 2
A 63
Q 1
Q 1
Q 3
A 99
```

**输出样例**

```c++
97
97
97
60
60
97
```

**样例解释**

> 最后的序列是 `97,14,60,96`。

**备注**

![2111618](https://gitee.com/peter95535/image-bed/raw/master/img/2111618.png)

**代码**

```c++
#include <iostream>
using namespace std;
const int N = 200010;
int n, m, p;
struct Node {
    int l, r, Max; // Max是区间[l, r]的最大值
}tr[N * 4];
void build(int u, int l, int r) { // u当前节点的下标，[l, r]是区间左右端点
    tr[u] = {l, r};
    // 区间只有一个数字
    if (l == r) return;
    int mid = l + r >> 1;
    // 左子树
    build(u << 1, l, mid);
    // 右子树
    build(u << 1 | 1, mid + 1, r);
    return;
}
int query(int u, int l, int r) {
    // 如果线段树的区间完全包含在所查询区间的内部，则直接返回最大值即可
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].Max;
    int mid = tr[u].l + tr[u].r >> 1;
    int Max = 0;
    /**
    	问题：为啥不判断两个区间不相交的情况？
    	回答：查看备注
    */
    // 如果左边区间有重合，则查询左边
    // 至于为啥不能写成query(u << 1, l, mid)，查看原理的样例模拟即可知晓
    if (l <= mid) Max = query(u << 1, l, r);
    // 如果右边区间有重合，则查询右边
    // 至于为啥不能写成query(u << 1, l, mid)，查看原理的样例模拟即可知晓
    if (r > mid) Max = max(Max, query(u << 1 | 1, l, r));
    return Max;
}
// 由子节点更新父节点
void pushup(int u) {
    tr[u].Max = max(tr[u << 1].Max, tr[u << 1 | 1].Max);
    return;
}
void modify(int u, int x, int c) {
    // 如果到达叶子结点即区间左右端点都是x，则修改值即可
    // 由于只有一个值，因此，区间最大值即为当前值本身
    if (tr[u].l == x && tr[u].r == x) tr[u].Max = c;
    else {
        int mid = tr[u].l + tr[u].r >> 1;
        // 和左边有交集
        if (x <= mid) modify(u << 1, x, c);
        // 和右边有交集
        if (x > mid) modify(u << 1 | 1, x, c);
        // 由子节点更新父节点
        pushup(u);
    }
    return;
}
int main() {
    scanf("%d%d", &m, &p);
    // 最多有m个操作，因此，区间右端点为m
    build(1, 1, m);
    int x, last = 0;
    char op[2];
    while (m -- ) {
        scanf("%s%d", op, &x);
        if (op[0] == 'Q') {
            // 查询最后x个数字的最大值，其区间为[n - x + 1, n]
            last = query(1, n - x + 1, n);
            cout << last << endl;
        }
        else {
            // 修改第n + 1个值
            modify(1, n + 1, (x + 0LL + last) % p);
            n ++;
        }
    }
    return 0;
}
```

**标签**

`线段树`

##### [AcWing 245. 你能回答这些问题吗](https://www.acwing.com/problem/content/246/)

**题目描述**

> 给定长度为 `N` 的数列 `A`，以及 `M` 条指令，每条指令可能是以下两种之一：
>
> 1. `1 x y`，查询区间 `[x,y]` 中的最大连续子段和，即 $max_{x≤l≤r≤y}\{∑\limits_{i=l}^rA[i]\}$。
> 2. `2 x y`，把 `A[x]` 改成 `y`。
>
> 对于每个查询指令，输出一个整数表示答案。

**输入格式**

> 第一行两个整数 `N,M`。
>
> 第二行 `N` 个整数 `A[i]`。
>
> 接下来 `M` 行每行 `3` 个整数 `k,x,y`，`k=1` 表示查询（此时如果 `x>y`，请交换 `x,y`），`k=2` 表示修改。

**输出格式**

> 对于每个查询指令输出一个整数表示答案。
>
> 每个答案占一行。

**数据范围**

> + $N≤500000,M≤100000$,
> + $−1000≤A[i]≤1000$

**输入样例**

```c++
5 3
1 2 -3 4 5
1 2 3
2 2 -1
1 3 2
```

**输出样例**

```c++
2
-1
```

**手写稿**

![2111320](https://gitee.com/peter95535/image-bed/raw/master/img/2111320.jpg)

**代码**

```c++
#include <iostream>
using namespace std;
const int N = 500010;
struct Node {
    int l, r; // 区间左右端点
    int maxSum; // 最大连续子区间和
    int lMaxSum, rMaxSum; // 最大前缀和和最大后缀和
    int sum; // 区间和
}tr[N * 4];
int n, m;
int g[N];
void pushup(Node& u, Node& l, Node& r) {
    u.maxSum = max(max(l.maxSum, r.maxSum), l.rMaxSum + r.lMaxSum);
    u.lMaxSum = max(l.lMaxSum, l.sum + r.lMaxSum);
    u.rMaxSum = max(r.rMaxSum, r.sum + l.rMaxSum);
    u.sum = l.sum + r.sum;
    return;
}
// 由父节点更新子节点
void pushup(int u) {
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
    return;
}
void build(int u, int l, int r) {
    // 到达叶子节点[l, r]是下标，记录所有属性值
    if (l == r) tr[u] = {l, r, g[l], g[l], g[l], g[l]};
    else {
        // 如果没有到达叶子结点，则只记录区间端点即可，别的值暂时无法得出
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        // 更新【父节点】中除了区间端点之外的其余属性值即可
        pushup(u);
    }
    return;
}
// 由于树状数组的节点是结构体，因此，返回值是节点【结构体】
Node query(int u, int l, int r) {
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    else {
        int mid = tr[u].l + tr[u].r >> 1;
        // 区间查询区间可能不存在
        // 如果查询的区间在线段树区间的左边
        if (r <= mid) return query(u << 1, l, r);
        // 如果查询的区间在线段树区间的右边
        else if (l > mid) return query(u << 1 | 1, l, r);
        else {
            // 如果查询的区间和左右端点有交集
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            // 使用结构体节点
            Node res;
            pushup(res, left, right);
            return res;
        }
    }
}
void modify(int u, int x, int c) {
    // 找到叶子节点
    if (tr[u].l == x && tr[u].r == x) tr[u] = {x, x, c, c, c, c};
    else {
        // 根据题目描述修改的区间一定是合法区间，一定会有相交
        int mid = tr[u].l + tr[u].r >> 1;
        // 如果和左区间有交集
        if (x <= mid) modify(u << 1, x, c);
        // 如果和右区间有交集
        else modify(u << 1 | 1, x, c);
        // 修改完记得更新父节点
        pushup(u);
    }
    return;
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &g[i]);
    build(1, 1, n);
    int k, x, y;
    while (m -- ) {
        scanf("%d%d%d", &k, &x, &y);
        if (k == 1) {
            // 如果左端点大于右端点，则交换端点即可
            if (x > y) swap(x, y);
            cout << query(1, x, y).maxSum << endl;
        }
        else modify(1, x, y);
    }
    return 0;
}
```

**标签**

`线段树`

##### [AcWing 246. 区间最大公约数](https://www.acwing.com/problem/content/247/)

**题目描述**

> 给定一个长度为 `N` 的数列 `A`，以及 `M` 条指令，每条指令可能是以下两种之一：
>
> 1. `C l r d`，表示把 `A[l],A[l+1],…,A[r]` 都加上 `d`。
> 2. `Q l r`，表示询问 `A[l],A[l+1],…,A[r]` 的最大公约数(`GCD`)。
>
> 对于每个询问，输出一个整数表示答案。

**输入格式**

> 第一行两个整数 `N,M`。
>
> 第二行 `N` 个整数 `A[i]`。
>
> 接下来 `M` 行表示 `M` 条指令，每条指令的格式如题目描述所示。

**输出格式**

> 对于每个询问，输出一个整数表示答案。
>
> 每个答案占一行。

**数据范围**

> + $N≤500000,M≤100000$,
> + $1≤A[i]≤10^{18}$,
> + $|d|≤10^{18}$

**输入样例**

```c++
5 5
1 3 5 7 9
Q 1 5
C 1 5 1
Q 1 5
C 3 3 6
Q 2 4
```

**输出样例**

```c++
1
2
4
```

**手写稿**

![2012309](https://gitee.com/peter95535/image-bed/raw/master/img/2012309.jpg)

**代码**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 500010;
struct Node {
    int l, r; // 区间左右端点
    LL sum, d; // 区间和和区间的最大公约数
}tr[N * 4];
int n, m;
LL g[N];
// 注意类型LL
LL gcd(LL a, LL b) {
    if (!b) return a;
    return gcd(b, a % b);
}
void pushup(Node& u, Node& l, Node& r) {
    u.sum = l.sum + r.sum;
    u.d = gcd(l.d, r.d);
    return;
}
void pushup(int u) {
    pushup(tr[u], tr[u << 1], tr[u << 1 | 1]);
    return;
}
void build(int u, int l, int r) {
    if (l == r) {
        // 注意类型LL
        LL a = g[r] - g[r - 1];
        tr[u] = {l, r, a, a};
    }
    else {
        tr[u] = {l, r};
        int mid = l + r >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        // 注意更新下父节点u
        pushup(u);
    }
    return;
}
Node query(int u, int l, int r) {
    // 如果线段树中的区间包含在所查询区间的内部，则直接返回
    if (tr[u].l >= l && tr[u].r <= r) return tr[u];
    else {
        int mid = tr[u].l + tr[u].r >> 1;
        // 如果查询区间出现在线段树区间的左边
        if (r <= mid) return query(u << 1, l, r);
        // 如果查询区间出现在线段树区间的右边
        else if (l > mid) return query(u << 1 | 1, l, r);
        else {
            // 如果查询区间处于线段树区间的两边
            auto left = query(u << 1, l, r);
            auto right = query(u << 1 | 1, l, r);
            Node res;
            pushup(res, left, right);
            return res;
        }
    }
}
void modify(int u, int x, LL c) {
    if (tr[u].l == x && tr[u].r == x) {
        // 注意类型LL
        LL a = tr[u].sum + c;
        tr[u] = {x, x, a, a};
    }
    else {
        int mid = tr[u].l + tr[u].r >> 1;
        // 如果数x位于左区间
        if (x <= mid) modify(u << 1, x, c);
        // 如果数x位于右区间
        else modify(u << 1 | 1, x, c);
        // 更新父节点u
        pushup(u);
    }
    return;
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%lld", &g[i]);
    build(1, 1, n);
    char op[2];
    int l, r;
    LL d;
    while (m -- ) {
        scanf("%s", op);
        if (op[0] == 'Q') {
            scanf("%d%d", &l, &r);
            // 注意最大公约数可能是负数，记得取绝对值
            cout << abs(gcd(query(1, 1, l).sum, query(1, l + 1, r).d)) << endl; 
        }
        else {
            scanf("%d%d%lld", &l, &r, &d);
            modify(1, l, d);
            // 记得判断是否越界
            if (r + 1 <= n) modify(1, r + 1, -d);
        }
    }
    return 0;
}
```

**标签**

`差分`、`树状数组`

##### [AcWing 243. 一个简单的整数问题2](https://www.acwing.com/problem/content/244/)

**题目描述**

> 给定一个长度为 `N` 的数列 `A`，以及 `M` 条指令，每条指令可能是以下两种之一：
>
> 1. `C l r d`，表示把 `A[l],A[l+1],…,A[r]` 都加上 `d`。
> 2. `Q l r`，表示询问数列中第 `l∼r` 个数的和。
>
> 对于每个询问，输出一个整数表示答案。

**输入格式**

> 第一行两个整数 `N,M`。
>
> 第二行 `N` 个整数 `A[i]`。
>
> 接下来 `M` 行表示 `M` 条指令，每条指令的格式如题目描述所示。

**输出格式**

> 对于每个询问，输出一个整数表示答案。
>
> 每个答案占一行。

**数据范围**

> + $1≤N,M≤10^5$,
> + $|d|≤10000$,
> + $|A[i]|≤10^9$

**输入样例**

```c++
10 5
1 2 3 4 5 6 7 8 9 10
Q 4 4
Q 1 10
Q 2 4
C 3 6 3
Q 2 4
```

**输出样例**

```c++
4
55
9
15
```

**步骤**

> 1. `pushdown` 操作（懒标记）
>     + 用途
>         + 将当前区间中的每一个数字都添加上 `d`
>     + 步骤
>         + 第一步：查看当前根节点 `u` 所在区间是否属于查询的区间
>         + 第二步：更新相对应的信息
>         + 第三步：将懒标记 `add` 设置为 `0`，表示已经下放完毕
> 2. 注意 `pushdown` 和 `pushup` 的位置

**代码**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 100010;
struct Node {
    int l, r;
    LL sum; // 区间和
    int add; // 懒标记
}tr[N * 4];
int n, m;
int g[N];
void pushup(int u) {
    // 懒标记【不包含】当前节点
    tr[u].sum = tr[u << 1].sum + tr[u << 1 | 1].sum;
    return;
}
void pushdown(int u) {
    auto &root = tr[u], &left = tr[u << 1], &right = tr[u << 1 | 1];
    // 如果有懒标记，则将懒标记进行下放
    if (root.add) {
        // 左孩子的懒标记进行累加，同时，区间和也需要累加
        left.add += root.add, left.sum += (LL)(left.r - left.l + 1) * root.add;
        // 右孩子的懒标记进行累加，同时，区间和也需要累加
        right.add += root.add, right.sum += (LL)(right.r - right.l + 1) * root.add;
        // 懒标记置为0
        root.add = 0;
    }
    return;
}
void build(int u, int l, int r) {
    // 如果到达叶子结点，则更新相对应的信息
    if (l == r) tr[u] = {l, r, g[l], 0};
    else {
        // 更新区间的左右端点信息
        tr[u] = {l, r};
        int mid = tr[u].l + tr[u].r >> 1;
        // 如果和左区间有交集，建立树状数组
        build(u << 1, l, mid);
        // 如果和右区间有交集，建立树状数组
        build(u << 1 |1, mid + 1, r);
        // 由子节点更新父节点
        pushup(u);
    }
    return;
}
void modify(int u, int l, int r, int d) {
    // 如果树状数组区间的左右端点包含在要查询的区间内部，则直接更新相对应的信息
    if (tr[u].l >= l && tr[u].r <= r) {
        tr[u].sum += (LL)(tr[u].r - tr[u].l + 1) * d;
        tr[u].add += d;
    }
    else { // 必须要进行分裂
        // 分裂之前必须要进行pushdown操作，即下放懒标记操作
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        // 如果和左区间有交集
        if (l <= mid) modify(u << 1, l, r, d);
        // 如果和右区间有交集
        if (r > mid) modify(u << 1 | 1, l, r, d);
        // 由子节点信息更新父节点信息
        pushup(u);
    }
    return;
}
LL query(int u, int l, int r) {
    // 如果树状数组区间的左右端点包含在要查询的区间内部，则直接更新相对应的信息
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].sum;
    // 要进行分裂
    pushdown(u);
    int mid = tr[u].l + tr[u].r >> 1;
    LL sum = 0;
    // 如果和左边区间有交集
    if (l <= mid) sum += query(u << 1, l, r);
    // 如果和右边区间有交集
    if (r > mid) sum += query(u << 1 | 1, l, r);
    return sum;
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &g[i]);
    build(1, 1, n);
    while (m -- ) {
        char op[2];
        int l, r, d;
        scanf("%s", op);
        if (op[0] == 'C') {
            scanf("%d%d%d", &l, &r, &d);
            modify(1, l, r, d);
        }
        else {
            scanf("%d%d", &l, &r);
            cout << query(1, l, r) << endl;
        }
    }
    return 0;
}
```

**标签**

`树状数组`、`懒标记`

##### [AcWing 247. 亚特兰蒂斯](https://www.acwing.com/problem/content/249/)

**题目描述**

> 有几个古希腊书籍中包含了对传说中的亚特兰蒂斯岛的描述。
>
> 其中一些甚至包括岛屿部分地图。
>
> 但不幸的是，这些地图描述了亚特兰蒂斯的不同区域。
>
> 您的朋友 `Bill` 必须知道地图的总面积。
>
> 你自告奋勇写了一个计算这个总面积的程序。

**输入格式**

> 输入包含多组测试用例。
>
> 对于每组测试用例，第一行包含整数 `n`，表示总的地图数量。
>
> 接下来 `n` 行，描绘了每张地图，每行包含四个数字 $x_1,y_1,x_2,y_2$（不一定是整数），$(x_1,y_1)$ 和 $(x_2,y_2)$ 分别是地图的左上角位置和右下角位置。
>
> 注意，坐标轴 `x` 轴从上向下延伸，`y` 轴从左向右延伸。
>
> 当输入用例 `n=0` 时，表示输入终止，该用例无需处理。

**输出格式**

> 每组测试用例输出两行。
>
> 第一行输出 `Test case #k`，其中 `k` 是测试用例的编号，从 `1` 开始。
>
> 第二行输出 `Total explored area: a`，其中 `a` 是总地图面积（即此测试用例中所有矩形的面积并，注意如果一片区域被多个地图包含，则在计算总面积时只计算一次），精确到小数点后两位数。
>
> 在每个测试用例后输出一个空行。

**数据范围**

> + $1≤n≤10000$,
> + $0≤x1<x2≤100000$,
> + $0≤y1<y2≤100000$
> + 注意，本题 $n$ 的范围上限加强至 $10000$。

**输入样例**

```c++
2
10 10 20 20
15 15 25 25.5
0
```

**输出样例**

```c++
Test ccase #1
Total explored area: 180.00 
```

**样例解释**

> 样例所示地图覆盖区域如下图所示，两个矩形区域所覆盖的总面积，即为样例的解。

![2051745.png](https://gitee.com/peter95535/image-bed/raw/master/img/19_4acba44c27-%E6%97%A0%E6%A0%87%E9%A2%98-20220205174554149.png)

**手写稿**

![2071347](https://gitee.com/peter95535/image-bed/raw/master/img/2071347.png)

**疑点详解**

> 1. 线段树中存储的节点是线段不是点，注意两者之间的对应关系
>
>     ![2071518](https://gitee.com/peter95535/image-bed/raw/master/img/2071518.png)
>
> 1. 离散化的原因
>
>     + 注意树状数组中的值就是下标，并且`y` 轴上的端点不一定是整数，有可能是小数，因此，为了防止出现小数的情况，故使用离散化处理
>
>     + 举例说明 `g = [1, 1.5, 2, 5]`
>         + 如果不使用离散化，则不能计算 `g[1 ~ 1.5]` 的和
>         + 如果使用离散化，则 `1，1.5，2，2.5` 所对应的下标为 `0，1，2，3` 都是整数，则可以计算
>
> 1. 树状数组中的节点的属性值 cnt 分类讨论的原因
>
>     ![2071546](https://gitee.com/peter95535/image-bed/raw/master/img/2071546.png)

**代码**

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
const int N = 10010;
struct Segment {
    double x, y1, y2;
    // 表明当前线段是左边界还是右边界，左边界的权值为1，右边界的权值为-1
    int k;
    bool operator < (Segment &T) { // 按照横坐标x进行排序
        return x < T.x;
    }
}segs[N * 2]; // 1个矩形对应2个横坐标，因此，N个矩形对应2 * N个横坐标
struct Node {
    int l, r;
    int cnt; // 当前区间被覆盖的层数
    double len; // 当前y轴上大于1的区间长度，注意变量的类型
}tr[N * 8]; // 左边界横坐标对应4N空间，右边界横坐标对应4N空间，共8N空间
int n;
vector<double> ys; // 存储y轴上所有的无重复的点，用来做离散化，注意变量类型
// 查找大于等于y的数据的下标所在位置
int find(double y) {
    return lower_bound(ys.begin(), ys.end(), y) - ys.begin();
}
void pushup(int u) {
    // 如果当前线段树区间被覆盖，则当前区间的长度即为线段树的长度len
    if (tr[u].cnt) tr[u].len = ys[tr[u].r + 1] - ys[tr[u].l];
    else if (tr[u].l != tr[u].r) // 如果不是叶节点
        tr[u].len = tr[u << 1].len + tr[u << 1 | 1].len;
    else tr[u].len = 0; // 如果是叶节点记得初始化长度为0
    return;
}
void build(int u, int l, int r) {
    // 初始化树状数组，不用管tr的区间问题，即使l != r也要进行初始化
    tr[u] = {l, r, 0, 0};
    if (l != r) {
        int mid = l + r >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }
    return;
}
void modify(int u, int l, int r, int k) {
    if (tr[u].l >= l && tr[u].r <= r) {
        tr[u].cnt += k;
        // 计算长度，因为k不一定为正数，如果cnt == 0，则需要通过当前根节点的子节点进行计算
        pushup(u);
    }
    else {
        int mid = tr[u].l + tr[u].r >> 1;
        if (l <= mid) modify(u << 1, l, r, k);
        if (r > mid) modify(u << 1 | 1, l, r, k);
        // 计算长度，因为k不一定为正数，如果cnt == 0，则需要通过当前根节点的子节点进行计算
        pushup(u);
    }
    return;
}
int main() {
    int T = 0;
    while (scanf("%d", &n), n) {
        ys.clear(); // 多组输入，注意每次清空ys里面的数据
        for (int i = 0, j = 0; i < n; i ++ ) {
            double x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            segs[j ++ ] = {x1, y1, y2, 1};
            segs[j ++ ] = {x2, y1, y2, -1};
            ys.push_back(y1);
            ys.push_back(y2);
        }
        // 对于y轴上的点进行排序，方便去重处理
        sort(ys.begin(), ys.end());
        // 去重的前提是有序序列，因此，事先要排序，原因在于此
        ys.erase(unique(ys.begin(), ys.end()), ys.end());
        // 建立线段树，区间的个数等于点的个数减一，点的范围[0, n - 1],换算区间的话，再减一，区间的范围为[0, n - 2]
        build(1, 0, ys.size() - 2);
        // 对于segs进行排序
        sort(segs, segs + n * 2);
        // 计算总面积
        double res = 0;
        for (int i = 0; i < n * 2; i ++ ) {
            if (i > 0) res += tr[1].len * (segs[i].x - segs[i - 1].x);
            modify(1, find(segs[i].y1), find(segs[i].y2) - 1, segs[i].k);
        }
        printf("Test case #%d\nTotal explored area: %.2lf\n\n", ++T, res);
    }
    return 0;
}
```

**标签**

`树状数组`、`扫描线`

##### [AcWing 1277. 维护序列](https://www.acwing.com/problem/content/1279/)

**题目描述**

> 老师交给小可可一个维护数列的任务，现在小可可希望你来帮他完成。
>
> 有长为 `N` 的数列，不妨设为 $a_1,a_2,…,a_N$。
>
> 有如下三种操作形式：
>
> 1. 把数列中的一段数全部乘一个值；
> 2. 把数列中的一段数全部加一个值；
> 3. 询问数列中的一段数的和，由于答案可能很大，你只需输出这个数模 `P` 的值。

**输入格式**

> 第一行两个整数 `N` 和 `P`；
>
> 第二行含有 `N` 个非负整数，从左到右依次为 $a_1,a_2,…,a_N$；
>
> 第三行有一个整数 `M`，表示操作总数；
>
> 从第四行开始每行描述一个操作，输入的操作有以下三种形式：
>
> - 操作 `1`：`1 t g c`，表示把所有满足 $t≤i≤g$ 的 $a_i$ 改为 $a_i×c$；
> - 操作 `2`：`2 t g c`，表示把所有满足 $t≤i≤g$ 的 $a_i$ 改为 $a_i+c$；
> - 操作 `3`：`3 t g`，询问所有满足 $t≤i≤g$ 的 $a_i$ 的和模 `P` 的值。
>
> 同一行相邻两数之间用一个空格隔开，每行开头和末尾没有多余空格。

**输出格式**

对每个操作 `3`，按照它在输入中出现的顺序，依次输出一行一个整数表示询问结果。

**数据范围**

> + $1≤N,M≤10^5$,
> + $1≤t≤g≤N$,
> + $0≤c,a_i≤10^9$,
> + $1≤P≤10^9$

**输入样例**

```c++
7 43
1 2 3 4 5 6 7
5
1 2 5 5
3 2 4
2 3 7 9
3 1 3
3 4 7
```

**输出样例**

```c++
2
35
8
```

**样例解释**

> 初始时数列为 `{1,2,3,4,5,6,7}`；
>
> 经过第 `1` 次操作后，数列为 `{1,10,15,20,25,6,7}`；
>
> 对第 `2` 次操作，和为 `10+15+20=45`，模 `43` 的结果是 `2`；
>
> 经过第 `3` 次操作后，数列为 `{1,10,24,29,34,15,16}`；
>
> 对第 `4` 次操作，和为 `1+10+24=35`，模 `43` 的结果是 `35`；
>
> 对第 `5` 次操作，和为 `29+34+15+16=94`，模 `43` 的结果是 `8`。

**解题步骤**

> 1. 线段树中维护五个信息
>
>     + 区间左右端点 `[l, r]`
>     + 区间和 `sum`
>     + 懒标记 `add` 和 `mul`，表示区间内的每个数字添加 `add` 和 乘以 `mul`
>
> 2. 当线段树中的节点同时含有懒标记 `add` 和 `mul` 的时候，如何确定顺序，以方便好维护信息？
>
>     + 分两种情况：先 `add` 在 `mul` 和先 `mul` 再 `add`
>
>         + 第一种情况：先 `add` 再 `mul`
>
>             ![2081729](https://gitee.com/peter95535/image-bed/raw/master/img/2081729.png)
>
>         + 第二种情况：先 `mul` 再 `add`
>
>             ![2081730](https://gitee.com/peter95535/image-bed/raw/master/img/2081730.png)
>
>         + 综上所述，顺序应该为先 `mul` 再 `add` 即可
>
>     + 确定完顺序之后，按照顺序先 `mul` 再 `add` 的操作，进行分析，查看值的变化
>
>         ![2082022](https://gitee.com/peter95535/image-bed/raw/master/img/2082022.png)
>
>         + 备注
>             + `x ✖️ mul + add` 是原来的数，也就是区间和 `sum`
>             + 按照顺序先 `mul` 再 `add`，以方便区分变量，故使用 `mul'` 和 `add'` 来表示即可
>
> 3. 使用先 `mul` 再 `add` 的顺序简化区间加和区间乘操作
>
>     + 区间加
>         + 令 `mul = 1，add = d` 即可
>     + 区间乘
>         + 令 `mul = d，add = 0` 即可

**代码**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 100010;
struct Node {
    int l, r; // 左右区间
    LL sum; // 区间和
    int mul, add; // 区间乘懒标记mul和区间加懒标记add
}tr[N * 4];
int n, m, p;
int g[N];
void eval(Node& root, int mul, int add) {
    // 原理查看解析
    root.sum = (root.sum * mul + (LL)(root.r - root.l + 1) * add) % p;
    root.mul = (LL)root.mul * mul % p;
    root.add = ((LL)root.add * mul + add) % p;
    return;
}
void pushup(int u) {
    // 记得取模
    tr[u].sum = (tr[u << 1].sum + tr[u << 1 | 1].sum) % p;
    return;
}
void pushdown(int u) {
    eval(tr[u << 1], tr[u].mul, tr[u].add);
    eval(tr[u << 1 | 1], tr[u].mul, tr[u].add);
    // 记得将懒标记清空
    tr[u].mul = 1, tr[u].add = 0;
    return;
}
void build(int u, int l, int r) {
    if (l == r) tr[u] = {l, r, g[l], 1, 0};
    else {
        // sum先随便设置一个值
        tr[u] = {l, r, 0, 1, 0};
        int mid = tr[u].l + tr[u].r >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }
    return;
}
void modify(int u, int l, int r, int mul, int add) {
    if (tr[u].l >= l && tr[u].r <= r) eval(tr[u], mul, add);
    else { // 区间要进行分裂
        // 懒标记下传
        pushdown(u);
        int mid = tr[u].l + tr[u].r >> 1;
        // 左边有交集
        if (l <= mid) modify(u << 1, l, r, mul, add);
        // 右边有交集
        if (r > mid) modify(u << 1 | 1, l, r, mul, add);
        // 由子节点更新父节点
        pushup(u);
    }
    return;
}
int query(int u, int l, int r) {
    if (tr[u].l >= l && tr[u].r <= r) return tr[u].sum;
    // 查询的时候需要将懒标记下传
    pushdown(u);
    int sum = 0;
    int mid = tr[u].l + tr[u].r >> 1;
    // 左边有交集
    if (l <= mid) sum += query(u << 1, l, r);
    // 右边有交集，记得取模
    if (r > mid) sum = (sum + query(u << 1 | 1, l, r)) % p;
    return sum;
}
int main() {
    scanf("%d%d", &n, &p);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &g[i]);
    // 建立线段树
    build(1, 1, n);
    scanf("%d", &m);
    while (m -- ) {
        int num, l, r, k;
        scanf("%d", &num);
        // 区间乘
        if (num == 1) {
            scanf("%d%d%d", &l, &r, &k);
            modify(1, l, r, k, 0);
        }
        // 区间加
        else if (num == 2) {
            scanf("%d%d%d", &l, &r, &k);
            modify(1, l, r, 1, k);
        }
        // 区间查询
        else {
            scanf("%d%d", &l, &r);
            cout << query(1, l, r) << endl;
        }
    }
    return 0;
}
```

**标签**

`线段树`

#### 树状数组

##### 原理

**手写稿**

![1262039](https://gitee.com/peter95535/image-bed/raw/master/img/1262039.png)

##### 注意事项

> 1. <font style="color: red">**先搞明白树状数组存的值的意义，此点至关重要！！！**</font>

##### [P3374 【模板】树状数组 1](https://www.luogu.com.cn/problem/P3374)

**题目描述**

> 如题，已知一个数列，你需要进行下面两种操作：
>
> - 将某一个数加上 `x`
> - 求出某区间每一个数的和

**输入格式**

> 第一行包含两个正整数 `n,m`，分别表示该数列数字的个数和操作的总个数。
>
> 第二行包含 `n` 个用空格分隔的整数，其中第 `i` 个数字表示数列第 `i` 项的初始值。
>
> 接下来 `m` 行每行包含 `3` 个整数，表示一个操作，具体如下：
>
> - `1 x k` 含义：将第 `x` 个数加上 `k`
> - `2 x y` 含义：输出区间 `[x,y]` 内每个数的和

**输出格式**

> 输出包含若干行整数，即为所有操作 `2` 的结果。

**输入 #1**

```c++
5 5
1 5 4 2 3
1 1 3
2 2 5
1 3 -1
1 4 2
2 1 4
```

**输出 #1**

```c++
14
16
```

**数据范围**

> 对于 `30%` 的数据，$1≤n≤8$，$1≤m≤10$；
> 对于 `70%` 的数据，$1≤n,m≤10^4$；
> 对于 `100%` 的数据，$1≤n,m≤5×10^5$。

**样例说明**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/2256.png)

故输出结果`14、16`

**代码**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 500010;
int n, m;
LL tr[N];
int lowbit(int x) {
    return x & -x;
}
void add(int x, int c) {
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
    return;
}
int sum(int x) {
    LL res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1, t; i <= n; i ++ ) {
        scanf("%d", &t);
        // 一开始在树状数组的下标为i的位置上添加t
        add(i, t);
        // 注意必须使用add函数，因为add函数当中包含对其他下标的值的更新
    }
    while (m -- ) {
        int op, x, y;
        scanf("%d%d%d", &op, &x, &y);
        if (op == 1) add(x, y);
        else cout << sum(y) - sum(x - 1) << endl;
    }
    return 0;
}
```

**标签**

`树状数组`

##### [AcWing 241. 楼兰图腾](https://www.acwing.com/problem/content/243/)

**题目描述**

> 在完成了分配任务之后，西部 `314` 来到了楼兰古城的西部。
>
> 相传很久以前这片土地上(比楼兰古城还早)生活着两个部落，一个部落崇拜尖刀(`V`)，一个部落崇拜铁锹(`∧`)，他们分别用 `V` 和 `∧` 的形状来代表各自部落的图腾。
>
> 西部 `314` 在楼兰古城的下面发现了一幅巨大的壁画，壁画上被标记出了 `n` 个点，经测量发现这 `n` 个点的水平位置和竖直位置是两两不同的。
>
> 西部 `314` 认为这幅壁画所包含的信息与这 `n` 个点的相对位置有关，因此不妨设坐标分别为 $(1,y_1),(2,y_2),…,(n,y_n)，$其中 $y_1∼y_n$ 是 `1` 到 `n` 的一个排列。
>
> 西部 `314` 打算研究这幅壁画中包含着多少个图腾。
>
> 如果三个点 $(i,y_i),(j,y_j),(k,y_k)$ 满足 `1≤i<j<k≤n` 且 $y_i>y_j,y_j<y_k$，则称这三个点构成 `V` 图腾;
>
> 如果三个点 $(i,y_i),(j,y_j),(k,y_k)$ 满足 `1≤i<j<k≤n` 且 $y_i<y_j,y_j>y_k$，则称这三个点构成 `∧` 图腾;
>
> 西部 `314` 想知道，这 `n` 个点中两个部落图腾的数目。
>
> 因此，你需要编写一个程序来求出 `V` 的个数和 `∧` 的个数。

**输入格式**

> 第一行一个数 `n`。
>
> 第二行是 `n` 个数，分别代表 `y1，y2,…,yn`。

**输出格式**

> 两个数，中间用空格隔开，依次为 `V` 的个数和 `∧` 的个数。

**数据范围**

> 对于所有数据，`n≤200000`，且输出答案不会超过 `int64`。
> $y_1∼y_n$ 是 `1` 到 `n` 的一个排列。

**输入样例**

```c++
5
1 5 3 2 4
```

**输出样例**

```c++
3 4
```

**手写稿**

> 1. 使用 `g[i]` 表示原数组；`tr[i]` 表示在树状数组中<font style="color: red">下标为 `i` </font>的位置上有一个数字，使用 `1` 做标记；如果下标为 `i` 的位置上没有数字，使用 `0` 做标记，由于数组初始化为 `0`，因此，不需要再额外做标记 ；`great[i]` 表示 `g` 数组中比 `g[i]` 大的数字的个数；`lesser[i]` 表示 `g` 数组中比 `g[i]` 小的数字的个数
> 2. 以 `g` 数组的每一个下标 `i` 为索引，寻找 `i` 左边比 `g[i]` 大的数字的个数和 `i` 右边比 `g[i]` 大的数字的个数，根据乘法原理，两者即可求得 $\vee$ 的个数；同理，求得 $\wedge$ 的个数
> 3. 两次遍历【<font style="color: red">极易混淆需重点阅读</font>】
>     + 第一次遍历【<font style="color: red">从前往后</font>】
>         + 在当前情况（从前往后）下，`great[i]` 表示 `g` 数组中所有下标在 `i` <font style="color: red">之前</font>的所有数字当中比 `g[i]` <font style="color: red">大</font> 的数字的个数；`less[i]` 表示 `g` 数组中所有下标在 `i` <font style="color: red">之前</font>的所有数字当中比 `g[i]` <font style="color: red">小 </font>的数字的个数
>     + 第二次遍历【<font style="color: red">从后往前</font>】
>         + 在当前情况（从后往前）下，`great[i]` 表示 `g` 数组中所有下标在 `i` <font style="color: red">之后</font>的所有数字当中比 `g[i]` <font style="color: red">大</font> 的数字的个数；`less[i]` 表示 `g` 数组中所有下标在 `i` <font style="color: red">之后</font>的所有数字当中比 `g[i]` <font style="color: red">小</font> 的数字的个数

**代码**

```c++
#include <iostream>
#include <cstring>
using namespace std;
typedef long long LL;
const int N = 200010;
int n;
int g[N], tr[N], great[N], lesser[N];
int lowbit(int x) {
    return x & -x;
}
void add(int x, int c) {
    for (int i = x; i <= n; i += lowbit(i) ) tr[i] += c;
    return;
}
int sum(int x) {
    int res = 0; 
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}
int main() {
    scanf("%d", &n);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &g[i]);
    // 【从前往后】遍历数组
    for (int i = 1; i <= n; i ++ ) {
        int y = g[i];
        // great[i]表示在【下标】i【之前】的所有数字当中比y【大】的数字的个数
        great[i] = sum(n) - sum(y);
        // lesser[i]表示在【下标】i【之前】的所有数字当中比y【小】的数字的个数
        lesser[i] = sum(y - 1);
        // 在下标为y的位置上添加1，表示在下标为y的位置上有一个数字
        add(y, 1);
    }
    // 初始化数组
    memset(tr, 0, sizeof tr);
    LL res1 = 0, res2 = 0;
    // 【从后往前】遍历数组
    for (int i = n; i; i -- ) {
        int y = g[i];
        // sum(n) - sum(y) 表示在【下标】为i【之后】的所有数字当中比y【大】的数字的个数
        res1 += great[i] * (LL)(sum(n) - sum(y));
        // sum(y - 1) 表示在【下标】为i【之后】的所有数字当中比y【小】的数字的个数
        res2 += lesser[i] * (LL)sum(y - 1);
        // 在下标为y的位置上添加1，表示在下标为y的位置上有一个数字
        add(y, 1);
    }
    printf("%lld %lld\n", res1, res2);
    return 0;
}
```

**标签**

`树状数组`

##### [AcWing 242. 一个简单的整数问题](https://www.acwing.com/problem/content/description/248/)

**题目描述**

> 给定长度为 `N` 的数列 `A`，然后输入 `M` 行操作指令。
>
> 第一类指令形如 `C l r d`，表示把数列中第 `l∼r` 个数都加 `d`。
>
> 第二类指令形如 `Q x`，表示询问数列中第 `x` 个数的值。
>
> 对于每个询问，输出一个整数表示答案。

**输入格式**

> 第一行包含两个整数 `N` 和 `M`。
>
> 第二行包含 `N` 个整数 `A[i]`。
>
> 接下来 `M` 行表示 `M` 条指令，每条指令的格式如题目描述所示。

**输出格式**

> 对于每个询问，输出一个整数表示答案。
>
> 每个答案占一行。

**数据范围**

> + $1≤N,M≤10^5$,
> + $|d|≤10000$,
> + $|A[i]|≤10^9$

**输入样例**

```c++
10 5
1 2 3 4 5 6 7 8 9 10
Q 4
Q 1
Q 2
C 1 6 3
Q 2
```

**输出样例**

```c++
4
1
2
5
```

**步骤**

> 1. 树状数组可以解决的问题：
>     + 区间求和
>     + 单点更新
>
> 2. 本题的要求有两个操作
>
>     + 将原数组变成树状数组（差分数组）
>
>     + 将题目中的两个操作变成树状数组的经典操作
>
>         + 第一个操作是数列中的第 `l ～ r` 个数字都加上 `d`，分若干个步骤将其转化为树状数组的经典操作之一即单点更新
>             + 第 `l ～ r` 个数字添加 `d`，由差分数组 `tr` 可知，`tr[l] += d，tr[r + 1] -= d` 即可完成操作，对应树状数组经典操作的单点更新
>
>         + 第二个操作是查询数列中的第 `x` 个数字，分若干个步骤将其转化为树状数组的经典操作之一即区间求和
>             + 修改第 `x` 个数字，由差分数组 `tr` 可知，`sum[1 ~ x]` 即为第 `x` 个数字的值，对应树状数组的区间求和操作

**代码**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 100010;
int n, m;
int g[N], tr[N];
int lowbit(int x) {
    return x & -x;
}
void add(int x, int c) {
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
    return;
}
int sum(int x) {
    int res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}
int main() {
    scanf("%d%d",&n, &m);
    for (int i = 1; i <= n; i ++ ) {
        scanf("%d", &g[i]);
        // 构建差分树状数组
        add(i, g[i] - g[i - 1]);
    }
    char op[2];
    while (m -- ) {
        scanf("%s", op);
        if (op[0] == 'C') {
            int l, r, d;
            scanf("%d%d%d", &l, &r, &d);
            // tr[l] += d;
            add(l, d);
            // tr[r + 1] -= d;
            add(r + 1, -d);
        }
        else {
            int x;
            scanf("%d", &x);
            // 求和
            cout << sum(x) << endl;
        }
    }
    return 0;
}
```

##### [AcWing 243. 一个简单的整数问题2](https://www.acwing.com/problem/content/244/)

**题目描述**

> 给定一个长度为 `N` 的数列 `A`，以及 `M` 条指令，每条指令可能是以下两种之一：
>
> 1. `C l r d`，表示把 `A[l],A[l+1],…,A[r]` 都加上 `d`。
> 2. `Q l r`，表示询问数列中第 `l∼r` 个数的和。
>
> 对于每个询问，输出一个整数表示答案。

**输入格式**

> 第一行两个整数 `N,M`。
>
> 第二行 `N` 个整数 `A[i]`。
>
> 接下来 `M` 行表示 `M` 条指令，每条指令的格式如题目描述所示。

**输出格式**

> 对于每个询问，输出一个整数表示答案。
>
> 每个答案占一行。

**数据范围**

> + $1≤N,M≤10^5,$
> + $|d|≤10000,$
> + $|A[i]|≤10^9$

**输入样例**

```c++
10 5
1 2 3 4 5 6 7 8 9 10
Q 4 4
Q 1 10
Q 2 4
C 3 6 3
Q 2 4
```

**输出样例**

```c++
4
55
9
15
```

**手写稿**

![1292230](https://gitee.com/peter95535/image-bed/raw/master/img/1292230.png)

**代码**

```c++
#include <iostream>
using namespace std;
typedef long long LL;
const int N = 100010;
int n, m;
int g[N];
LL tr1[N], tr2[N];
int lowbit(int x) {
    return x & -x;
}
void add(LL tr[], int x, LL c) {
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
    return;
}
LL sum(LL tr[], LL x) {
    LL res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}
LL prefix_sum(int x) {
    return sum(tr1, x) * (x + 1) - sum(tr2, x);
}
int main() {
    scanf("%d%d", &n, &m);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &g[i]);
    for (int i = 1; i <= n; i ++ ) {
        int b = g[i] - g[i - 1];
        // 第一个数组tr1存的是红+紫
        add(tr1, i, b);
        // 第二个数组tr2存的是紫
        add(tr2, i, (LL)i * b);
    }
    while (m -- ) {
        char op[2];
        scanf("%s", op);
        if (op[0] == 'C') {
            int l, r, d;
            scanf("%d%d%d", &l, &r, &d);
            add(tr1, l, d), add(tr2, l, l * d);
            add(tr1, r + 1, -d), add(tr2, r + 1, (r + 1) * -d);
        }
        else {
            int l, r;
            scanf("%d%d", &l, &r);
            cout << prefix_sum(r) - prefix_sum(l - 1) << endl;
        }
    }
    return 0;
}
```

**标签**

`树状数组`

##### [AcWing 244. 谜一样的牛](https://www.acwing.com/problem/content/245/)

**题目描述**

> 有 `n` 头奶牛，已知它们的身高为 `1∼n` 且各不相同，但不知道每头奶牛的具体身高。
>
> 现在这 `n` 头奶牛站成一列，已知第 `i` 头牛前面有 $A_i$ 头牛比它低，求每头奶牛的身高。

**输入格式**

> 第 `1` 行：输入整数 `n`。
>
> 第 `2..n` 行：每行输入一个整数 $A_i$，第 `i` 行表示第 `i` 头牛前面有 $A_i$ 头牛比它低。
> （注意：因为第 `1` 头牛前面没有牛，所以并没有将它列出）

**输出格式**

> 输出包含 `n` 行，每行输出一个整数表示牛的身高。
>
> 第 `i` 行输出第 `i` 头牛的身高。

**数据范围**

> + $1≤n≤10^5$

**输入样例**

```c++
5
1
2
1
0
```

**输出样例**

```c++
2
4
5
3
1
```

**手写稿**

![1301406](https://gitee.com/peter95535/image-bed/raw/master/img/1301406.png)

**步骤**

> 1. `h[i]` 代表在第 `i` 头牛之前有 `h[i]` 头牛比当前第 `i` 头牛矮
> 2. 从后往前遍历，在剩下的数字（包括当前数字）当中寻找第 `h[i] + 1` 小的数字即可
> 3. 样例模拟（从后往前）
>     + 初始化：牛的高度可选择范围 `1，2，3，4，5`（不一定有序）
>     + 第一步：倒数第一个数字是 `0`，代表在牛的高度可选择范围`[1, 2, 3, 4, 5]`内有 `0` 头牛比自己矮，即在牛的高度可选择范围`[1, 2, 3, 4, 5]`内寻找第 `1(h[i] + 1)` 小的高度的牛，即为 `1`，此时，剩下的牛的高度选择可为 `2，3，4，5`
>     + 第二步：倒数第二个数字是 `1`，代表在牛的高度可选择范围`[2, 3, 4, 5]`内有 `1` 头牛比自己矮，即在牛的高度可选择范围`[2, 3, 4, 5]`内寻找第 `2(h[i] + 1)` 小的高度的牛，即为 `3`，此时，剩下的牛的高度选择可为 `2，4，5`
>     + 第三步：倒数第三个数字是 `2`，代表在牛的高度可选择范围`[2, 4, 5]`内有 `2` 头牛比自己矮，即在牛的高度可选择范围`[2, 4, 5]`内寻找第 `3(h[i] + 1)` 小的高度的牛，即为 `5`，此时，剩下的牛的高度选择可为 `2，4`
>     + 第四步：倒数第四个数字是 `1`，代表在牛的高度可选择范围`[2, 4]`内有 `1` 头牛比自己矮，即在牛的高度可选择范围`[2, 4]`内寻找第 `2(h[i] + 1)` 小的高度的牛，即为 `4`，此时，剩下的牛的高度可选择为 `2`
>     + 第五步：剩下的牛的高度为 `2`，故答案为 `[2, 4, 5, 3, 1]`
> 4. 实现方式：树状数组 + 二分
>     + 树状数组 `tr[i]` 表示高度为 `i` 是否可以使用，`1` 代表可使用，否则，不可使用
>         + 树状数组初始化为 `1`，表明一开始，每个数字都可以使用
>     + 树状数组 `sum[i]` 前缀和是单调的，故可以使用二分
> 5. 注意事项
>     + 二分的是【<font style = "color: red">**牛的高度**</font>】
>     + 树状数组 `tr[i]` 表示高度为 `i` 的牛是否可以使用，`1` 代表可以使用，否则，不可使用

**代码**

```c++
#include <iostream>
using namespace std;
const int N = 100010;
int n;
int h[N], tr[N], ans[N];
int lowbit(int x) {
    return x & -x;
}
void add(int x, int c) {
    for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
    return;
}
int sum(int x) {
    int res = 0;
    for (int i = x; i; i -= lowbit(i)) res += tr[i];
    return res;
}
int main() {
    scanf("%d", &n);
    // 下标从2开始，因为第一头牛前面没有比它矮的，默认为0，题目中已经标注
    for (int i = 2; i <= n; i ++ ) scanf("%d", &h[i]);
    // 每头牛初始化为1，表明当前这头牛的高度i可以被使用【未被剔除】
    for (int i = 1; i <= n; i ++ ) add(i, 1);
    for (int i = n; i; i --) {
        // 寻找i前面的第h[i]小的数，算上当前值，故寻找第h[i] + 1个数字
        int k = h[i] + 1;
        // 二分的是牛的高度，即查看当前牛的高度是否满足答案
        int l = 1, r = n;
        while (l < r) {
            int mid = l + r >> 1;
            // mid代表牛的高度
            if (sum(mid) >= k) r = mid;
            else l = mid + 1;
        }
        // 将结果加入答案
        ans[i] = r;
        // 将已经加入答案的数字进行排除
        add(r, -1);
    }
    for (int i = 1; i <= n; i ++ ) cout << ans[i] << endl;
    return 0;
}
```

**标签**

`树状数组`

##### [LeetCode 307. 区域和检索 - 数组可修改](https://leetcode-cn.com/problems/range-sum-query-mutable/)

**题目描述**

> 给你一个数组 `nums` ，请你完成两类查询。
>
> 其中一类查询要求 更新 数组 `nums` 下标对应的值
> 另一类查询要求返回数组 `nums` 中索引 `left` 和索引 `right` 之间（ 包含 ）的 `nums` 元素的 和 ，其中 `left <= right`
> 实现 `NumArray` 类：
>
> + `NumArray(int[] nums)` 用整数数组 `nums` 初始化对象
> + `void update(int index, int val)` 将 `nums[index]` 的值 更新 为 `val`
> + `int sumRange(int left, int right)` 返回数组 `nums` 中索引 `left` 和索引 `right` 之间（ 包含 ）的 `nums` 元素的 和 `（即，nums[left] + nums[left + 1], ..., nums[right]）`

**示例 1**

> 输入：
> `["NumArray", "sumRange", "update", "sumRange"]
> [[[1, 3, 5]], [0, 2], [1, 2], [0, 2]]`
> 输出：
> `[null, 9, null, 8]`
>
> 解释：
> `NumArray numArray = new NumArray([1, 3, 5]);
> numArray.sumRange(0, 2); // 返回 1 + 3 + 5 = 9
> numArray.update(1, 2);   // nums = [1,2,5]
> numArray.sumRange(0, 2); // 返回 1 + 2 + 5 = 8`

**提示**

> + $1 <= nums.length <= 3 * 10^4$
> + $-100 <= nums[i] <= 100$
> + $0 <= index < nums.length$
> + $-100 <= val <= 100$
> + $0 <= left <= right < nums.length$
> + $调用 pdate 和 sumRange 方法次数不大于 3 * 10^4 $

**解题步骤**

> 1. 树状数组解决的基本问题
>
>     + 单点更新
>     + 区间求和
>
> 2. 本题单点更新略有特殊，本题的单点更新不是将某个值 `tr[i] += val` ，而是 `tr[i] = val` ，因此，本题的 `val` 应该为 `val = val - nums[i]` ，这样，`tr[i] += val` 变成 `tr[i] = tr[i] + val - nums[i] = val` ，同时，将 `nums[i] = val` 才符合题意
>
> 3. 必须更新的原因
>
>     + 模拟样例
>
>         + 输入
>
>             > `["NumArray","update","sumRange","update","sumRange"]`
>             > `[[[7]],[0,2],[0,0],[0,9],[0,0]]`
>
>         + 输出
>
>             > `[null,null,2,null,9]`

**代码**

```c++
class NumArray {
public:
    int n;
    vector<int> tr, nums;
    int lowbit(int x) {
        return x & -x;
    }
    void add(int x, int c) {
        for (int i = x; i <= n; i += lowbit(i)) tr[i] += c;
        return;
    }
    int sum(int x) {
        int res = 0;
        for (int i = x; i; i -= lowbit(i)) res += tr[i];
        return res;
    }
    NumArray(vector<int>& _nums) {
        nums = _nums;
        n = nums.size();
        nums.resize(n + 1);
        tr = vector<int>(n + 5);
        // 构建树状数组
        for (int i = 0; i < n; i ++ ) add(i + 1, nums[i]);
    }
    
    void update(int index, int val) {
        // 树状数组下标从1开始
        add(index + 1, val - nums[index]);
        // 更新原数组
        // 更新缘由在解题步骤中
        nums[index] = val;
        return;
    }
    
    int sumRange(int left, int right) {
        left ++, right ++;
        return sum(right) - sum(left - 1);
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * obj->update(index,val);
 * int param_2 = obj->sumRange(left,right);
 */
```

**标签**

`树状数组`

#### 二叉树

##### [LeetCode 297. 二叉树的序列化与反序列化](https://leetcode-cn.com/problems/serialize-and-deserialize-binary-tree/)

**题目描述**

> 序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。
>
> 请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。
>
> 提示: 输入输出格式与 `LeetCode` 目前使用的方式一致，详情请参阅 `LeetCode` 序列化二叉树的格式。你并非必须采取这种方式，你也可以采用其他的方法解决这个问题。

**示例 1**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/serdeser.jpg)

> 输入：`root = [1,2,3,null,null,4,5]`
> 输出：`[1,2,3,null,null,4,5]`

**示例 2**

> 输入：`root = []`
> 输出：`[]`

**示例 3**

> 输入：`root = [1]`
> 输出：`[1]`

**示例 4**

> 输入：`root = [1,2]`
> 输出：`[1,2]`

**提示**

> + $树中结点数在范围 [0, 10^4] 内$
> + $-1000 <= Node.val <= 1000$

**手写稿**

![WX20220121-142021@2x](https://gitee.com/peter95535/image-bed/raw/master/img/WX20220121-142021@2x.png)

**代码**

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:
    string str;
    void dfs_s(TreeNode* root) {
        if (!root) {
            str += "#,";
            return;
        }
        str += to_string(root -> val) + ",";
        dfs_s(root -> left);
        dfs_s(root -> right);
        return;
    }
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        // 深搜
        dfs_s(root);
        return str;
    }
    TreeNode* dfs_d(string& data, int &u) {
        if (data[u] == '#') {
            // 跳过#和后面的,
            u += 2;
            return NULL;
        }
        int k = u;
        while (data[u] != ',') u ++;
        auto root = new TreeNode(stoi(data.substr(k, u - k)));
        // 跳过,
        u ++;
        root -> left = dfs_d(data, u);
        root -> right = dfs_d(data, u);
        return root;
    }
    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        int u = 0;
        return dfs_d(data, u);
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```

**标签**

`dfs`、`数据结构`

##### [LeetCode 331. 验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

**题目描述**

>   序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 `#`。
>
>   ```c++
>   	_9_
>       /   \
>      3     2
>     / \   / \
>    4   1  #  6
>   / \ / \   / \
>   # # # #   # #
>   ```
>
>   例如，上面的二叉树可以被序列化为字符串 `"9,3,4,#,#,1,#,#,2,#,6,#,#"`，其中 `#` 代表一个空节点。
>
>   给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。
>
>   每个以逗号分隔的字符或为一个整数或为一个表示 `null` 指针的 `'#'` 。
>
>   你可以认为输入格式总是有效的，例如它永远不会包含两个连续的逗号，比如 `"1,,3"` 。

**示例 1**

>   输入: `"9,3,4,#,#,1,#,#,2,#,6,#,#"`
>   输出: `true`

**示例 2**

>   输入: `"1,#"`
>   输出: `false`

**示例 3**

>   输入: `"9,#,#,1"`
>   输出: `false`

**手写稿**

>   ![321113](https://gitee.com/peter95535/image-bed/raw/master/img/321113-20220302123253489.png)
>
>   若代码不理解，建议模拟样例

**代码**

```c++
class Solution {
public:
    int k = 0;
    bool isValidSerialization(string s) {
        // 以逗号,结尾
        s += ',';
        if (!dfs(s)) return false;
        // 如果还没有计算到结尾，说明不符合条件，样例：3,#,#,1
        return k == s.size();
    }
    bool dfs(string& s) {
        // 如果已经到达元素结尾，返回false
        if (k == s.size()) return false;
        // 跳过#和,
        if (s[k] == '#') return k += 2, true;
        // 跳过数字
        while (isdigit(s[k])) k ++ ;
        // 跳过逗号
        k ++ ;
        // 遍历左子树和右子树
        return dfs(s) && dfs(s);
    }
};
```

**标签**

`二叉树`、`dfs`

### 字符串专题

#### KMP

###### [Leetode 214. 最短回文串](https://leetcode-cn.com/problems/shortest-palindrome/)

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

**标签**

`字符串`、`回文串`、`KMP`

### 扫描线问题

#### [LeetCode 218. 天际线问题](https://leetcode-cn.com/problems/the-skyline-problem/)

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

**标签**

`扫描线`

### 对顶堆

#### [LeetCode 295. 数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)

**题目描述**

> 中位数是有序列表中间的数。如果列表长度是偶数，中位数则是中间两个数的平均值。
>
> 例如，
>
> `[2,3,4]` 的中位数是 `3`
>
> `[2,3]` 的中位数是 `(2 + 3) / 2 = 2.5`
>
> 设计一个支持以下两种操作的数据结构：
>
> `void addNum(int num)` - 从数据流中添加一个整数到数据结构中。
> `double findMedian()` - 返回目前所有元素的中位数。

**示例**

> `addNum(1)`
> `addNum(2)`
> `findMedian() -> 1.5`
> `addNum(3) `
> `findMedian() -> 2`

**进阶**

> 如果数据流中所有整数都在 `0` 到 `100` 范围内，你将如何优化你的算法？
> 如果数据流中 `99%` 的整数都在 `0` 到 `100` 范围内，你将如何优化你的算法？

**手写稿**

![WX20220120-215203@2x](https://gitee.com/peter95535/image-bed/raw/master/img/WX20220120-215203@2x.png)

**记忆技巧**

> **大根堆**
>
> > 1. 记忆技巧：大根堆即为把<font style="color:red">**小于等于(`less`)**</font>当前数的数字往下按
> > 2. 定义：`priority_queue<int, vector<int>> q;`
>
> **小根堆**
>
> > 1. 记忆技巧：小根堆即为把<font style="color:red">**大于等于(`greater`)**</font>当前数的数字往下按
> > 2. 定义：`priority_queue<int, vector<int>, greater<int>> q;`

**代码**

```c++
class MedianFinder {
public:
    // 大根堆
    priority_queue<int, vector<int>> left;
    // 小根堆
    priority_queue<int, vector<int>, greater<int>> right;
    MedianFinder() {}
    
    void addNum(int num) {
        if (left.empty() || num <= left.top()) {
            left.push(num);
            // 左边添加完数据之后，如果出现左边元素的个数大于右边元素的个数 + 1
            // 因为保证左边元素的数量等于右边元素的数量或者等于右边元素的数量 + 1
            // 则说明需要进行调整，调整即可
            if (left.size() > right.size() + 1) right.push(left.top()), left.pop();
        }
        else {
            right.push(num);
            // 同理
            if (right.size() > left.size()) left.push(right.top()), right.pop();
        }
        return;
    }
    
    double findMedian() {
        if (left.size() + right.size() & 1) return left.top();
        return (left.top() + right.top()) / 2.0;
    }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

**标签**

`优先队列`、`对顶堆`

### 深度优先搜索

#### [LeetCode 306. 累加数](https://leetcode-cn.com/problems/additive-number/)

**题目描述**

> 累加数 是一个字符串，组成它的数字可以形成累加序列。
>
> 一个有效的 累加序列 必须 至少 包含 `3` 个数。除了最开始的两个数以外，序列中的每个后续数字必须是它之前两个数字之和。
>
> 给你一个只包含数字 `'0'-'9'` 的字符串，编写一个算法来判断给定输入是否是 累加数 。如果是，返回 `true` ；否则，返回 `false` 。
>
> 说明：累加序列里的数，除数字 `0` 之外，不会 以 `0` 开头，所以不会出现 `1, 2, 03` 或者 `1, 02, 3` 的情况。

**示例 1**

> 输入：`"112358"`
> 输出：`true`
> 解释：累加序列为: `1, 1, 2, 3, 5, 8 。1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8`

**示例 2**

> 输入：`"199100199"`
> 输出：`true`
> 解释：累加序列为: `1, 99, 100, 199。1 + 99 = 100, 99 + 100 = 199`

**提示**

> + $1 <= num.length <= 35$
> + $num 仅由数字（0 - 9）组成$

**进阶**

> 你计划如何处理由过大的整数输入导致的溢出?

**步骤**

> 1. 深度优先搜索，爆搜 `num` 字符串，枚举每个数字字符并且将其记录在 `path` 数组当中 ，查看是否满足答案
> 2. 递归出口
>    + 如果 `path` 的个数大于等于 `3` 个，进行判断 `path[n - 3] + path[n - 2] == path[n - 1]` 是否成立，分以下两种情况
>      + 如果条件成立，则继续向下操作
>      + 如果条件不成立，则直接截去当前分支，即返回 `false`
>    + 如果当前枚举的字符位置 `u` 为最后一个位置，则进行如下判断
>      + 如果 `path` 的个数大于等于 `3` 个，则说明存在一组答案，返回 `true`
>      + 否则，返回 `false`
> 3. 遍历字符串即可

**代码**

```c++
class Solution {
public:
    string add(string& a, string& b) {
        vector<int> A, B, C;
        for (int i = a.size() - 1; i >= 0; i -- ) A.push_back(a[i] - '0');
        for (int i = b.size() - 1; i >= 0; i -- ) B.push_back(b[i] - '0');
        for (int i = 0, t = 0; i < A.size() || i < B.size() || t; i ++ ) {
            if (i < A.size()) t += A[i];
            if (i < B.size()) t += B[i];
            C.push_back(t % 10);
            t /= 10;
        }
        string res;
        for (int i = C.size() - 1; i >= 0; i -- ) res += to_string(C[i]);
        return res;
    }
    bool dfs(string& num, vector<string>& path, int u) {
        int n = path.size();
        // 如果个数大于等于3个
        if (n >= 3) {
            string a = path[n - 3];
            string b = path[n - 2];
            string c = path[n - 1];
            string d = add(a, b);
            // 如果不相等，返回false
            if (d != c) return false;
        }
        // 如果枚举到最后一个字符
        if (u == num.size()) {
            // 如果个数大于等于3个，返回true
            if (path.size() >= 3) return true;
            return false;
        }
        for (int i = u; i < num.size(); i ++ ) {
            // 从u开始截取长度为i - u + 1的字符串
            path.push_back(num.substr(u, i - u + 1));
            // 如果能够搜索到正确答案，则返回true
            if (dfs(num, path, i + 1)) return true;
            // 回溯
            path.pop_back();
            // 如果出现前导0，则返回false
            // 注意，在后面添加此条件，说明当前字符已经被遍历过
            // 即0已经被遍历过，接下来要遍历的是形如01，02之类的数字，必不满足条件，返回即可
            if (num[u] == '0') return false;
        }
        return false;
    }
    bool isAdditiveNumber(string num) {
        vector<string> path;
        return dfs(num, path, 0);
    }
};
```

**标签**

`dfs`、`高精度`

### 贪心

#### [LeetCode 316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)

**问题描述**

> 给你一个字符串 `s` ，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证 返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

**示例 1**

> 输入：`s = "bcabc"`
> 输出：`"abc"`

**示例 2**

> 输入：`s = "cbacdcbc"`
> 输出：`"acdb"`

**提示**

> + $1 <= s.length <= 10^4$
> + $s$ 由小写英文字母组成
> + 注意：该题与 [`1081`](https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters) 相同

**手写稿**

![2181044](https://gitee.com/peter95535/image-bed/raw/master/img/2181044.png)

**代码**

```c++
class Solution {
public:
    string removeDuplicateLetters(string s) {
        string res;
        unordered_map<char, int> idxs;
        unordered_map<char, bool> is_appear;
        // 记录每个字符的下标所在的最后一位的位置
        for (int i = 0; i < s.size(); i ++ ) idxs[s[i]] = i;
        for (int i = 0; i < s.size(); i ++ ) {
            // 如果当前字符出现过，则直接进行下一次循环
            if (is_appear[s[i]]) continue;
            // 判断是否可以删除当前字符
            while (res.size() && res.back() > s[i] && idxs[res.back()] > i) {
                is_appear[res.back()] = false;
                res.pop_back();
            }
            res += s[i];
            is_appear[s[i]] = true;
        }
        return res;
    }
};
```

**标签**

`贪心`

### 链表专题

#### [LeetCode 328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

**题目描述**

> 给定单链表的头节点 `head` ，将所有索引为奇数的节点和索引为偶数的节点分别组合在一起，然后返回重新排序的列表。
>
> 第一个节点的索引被认为是 奇数 ， 第二个节点的索引为 偶数 ，以此类推。
>
> 请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。
>
> 你必须在 `O(1)` 的额外空间复杂度和 `O(n)` 的时间复杂度下解决这个问题。

**示例 1**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/oddeven-linked-list.jpg)

> 输入: `head = [1,2,3,4,5]`
> 输出: `[1,3,5,2,4]`

**示例 2**

![img](https://gitee.com/peter95535/image-bed/raw/master/img/oddeven2-linked-list.jpg)

> 输入: `head = [2,1,3,5,6,4,7]`
> 输出: `[2,3,6,7,1,5,4]`

**提示**

> + $n ==  链表中的节点数$
> + $0 <= n <= 10^4$
> + $-10^6 <= Node.val <= 10^6$

**手写稿**

![312202](https://gitee.com/peter95535/image-bed/raw/master/img/312202.png)

**代码**

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* p) {
        // odd是奇数链表，even是偶数链表
        auto odd = new ListNode(-1), even = new ListNode(-1);
        ListNode *a = odd, *b = even;
        while (p) {
            auto q = p -> next;
            p -> next = a -> next;
            a = a -> next = p;
            if (!q) break;
            auto r = q -> next;
            q -> next = b -> next;
            b = b -> next = q;
            p = r;
        }
        a -> next = even -> next;
        return odd -> next;
    }
};
```

**标签**

`链表`

### 迭代器专题

#### [LeetCode 341. 扁平化嵌套列表迭代器](https://leetcode-cn.com/problems/flatten-nested-list-iterator/)

**题目描述**

>   给你一个嵌套的整数列表 `nestedList` 。每个元素要么是一个整数，要么是一个列表；该列表的元素也可能是整数或者是其他列表。请你实现一个迭代器将其扁平化，使之能够遍历这个列表中的所有整数。
>
>   实现扁平迭代器类 `NestedIterator` ：
>
>   +   `NestedIterator(List<NestedInteger> nestedList)` 用嵌套列表 `nestedList` 初始化迭代器。
>   +   `int next()` 返回嵌套列表的下一个整数。
>   +   `boolean hasNext()` 如果仍然存在待迭代的整数，返回 `true` ；否则，返回 `false` 。
>       你的代码将会用下述伪代码检测：
>
>   ```c++
>   initialize iterator with nestedList
>   res = []
>   while iterator.hasNext()
>       append iterator.next() to the end of res
>   return res
>   ```
>
>
>   如果 `res` 与预期的扁平化列表匹配，那么你的代码将会被判为正确。

**示例 1**

>   输入：`nestedList = [[1,1],2,[1,1]]`
>   输出：`[1,1,2,1,1]`
>   解释：通过重复调用 `next` 直到 `hasNext` 返回 `false`，`next` 返回的元素的顺序应该是: `[1,1,2,1,1]`。

**示例 2**

>   输入：`nestedList = [1,[4,[6]]]`
>   输出：`[1,4,6]`
>   解释：通过重复调用 `next` 直到 `hasNext` 返回 `false`，`next` 返回的元素的顺序应该是: `[1,4,6]`。

**提示**

>   +   $1 <= nestedList.length <= 500$
>   +   $嵌套列表中的整数值在范围 [-106, 106] 内$

**手写稿**

![331157](https://gitee.com/peter95535/image-bed/raw/master/img/331157.png)

**代码一：树的遍历【递归写法】**

```c++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    vector<int> q;
    int k;
    void dfs(NestedInteger& u) {
        // 如果是叶子结点
        if (u.isInteger()) q.push_back(u.getInteger());
        // 遍历根节点的孩子节点
        else for (auto& u1 : u.getList()) dfs(u1);
        return;
    }
    NestedIterator(vector<NestedInteger> &nestedList) {
        k = 0;
        for (auto& u : nestedList) dfs(u);
    }
    
    int next() {
        return q[k ++];
    }
    
    bool hasNext() {
        return k < q.size();
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```

**代码二：迭代**

```c++
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    stack<NestedInteger> stk;
    NestedIterator(vector<NestedInteger> &nestedList) {
        for (int i = nestedList.size() - 1; i >= 0; i -- ) 
            stk.push(nestedList[i]);
    }
    
    int next() {
        int k = stk.top().getInteger(); stk.pop();
        return k;
    }
    
    bool hasNext() {
        while (stk.size()) {
            auto cur = stk.top();
            if (cur.isInteger()) return true;
            stk.pop();
            for (int i = cur.getList().size() - 1; i >= 0; i -- ) stk.push(cur.getList()[i]);
        }
        return false;
    }
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```

**标签**

`设计数据结构`、`递归`、`迭代器`

### 排序专题

#### 快排

##### [AcWing 786. 第k个数](https://www.acwing.com/problem/content/description/788/)

**题目描述**

> 给定一个长度为 `n` 的整数数列，以及一个整数 `k`，请用快速选择算法求出数列从小到大排序后的第 `k` 个数。

**输入格式**

> 第一行包含两个整数 `n` 和 `k`。
>
> 第二行包含 `n` 个整数（所有整数均在 $1∼10^9$ 范围内），表示整数数列。

**输出格式**

> 输出一个整数，表示数列的第 `k` 小数。

**数据范围**

> + $1≤n≤100000$,
> + $1≤k≤n$

**输入样例**

```c++
5 3
2 4 1 5 3
```

**输出样例**

```c++
3
```

**手写稿**

![2220937](https://gitee.com/peter95535/image-bed/raw/master/img/2220937.png)

**代码**

```c++
#include <iostream>
using namespace std;
const int N = 100010;
int n, k;
int g[N];
int quick_sort(int g[], int l, int r, int k) {
    if (l == r) return g[l];
    int x = g[l];
    int i = l - 1, j = r + 1;
    while (i < j) {
        // 为啥不能写等号，如果全部都是相同值会越界
        do i ++ ; while(g[i] < x);
        do j -- ; while(g[j] > x);
        if (i < j) swap(g[i], g[j]);
    }
    // 统计左边小于等于x的数目
    int sl = j - l + 1;
    // 递归左区间
    if (k <= sl) return quick_sort(g, l, j, k);
    // 递归右区间
    return quick_sort(g, j + 1, r, k - sl);
}
int main() {
    scanf("%d%d", &n, &k);
    for (int i = 1; i <= n; i ++ ) scanf("%d", &g[i]);
    cout << quick_sort(g, 1, n, k) << endl;
    return 0;
}
```

**标签**

`快速排序`、`快速选择算法`

##### [LeetCode 324. 摆动排序 II](https://leetcode-cn.com/problems/wiggle-sort-ii/)

**题目描述**

> 给你一个整数数组 `nums`，将它重新排列成 `nums[0] < nums[1] > nums[2] < nums[3]...` 的顺序。
>
> 你可以假设所有输入数组都可以得到满足题目要求的结果。

**示例 1**

> 输入：`nums = [1,5,1,1,6,4]`
> 输出：`[1,6,1,5,1,4]`
> 解释：`[1,4,1,5,1,6]` 同样是符合题目要求的结果，可以被判题程序接受。

**示例 2**

> 输入：`nums = [1,3,2,2,3,1]`
> 输出：`[2,3,1,3,1,2]`

**提示**

> + $1 <= nums.length <= 5 * 10^4$
> + $0 <= nums[i] <= 5000$
> + $题目数据保证，对于给定的输入 nums ，总能产生满足题目要求的结果$

**进阶**

> 你能用 `O(n)` 时间复杂度和 / 或原地 `O(1)` 额外空间来实现吗？

**手写稿**

![311119](https://gitee.com/peter95535/image-bed/raw/master/img/311119.png)

**代码**

```c++
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        int n = nums.size();
        // 找到中位数的迭代器
        auto midptr = nums.begin() + n / 2;
        nth_element(nums.begin(), midptr, nums.end());
        int mid = *midptr;
        // 三数排序，同LeetCode75题
        for (int i = 0, j = 0, k = n - 1; i <= k; ) {
            if (nums[i] < mid) swap(nums[i ++ ], nums[j ++ ]);
            else if (nums[i] == mid) i ++;
            else swap(nums[i], nums[k --]);
        }
        if (n & 1) ++ midptr;
        auto left = vector<int>(nums.begin(), midptr);
        // 逆序
        reverse(left.begin(), left.end());
        auto right = vector<int>(midptr, nums.end());
        // 逆序
        reverse(right.begin(), right.end());
        // 将前半部分放置在偶数位置
        for (int i = 0; i < left.size(); i ++ ) nums[i * 2] = left[i];
        // 将后半部分放置在奇数位置
        for (int i = 0; i < right.size(); i ++ ) nums[i * 2 + 1] = right[i];
        return;
    }
};
```

**标签**

`快速选择排序`

#### 计数排序

##### 原理

**简介**

>   计数排序不是基于比较的排序算法，其核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。 作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。

**动图演示**

![v2-3c7ddb59df2d21b287e42a7b908409cb_b](https://gitee.com/peter95535/image-bed/raw/master/img/v2-3c7ddb59df2d21b287e42a7b908409cb_b.gif)

##### [LeetCode 347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

**题目描述**

>   给你一个整数数组 `nums` 和一个整数 `k` ，请你返回其中出现频率前 `k` 高的元素。你可以按 任意顺序 返回答案。

**示例 1**

>   输入: `nums = [1,1,1,2,2,3], k = 2`
>   输出: `[1,2]`

**示例 2**

>   输入: `nums = [1], k = 1`
>   输出: `[1]`

**提示**

>   +   $1 <= nums.length <= 10^5$
>   +   $k 的取值范围是 [1, 数组中不相同的元素的个数]$
>   +   $题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的$

**进阶**

>   你所设计算法的时间复杂度 必须 优于 `O(n log n)` ，其中 `n` 是数组大小。

**手写稿**

![340943](https://gitee.com/peter95535/image-bed/raw/master/img/340943.png)

**代码**

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        int n = nums.size();
        // 统计每个数字出现的次数
        for (auto& num : nums) cnt[num] ++;
        vector<int> s(n + 1);
        for (auto [x, c] : cnt) s[c] ++;
        int i = n, t = 0;
        while (t < k) t += s[i --];
        vector<int> res;
        for (auto [x, c] : cnt)
            if (c > i) res.push_back(x);
        return res;
    }
};
```

**标签**

`计数排序`

#### 归并排序

##### 多路归并

###### [LeetCode 355. 设计推特](https://leetcode-cn.com/problems/design-twitter/)

**题目描述**

>   设计一个简化版的推特`(Twitter)`，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近 `10` 条推文。
>
>   实现 `Twitter` 类：
>
>   +   `Twitter()` 初始化简易版推特对象
>   +   `void postTweet(int userId, int tweetId)` 根据给定的 `tweetId` 和 `userId` 创建一条新推文。每次调用此函数都会使用一个不同的 `tweetId` 。
>   +   `List<Integer> getNewsFeed(int userId)` 检索当前用户新闻推送中最近  `10` 条推文的 `ID` 。新闻推送中的每一项都必须是由用户关注的人或者是用户自己发布的推文。推文必须 按照时间顺序由最近到最远排序 。
>   +   `void follow(int followerId, int followeeId)` `ID` 为 `followerId` 的用户开始关注 `ID` 为 `followeeId` 的用户。
>   +   `void unfollow(int followerId, int followeeId)` `ID` 为 `followerId` 的用户不再关注 `ID` 为 `followeeId` 的用户。

**示例**

>   输入
>   `["Twitter", "postTweet", "getNewsFeed", "follow", "postTweet", "getNewsFeed", "unfollow", "getNewsFeed"]
>   [[], [1, 5], [1], [1, 2], [2, 6], [1], [1, 2], [1]]`
>   输出
>   `[null, null, [5], null, null, [6, 5], null, [5]]`
>
>   解释
>   `Twitter twitter = new Twitter();`
>   `twitter.postTweet(1, 5);` // 用户 `1` 发送了一条新推文 (用户 `id = 1`, 推文 `id = 5`)
>   `twitter.getNewsFeed(1);`  // 用户 `1` 的获取推文应当返回一个列表，其中包含一个 `id` 为 `5` 的推文
>   `twitter.follow(1, 2);`    // 用户 `1` 关注了用户 `2`
>   `twitter.postTweet(2, 6);` // 用户 `2` 发送了一个新推文 (推文 `id = 6`)
>   `twitter.getNewsFeed(1);`  // 用户 `1` 的获取推文应当返回一个列表，其中包含两个推文，`id` 分别为 `-> [6, 5] `。推文 `id 6` 应当在推文 `id 5` 之前，因为它是在 `5` 之后发送的
>   `twitter.unfollow(1, 2);`  // 用户 `1` 取消关注了用户 `2`
>   `twitter.getNewsFeed(1);`  // 用户 `1` 获取推文应当返回一个列表，其中包含一个 `id` 为 `5` 的推文。因为用户 `1` 已经不再关注用户 `2`

**提示**

>   +   $1 <= userId, followerId, followeeId <= 500$
>   +   $0 <= tweetId <= 10^4$
>   +   $所有推特的 ID 都互不相同$
>   +   $postTweet、getNewsFeed、follow 和 unfollow 方法最多调用 3 * 10^4 次$

**手写稿**

![342158](https://gitee.com/peter95535/image-bed/raw/master/img/342158.png)

**代码**

```c++
typedef pair<int, int> PII;
#define x first
#define y second
class Twitter {
public:
    // 存储发布推文信息
    unordered_map<int, vector<PII>> tweets;
    // 存储用户的关注列表
    unordered_map<int, unordered_set<int>> followers;
    // 时间戳
    int ts;
    Twitter() {
        ts = 0;
    }
    
    void postTweet(int userId, int tweetId) {
        tweets[userId].push_back({ts ++ , tweetId});
        return;
    }
    
    vector<int> getNewsFeed(int u) {
        // 多路归并
        priority_queue<vector<int>> heap;
        followers[u].insert(u);
        // 遍历u的所有关注者【包括自己】，其原因是因为自己的推文也算在最新的10条推文内
        for (auto user : followers[u]) {
            // 获取当前用户所有推文
            auto& ts = tweets[user];
            // 如果当前用户还有剩余的推文
            if (ts.size()) {
                // 找到当前用户的【最新发布】的推文的位置【数组的最后一个元素的下标】
                int i = ts.size() - 1;
                // 【推文发布的时间，推文编号，推文的位置，所属用户】
                heap.push({ts[i].x, ts[i].y, i, user});
            }
        }
        vector<int> res;
        for (int i = 0; i < 10 && heap.size(); i ++ ) { // 最近的10条推文
            auto t = heap.top(); heap.pop();
            // 获取推文的编号
            res.push_back(t[1]);
            // 获取当前推文所属的下标位置
            int j = t[2];
            // 如果还有推文
            if (j) {
                // 多路归并的指针前移
                j -- ;
                // 获取当前推文所属的用户
                auto& user = t[3];
                // 获取用户的所有推文
                auto& ts = tweets[user];
                // 保存当前指针对应的推文的信息
                // 【推文发布的时间，推文编号，推文的位置，所属用户】
                heap.push({ts[j].x, ts[j].y, j, user});
            }
        }
        return res;
    }
    
    void follow(int followerId, int followeeId) {
        followers[followerId].insert(followeeId);
        return;
    }
    
    void unfollow(int followerId, int followeeId) {
        followers[followerId].erase(followeeId);
        return;
    }
};

/**
 * Your Twitter object will be instantiated and called as such:
 * Twitter* obj = new Twitter();
 * obj->postTweet(userId,tweetId);
 * vector<int> param_2 = obj->getNewsFeed(userId);
 * obj->follow(followerId,followeeId);
 * obj->unfollow(followerId,followeeId);
 */
```

**标签**

`多路归并`

### End







