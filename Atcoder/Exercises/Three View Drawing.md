摘要：Ad-hoc，构造

[传送门：https://www.luogu.com.cn/problem/AT_arc175_e](https://www.luogu.com.cn/problem/AT_arc175_e)

## 题意

给定 $N, K$，你需要选出 $K$ 个三元组，满足以下条件：

- $0 \le x_i, y_i, z_i < N$；
- $S = \{(x_i, y_i) | 1 \le i \le K\} = \{(y_i, z_i) | 1 \le i \le K\} = \{(z_i, x_i) | 1 \le i \le K\}$；
- $(x_i, y_i) \ne (x_j, y_j), \forall i \ne j$；
- $(y_i, z_i) \ne (y_j, z_j), \forall i \ne j$；
- $(z_i, x_i) \ne (z_j, x_j), \forall i \ne j$；

输出任意一组构造。

## 分析思路

我们考虑 $K = N ^ 2$ 时的情形，再进行删除。

若 $K = N ^ 2$，则显然 $S = \{(x, y) | 0 \le x, y < N\}$。考虑由 $x, y$ 唯一确定 $z$。我们取 $z = (-x - y) \bmod N$，则有 $x + y + z \equiv 0 \pmod N$。这样做的好处下文会提到。

考虑删除。我们记 $D = N ^ 2 - K$，即为要删除的个数。如果我们删除一个三元组 $(a, b, c)$，若其没有特殊性（指 $a = b = c$）我们就一定要删除 $(b, c, a)$，$(c, a, b)$。这启发我们考察 $D \bmod 3$。

若 $D \bmod 3 = 0$，正常删除即可。

若 $D \bmod 3 = 1$，在正常删除的基础上删除 $(0, 0, 0)$ 即可。

若 $D \bmod 3 = 2$，我们发现 $a = b = c, a + b + c \equiv 0 \pmod N$ 有大于一组解当且仅当 $N \equiv 0 \pmod 3$，这时候我们可以在正常删除的基础上删除 $\left(0,0,0\right), \left(\frac{N}{3},\frac{N}{3},\frac{N}{3}\right)$。

最后就是最困难的 $D \bmod 3 = 2, N \bmod 3 \neq 0$ 了。这时候我们可以选择一个尚未被删除的 $(a, a, (-2a) \bmod N)$，删除它对应的三个三元组，再插入 $(a, a, a)$ 即可做到删除两个的操作。 

## 代码

```cpp
#include <bits/stdc++.h>
#ifdef LOCAL
#include "debug.h"
#else
#define dbg(x...)
#endif
using namespace std;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int n, k;
    cin >> n >> k;
    set<array<int, 3>> S;
    vector<array<int, 3>> E;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            int k = (2 * n - i - j) % n;
            S.insert({i, j, k});
            pair<int, int> p1{i, j}, p2{j, k}, p3{k, i};
            if (p1 != p2 && p2 != p3 && p1 != p3) {
                E.push_back({i, j, k});
            }
        }
    }
    k = n * n - k;
    auto erase_triple = [&](int a, int b, int c) -> bool {
        if (!S.contains({a, b, c}) || !S.contains({b, c, a}) || !S.contains({c, a, b})) {
            return false;
        }
        S.erase({a, b, c});
        S.erase({b, c, a});
        S.erase({c, a, b});
        return true;
    };
    while (k >= 3) {
        assert(!E.empty());
        while (!erase_triple(E.back()[0], E.back()[1], E.back()[2])) {
            E.pop_back();
        }
        E.pop_back(), k -= 3;
    }
    if (k == 1) {
        S.erase({0, 0, 0});
    } else if (k == 2) {
        if (n % 3 == 0) {
            S.erase({0, 0, 0});
            S.erase({n / 3, n / 3, n / 3});
        } else {
            for (int i = 1; i < n; i++) {
                if (erase_triple(i, i, (3 * n - 2 * i) % n)) {
                    S.insert({i, i, i});
                    break;
                }
            }
        }
    }
    for (auto &x : S) {
        for (int i = 0; i < 3; i++) {
            cout << x[i] << " \n"[i == 2];
        }
    }
    return 0;
}

```
