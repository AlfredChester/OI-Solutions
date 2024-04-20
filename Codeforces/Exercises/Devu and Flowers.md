摘要：容斥原理，组合数学

[传送门：https://www.luogu.com.cn/problem/CF451E](https://www.luogu.com.cn/problem/CF451E)

## 题意

Devu 有 $n$ 个花瓶，第 $i$ 个花瓶里有 $a_i$ 朵花。他现在要选择 $s$ 朵花。

你需要求出有多少种方案。两种方案不同当且仅当两种方案中至少有一个花瓶选择花的数量不同。

答案对 $10^9+7$ 取模。

$1\le n\le 20,0\le a_i\le 10^{12},0\le s\le 10^{14}$

## 分析思路

### 一、不考虑制约

我们先来考虑如果我们有 $n$ 个花瓶，$s$ 朵花，每个花瓶里有无限多的花所对应的方案数。

我们记 $x_i$ 表示从第 $i$ 个花瓶中拿多少朵花。问题等价于找到：

$$
x_1 + x_2 + \cdots + x_n = s
$$

的非负整数解的个数。

由于非负整数解难以统计，我们把每一位都加 $1$。记 $y_i = x_i + 1$。我们只要找到：

$$
y_1 + y_2 + \cdots + y_n = s + n
$$

的正整数解。由隔板法得答案为 $C^{n-1}_{s+n-1}$。

### 二、考虑制约

注意到我们只要减去非法方案。我们先来考虑不满足制约 $a_1$ 的方案数，也即 $x_1 > a_1$ 的方案数。我们将总数 $s$ 先减去 $a_1 + 1$，然后就可以转化为不考虑制约的情况进行计算。可以得到答案为 $C^{n-1}_{s+n-1-(a_1 + 1)}$。

多个制约同时考虑类似，例如我们计算不满足制约 $a_1, a_2$ 的方案数。类似上述分析可以的得到答案为 $C^{n-1}_{s+n-1-(a_1 + 1) - (a_2+1)}$。

最后对答案进行一下加减合并即可。

### 三、计算组合数

由组合数的定义：

$$
C^m_n = \frac{n!}{m!(n-m)!} = \frac{n!}{(n-m)!} \times \frac{1}{m!}
$$

这两部分都只有 $n$ 项。暴力计算即可。

$\frac{1}{m!}$ 是定值，可以提前计算。

时间复杂度 $O\left(\log p + 2 ^ n * n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
const i64 p = 1e9 + 7;
i64 n, s, f, B = 1, ans, A[20];
inline i64 fastPow(i64 base, i64 index) {
    i64 ans = 1;
    while (index) {
        if (index & 1) ans = ans * base % p;
        index >>= 1, base = base * base % p;
    }
    return ans;
}
inline i64 C(i64 a, i64 b) {
    i64 A = 1;
    if (a < b) return 0;
    for (i64 i = a; i > a - b; i--) {
        A = i % p * A % p;
    }
    return A * B % p;
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, const char *argv[]) {
    optimizeIO(), cin >> n >> s;
    for (int i = 1; i < n; i++) B = B * i % p;
    for (int i = 0; i < n; i++) {
        cin >> A[i];
    }
    B = fastPow(B, p - 2);
    for (int S = 0; S < (1 << n); S++) {
        i64 a = s + n - 1, b = n - 1, f = 1;
        for (int i = 0; i < n; i++) {
            if ((S >> i) & 1) {
                f *= -1, a -= A[i] + 1;
            }
        }
        ans = (ans + f * C(a, b) + p) % p;
    }
    cout << ans % p << endl;
    return 0;
}

```
