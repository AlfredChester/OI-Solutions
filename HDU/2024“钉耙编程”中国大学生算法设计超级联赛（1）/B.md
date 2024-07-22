摘要：动态规划，数学，期望

[传送门：https://vjudge.net/problem/HDU-7434](https://vjudge.net/problem/HDU-7434)

赛时我们 $O(nk)$ 常数小暴力飞过去了，应该是因为连续内存访问的原因可以通过，然而这题有更好的做法。

## 题意

$n$ 轮游戏，第 $i$ 轮可以付出 $a_{i, j}$ 的代价获得 $j$ 个星星（$1 \le j \le 4$）或跳过这一轮。求恰好获得 $k$ 颗星星的最少代价。$\sum n \le 10^5$。

## 分析思路

注意到 $i, j$ 两轮的先后顺序并不影响最终的答案，那么我们可以任意交换两轮的先后顺序。考虑随机打乱。我们考虑朴素的 dp，假设我们当前确定了前 $i$ 轮进行 $5$ 种决策中的哪种（前提不超过 $k$ 颗星星）。由于我们随机打乱了两轮，那么到第 $i$ 轮获得星星的期望为 $E = i \times \frac{k}{n}$。我们只要在这个期望值的附近暴力更新 dp 即可。

具体来说，此题中重量的最大值为 $v = 4$，所以我们取 $B = 4 \sqrt k$，暴力更新 $[E - B, E + B]$ 的 dp，时间复杂度 $O\left(n \sqrt k \right)$。[这篇论文](https://arxiv.org/pdf/1309.4029.pdf) 指出 $n = 10^6, k = 5 \times 10 ^ 5, v = 1$ 时，取 $B = 1346 \approx 2\sqrt k$ 即可得到错误率约为 $10^{-7}$ 的优秀算法。然而为什么有如此低的错误率并没有看懂，望某天有人能解答。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int t, n, k;
inline void solve(void) {
    cin >> n >> k;
    vector<array<int, 4>> a(n + 1);
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < 4; j++) {
            cin >> a[i][j];
        }
    }
    mt19937 rng(time(0));
    const int B = 4 * ceil(sqrt(k));
    vector<long long> dp(k + 1, 1e18);
    shuffle(a.begin() + 1, a.end(), rng), dp[0] = 0;
    for (int i = 1; i <= n; i++) {
        double E = 1.0 * i * k / n;
        for (int j = E + B; j >= E - B; j--) {
            if (j > k) continue;
            for (int p = 0; p < 4; p++) {
                if (j - p - 1 >= 0) {
                    dp[j] = min(dp[j], dp[j - p - 1] + a[i][p]);
                }
            }
        }
    }
    cout << dp[k] << '\n';
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
