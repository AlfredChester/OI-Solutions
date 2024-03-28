# Codeforces Round 918 (Div. 4) C - Can I Square? 题解

## 题意

$ t $ 组数据。每次给定一个长度为 $ n $ 的数组 $ a $，求其和是否为完全平方数。

## 分析思路

题意基本已经告诉了我们解法。用代码判断完全平方数即可。

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
