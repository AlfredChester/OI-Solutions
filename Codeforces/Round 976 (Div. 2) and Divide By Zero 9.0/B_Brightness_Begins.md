摘要：数学，二分

[传送门：https://www.luogu.com.cn/problem/CF2020B](https://www.luogu.com.cn/problem/CF2020B)

## 题意

有一排共 $n$ 盏灯，编号为 $1 \sim n$，一开始全是开着的。我们将会进行 $n$ 轮操作，对于第 $i$（$i = 1, 2, \dots, n$）轮操作，我们将所有编号为 $i$ 的倍数的灯翻转状态，即从开变成关或从关变成开。已知最后有 $k$ 盏灯仍然亮着，求满足条件的 $n$ 的最小值。

## 分析思路

考虑编号为 $i$ 的灯会被翻转 $\mathrm{d}(i)$ 次。如果 $x$ 号灯最后变为了关的状态，那就意味着 $\mathrm{d}(x)$ 为奇数。设 $x = p_1^{a_1} \times p_2^{a_2} \times \cdots \times p_k^{a_k}$。有 $\mathrm{d}(x) = \prod_{i = 1}^k (a_i + 1)$ 为奇数，那也就是说 $a_i$ 全部都是偶数，即 $x$ 为完全平方数。我们知道 $n$ 以内完全平方数的个数为 $\left\lfloor \sqrt n \right\rfloor$。所以有 $n - \left\lfloor \sqrt n \right\rfloor = k$。由于 $n - \left\lfloor \sqrt n \right\rfloor$ 单调不降，我们可以对 $n$ 进行二分。注意二分上界需要开大，因为 $k = 10^{18}$ 时有 $n = 10^{18} + 10^9$。时间复杂度 $O(\log k)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
template <class T>
inline T floor_sqrt(T x) {
    T res = std::sqrt(x);
    if (res * res > x) res--;
    return res;
}
inline void solve(void) {
    i64 k;
    cin >> k;
    i64 L = 0, R = LLONG_MAX, mid;
    while (L < R) {
        mid = (L + R) >> 1;
        if (mid - floor_sqrt(mid) < k) {
            L = mid + 1;
        } else {
            R = mid;
        }
    }
    cout << L << '\n';
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int t = 1;
    optimizeIO(), cin >> t;
    while (t--) solve();
    return 0;
}

```
