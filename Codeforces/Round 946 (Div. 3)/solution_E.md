摘要：动态规划，背包

[传送门：https://www.luogu.com.cn/problem/CF1974E](https://www.luogu.com.cn/problem/CF1974E)

## 题意

你是一个物理学家。一开始你没有钱，每个月的末尾你会得到 $x$ 英镑。在第 $i$ 个月里，你可以付出 $c_i$ 英镑，获取 $h_i$ 的幸福。

在任何时刻你都不能欠钱，问你在 $m$ 个月过后最多能获得多少幸福。

保证 $\sum h_i \leq 10^5$。

## 分析思路

注意到 $\sum h_i \leq 10^5$ 这一特殊的性质。考虑 `dp`。设 $dp_{i, j}$ 表示到第 $i$ 个月的末尾为止，获取 $j$ 的幸福值所剩余的最多钱数。容易得到转移式：

$$
\begin{equation*}
dp_{i, j}=\left\{
\begin{array}{lcl}
dp_{i - 1, j} & & {0 \leq j < h_i}\\
\max(dp_{i - 1, j}, dp_{i - 1, j - h_i} - c_i) & & {j \geq h_i}\\
\end{array} \right.
\end{equation*}
$$

计算完 $dp_i$ 一整行之后，对于 $dp_{i, j} \geq 0$ 的所有 $dp_{i, j}$, 令 $dp_{i, j} \gets dp_{i, j} + x$。

最后找到最大的 $j$，使得 $dp_{m, j} \geq 0$，即为答案。

实现上发现第一个维度没有用，删除即可。

总时间复杂度 $O\left(m \sum h\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
i64 t, m, x, sum, h, c;
inline void solve(void) {
    vector<i64> dp{0};
    cin >> m >> x, sum = 0;
    for (int i = 1; i <= m; i++) {
        cin >> c >> h, sum += h;
        dp.resize(sum + 1, -1);
        for (int j = sum; j >= h; j--) {
            if (dp[j - h] >= c) {
                dp[j] = max(dp[j], dp[j - h] - c);
            }
        }
        for (int j = 0; j <= sum; j++) {
            if (dp[j] >= 0) dp[j] += x;
        }
    }
    for (int i = sum; i >= 0; i--) {
        if (dp[i] >= 0) {
            cout << i << '\n';
            return;
        }
    }
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> t;
    while (t--) solve();
    return 0;
}

```
