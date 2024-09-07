摘要：计数

[传送门：https://www.luogu.com.cn/problem/AT_agc001_e](https://www.luogu.com.cn/problem/AT_agc001_e)

## 题意

给定两个长度为 $n$ 的数组 $a, b$，求：

$$
\sum_{i = 1}^n \sum_{j = i + 1}^n \binom{a_i + b_i + a_j + b_j}{a_i + a_j}
$$

## 分析思路

首先观察到这样的形式: $\binom{x + y}{x}$。这可以理解为从 $(0, 0)$ 走到 $(x, y)$ 的方案数。于是这个问题就转换为了从 $(0, 0)$ 走到所有 $(a_i + a_j, b_i + b_j)$ 的方案数。这等价于从 $(-a_i, -b_i)$ 走到 $(a_j, b_j)$ 的方案数。显然这可以进行 $O(V^2)$ 的 dp。由于需要满足 $i < j$，我们可以减去所有从 $i$ 走到 $i$ 的方案数再除以 $2$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
constexpr int D = 2001;
constexpr int N = 200010;
template <int mod>
inline int down(int x) { return x >= mod ? x - mod : x; }
template <int mod>
struct ModInt {
    int x;
    ModInt(void) = default;
    ModInt(int x) : x(x) {}
    friend istream &operator>>(istream &in, ModInt &a) { return in >> a.x; }
    friend ostream &operator<<(ostream &out, ModInt a) { return out << a.x; }
    friend ModInt operator+(ModInt a, ModInt b) { return down<mod>(a.x + b.x); }
    friend ModInt operator-(ModInt a, ModInt b) { return down<mod>(a.x - b.x + mod); }
    friend ModInt operator*(ModInt a, ModInt b) { return (__int128)a.x * b.x % mod; }
    friend ModInt operator/(ModInt a, ModInt b) { return a * ~b; }
    friend ModInt operator^(ModInt a, long long b) {
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
    friend ModInt &operator^=(ModInt &a, long long b) { return a = a ^ b; }
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
using mint = ModInt<1000000007>;
template <class mint>
struct Comb {
    int n;
    std::vector<mint> _fac, _invfac, _inv;
    Comb(void) : n{0}, _fac{1}, _invfac{1}, _inv{0} {}
    Comb(int n) : Comb() { init(n); }
    inline void init(int m) {
        _fac.resize(m + 1), _inv.resize(m + 1), _invfac.resize(m + 1);
        for (int i = n + 1; i <= m; i++) {
            _fac[i] = _fac[i - 1] * i;
        }
        _invfac[m] = ~_fac[m];
        for (int i = m; i > n; i--) {
            _invfac[i - 1] = _invfac[i] * i;
            _inv[i] = _invfac[i] * _fac[i - 1];
        }
        n = m;
    }
    inline mint fac(int m) {
        if (m > n) init(m);
        return _fac[m];
    }
    inline mint invfac(int m) {
        if (m > n) init(m);
        return _invfac[m];
    }
    inline mint inv(int m) {
        if (m > n) init(m);
        return _inv[m];
    }
    inline mint binom(int n, int m) {
        if (n < m || m < 0) return 0;
        return fac(n) * invfac(m) * invfac(n - m);
    }
};
Comb<mint> comb;
mint dp[4010][4010];
int n, a[N], b[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i] >> b[i];
        dp[D - a[i]][D - b[i]]++;
    }
    for (int i = 1; i < 4010; i++) {
        for (int j = 1; j < 4010; j++) {
            dp[i][j] += dp[i - 1][j] + dp[i][j - 1];
        }
    }
    mint ans = 0;
    for (int i = 1; i <= n; i++) {
        ans += dp[D + a[i]][D + b[i]];
        ans -= comb.binom(2 * a[i] + 2 * b[i], 2 * a[i]);
    }
    cout << ans / 2 << endl;
    return 0;
}

```
