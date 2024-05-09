摘要：数论，构造

[传送门：https://www.luogu.com.cn/problem/CF1968C](https://www.luogu.com.cn/problem/CF1968C)

## 题意

给定一个数列 $x_2, x_3, \cdots, x_n$，试构造一个数列 $a_1, a_2, \cdots, a_n$，使得 $\forall i \in [2, n]$，$x_i = a_i \bmod a_{i-1}$。

## 分析思路

因为需要 $1 \leq a_i \leq 10^9$，所以我们希望 $a_i$ 尽可能小。

由于 $x_i = a_i \bmod a_{i-1}$，一种可行的构造即为 $x_i = a_i - a_{i - 1}$，也就是 $a_i = x_i + a_{i - 1}$。

由于 $1 \leq x_i, n \leq 500$，于是我们任意设置一个大于 $500$ 的 $a_1$，然后按照上述递推式即可得出一组合法构造。

时间复杂度 $O\left(n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 510;
using i64 = long long;
i64 t = 1, n, x[N];
inline void solve(void) {
    cin >> n;
    int cur = 1000;
    cout << cur << ' ';
    for (int i = 1; i < n; i++) {
        cin >> x[i];
        cout << (cur += x[i]) << ' ';
    }
    cout << '\n';
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
