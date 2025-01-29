摘要：双指针，贪心

[传送门：https://www.luogu.com.cn/problem/P11639](https://www.luogu.com.cn/problem/P11639)

## 题意

有一个下落式音游，谱面是一个长度为 $m+1$ 的线段。音游有 $n+m$ 秒，在前 $n$ 秒中，第 $i$ 秒会在高度 $1$ 处出现一个集合，这个集合可以用一个可重无序集 $S_i$ 来表示，大小为 $a_i$，每秒末尾这个集合的高度会增加 $1$。

手指一开始在高度 $1$，每次可以 `flip` 消除手指在的高度的集合的一段值域。代价为值域的长度。然后可以选择将手指的高度加 $1$。若有非空的集合到达 $m+1$ 处就失败了。求在不失败的情况下最少需要花费多少代价？

## 分析思路

如果 $m$ 无限大，最优的策略显然就是手指一直跟着目前没有被消除的集合，消除它其中一段值域的连续段（因为如果“放走”了一个非空的集合，就永远追不上了）。这时候答案就是所以 $S_i$ 的每个值域连续段的长度之和。

而有了 $m$ 的限制，也就意味着我们可能需要用更少的步数完成所有消除。考虑答案会多出来的部分。具体来说，如果有两个值域连续段 $[l_1, r_1], [l_2, r_2]$，其中 $l_2 - r_1 > 1$，我们可以选择花费两个单位高度，分别消除两段，代价是 $(r_1 - l_1 + 1) + (r_2 - l_2 + 1)$；我们也可以只花费一个单位高度，代价是 $r_2 - l_1 + 1$，相比上一种答案增加了 $l_2 - r_1 - 1$。其实就是多花了 $(r_1, l_2)$ 的代价。我们把这些值域之间的空隙叫做 `gap`。

那么，贪心策略就很显然了，花费一个 `gap` 就能挣回一个单位的高度，那么显然挑最小的 `gap` 补上就行。如果有 $g$ 个 `gap`，那么有 $g + n$ 个值域连续段，要将高度限制在 $n + m - 1$ 内，就要删除 $g - m + 1$ 个 `gap`。通过排序和双指针处理值域连续段，贪心即可。时间复杂度 $O(\sum a_i \log a_i)$，可以通过。

其实本题蕴涵了反悔贪心的思想，读者可以自行思考。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
int n, m, x, S[50010];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    long long ans = 0;
    std::vector<int> G; // 存储所有 gap
    optimizeIO(), cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        cin >> x;
        for (int j = 1; j <= x; j++) {
            cin >> S[j];
        }
        sort(S + 1, S + 1 + x);
        for (int l = 1, r = 1; l <= x; l = r) { // 双指针处理值域连续段
            while (r <= x && S[r] - S[l] <= 1) r++;
            if (l != 1) G.push_back(S[l] - S[l - 1] - 1);
            ans += S[r - 1] - S[l] + 1; // 加上逃不掉的答案
        }
    }
    sort(G.begin(), G.end());
    for (int i = 0; i <= (int)G.size() - m; i++) {
        ans += G[i]; // 加上额外要补的答案
    }
    cout << ans << endl;
    return 0;
}

```
