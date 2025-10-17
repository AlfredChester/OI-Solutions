摘要：动态规划，组合数学

[传送门：https://www.luogu.com.cn/problem/P11498](https://www.luogu.com.cn/problem/P11498)

## 题意

给定三个整数 $n, m, k$ 以及 $m$ 条二元限制 $(l_i, r_i)$。要求你计算长度为 $n$，每个元素都为 $[0, k]$ 之间整数，满足 $\forall 1 \le i < j \le n$，有 $(a_i \ \mathrm {and} \ a_j) = a_j$，且对于每组二元限制有 $a_{l_i} \neq a_{r_i}$ 的序列 $a$ 的个数。

## 分析思路

$(a_i \ \mathrm {and} \ a_j) = a_j$ 显然是说把二进制看作集合的意义下 $a_i \subseteq a_j$。于是可以想到值域连续段的大小是 $O(\log V)$ 的。考虑集合大小相同的值其实没有那么大的本质区别。设 $dp_{i, j}$ 表示到第 $i$ 个位置，最后一个位置大小为 $j$ 的方案数。不考虑限制条件时，转移为：

$$
dp_{i, j} = \sum_{p = 0}^{i - 1} \sum_{q = 0}^{j - 1} \binom{j}{q} dp_{p, q}
$$

其中乘组合数是因为你从 $j$ 个扩展到 $q$ 个的方式有 $\binom{j}{q}$ 种。这个显然可以前缀和优化。

然后考虑限制条件。一个二元限制 $(l, r)$ 是要求 $[l, r]$ 之间一定要有一个值域连续段的分界点。要求每个区间之间必须包含一个分界点可以转化为没有两个分界点之间包含了一个区间。把所有二元限制挂到右端点上，显然我们只关心右端点相同的左端点最靠右的那个，记为 $L_r$。

回到转移。我们要求 $\max\{L_{p + 1}, L_{p + 2}, \dots, L_i\} \le p$（注意 $p$ 是不包含在当前值域连续段的）。对于每个 $p$ 单独考虑，随着 $i$ 的增大，式子左边不断增大。所以每个 $p$ 只会插入一次删除一次。使用 set 维护所有符合条件的 $p$ 即可。

最后答案即为 $\sum_{i = 0}^{\log V} dp_{n, i} \times f_i$，其中 $f_i$ 表示 $[0, k]$ 中 popcount 为 $i$ 的数的个数。这是经典问题，可以 $O(\log^2 V)$ 求出。

时间复杂度 $O(n \log^2 V + n \log n + \log^2 V)$，可以通过。实现上注意取模比较多可以全加起来统一取模。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int V = 60;
const int N = 300010;
const int P = 1e9 + 7;
using i64 = long long;
int ans[V], dp[N][V], mx[N];
int n, m, l, r, C[V][V], sum[V];
template <class T>
inline void chkmax(T &x, T y) {
    if (x < y) x = y;
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    long long k;
    optimizeIO(), cin >> n >> m >> k;
    for (int i = 0; i < V; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++) {
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % P;
        }
    }
    ans[__builtin_popcountll(k)]++;
    for (int i = 0; i < V; i++) {
        if (!(k >> i & 1)) continue;
        int up = __builtin_popcountll((k >> i) - 1);
        for (int j = 0; j <= i; j++) {
            ans[up + j] += C[i][j];
            if (ans[up + j] >= P) {
                ans[up + j] -= P;
            }
        }
    }
    for (int i = 1; i <= m; i++) {
        cin >> l >> r, chkmax(mx[r], l);
    }
    bool ok = true;
    std::multiset<int> S;
    for (int i = 1; i <= n; i++) {
        ok &= mx[i] == 0;
        auto E = S.lower_bound(mx[i]);
        for (auto it = S.begin(); it != E; it++) {
            for (int k = 0; k < V; k++) {
                sum[k] -= dp[*it][k];
                if (sum[k] < 0) sum[k] += P;
            }
        }
        S.erase(S.begin(), E);
        for (int nk = 0; nk < V; nk++) {
            __int128 tot = 0;
            for (int k = 0; k < nk; k++) {
                tot += 1ll * sum[k] * C[nk][k];
            }
            dp[i][nk] = tot % P;
        }
        if (ok) {
            for (int j = 0; j < V; j++) {
                dp[i][j] = (dp[i][j] + 1) % P;
            }
        }
        S.insert(i);
        for (int k = 0; k < V; k++) {
            sum[k] += dp[i][k];
            if (sum[k] >= P) sum[k] -= P;
        }
    }
    int res = 0;
    for (int j = 0; j < V; j++) {
        res = (res + 1ll * dp[n][j] * ans[j]) % P;
    }
    cout << res << endl;
    return 0;
}
```
