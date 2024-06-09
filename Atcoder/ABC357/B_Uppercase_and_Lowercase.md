摘要：字符串，模拟

[传送门：https://www.luogu.com.cn/problem/AT_abc357_b](https://www.luogu.com.cn/problem/AT_abc357_b)

## 题意

给定一个长度为奇数的，由大小写英语字母组成的字符串 $S$。如果 $S$ 中大写字母的个数大于小写字母的个数，则将 $S$ 中的所有小写字母转化为大写字母输出。否则将 $S$ 中的所有大写字母转化为小写字母输出。

## 分析思路

有人赛时用 `std::islower` 吃了五发罚时，我不说是谁。

依照题意我们需要统计大写字母和小写字母出现的次数。然而，`std::islower` 和 `std::isupper` 返回的不是布尔值！所以要么自己老老实实实现一个 `islower` 函数要么直接 `std::count` 也可以。

警示后人，警钟撅烂。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int cnt = 0;
    std::string s;
    optimizeIO(), cin >> s;
    for (char c = 'a'; c <= 'z'; c++) { // cnt for lowercases
        cnt += count(s.begin(), s.end(), c);
    }
    if (cnt < s.size() - cnt) {
        for (auto c : s) cout << (char)toupper(c);
    } else {
        for (auto c : s) cout << (char)tolower(c);
    }
    return 0;
}
```
