摘要：数学，枚举，桶

[传送门：https://www.luogu.com.cn/problem/AT_abc393_e](https://www.luogu.com.cn/problem/AT_abc393_e)

## 题意

给定一个长度为 $n$ 的数组 $a$，现在对于每个 $i = 1, 2, \dots, n$，求出在数组中包含 $a_i$ 的长度为 $k$ 的子序列的 $\gcd$ 的最大值。

## 分析思路

首先观察到对于相同的值答案应该是一样的，同时原问题也等价于我们选出长度大于等于 $k$ 的子序列的 $\gcd$ 的最大值，考虑把所有相同的值放到一起来看。

抛开优秀的常数，时间复杂度为 $O(n\times \max \mathrm{d}(V))$ 甚至 $O(n \sqrt V)$ 的做法是不可取的，也就是说我们没法对每个数分解质因数。那么可以考虑枚举答案。设当前的答案为 $g$，如果 $\sum_{g \mid x} cnt_x \ge k$，那么 $g$ 对于所有 $g \mid x$ 的值 $x$ 都是合法的答案，对这些值更新答案即可。

总复杂度为 $O(\frac{V}{\sum_{i=1}^V i} + n) = O(V \log V + n)$，可以通过本题。使用 Dirichlet 前缀和可以做到 $O(V \log \log V + n)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1200010;
const int V = 1000010;
int n, k, a[N], cnt[V], ans[V];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> k;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], cnt[a[i]]++;
    }
    for (int g = 1; g < V; g++) {
        int tot = 0;
        for (int i = g; i < V; i += g) {
            tot += cnt[i];
        }
        if (tot < k) continue;
        for (int i = g; i < V; i += g) {
            ans[i] = g;
        }
    }
    for (int i = 1; i <= n; i++) {
        cout << ans[a[i]] << '\n';
    }
    return 0;
}

```
