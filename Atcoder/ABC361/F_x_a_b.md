摘要：枚举，数学

[传送门：https://www.luogu.com.cn/problem/AT_abc361_f](https://www.luogu.com.cn/problem/AT_abc361_f)

## 题意

求在 $[1, n]$ 可被表示为 $a^b(b \ge 2)$ 的正整数 $x$ 的个数。

## 分析思路

撞 [P9118 [春季测试 2023] 幂次](https://www.luogu.com.cn/problem/P9118) 了，乐。

注意到 $a \le \sqrt[b]n$。当 $b \ge 3$ 时显然有 $a \le 10^6$，这是一个极其好的性质。这意味着我们可以在 $O(\sqrt[3]n \log n)$ 次枚举中得出所有 $b \ge 3$ 的 $x$ 的个数，且这个个数也在 $O(\sqrt[3]n \log n)$ 附近。于是我们可以使用 `std::unordered_set` 去重。

问题在于，当 $b = 2$ 时， $a \le 10^9$，显然不能枚举 $a$。注意到 $n$ 以内的完全平方数有 $\lfloor \sqrt n \rfloor$ 个，但问题在于这些数中有些可能已经被上一部分统计过了。于是我们在求解上一问的时候，对于完全平方数不放入即可即可。

最终答案为 $|S| + \lfloor \sqrt n \rfloor$，其中 $S = \{x | x = a^b, a \ge 1, b \ge 3, 1 \le x \le n\}$。时间复杂度 $O(\sqrt[3]n \log n)$，可以通过。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
inline bool isPerfectSquare(i64 x) {
    return (i64)sqrt(x) * (i64)sqrt(x) == x;
}
std::unordered_set<i64> s;
int main(int argc, char const *argv[]) {
    i64 n;
    scanf("%lld", &n);
    for (i64 i = 2; i <= 1e6; i++) {
        __int128 curr = i * i;
        while (curr <= n) {
            if (!isPerfectSquare(curr)) {
                s.insert(curr);
            }
            curr = curr * i;
        }
    }
    printf("%lu\n", s.size() + (size_t)sqrtl(n));
    return 0;
}

```
