摘要：搜索，模拟

[传送门：https://www.luogu.com.cn/problem/AT_abc361_d](https://www.luogu.com.cn/problem/AT_abc361_d)

## 题意

有一排 $n + 2$ 个格子。初始第 $n + 1$ 与第 $n + 2$ 个格子为空，前 $n$ 个格子的颜色用以 `W` 和 `B` 组成的字符串 $s$ 描述。

你可以进行若干次操作。每次操作，你可以选择两个非空的连续的位置 $x$ 与 $x + 1$，将这两个位置的颜色与当前的空位置 $k$ 与 $k + 1$ 交换。问经过若干次操作后，能否达到前 $n$ 个格子的颜色为字符串 $t$ 所描述的状态且第 $n + 1$ 与第 $n + 2$ 个格子为空？

若不能，输出 `-1`。若可以，输出最小的操作数。

## 分析思路

首先发现若颜色数量不一致一定无解。否则按题意搜索即可。我们用两个空位置（`.` 字符）占位，枚举每次交换的位置并进行进一步搜索。可以使用 `std::unordered_set` 来记录已经到达过的状态。容易发现总状态数为 $O(2^n \times n)$，时间复杂度即为 $O(2 ^ n \times n)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    int n;
    string s, t;
    optimizeIO(), cin >> n >> s >> t;
    int cs = count(s.begin(), s.end(), 'W');
    int ct = count(t.begin(), t.end(), 'W');
    if (cs != ct) {
        cout << "-1\n";
        return 0;
    }
    s += "..", t += "..";
    unordered_set<string> vis;
    queue<pair<string, int>> Q;
    Q.push({s, 0}), vis.insert(s);
    while (!Q.empty()) {
        auto [lS, step] = Q.front();
        Q.pop();
        if (lS == t) {
            cout << step << '\n';
            return 0;
        }
        int pos = lS.find_first_of('.');
        for (int i = 0; i < n + 1; i++) {
            if (i == pos || i == pos + 1 || i == pos - 1) {
                continue;
            }
            string ns = lS;
            swap(ns[i], ns[pos]), swap(ns[i + 1], ns[pos + 1]);
            if (!vis.count(ns)) {
                vis.insert(ns), Q.push({ns, step + 1});
            }
        }
    }
    cout << "-1\n";
    return 0;
}

```
