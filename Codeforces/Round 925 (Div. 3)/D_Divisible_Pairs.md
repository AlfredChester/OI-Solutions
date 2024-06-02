摘要：数据结构，统计

[传送门：https://www.luogu.com.cn/problem/CF1931D](https://www.luogu.com.cn/problem/CF1931D)

## 题意

给定两个参数 $x, y$ 和数组 $a_n$。

定义一对 $\langle i, j \rangle$（$1 \le i < j \le n$）是美丽对当且仅当 $x \mid a_i + a_j$ 且 $y \mid a_i - a_j$。

求 $a$ 中美丽对的个数。

## 分析思路

考虑如何对于一个 $i$ 如何快速统计出使得 $\langle i, j \rangle$ 是美丽对的 $j$（$1 \leq j < i$）的个数。

注意到美丽对中 $a_i + a_j \equiv 0 \pmod x$，移项得 $a_j \equiv - a_i \pmod x$；同理有 $a_j \equiv a_i \pmod y$。则通过 $-a_i \bmod x$ 和 $a_i \bmod y$ 的值可以快速得到 $a_j \bmod x$ 和 $a_j \bmod y$ 的值，使用 `std::map` 进行统计即可。

时间复杂度为 $O\left(n \log n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1000010;
typedef long long ll;
map<pair<ll, ll>, ll> cnt;
ll t, n, x, y, ans, a[N];
inline void solve(void) {
    cin >> n >> x >> y;
    cnt.clear(), ans = 0;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        ans += cnt[{(x - a[i] % x) % x, a[i] % y}];
        cnt[{a[i] % x, a[i] % y}]++;
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
