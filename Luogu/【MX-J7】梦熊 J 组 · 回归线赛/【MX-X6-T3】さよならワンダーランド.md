摘要：ST 表，参变分离

[传送门：https://www.luogu.com.cn/problem/P11157](https://www.luogu.com.cn/problem/P11157)

## 题意

给定一个长度为 $n$ 的序列 $a$，请对于每个 $i = 1, 2, \dots, n$，判断是否存在满足下列条件的 $j$，若存在，还需给出构造：

- $1 \le i + j \le n$；
- $a_i \le j \le a_{i + j}$。

## 分析思路

这题正解是个很好的 trick，没想到没见过居然能场切。

首先先对 $j$ 的范围有一些基本的刻画：$1 - i \le j \le n - i, j \ge a_i$。于是我们有 $\max(1 - i, a_i) \le j \le n - i$，这可以让我们完成初步的无解判定，以及写出一个 $O(n^2)$ 的暴力。分析暴力，我们发现 $j \le a_{i + j}$ 还是比较棘手，因为下标中带有 $j$ 而且不是纯粹的 $j$。

于是，我们可以考虑**参变分离**。令 $k = i + j$。则我们可以整理得到以下两个式子：

- $\max(1, a_i + i) \le k \le n$；
- $k - i \le a_k$，即 $i \ge k - a_k$。

注意到第一个式子实际上是刻画了 $k$ 的范围，而第二个式子是对选定 $k$ 的合法性的判断。显然我们在 $k$ 可取的范围内，取到最小的 $k - a_k$ 是一定不劣的。我们枚举 $i$，使用 ST 表对二元对 $(k - a_k, k)$ 取 $\min$ 合并，再进行一下判定即可。若合法答案即为 $j = k - i$。

时间复杂度 $O(n \log n)$，可以通过本题。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using i64 = long long;
const int N = 300010;
template <class T>
struct MinInfo {
    T val;
    MinInfo(void) { val = std::numeric_limits<T>::max(); }
    template <class InitT>
    MinInfo(InitT x) { val = x; }
    MinInfo operator+(MinInfo &x) {
        return {std::min(val, x.val)};
    }
};
template <class T>
class SparseTable {
private:
    int n;
    std::vector<std::vector<T>> ST;

public:
    SparseTable(void) {}
    SparseTable(int N) : n(N), ST(N, std::vector<T>(std::__lg(N) + 1)) {}
    template <class InitT>
    SparseTable(std::vector<InitT> &init) : SparseTable(init.size()) {
        for (int i = 0; i < n; i++) ST[i][0] = T(init[i]);
        for (int i = 1; (1 << i) <= n; i++) {
            for (int j = 0; j + (1 << i) - 1 < n; j++) {
                ST[j][i] = ST[j][i - 1] + ST[j + (1 << (i - 1))][i - 1];
            }
        }
    }
    inline T query(int l, int r) { // 0 based
        if (l > r) return T();
        int w = std::__lg(r - l + 1);
        return ST[l][w] + ST[r - (1 << w) + 1][w];
    }
};
int n, a[N], t[N];
inline void optimizeIO(void) {
    ios::sync_with_stdio(false);
    cin.tie(NULL), cout.tie(NULL);
}
int main(int argc, char const *argv[]) {
    optimizeIO(), cin >> n;
    vector<pair<int, int>> t(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> a[i], t[i] = {i - a[i], i};
    }
    SparseTable<MinInfo<pair<int, int>>> ST(t);
    for (int i = 1; i <= n; i++) {
        int l = max({1 - i, a[i]}), r = n - i;
        if (l > r) {
            cout << "0\n";
            continue;
        }
        l += i, r += i; // k \in [l, r]
        auto res = ST.query(l, r).val;
        int k = res.second, val = res.first;
        if (i >= val) {
            cout << "1 " << k - i << '\n';
        } else {
            cout << "0\n";
        }
    }
    return 0;
}

```
