摘要：数学，模拟

[传送门：https://www.luogu.com.cn/problem/CF1915C](https://www.luogu.com.cn/problem/CF1915C)

## 题意

$ t $ 组数据。每次给定一个长度为 $ n $ 的数组 $ a $，求其和是否为完全平方数。

## 分析思路

由题，我们只要计算数列的和 $x$，判断 $x$ 是否为完全平方数，即 $\lfloor \sqrt x \rfloor ^ 2 = x$ 即可。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
long long t, n, x, sum;
inline void solve(void) {
    cin >> n, sum = 0;
    for (int i = 1; i <= n; i++) {
        cin >> x, sum += x;
    }
    if ((ll)sqrtl(sum) * (ll)sqrtl(sum) == sum) {
        puts("YES");
    } else {
        puts("NO");
    }
}
int main(int argc, char const *argv[]) {
    cin >> t;
    while (t--) solve();
    return 0;
}

```
