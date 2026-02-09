摘要：dp

[传送门：https://www.luogu.com.cn/problem/AT_arc214_c](https://www.luogu.com.cn/problem/AT_arc214_c)

## 题意

有 $n$ 个正整数 $p_1, p_2, \dots, p_n$。需要把它们划分成四个不交的非空集合 $A, B, C, D$，使得 $\sum A = \sum B$ 且 $\sum C = \sum D$。求方案数对 $998244353$ 取模的结果。$4 \le n \le 500, \sum p \le 10^5$。

## 分析思路

考虑把 $(\sum A - \sum B, \sum C - \sum D)$ 看作二维向量。决策的过程就是在每一位选 $(p_i, 0), (-p_i, 0), (0, p_i), (0, -p_i)$，最终组成 $\vec{0}$ 的方案数。

这样做状态数是 $O(nV^2)$ 的，不能接受。考虑曼哈顿转切比雪夫。决策的过程相当于在每一位独立地，先在 $(\frac{p_i}{2}, \frac{p_i}{2}), (-\frac{p_i}{2}, -\frac{p_i}{2})$ 中选一个，再在 $(\frac{p_i}{2}, -\frac{p_i}{2}), (-\frac{p_i}{2}, \frac{p_i}{2})$ 中选一个。这样两维的决策就独立了。

设 $f_{i, j}$ 表示前 $i$ 个数，某一维为 $j$ 的方案数。每一位在这一维上的决策是 $+p$，不变或 $-p$。转移直接背包即可。我们不太希望转移有负数，所以直接 dp 选一半的方案数，决策只有选或不选。

但是可能会出现空集的情况。我们可以先算出总方案数，然后减去至少有一维为空集的方案数，也即一维在上述决策过程第一步只用了其中一个向量。答案即为 $f_{n, \frac{\sum p}{2}}^2 - 2 \times f_{n, \frac{\sum p}{2}}$。

## 代码

```cpp
#include <bits/stdc++.h>
#include "alfred/math/mod-int.hpp"
using namespace std;
const int N = 501;
int n, tot, a[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
m998 f[100010];
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], tot += a[i];
    }
    const int len = tot / 2;
    if (tot % 2 == 1) {
        cout << "0\n";
        return 0;
    }
    f[0] = 1;
    for (int i = 1; i <= n; i++) {
        for (int j = len; j >= a[i]; j--) {
            f[j] += f[j - a[i]];
        }
    }
    cout << f[len] * (f[len] - 2) << '\n';
    return 0;
}
```
