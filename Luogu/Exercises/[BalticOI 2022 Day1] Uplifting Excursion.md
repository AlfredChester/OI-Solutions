摘要：贪心，dp

[传送门：https://www.luogu.com.cn/problem/P8392](https://www.luogu.com.cn/problem/P8392)

## 题意

给定 $2m + 1$ 类物品，权值分别为 $-m, -m + 1, \dots, m$，数量分别有 $a_{-m}, a_{-m + 1}, \dots, a_{m}$ 个。现在需要你从中挑选若干个物品，使他们的权值和为 $L$，问最多可以选多少个物品，或者报告不可能。

## 分析思路

CSP-S 模拟赛出了，十分考察乱搞能力，让我第一次看到了贪心后拿 dp 微调的做法。

暴力显然可以背包，但是权值范围太大，考虑先想办法乱搞缩小权值的范围，然后再跑多重背包，然后往贪心方面想。

首先我们把所有物品都选上，显然可能过大或者过小。如果过大，我们就从权值为 $m$ 的物品开始往下删东西，因为删什么东西都会减少 $1$ 的收益，还不如减权值大的，如果过小反之亦然。如果我们只选正或者只选负连大小都满足不了，那显然是不可以的。

而这样做有一个好处：贪心出来的方案权值范围一定在 $[L - m, L]$ 之间，因为如果比 $L - m$ 更小，总可以删除至少一个负的来令他更靠近 $L - m$，最终大于 $L - m$（否则已经作为无解判断掉了，因为全选正的还是到不了范围），如果比 $L$ 更大同理可以调整。

然后进行背包。有一个比较显然的观察：$i$ 和 $-i$ 不会同时被选，不会有一个数被选超过 $m$ 次（否则有平替）。所以最多背包的权值为 $m^2$ 级别。进行二进制优化即可，时间复杂度 $O(m^3 \log m)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 310;
using i64 = long long;
template <class T>
struct NegativeArray {
    i64 delta;
    std::vector<T> _a;
    NegativeArray(int siz, T val = 0) {
        delta = siz, _a.assign(2 * siz + 2, val);
    }
    inline T &operator[](i64 x) {
        return _a[x + delta];
    }
};
i64 m, L, s1, s2, sum;
NegativeArray<i64> a(N), b(N);
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> m >> L;
    for (int i = -m; i <= m; i++) {
        cin >> a[i], b[i] = a[i];
        i < 0 ? (s1 += i * a[i]) : (s2 += i * a[i]);
    }
    if (L < s1 || L > s2) {
        cout << "impossible\n";
        return 0;
    }
    sum = s1 + s2;
    if (sum > L) {
        for (int i = m; i > 0; i--) {
            b[i] -= min((sum - L) / i, a[i]);
            sum -= i * (a[i] - b[i]);
        }
    } else {
        for (int i = -m; i < 0; i++) {
            b[i] -= min((sum - L) / i, a[i]);
            sum -= i * (a[i] - b[i]);
        }
    }
    NegativeArray<i64> dp(m * m, -1e18);
    auto solve = [&](i64 w, i64 v, i64 c) {
        // 重量，价值，个数
        if (w > 0) {
            for (i64 s = 1; c > 0; c -= s, s <<= 1) {
                const i64 k = min(s, c);
                for (i64 i = m * m; i >= k * w - m * m; i--) {
                    dp[i] = max(dp[i], dp[i - k * w] + k * v);
                }
            }
        } else {
            for (i64 s = 1; c > 0; c -= s, s <<= 1) {
                const i64 k = min(s, c);
                for (i64 i = -m * m; i <= k * w + m * m; i++) {
                    dp[i] = max(dp[i], dp[i - k * w] + k * v);
                }
            }
        }
    };
    dp[sum - L] = 0;
    for (int i = -m; i <= m; i++) {
        dp[sum - L] += b[i];
    }
    for (int i = -m; i <= m; i++) {
        if (i == 0) continue;
        if (b[i] != 0) {
            solve(-i, -1, min(b[i], m * m));
        }
        if (a[i] != b[i]) {
            solve(i, 1, min(a[i] - b[i], m * m));
        }
    }
    if (dp[0] < 0) {
        cout << "impossible\n";
    } else {
        cout << dp[0] << '\n';
    }
    return 0;
}

```
