摘要：组合数学，二项式定理，扫描线

[传送门：https://www.luogu.com.cn/problem/AT_abc399_f](https://www.luogu.com.cn/problem/AT_abc399_f)

## 题意

给定正整数 $n, k$ 和一个长度为 $n$ 的数组 $a$，求 $S = \sum_{1 \le l \le r \le n} (\sum_{l \le i \le r} a_i)^k$。

## 分析思路

当 $k = 2$ 的时候，我们可以展开二项式定理，分别统计三项的结果。但 $k$ 很大时，如果直接对后面的式子进行展开，总共就会有 $\binom{n}{k}$ 项，无法进行统计。

那我们首先对 $k$ 次方内的区间求和做一个常见的优化——前缀和优化。记 $s_i = \sum_{j = 1}^i a_i$，特别地，$s_0 = 0$。于是我们有：

$$
\begin{aligned}
S &= \sum_{1 \le l \le r \le n} (\sum_{l \le i \le r} a_i)^k \\
&= \sum_{1 \le l \le r \le n} (s_r - s_{l - 1})^k
\end{aligned}
$$

求和的是一个二项式，进行展开：

$$
\begin{aligned}
S &= \sum_{1 \le l \le r \le n} (s_r - s_{l - 1})^k \\
&= \sum_{1 \le l \le r \le n} \sum_{i = 0}^k (-1)^{k - i} \binom{k}{i} s_r^i s_{l - 1}^{k - i}
\end{aligned}
$$

这样直接求时间复杂度是 $O(n^2k)$ 的，但是我们可以更换求和顺序，并且在枚举区间的时候使用扫描线序，即在扫 $l$ 或 $r$ 中的一个时快速统计出所有的另一个端点和它组成的区间对应的所有信息。

$$
\begin{aligned}
S &= \sum_{1 \le l \le r \le n} \sum_{i = 0}^k (-1)^{k - i}  \binom{k}{i} s_r^i s_{l - 1}^{k - i} \\
&= \sum_{1 \le r \le n} \sum_{i = 0}^k (-1)^{k - i} \binom{k}{i} s_r^i  \sum_{1 \le l \le r} s_{l - 1}^{k - i}
\end{aligned}
$$

而 $\sum_{1 \le l \le r} s_{l - 1}^{k - i}$ 是可以在扫描过程中算出的。特别对于拆前缀和要注意 $l = 1$ 时，$s_{l - 1} = s_0 = 0$ 对应的贡献。

时间复杂度 $O(nk)$，可以通过。

## 代码

```cpp
// 组合数的实现：https://alfredchester.github.io/cpp-templates/src/alfred/math/comb.hpp
#include "alfred/all"
#include "alfred/debug"
using namespace std;
const int N = 200010;
Comb<m998> comb;
m998 a[N], s[N], sum[11], ans; // sum[i]: s[0]^i + s[1]^i + ... + s[r - 1]^i
inline m998 sgn(int x) {
    return x & 1 ? -1 : 1;
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int n, k;
    optimizeIO(), cin >> n >> k;
    for (int i = 1; i <= n; i++) {
        cin >> a[i], s[i] = s[i - 1] + a[i];
    }
    sum[0] = 1; // 特别地，0^0 = 1，否则结果会错
    for (int i = 1; i <= n; i++) {
        m998 pw = 1;
        for (int j = 0; j <= k; j++) {
            ans += sgn(k - j) * comb.binom(k, j) * pw * sum[k - j];
            pw *= s[i];
        }
        pw = 1;
        for (int j = 0; j <= k; j++) {
            sum[j] += pw, pw *= s[i];
        }
    }
    cout << ans << '\n';
    return 0;
}

```
