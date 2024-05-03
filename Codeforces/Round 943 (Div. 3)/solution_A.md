摘要：数论，暴力枚举

[传送门：https://www.luogu.com.cn/problem/CF1968A](https://www.luogu.com.cn/problem/CF1968A)

## 题意

给定一个正整数 $x$，求正整数 $y \in [1, x)$ 使得 $\gcd(x, y) + y$ 最大，若有多个 $y$ 输出任意一个均可。 

## 分析思路

注意到 $2 \leq x \leq 1000$，可以暴力枚举 $y$，计算出对应的 $\gcd(x, y) + y$ 并取最大值。

时间复杂度 $O\left(x \log x\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
i64 t = 1, n, m;
inline void solve(void) {
    cin >> n;
    i64 ans = 1;
    for (int i = 1; i < n; i++) {
        if (gcd(n, ans) + ans < gcd(n, i) + i) {
            ans = i;
        }
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
