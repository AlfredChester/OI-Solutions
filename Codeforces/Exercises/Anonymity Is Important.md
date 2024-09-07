摘要：数据结构，二维数点

[传送门：https://www.luogu.com.cn/problem/CF1641C](https://www.luogu.com.cn/problem/CF1641C)

## 题意

有一排病人，一定是生病或者不生病两种状态中的一种。医生可以进行两种诊断操作：

- `0 l r 0` 表示 $[l, r]$ 中一定没有人生病；
- `0 l r 1` 表示 $[l, r]$ 中至少有一人生病；

还需要你支持一种查询操作：

- `1 x` 查询第 $x$ 个病人的状态。如果能确定状态输出 `YES/NO`，否则输出 `N/A`。  

## 分析思路

我们发现一个人的状态是 `NO`，当且仅当这个人被一个操作 $0$ 覆盖。如果我们维护一个序列 $b$，初始全部为 $1$，操作 $0$ 就是区间覆盖为 $0$，查询先看看是不是已经变成 $0$ 了，如果是的话那就是 `NO`。

然后就是确定到底是 `YES` 还是 `N/A` 了。我们画一下可能的样子：

![](https://cdn.luogu.com.cn/upload/image_hosting/diul4ahl.png)

如果存在一个操作一，他的两个端点分别在 $x$ 两边的连续一段 $0$ 中间，那么就可以判定为 `YES`，否则无法判定。

我们二分连续最大的 $0$，于是可以把这个问题变成一个二维数点问题：对于操作 $1$ 我们在二维平面上插入 $(l, r)$；对于还没有被排除为 `NO` 的查询操作，我们二分出左边连续 $0$ 的左端点 $l_0$、右边边连续 $0$ 的右端点 $r_0$，查找矩形 $(l_0, x - 1), (x + 1, r_0)$ 内有没有点。使用树套树的暴力写法可以做到 $O\left(n \log^2 n\right)$。如果把二分改成线段树上二分，而且在线段树上维护的是 $[l_0, x - 1]$ 的任何数作为操作 $1$ 的 $l$ 的 $\min r$，判断 $\min r$ 是否属于 $[x + 1, r_0]$，可以做到 $O\left(n \log n\right)$。

（懒得重写为线段树上二分于是写了线段树套二分）

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
struct Node {
    int l, r, lazy, sum, minR;
    inline int mid(void) { return (l + r) >> 1; }
    inline int len(void) { return r - l + 1; }
    inline void modify(int x) {
        sum = x * len(), lazy = x;
    }
    inline void insert(int x) {
        minR = min(minR, x);
    }
} tr[N << 2];
int n, q, opt, l, r, v;
inline int ls(int p) { return p << 1; }
inline int rs(int p) { return p << 1 | 1; }
inline void build(int p, int l, int r) {
    tr[p] = {l, r, -1, r - l + 1, (int)1e9};
    if (l != r) {
        int m = (l + r) >> 1;
        build(ls(p), l, m), build(rs(p), m + 1, r);
    }
}
inline void pushdown(int p) {
    if (tr[p].lazy != -1) {
        tr[ls(p)].modify(tr[p].lazy);
        tr[rs(p)].modify(tr[p].lazy);
        tr[p].lazy = -1;
    }
}
inline void maintain(int p) {
    tr[p].sum = tr[ls(p)].sum + tr[rs(p)].sum;
}
inline int query_sum(int p, int l, int r) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].sum;
    }
    pushdown(p);
    int m = tr[p].mid(), ans = 0;
    if (l <= m) ans += query_sum(ls(p), l, r);
    if (r > m) ans += query_sum(rs(p), l, r);
    return ans;
}
inline void modify(int p, int l, int r, int v) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].modify(v);
    }
    pushdown(p);
    if (l <= tr[p].mid()) modify(ls(p), l, r, v);
    if (r > tr[p].mid()) modify(rs(p), l, r, v);
    maintain(p);
}
inline void insert(int p, int x, int v) {
    tr[p].insert(v);
    if (tr[p].l != tr[p].r) {
        insert(x <= tr[p].mid() ? ls(p) : rs(p), x, v);
    }
}
inline int find_left(int x) {
    int L = 0, R = x - 1, mid;
    while (L < R) {
        mid = (L + R) >> 1;
        if (query_sum(1, mid + 1, x - 1) == 0) {
            R = mid;
        } else {
            L = mid + 1;
        }
    }
    return L;
}
inline int find_right(int x) {
    int L = x + 1, R = n + 1, mid;
    while (L < R) {
        mid = (L + R + 1) >> 1;
        if (query_sum(1, x + 1, mid - 1) == 0) {
            L = mid;
        } else {
            R = mid - 1;
        }
    }
    return L;
}
inline int query_r(int p, int l, int r) {
    if (l <= tr[p].l && tr[p].r <= r) {
        return tr[p].minR;
    }
    pushdown(p);
    int m = tr[p].mid(), ans = 1e9;
    if (l <= m) ans = min(ans, query_r(ls(p), l, r));
    if (r > m) ans = min(ans, query_r(rs(p), l, r));
    return ans;
}
int main(int argc, const char *argv[]) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
    cin >> n >> q, build(1, 0, n + 1);
    while (q--) {
        cin >> opt >> l;
        if (opt == 0) {
            cin >> r >> v;
            if (v == 0) {
                modify(1, l, r, 0);
            } else {
                insert(1, l, r);
            }
        } else {
            if (query_sum(1, l, l) == 0) {
                cout << "NO\n";
                continue;
            }
            int L = find_left(l), R = find_right(l);
            if (query_r(1, L + 1, l) < R) {
                cout << "YES\n";
            } else {
                cout << "N/A\n";
            }
        }
    }
    return 0;
}
```
