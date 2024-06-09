摘要：数据结构，线段树，数学

[传送门：https://www.luogu.com.cn/problem/AT_abc357_f](https://www.luogu.com.cn/problem/AT_abc357_f)

## 题意

给定两个长度为 $N$ 的序列 $A_N, B_N$，现在需要维护 $Q$ 次以下三种操作：

- `1 l r x` 将 $A_l, A_{l + 1}, \cdots, A_r$ 全部加 $x$；
- `2 l r x` 将 $B_l, B_{l + 1}, \cdots, B_r$ 全部加 $x$；
- `3 l r` 求 $\sum_{i = l}^r A_i \times B_i \bmod 998244353$。

## 分析思路

区间加考虑线段树维护。我们以操作一为例进行一下分析。

如果区间 $A_l, A_{l + 1}, \cdots, A_r$ 全部加了 $x$，那么该区间查询的结果会变为：

$$
\begin{aligned}
ans &= \sum_{i = l}^r (A_i + x) \times B_i \\
&=  \sum_{i = l}^r A_i \times B_i + \sum_{i = l}^r x \times B_i \\
&= \sum_{i = l}^r A_i \times B_i + x \times \sum_{i = l}^r B_i
\end{aligned}
$$

也就是说，我们还需要维护 $B_l, B_{l + 1}, \cdots, B_r$ 的和，这样我们就能快速完成某段区间的操作一了。

操作二同理。记得进行操作前先进行 pushdown 操作。

时间复杂度 $O\left((n + q) \log n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
template <int mod>
inline uint64_t down(uint64_t x) { return x >= mod ? x - mod : x; }
template <int mod>
struct ModInt {
    uint64_t x;
    ModInt() = default;
    ModInt(uint64_t x) : x(x % mod) {}
    friend istream &operator>>(istream &in, ModInt &a) { return in >> a.x; }
    friend ostream &operator<<(ostream &out, ModInt a) { return out << a.x; }
    friend ModInt operator+(ModInt a, ModInt b) { return down<mod>(a.x + b.x); }
    friend ModInt operator-(ModInt a, ModInt b) { return down<mod>(a.x - b.x + mod); }
    friend ModInt operator*(ModInt a, ModInt b) { return (__int128)a.x * b.x % mod; }
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
using mint = ModInt<998244353>;
const int N = 400010;
struct Node {
    int l, r;
    mint sa, sb, sum, la, lb;
    inline int mid(void) { return (l + r) >> 1; }
    inline int len(void) { return r - l + 1; }
    inline void addA(mint x) {
        sum += sb * x, sa += len() * x, la += x;
    }
    inline void addB(mint x) {
        sum += sa * x, sb += len() * x, lb += x;
    }
} tr[N << 2];
int n, q, a[N], b[N], opt, l, r, x;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
inline int ls(int p) { return p << 1; }
inline int rs(int p) { return p << 1 | 1; }
inline void maintain(int p) {
    tr[p].sa = tr[ls(p)].sa + tr[rs(p)].sa;
    tr[p].sb = tr[ls(p)].sb + tr[rs(p)].sb;
    tr[p].sum = tr[ls(p)].sum + tr[rs(p)].sum;
}
inline void pushdown(int p) {
    if (tr[p].la != 0) {
        tr[ls(p)].addA(tr[p].la);
        tr[rs(p)].addA(tr[p].la);
        tr[p].la = 0;
    }
    if (tr[p].lb != 0) {
        tr[ls(p)].addB(tr[p].lb);
        tr[rs(p)].addB(tr[p].lb);
        tr[p].lb = 0;
    }
}
inline void build(int p, int l, int r) {
    tr[p] = {l, r};
    if (l == r) {
        tr[p] = {l, r, a[l], b[l], (mint)a[l] * b[l]};
        return;
    }
    int m = (l + r) >> 1;
    build(ls(p), l, m);
    build(rs(p), m + 1, r);
    maintain(p);
}
inline void addA(int p, int l, int r, mint x) {
    pushdown(p);
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].addA(x);
    }
    int m = tr[p].mid();
    if (l <= m) addA(ls(p), l, r, x);
    if (r > m) addA(rs(p), l, r, x);
    maintain(p);
}
inline void addB(int p, int l, int r, mint x) {
    pushdown(p);
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].addB(x);
    }
    int m = tr[p].mid();
    if (l <= m) addB(ls(p), l, r, x);
    if (r > m) addB(rs(p), l, r, x);
    maintain(p);
}
inline mint query(int p, int l, int r) {
    pushdown(p);
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].sum;
    }
    mint ans = 0;
    int m = tr[p].mid();
    if (l <= m) ans += query(ls(p), l, r);
    if (r > m) ans += query(rs(p), l, r);
    return ans;
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> q;
    for (int i = 1; i <= n; i++) cin >> a[i];
    for (int i = 1; i <= n; i++) cin >> b[i];
    build(1, 1, n);
    while (q--) {
        cin >> opt >> l >> r;
        if (opt == 1) {
            cin >> x, addA(1, l, r, x);
        } else if (opt == 2) {
            cin >> x, addB(1, l, r, x);
        } else {
            cout << query(1, l, r) << '\n';
        }
    }
    return 0;
}

```
