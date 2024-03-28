# Codeforces Round 918 (Div. 4) B - Not Quite Latin Square 题解

## 题意

Latin 矩阵是一个 $3 \times 3 $ 的，每一行、每一列都各有一个 $ \texttt{A} $，$ \texttt{B} $ 和 $ \texttt{C} $ 的矩阵。

$ t $ 组数据。每次输入一个残缺的 Latin 矩阵（缺失位置用 $\texttt{?}$ 表示），求缺失的位置是什么字母。

## 分析思路

利用异或的性质。两个相同的字符异或和为 $ 0 $；$ 0 $ 异或一个字符这个字符本身。

所以将 $ \texttt{A} $，$ \texttt{B} $ 和 $ \texttt{C} $ 异或起来，然后异或上 $ \texttt{?} $ 所处那一行或那一列剩下的两个字符，即可得答案。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
inline void solve(void) {
    char c[3][3];
    int x = 0, y = 0;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            cin >> c[i][j];
            if (c[i][j] == '?') x = i, y = j;
        }
    }
    char A = 'A' ^ 'B' ^ 'C';
    for (int i = 0; i < 3; i++) {
        if (i != y) A ^= c[x][i];
    }
    cout << A << endl;
}
int main(int argc, char const *argv[]) {
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}

```