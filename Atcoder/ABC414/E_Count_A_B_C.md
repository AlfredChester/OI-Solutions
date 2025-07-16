摘要：整除分块

[传送门：https://www.luogu.com.cn/problem/AT_abc414_e](https://www.luogu.com.cn/problem/AT_abc414_e)

## 题意

给定正整数 $n$，询问有多少个三元组 $(a, b, c)$ 满足 $1 \leq a, b, c \leq n$，$a, b, c$ 互不相同且 $a \bmod b = c$，答案对 $998244353$ 取模。

## 分析思路

考虑实际上有效的三元组是 $O(n^2)$ 级别的，因为确定了 $a, b$ 就能唯一确定 $c$，但是为什么答案不恰好 $n^2$ 呢？一种情况是 $a \bmod b = 0$，此时 $c = 0$，不满足 $c \ge 1$ 的条件；还有一种情况是 $a < b$，此时 $a = c$，不满足 $a \neq c$ 的条件。注意 $b = c$ 不可能，而 $a \bmod b = 0$ 包含了 $a = b$ 的情况。

因此对于某个 $b$，它对应的答案就是 $n - \lfloor \frac{n}{b} \rfloor - (b - 1)$，即刨去 $b$ 的倍数和小于 $b$ 的数。问题转化为求 $\sum_{b = 1}^n n - \lfloor \frac{n}{b} \rfloor - (b - 1)$。使用整除分块，由于 $n - \lfloor \frac{n}{b} \rfloor + 1$ 不变，每一块内是等差数列的经典形式。因此我们可以在 $O(\sqrt n)$ 的时间复杂度内解决本问题。

## 代码

```cpp
#include "alfred/all"
#include "alfred/debug"
using namespace std;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    i64 n;
    optimizeIO(), cin >> n;
    m998 ans = 0, i2 = m998(2).inv(); // 预处理 2 的逆元
    for (i64 l = 1, r; l <= n; l = r + 1) {
        r = min(n / (n / l), n);
        i64 c = n - n / l + 1;
        i64 L = c - r, R = c - l;
        ans += m998(L + R) * (r - l + 1) * i2;
    }
    cout << ans << endl;
    return 0;
}

```
