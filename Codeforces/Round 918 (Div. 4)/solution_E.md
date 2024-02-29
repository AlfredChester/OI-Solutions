# Codeforces Round 918 (Div. 4) E - Romantic Glasses 题解

## 题意

$ t $ 组数据。每次给定一个数列 $ a $ ，判断是否存在一个区间 $ [l, r] $ 使得：

如果 $ l, r $ 同奇偶，则 $a_l + a_{l+2} + a_{l+4} + ... + a_r = a_{l+1} + a_{l+3} + ... + a_{r-1}$ 。

如果 $ l, r $ 异奇偶，则 $a_l + a_{l+2} + a_{l+4} + ... + a_{r-1} = a_{l+1} + a_{l+3} + ... + a_{r}$ 。

## 分析思路

记 $ x_i $ 表示到第 $ i $ 个位置，所有奇数位上的数的和。记 $ y_i $ 表示到第 $ i $ 个位置，所有偶数位上的数的和。

选定一个区间 $ [l, r] $ 。判断条件等价于 $x_r - x_{l - 1} = y_r - y_{l - 1}$ 。变形可得 $x_r - y_r = x_{l-1}-y_{l-1}$ 。在 `std::set` 中记录每一个位置的 $x_i-r_i$，判断在第 $ i $ 个位置之前是否有相同的差即可。

注意对于第 $0$ 个位置， $x_i - y_i = 0$ 。需要插入到 `std::set` 中。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
long long t, n, a[N];
inline void solve(void) {
    set<long long> S;
    cin >> n, S.insert(0);
    long long x = 0, y = 0, f = 0;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
        if (i & 1) x += a[i];
        else y += a[i];
        if (S.count(x - y)) f = 1;
        S.insert(x - y);
    }
    puts(f ? "YES" : "NO");
}
int main(int argc, char const *argv[]) {
    cin >> t;
    while (t--) solve();
    return 0;
}

```
