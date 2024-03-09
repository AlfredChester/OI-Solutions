摘要：莫队，组合数学

[传送门：https://actiku.com/p/P2690](https://actiku.com/p/P2690)

## 题意

$t$ 次询问，每次给定 $n, m$，求 $S(n, m) = \displaystyle \sum_{i=0}^mC(n, i)$ 对 $10^9 + 7$ 取模的结果。

## 分析思路

由于询问次数很多，考虑能否从一个状态快速转移到另一个状态，这样我们就可以使用一些策略来决定转移的顺序，从而达到减少时间复杂度的目的。

首先很显然的，我们有 $S(n, m) = S(n, m - 1) + C(n, m)$。也就是说我们有快速移动第二个下标的能力。那是否我们可以类似地移动第一个下标呢？我们对 $S(n, m)$ 做一些数学上的变换：

$$
\begin{aligned}
S(n, m) &= C(n, 0) + \sum_{i=1}^m C(n, i) \\
        &= C(n - 1, 0) + \sum_{i=1}^m [C(n - 1, i) + C(n - 1, i - 1)] \\
        &= \sum_{i=0}^m C(n - 1, i) + \sum_{i=0}^{m-1}C(n - 1, m) \\
        &= 2S(n-1, m) - C(n - 1, m)
\end{aligned}
$$

于是，我们便有了移动第一个下标的能力。预处理逆元 + 莫队可以做到 $O(n \log p + n\sqrt n)$ 的时间复杂度，其中 $p = 10^9 + 7$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
template <int mod>
inline unsigned down(unsigned x) { return x >= mod ? x - mod : x; }
template <int mod>
struct ModInt {
    unsigned int x;
    ModInt() = default;
    ModInt(unsigned int x) : x(x) {}
    friend istream &operator>>(istream &in, ModInt &a) { return in >> a.x; }
    friend ostream &operator<<(ostream &out, ModInt a) { return out << a.x; }
    friend ModInt operator+(ModInt a, ModInt b) { return down<mod>(a.x + b.x); }
    friend ModInt operator-(ModInt a, ModInt b) { return down<mod>(a.x - b.x + mod); }
    friend ModInt operator*(ModInt a, ModInt b) { return 1ULL * a.x * b.x % mod; }
    friend ModInt operator/(ModInt a, ModInt b) { return a * ~b; }
    friend ModInt operator^(ModInt a, int b) {
        ModInt ans = 1;
        for (; b; b >>= 1, a *= a)
            if (b & 1) ans *= a;
        return ans;
    }
    friend ModInt operator~(ModInt a) { return a ^ (mod - 2); }
    friend ModInt operator-(ModInt a) { return down<mod>(mod - a.x); }
    friend ModInt &operator+=(ModInt &a, ModInt b) { return a = a + b; }
    friend ModInt &operator-=(ModInt &a, ModInt b) { return a = a - b; }
    friend ModInt &operator*=(ModInt &a, ModInt b) { return a = a * b; }
    friend ModInt &operator/=(ModInt &a, ModInt b) { return a = a / b; }
    friend ModInt &operator^=(ModInt &a, int b) { return a = a ^ b; }
    friend ModInt &operator++(ModInt &a) { return a += 1; }
    friend ModInt operator++(ModInt &a, int) {
        ModInt x = a;
        a += 1;
        return x;
    }
    friend ModInt &operator--(ModInt &a) { return a -= 1; }
    friend ModInt operator--(ModInt &a, int) {
        ModInt x = a;
        a -= 1;
        return x;
    }
    friend bool operator==(ModInt a, ModInt b) { return a.x == b.x; }
    friend bool operator!=(ModInt a, ModInt b) { return !(a == b); }
};
struct Query {
    int l, r, idx;
} Q[N];
using mint = ModInt<1000000007>;
mint cur, fac[N], inv[N], ans[N];
inline void init(void) {
    fac[0] = inv[0] = 1;
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
    for (int i = 1; i < N; i++) {
        fac[i] = fac[i - 1] * i, inv[i] = ~fac[i];
    }
}
inline mint C(int n, int m) {
    return fac[n] * inv[m] * inv[n - m];
}
/*
S(n, m) = C(n, 0) + C(n, 1) + ... + C(n, m)
        = C(n - 1, 0) + (C(n - 1, 1) + C(n - 1, 0)) +
                        (C(n - 1, 2) + C(n - 1, 1)) +
                        ... +
                        (C(n - 1, m) + C(n - 1, m - 1))
        = 2S(n - 1, m) - C(n - 1, m)

S(n, m) = (S(n + 1, m) + C(n, m)) / 2
*/
int t, n, m, mx, l, r;
inline void moveL(int &l, int x) {
    if (x == 1) cur = 2 * cur - C(l++, r);
    else cur = (cur + C(--l, r)) / 2;
}
// S(n, m) = S(n, m - 1) + C(n, m)
inline void moveR(int &r, int x) {
    if (x == 1) cur += C(l, ++r);
    if (x == -1) cur -= C(l, r--);
}
int main(int argc, char const *argv[]) {
    init(), cin >> t;
    for (int i = 1; i <= t; i++) {
        cin >> n >> m, Q[i] = {n, m, i}, mx = max(mx, n);
    }
    const int B = sqrt(mx);
    sort(Q + 1, Q + 1 + t, [=](Query &x, Query &y) {
        if (x.l / B != y.l / B) return x.l < y.l;
        return (x.l / B) & 1 ? x.r < y.r : x.r > y.r;
    });
    cur = 2, l = r = 1;
    for (int i = 1; i <= t; i++) {
        while (l < Q[i].l) moveL(l, 1);
        while (r < Q[i].r) moveR(r, 1);
        while (Q[i].l < l) moveL(l, -1);
        while (Q[i].r < r) moveR(r, -1);
        ans[Q[i].idx] = cur;
    }
    for (int i = 1; i <= t; i++) {
        cout << ans[i] << '\n';
    }
    return 0;
}

```