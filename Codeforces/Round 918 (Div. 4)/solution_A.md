# Codeforces Round 918 (Div. 4) A - Odd One Out 题解

## 题意

$ t $ 组数据，每次输入三个整数 $ a, b, c $ ，求这三个数中不同于其他两个的那个数。

## 分析思路

利用异或的性质。两个相同的数异或和为 $ 0 $ ；$ 0 $ 异或一个数等于它本身。所以将这三个数异或起来，比对与原来哪个数相同即可。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
inline void solve(void) {
    int a, b, c;
    cin >> a >> b >> c;
    const int d = a ^ b ^ c;
    if (d == a) printf("%d\n", a);
    if (d == b) printf("%d\n", b);
    if (d == c) printf("%d\n", c);
}
int main(int argc, char const *argv[]) {
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```