摘要：枚举

[传送门：https://www.luogu.com.cn/problem/CF1931A](https://www.luogu.com.cn/problem/CF1931A)

## 题意

将每个拉丁字母从 `a` 到 `z` 依次编号为 $1$ 到 $26$，定义一个字符串的编码为其每个字母的编号的和，如 `cat` 的编码为 $3 + 1 + 20 = 24$。

现给定一个正整数 $n$。请求出编码为 $n$ 且字典序最小的长度为 $3$ 的字符串。

## 分析思路

注意到 $t \leq 100$，$26 ^ 3 = 17576$，考虑暴力枚举每个字符的编号，并验证字符串的编码是否为 $n$。

如果循环嵌套枚举第一、第二、第三个字符的编号，可以证明找到的第一组解即为字典序最小的解。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int t, n;
inline void solve(void) {
    cin >> n;
    for (int i = 1; i <= 26; i++) {
        for (int j = 1; j <= 26; j++) {
            for (int k = 1; k <= 26; k++) {
                if (i + j + k == n) {
                    cout << char(i + 'a' - 1);
                    cout << char(j + 'a' - 1);
                    cout << char(k + 'a' - 1);
                    cout << '\n';
                    return;
                }
            }
        }
    }
}
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> t;
    while (t--) solve();
    return 0;
}

```