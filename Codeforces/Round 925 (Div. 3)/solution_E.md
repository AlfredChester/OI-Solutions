# Codeforces Round 925 (Div. 3) E - Anna and the Valentine's Day Gift 题解

## 题意

`Sasha` 和 `Anna` 在玩游戏。初始他们有一个列表 $a_n$，两人轮流游玩，`Anna` 为先手。

- 轮到 `Anna` 时，她必须**从列表中选择一个元素** $a_i$，并将其反转。例如，如果 `Anna` 选择了数值为 $42$ 的元素，它就会变成 $24$；如果 `Anna` 选择了数值为 $1580$ 的元素，它就会变成 $851$。注意前导零会被去掉。在这一轮之后，列表中元素的数量不会改变。
- 轮到 `Sasha` 时，他必须**从列表中提取两个**元素 $a_i$ 和 $a_j$（$i \ne j$），以任意顺序将它们连接起来，并将结果放回列表。例如，如果 `Sasha` 选择了等于 $2007$ 和 $19$ 的元素，他将从列表中删除这两个元素，并添加整数 $200719$ 或 $192007$。在这一轮之后，列表中的元素数会减少 $1$。

当 `Anna` 操作后，列表中只剩下一个元素时，游戏结束。如果这个数**不小于** $10 ^ m$，则 `Sasha` 获胜，否则 `Anna` 获胜。

给定 $n, m, a_n$，试判断最终谁会获胜。

## 分析思路

下文记 $|x| = \lfloor \log_{10}(x) \rfloor + 1$，即 $x$ 的数位个数；记 $\operatorname{merge}(x, y) = x \times 10 ^ {|y|} + y$，即为数字 $x$ 与 $y$ 拼接之后的结果；记 $\Delta{x}$ 为 $x$ 的后缀零的个数，即反转后长度的变化量。


我们并不关心具体的字符串内容，所以可以使用二元对 $(x, y)$ 表示后缀零的个数为 $x$，长度为 $y$ 的一个数字串。

考虑模拟两人的操作。注意到 `Anna` 要贪心地将最终数字串的长度变得尽可能小，所以要利用反转可能删除前导零的性质。

注意到反转后的前导零个数即为原数后缀零的个数，`Anna` 每次贪心地选择 $\Delta{x}$ 最大的 $x$ 进行反转即可。反转之后的后缀零个数一定为 $0$。所以若原先的二元对为 $(x, y)$，反转后的二元对则变为 $(0, y - x)$。

再来说说 `Sasha`。`Sasha` 要尽可能保护 $\Delta{x}$ 最大的 $x$，使得 `Anna` 无法去除 $x$ 的贡献。

注意到 $\Delta{\operatorname{merge}(x, y)} = \Delta y$。于是 `Sasha` 的贪心策略为，每次取出后缀零最多的二元对 $(x_1, y_1)$ 和后缀零最少的二元对 $(x_2, y_2)$，并合成为二元对 $(x_2, y_1 + y_2)$。

注意到 $x \geq 10 ^ m$ 的充要条件为 $|x| > m$，即最终拼成的数字串的长度大于 $m$。这即为最后进行判断的依据。

最后只要能动态维护二元对 $(x, y)$ 中以 $x$ 为第一关键字排序的一个列表即可，使用 `std::multiset` 实现。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int t, n, m;
inline int calcDelta(string &s) {
    for (int i = s.size() - 1; i >= 0; i--) {
        if (s[i] != '0') {
            return s.size() - i - 1;
        }
    }
    return 0; // s = "0", 虽然并不会发生
}
string str;
multiset<pair<int, int>> S;
inline void solve(void) {
    cin >> n >> m, S.clear();
    for (int i = 1; i <= n; i++) {
        cin >> str, S.insert({calcDelta(str), str.size()});
    }
    while (true) {
        auto p = *--S.end();
        S.erase(--S.end());
        S.insert({0, p.second - p.first});
        if (S.size() == 1) break;
        auto p1 = *S.begin(), p2 = *(--S.end());
        S.erase(S.begin()), S.erase(--S.end());
        S.insert({p1.first, p1.second + p2.second});
    }
    if (S.begin()->second > m) {
        cout << "Sasha\n";
    } else {
        cout << "Anna\n";
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
