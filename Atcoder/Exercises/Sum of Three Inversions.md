摘要：dp，状态压缩

[传送门：https://www.luogu.com.cn/problem/AT_stpc2025_1_c](https://www.luogu.com.cn/problem/AT_stpc2025_1_c)

## 题意

给定长度 $n$，三个常数 $x, y, k$，计数下列序列三元组 $(A, B, C)$ 的个数：

- $A, B, C$ 均为长度为 $n$ 的序列；
- $\forall i \in [1, n], (A_i, B_i, C_i)$ 是 $(1, 2, 3)$ 的一个排列。
- $A$ 中恰好有 $x$ 个 $1$，$y$ 个 $2$。
- 序列 $A, B, C$ 中的逆序对总数为 $k$。

$n \le 50$，答案对给定的常数 $m$ 取模。

## 分析思路

设 $z = n - x - y$，表示 $3$ 的个数。

可以直接对 $D_i = (A_i, B_i, C_i)$ 计数。考虑原序列中逆序对的和在 $D$ 中的体现。进行一些打表。可以发现大多数长度为 $3$ 的排列前后之间可以贡献的逆序对数量为 $1$，相同的排列之间贡献为 $0$，有一些排列前后之间可以贡献 $2$ 个逆序对：

- $(1, 2, 3)$ 之后的 $(3, 1, 2)$，$(3, 1, 2)$ 之后的 $(2, 3, 1)$，$(2, 3, 1)$ 之后的 $(1, 2, 3)$；
- $(1, 3, 2)$ 之后的 $(2, 1, 3)$，$(2, 1, 3)$ 之后的 $(3, 2, 1)$，$(3, 2, 1)$ 之后的 $(1, 3, 2)$。

可以发现这些排列可以分为两组，只有组内可能贡献 $2$ 或 $0$ 个逆序对。两组之间几乎独立。考虑设定基准贡献为一个逆序对，那么特殊的对之间的贡献就变成 $+1$ 或 $-1$ 了，而两组之间没有贡献，两组内的结构是相同的。

设 $f_{c_1, c_2, c_3, i}$ 表示现在分别有 $c_1, c_2, c_3$ 个组内的第一个为 $1$ 排列，第一个为 $2$ 排列，第一个为 $3$ 排列，当前逆序对贡献为 $i$ 的方案数。有如下转移：

$$
f_{c_1 + 1, c_2, c_3, i + (c_2 - c_1)} \gets f_{c_1, c_2, c_3, i} \\
f_{c_1, c_2 + 1, c_3, i + (c_3 - c_2)} \gets f_{c_1, c_2, c_3, i} \\
f_{c_1, c_2, c_3 + 1, i + (c_1 - c_3)} \gets f_{c_1, c_2, c_3, i}
$$

然后考虑把第一组插入到第二组里面。最终答案为：

$$
\sum_{a = 0}^x \sum_{b = 0}^y \sum_{c = 0}^z \sum_{i = -\frac{n(n-1)}{2}}^{\frac{n(n-1)}{2}} \binom{n}{a + b + c} f_{a, b, c, i} f_{x - a, y - b, z - c, k - i}
$$

时空复杂度为 $O(n^5)$，真能跑满吗？

分析一下，$c_1 + c_2 + c_3 \le n$，取值方案只有 $\binom{n + 3}{3}$ 种。再加上 $-\binom{c_1 + c_2 + c_3}{2} \le i \le \binom{c_1 + c_2 + c_3}{2}$。实测状态数大概只有 $10^7$ 左右。主播偷懒使用了记忆化搜索。


## 代码

```cpp
#include <bits/extc++.h>
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
int n, x, y, k, m, z, p;
int v1, v2, C[51][51], pw[51];
__gnu_pbds::gp_hash_table<int, int> dp[51][51];
inline int calc_hash(int c4, int inv) {
    return inv * v1 + c4;
}
int dfs(int c0, int c3, int c4, int inv) {
    if (c0 == 0 && c3 == 0 && c4 == 0) {
        return inv == 0;
    }
    int hsh = calc_hash(c4, inv);
    if (dp[c0][c3].find(hsh) != dp[c0][c3].end()) {
        return dp[c0][c3][hsh];
    }
    i64 res = 0;
    if (c0) res += dfs(c0 - 1, c3, c4, inv - (c3 - c0 + 1));
    if (c3) res += dfs(c0, c3 - 1, c4, inv - (c4 - c3 + 1));
    if (c4) res += dfs(c0, c3, c4 - 1, inv - (c0 - c4 + 1));
    return dp[c0][c3][hsh] = res % m;
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    i64 ans = 0;
    cin >> n >> x >> y >> k >> m, v1 = n + 1;
    z = n - x - y, p = n * (n - 1) / 2;
    for (int i = 0; i <= n; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++) {
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % m;
        }
    }
    for (int c0 = 0; c0 <= x; c0++) {         // a
        for (int c3 = 0; c3 <= y; c3++) {     // b
            for (int c4 = 0; c4 <= z; c4++) { // c
                i64 cur = 0;
                for (int d1 = -p; d1 <= p; d1++) {
                    int d2 = k - p - d1;
                    if (-p <= d2 && d2 <= p) {
                        cur = (cur + 1ll * dfs(c0, c3, c4, d1) * dfs(x - c0, y - c3, z - c4, d2)) % m;
                    }
                }
                ans = (ans + cur * C[n][c0 + c3 + c4]) % m;
            }
        }
    }
    cout << ans << endl;
    return 0;
}

```
