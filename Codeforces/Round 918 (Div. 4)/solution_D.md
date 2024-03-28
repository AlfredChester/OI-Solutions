# Codeforces Round 918 (Div. 4) D - Unnatural Language Processing 题解

## 题意

定义 $ \texttt{a} $ 和 $ \texttt{e} $ 为元音，记作 $ \texttt{V} $。

定义 $ \texttt{b} $，$ \texttt{c} $ 和 $ \texttt{d} $ 为辅音，记作 $ \texttt{C} $。

定义 $ \texttt{CV} $ 或 $ \texttt{CVC} $ 的字母组合为一个音节。现在给定一个未划分音节的单词，保证按照上述划分方法有唯一分法。求这个单词的划分（分隔处用 `.` 表示）。

## 分析思路

如果遇到了一个 $ \texttt{V} $，则需要考虑是否结束这个音节。

如果这个字母后面的第二个字母是一个 $ \texttt{C}$，则代表当前音节是 $ \texttt{CVC} $ 类型的，应当在下一个字母后面输出 `.`。

否则，代表当前音节是 $ \texttt{CV} $ 类型的，应当这个字母后面输出 `.`。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
inline int tp(char x) {
    return x == 'a' || x == 'e' ? 1 : 2;
}   // 1 代表 V, 2 代表 C
char c[N];
int t, n, tg; // tg 代表下一个输出句号的位置的标记
inline void solve(void) {
    cin >> n >> (c + 1);
    for (int i = 1; i <= n; i++) {
        cout << c[i];
        if (i == tg) cout << '.', tg = 0;
        if (i >= n - 1) continue;
        if (tp(c[i]) == 1) {
            if (tp(c[i + 2]) == 2) {
                tg = i + 1;
            } else {
                cout << '.';
            }
        }
    }
    cout << '\n';
}

int main(int argc, char const *argv[]) {
    cin >> t;
    while (t--) solve();
    return 0;
}

```
