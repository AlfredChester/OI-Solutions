摘要：线段树，时光回溯

[传送门：https://www.luogu.com.cn/problem/CF1477B](https://www.luogu.com.cn/problem/CF1477B)

## 题意

给你两个二进制字符串 $s, t$。$q$ 次操作，每次给定一个区间 $[l, r]$，查询 $s_l, s_{l + 1}, \dots, s_r$ 是否都相等。然后你可以修改这个区间里面 **严格小于区间长度一半个** 字符，来影响下一次操作的检查操作。问若干次操作之后能否从字符串 $s$ 变成 $t$。 

## 分析思路

这题比较符合自觉的做法是试图每次在 $s$ 的基础上修改，尽量往 $t$ 上靠。然而这样做没有办法每次确定给一个明确具体的策略，看似不可做，怎么办？

注意到如果存在一种合法的操作方法能从 $s$ 变成 $t$，那么一定也可以从 $t$ 变成 $s$，只不过我们允许先修改，再查询。

于是策略变得明朗：我们以 $t$ 为初始状态，倒着处理询问。如果当前这段区间 $1$ 多，**由于操作之后我们一定要能通过检查**，事实上我们只能把全部的 $0$ 改成 $1$。如果区间 $0$ 多同理。如果一样多，那就不行。实际上我们是没有任何选择地进行操作。最后检查终止状态是否为 $s$ 即可。

于是我们用线段树维护区间和，区间推平，时间复杂度 $O(n \log n)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
struct Node {
    int l, r, lazy, sum;
    inline int mid(void) { return (l + r) >> 1; }
    inline void assign(int x) {
        lazy = x, sum = (r - l + 1) * x;
    }
} tr[N << 2];
int t, n, q, l[N], r[N], s[N], f[N];
inline int ls(int p) { return p << 1; }
inline int rs(int p) { return p << 1 | 1; }
inline void pushdown(int p) {
    if (tr[p].lazy != -1) {
        tr[ls(p)].assign(tr[p].lazy);
        tr[rs(p)].assign(tr[p].lazy);
        tr[p].lazy = -1;
    }
}
inline void build(int p, int l, int r) {
    tr[p] = {l, r, -1, 0};
    if (l == r) {
        tr[p].sum = f[l];
    } else {
        const int m = (l + r) >> 1;
        build(ls(p), l, m), build(rs(p), m + 1, r);
        tr[p].sum = tr[ls(p)].sum + tr[rs(p)].sum;
    }
}
inline void modify(int p, int l, int r, int v) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].assign(v);
    }
    pushdown(p);
    if (l <= tr[p].mid()) modify(ls(p), l, r, v);
    if (r > tr[p].mid()) modify(rs(p), l, r, v);
    tr[p].sum = tr[ls(p)].sum + tr[rs(p)].sum;
}
inline int query(int p, int l, int r) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].sum;
    }
    pushdown(p);
    int m = tr[p].mid(), ans = 0;
    if (l <= m) ans += query(ls(p), l, r);
    if (r > m) ans += query(rs(p), l, r);
    return ans;
}
inline void solve(void) {
    char c;
    cin >> n >> q;
    for (int i = 1; i <= n; i++) cin >> c, s[i] = c ^ 48;
    for (int i = 1; i <= n; i++) cin >> c, f[i] = c ^ 48;
    for (int i = 1; i <= q; i++) {
        cin >> l[i] >> r[i];
    }
    build(1, 1, n);
    for (int i = q; i >= 1; i--) {
        int one = query(1, l[i], r[i]);
        int zero = r[i] - l[i] + 1 - one;
        if (zero > one) {
            modify(1, l[i], r[i], 0);
        } else if (zero == one) {
            cout << "NO\n";
            return;
        } else {
            modify(1, l[i], r[i], 1);
        }
    }
    for (int i = 1; i <= n; i++) {
        if (query(1, i, i) != s[i]) {
            cout << "NO\n";
            return;
        }
    }
    cout << "YES\n";
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
