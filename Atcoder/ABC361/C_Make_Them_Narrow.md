摘要：贪心，枚举，极差

[传送门：https://www.luogu.com.cn/problem/AT_abc361_c](https://www.luogu.com.cn/problem/AT_abc361_c)

## 题意

给定一个长度为 $n$ 的序列 $a_n$，从中任意删除 $k$ 个数字，使得剩余数字的极差最小。

## 分析思路

注意到极差的一个性质：如果删除一个数之后，数列的极差改变，那删除的一定是某个最大值或最小值。考虑先对 $a$ 进行排序。

由于一共删除了 $k$ 个数，我们枚举删除的最小值的个数 $i$，则剩下的数为 $a_{i + 1}, a_{i + 2}, \dots a_{i + n - k}$。由于 $a$ 已经排序，这段数的极差即为 $a_{i + n - k} - a_{i + 1}$。取极差的最小值即可。

时间复杂度为排序带来的 $O\left(n \log n\right)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 200010;
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int n, k, a[N], ans = 1e9;
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n >> k;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    sort(a + 1, a + 1 + n);
    for (int i = 0; i <= k; i++) {
        ans = min(ans, a[i + n - k] - a[i + 1]);
    }
    cout << ans << endl;
    return 0;
}

```
