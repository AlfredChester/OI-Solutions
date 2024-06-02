摘要：容斥，数据结构

[传送门：https://www.luogu.com.cn/problem/CF1974C](https://www.luogu.com.cn/problem/CF1974C)

## 题意

给定一个长度为 $n$ 的数列 $a$。考虑 $n - 2$ 个关于 $a$ 的三元对 $[a_j, a_{j + 1}, a_{j + 2}]$。我们定义两个三元对是美丽的，当且仅当他们只有一个位置不同。

求这 $n - 2$ 个三元对中，美丽的三元对数量。

## 分析思路

考虑容斥。先统计出**有**两个位置不同的对数 $x$。我们首先枚举这两个位置 $A, B$ $(0 \leq A \leq B \leq 2)$，考虑 $a_{j + A}, a_{j + B}$ 相同的对数，这可以用 `std::map` 辅助求出。

最后有一些**三个位置都相同**的对被重复统计了三次，记它们的数量为 $y$。同样的我们可以用 `std::map` 辅助求出。

最终答案即为 $x - 3y$，时间复杂度 $O\left(n \log n\right)$，可以通过。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
using i64 = long long;
i64 t, n, ans, a[N];
inline void solve(void) {
    cin >> n, ans = 0;
    for (int i = 1; i <= n; i++) cin >> a[i];
    for (int i = 0; i < 3; i++) {
        for (int j = i + 1; j < 3; j++) {
            map<pair<int, int>, int> cnt;
            for (int k = 1; k <= n - 2; k++) {
                ans += cnt[{a[k + i], a[k + j]}]++;
            }
        }
    }
    map<array<i64, 3>, int> cnt;
    for (int i = 1; i <= n - 2; i++) {
        ans -= 3 * cnt[{a[i], a[i + 1], a[i + 2]}]++;
    }
    cout << ans << '\n';
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
