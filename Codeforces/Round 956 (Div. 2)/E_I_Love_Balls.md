摘要：数学，概率，期望

[传送门：https://www.luogu.com.cn/problem/CF1983E](https://www.luogu.com.cn/problem/CF1983E)

## 题意

Alice 和 Bob 在玩游戏。他们面前有 $n$ 个球，第 $i$ 个球的价值为 $v_i$，且前 $k$ 个球为“特殊球”，Alice 为先手。每一轮，Alice 或 Bob 会随机从剩余的球中选取一个球并把它取出，获得它的价值。如果这个球为特殊球，则下一轮的玩家不变，否则下一轮的玩家改变。问 Alice 和 Bob 各自得到的价值的期望对 $10^9 + 7$ 取模的值。

## 分析思路

首先我们考虑 Alice 和 Bob 拿球的序列，一定如下图：

![](https://cdn.luogu.com.cn/upload/image_hosting/jmkd4qpl.png)

我们记普通球的个数为 $m$。记特殊球的价值和为 $s_s$，普通球的价值和为 $s_n$。

我们先计算 Alice 获得的价值的期望。首先考虑普通球。因为 Alice 为先手，我们可以发现在 $m$ 个普通球中，Alice 会拿走 $a = \lceil \frac{m}{2} \rceil$ 个，而 Bob 会拿走 $b = \lfloor \frac{m}{2} \rfloor$ 个。所以对于每个普通球 $i$，Alice 拿走的期望为 $v_i \times \frac{a}{a + b}$。

然后我们再来考虑特殊球。注意到特殊球可能存在的位置即为由普通球构成的 $m + 1$ 个连续段，且每个特殊球插入这些连续段可以看作是独立时间。在 $m + 1$ 个连续段中，会被 Alice 拿走的有 $c = \lceil \frac{m + 1}{2} \rceil$ 个，会被 Bob 拿走的有 $d = \lfloor \frac{m + 1}{2} \rfloor$ 个。所以对于每个特殊球 $i$，Alice 拿走的期望为 $v_i \times \frac{c}{c + d}$。

最后将两部分累加即可。Bob 拿走的期望等于总和减去 Alice 拿走的期望。时间复杂度 $O\left(n\right)$，可以通过。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
template <int mod>
inline int64_t down(int64_t x) { return x >= mod ? x - mod : x; }
template <int mod>
struct ModInt {
    int64_t x;
    ModInt() = default;
    ModInt(int64_t x) : x((x % mod + mod) % mod) {}
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
inline void solve(void) {
    int n, k;
    cin >> n >> k;
    const int ns = n - k; // non_special count.
    std::vector<mint> v(n);
    for (auto &x : v) cin >> x;
    mint spec = accumulate(v.begin(), v.begin() + k, mint(0));   // ss
    mint non_spec = accumulate(v.begin() + k, v.end(), mint(0)); // sn
    mint spec_frac = mint(ns / 2 + 1) / (ns + 1);
    mint non_spec_frac = mint((ns + 1) / 2) / ns;
    mint alice = spec_frac * spec + non_spec_frac * non_spec;
    cout << alice << ' ' << spec + non_spec - alice << '\n';
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
