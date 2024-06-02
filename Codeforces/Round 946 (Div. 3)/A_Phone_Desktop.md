摘要：贪心

[传送门：https://www.luogu.com.cn/problem/CF1974A](https://www.luogu.com.cn/problem/CF1974A)

## 题意

有一种手机，桌面有若干页，每一页都是 $3 \times 5$ 的网格。

现有 $x$ 个大小为 $1 \times 1$ 的图标和 $y$ 个大小为 $2 \times 2$ 的图标，求所需要的最小页数，使得所有图标都被不交地放置在了某一页上。

## 分析思路

考虑贪心。注意到先放 $2 \times 2$ 的图标一定不劣，所以可以先放这些图标，至少去要 $\lceil \frac{y}{2} \rceil$ 页。

然后统计在这么多页里放下了这些 $2 \times 2$ 的图标之后，还剩下多少格，贪心地放 $1 \times 1$ 的图标直到放完。

时间复杂度 $O\left(1\right)$，可以通过。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
i64 t, n, m;
inline void solve(void) {
    cin >> n >> m;
    i64 ans1 = (m + 1) / 2;
    i64 rest = ans1 * 7 + (m & 1) * 4;
    if (rest >= n) {
        cout << ans1 << '\n';
    } else {
        cout << ans1 + (n - rest + 14) / 15 << '\n';
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
