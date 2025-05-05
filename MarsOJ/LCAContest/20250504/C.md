摘要：dp，前缀和，状态压缩

[传送门：http://47.100.33.97/d/noipmoni/p/T250504C](http://47.100.33.97/d/noipmoni/p/T250504C)

## 题意

给定一个长度为 $n$ 的数组 $w$。对于一个长度为 $n$，满足 $\forall i \in [1, n], a_i \in [1, n]$ 的数组 $a$，定义 $f(a) = \prod_{i = 1}^{n - 2} w_{\max(a_i, a_{i + 1}, a_{i + 2})}$。求 $\sum_{a \in D_f} f(a)$。

## 分析思路

考试的时候往正解这方面想了，可惜没写出正解 QAQ。官方题解写的不太清楚，这里补充一下。

### I. 立方算法

因为有用的只有最后三个数，我们枚举目前枚举到的位置 $i$，$j = a_{i - 2}, k = a_{i - 1}, l = a_{i}$。因为有乘法分配律这个东西我们可以在之前的答案上整体乘上这一位的贡献，即 $w_{\max(j, k, l)}$。初始状态可以通过直接枚举最开始的三个数得到。可以写出这样的转移方程式：

$$
f_{i, k, l} = \sum_{j = 1}^n f_{i - 1, j, k} \times w_{\max(j, k, l)}
$$

$\max(k, l)$ 对于给定的状态 $(i, k, l)$ 是一个定值，把 $\max(i, k, l)$ 写成 $\max(j, \max(k, l))$ 的形式，再分类讨论拆掉 $\max$：

$$
\begin{aligned}
f_{i, k, l} &= \sum_{j = 1}^{\max(k, l)} f_{i - 1, j, k} \times w_{\max(k, l)} + \sum_{j = \max(k, l) + 1}^n f_{i - 1, j, k} \times w_j\\
&= w_{\max(k, l)} \sum_{j = 1}^{\max(k, l)} f_{i - 1, j, k} + \sum_{j = \max(k, l) + 1}^n f_{i - 1, j, k} \times w_j
\end{aligned}
$$

然后这个东西很明显能前缀和优化，拼一下特殊性质我们就有 $60 + 10 = 70$ 分了。（吐槽一下，$n^3$ 部分分也太卡常了吧啊喂）

### II. 正解

考虑我们其实只关心 $\max(j, k)$。试着把状态压缩到二维。定义 $f_{i, j, 0/1}$ 表示 $\max(a_{i - 1}, a_i) = j$，$a_{i - 1} > a_i$ 或 $a_{i - 1} \le a_i$ 的答案。注意对于 $f_{i, j, 0}$，我们先不乘方案数，因为方案数可能被两边的数限制。

考虑分情况写出转移方程：

对于 $f_{i, j, 0}$，最后两个数的排列就是 $a_{i - 1} = j, a_i<j$。

分类讨论，如果最大值是 $j$，$a_{i - 2}$ 就小于等于 $j$，贡献有 $f_{i - 1, j, 1} \times w_j$；如果最大值大于 $j$，那么只能是 $a_{i - 2} > j$，贡献就是 $f_{i - 1, k>j, 0} \times w_k$，注意这里因为 $a_{i - 1}$ 被确定下来不用乘方案数，于是我们得到了 $f_{i, j, 0}$ 的转移：

$$
f_{i, j, 0} = f_{i - 1, j, 1} \times w_j + \sum_{k = j+1}^n f_{i - 1, k, 0} \times w_k
$$

同样的我们分析对于 $f_{i, j, 1}$，最后两个数的排列就是 $a_{i - 1} \le a_i = j$。

分类讨论，如果最大值是 $j$，$a_{i - 2}$ 就小于等于 $j$，再分类讨论 $a_{i - 2}$ 与 $a_{i - 1}$ 的大小关系。若 $a_{i - 2} \le a_{i - 1}$，贡献则为 $f_{i - 1, k, 1} \times w_j$，若 $a_{i - 2} > a_{i - 1}$，贡献则为 $f_{i - 1, k, 0} \times (k - 1) \times w_j$，这里就是 $a_{i - 1}$ 被 $a_{i - 2}$ 限制只有 $k - 1$ 种方案。

如果最大值大于 $j$，那么只能是 $a_{i - 2} > j$，贡献就是 $f_{i - 1, k>j, 0} \times w_k$，这里 $a_{i - 1}$ 没有确定下来，乘方案数 $j$，于是我们得到了 $f_{i, j, 1}$ 的转移：

$$
f_{i, j, 1} = w_j \sum_{k = 1}^{j - 1} [f_{i - 1, k, 1} + (k - 1)f_{i - 1, k, 0}] + j \sum_{k = j+1}^n f_{i - 1, k, 0} \times w_k
$$

使用前缀和优化即可做到 $O(n^2)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 4010;
const int p = 998244353;
int n, w[N], dp[2][N][2], pre[N], suf[N];
// 0: a[i - 1] > a[i], 1: a[i - 1] <= a[i]
// 该位的贡献 (max 可重复贡献) w[max(k, j)].
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline void make_sum(int i) {
    i %= 2;
    for (int j = 1; j <= n; j++) {
        pre[j] = (pre[j - 1] + dp[i][j][1] + dp[i][j][0] * (j - 1ll)) % p;
    }
    for (int j = n; j >= 1; j--) {
        suf[j] = (suf[j + 1] + 1ll * dp[i][j][0] * w[j]) % p;
    }
}
int main(int argc, char const *argv[]) {
#ifndef LOCAL
    freopen("sequence.in", "r", stdin);
    freopen("sequence.out", "w", stdout);
#endif
    optimizeIO(), cin >> n;
    for (int i = 1; i <= n; i++) cin >> w[i];
    for (int i = 1; i <= n; i++) {
        dp[0][i][0] = 1, dp[0][i][1] = i;
        // dp[2][i][0] = 1: 下次取到最大值，再算出中间值的取值范围，再计算。
    }
    for (int i = 3; i <= n; i++) {
        make_sum(i - 1);
        int cur = i & 1, lst = cur ^ 1;
        for (int j = 1; j <= n; j++) {
            dp[cur][j][0] = (1ll * dp[lst][j][1] * w[j] + suf[j + 1]) % p;
            dp[cur][j][1] = (1ll * pre[j] * w[j] + 1ll * suf[j + 1] * j) % p;
        }
    }
    long long ans = 0;
    for (int i = 1; i <= n; i++) {
        ans += dp[n & 1][i][0] * (i - 1ll) + dp[n & 1][i][1];
    }
    cout << ans % p << endl;
    return 0;
}

```
