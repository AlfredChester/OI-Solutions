摘要：贪心、双指针

[传送门：http://47.100.33.97/d/noipmoni/p/T250413B](http://47.100.33.97/d/noipmoni/p/T250413B)

## 题意

有 $n$ 个硬币和游戏机，第 $i$ 个硬币坐标为 $a_i$，第 $i$ 个游戏机坐标为 $b_i$。

你一开始在大街的最左端，位置是 $0$，你路过硬币就会捡起来。问在每个街机游戏机游玩一次需要移动的最小距离。你可以同时携带多枚硬币，你可以在大街上自由往左或往右移动。

## 分析思路

这个做法有可能是假的，欢迎大家来叉。

首先把所有硬币和游戏机混起来排成一列。因为硬币和游戏机个数一样，必须走到过最右边，想到如果你有钱的话直接往右走然后开。如果开始欠钱了就找一段最短的区间，拿了之后能还清债顺便把这个区间所有的游戏机都开了，然后加上这段区间长度的两倍，可以通过双指针实现。（如果再长一点，就会浪费来回，不合算）

然后你发现你没过样例。样例给出的方案是最后到位置 $7$ 结束。这类似于一种“尾杀”。具体来说，因为硬币个数和游戏机个数一样，如果你从一个不欠钱（或者刚开始欠钱）的游戏机走到最后一个地方再回来，也能解锁所有的游戏机。

然后就做完了。这个算法有它的合理性是因为在中间位置你不能进行类似的“尾杀”操作，因为总是要走到最右的位置。时间复杂度 $O(n)$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int n, x, have;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
#ifndef LOCAL
    freopen("game.in", "r", stdin);
    freopen("game.out", "w", stdout);
#endif
    optimizeIO(), cin >> n;
    vector<array<int, 2>> P;
    for (int i = 1; i <= n; i++) {
        cin >> x, P.push_back({x, -1});
    }
    for (int i = 1; i <= n; i++) {
        cin >> x, P.push_back({x, 1});
    }
    sort(P.begin(), P.end());
    long long ans = P.back()[0], mn = 1e18;
    for (int i = 0, j; i < 2 * n;) {
        if (P[i][1] == -1 && have == 0) {
            have--, j = i + 1;
            while (j < 2 * n && have < 0) {
                have += P[j][1], j++;
            }
            mn = min(mn, ans + P.back()[0] - P[i][0]);
            ans += 2 * (P[j - 1][0] - P[i][0]), i = j;
        } else {
            have += P[i][1], i++;
        }
    }
    cout << min(mn, ans) << endl;
    return 0;
}
```
