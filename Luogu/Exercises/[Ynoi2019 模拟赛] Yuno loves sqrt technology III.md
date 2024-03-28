摘要：分块，众数

[传送门：https://www.luogu.com.cn/problem/P5048](https://www.luogu.com.cn/problem/P5048)

## 题意

给定长度为 $n$ 的序列 $a$，$m$ 次询问区间 $[l, r]$ 众数出现的次数，强制在线。

## 分析思路

类似 [P4168 [Violet] 蒲公英](https://www.luogu.com.cn/article/mtr91s0c) 一题，我们可以在 $O\left(n \sqrt n\right)$ 的时间复杂度内求解出第 $i$ 块到第 $j$ 块之间众数出现的次数 $mode_{i, j}$。

由于本题空间限制较为严格，所以需要控制空间复杂度为 $O\left(n\right)$。

具体实现，我们可以用 `std::vector<int> pos[N]`，其中 $pos[i]$ 表示所有值为 $i$ 的位置的集合。用 $pos2[i]$ 表示第 $i$ 个元素在 $pos[a[i]]$ 中的位置。

注意到处理出来整块众数之后最多答案会增加 $2\sqrt n$，暴力统计即可，具体方法如下：

- 对于左边的边角元素 $x$ ，我们在相应的vector里找到下标为 $pos2_x + ans $的元素 $y$ ，若 $y \leq r$，则说明该数值在范围内有至少 $ans + 1$ 个数，暴力 ++ans 即可。
- 对于右边的边角元素 $x$ ，我们在相应的vector里找到下标为 $pos2_x - ans $的元素 $y$ ，若 $y \geq l$，则说明该数值在范围内有至少 $ans + 1$ 个数，暴力 ++ans 即可。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 500001, M = 708;
namespace fastIO {
    char c, f, e = 0;
    namespace usr {
        template <class _Tp>
        inline int read(_Tp &x) {
            x = f = 0, c = getchar();
            while (!isdigit(c) && !e) f = c == '-', e |= c == EOF, c = getchar();
            while (isdigit(c) && !e) x = (x << 1) + (x << 3) + (c ^ 48), c = getchar();
            return (e |= c == EOF) ? 0 : ((f ? x = -x : 0), 1);
        }
        template <class _Tp>
        inline void write(_Tp x) {
            if (x < 0) putchar('-'), x = -x;
            if (x > 9) write(x / 10);
            putchar((x % 10) ^ 48);
        }
        template <typename T, typename... V>
        inline void read(T &t, V &...v) { read(t), read(v...); }
        template <typename T, typename... V>
        inline void write(T t, V... v) {
            write(t), putchar(' '), write(v...);
        }
    }
}
using namespace fastIO::usr;
vector<int> pos[N]; // pos[i]: 有哪些下标的值为 i
int B, x, a[N], bel[N], cnt[N];
int n, m, l, r, mode[M][M], pos2[N]; // pos2[i]: a[i] 在 pos[a[i]] 中的下标
inline int query(int l, int r) {
    int now = 0;
    if (bel[l] + 1 >= bel[r]) {
        for (int i = l; i <= r; i++) cnt[a[i]] = 0;
        for (int i = l; i <= r; i++) {
            now = max(now, ++cnt[a[i]]);
        }
        return now;
    }
    const int L = bel[l], R = bel[r];
    now = mode[bel[l] + 1][bel[r] - 1];
    for (int i = l; bel[i] == L; i++) {
        int p = pos2[i];
        while (p + now < pos[a[i]].size() &&
               pos[a[i]][p + now] <= r) {
            ++now;
        }
    }
    for (int i = r; bel[i] == R; i--) {
        int p = pos2[i];
        while (p - now >= 0 && pos[a[i]][p - now] >= l) {
            ++now;
        }
    }
    return now;
}
int main(int argc, char const *argv[]) {
    read(n, m), B = sqrt(n);
    for (int i = 1; i <= n; i++) {
        read(a[i]), bel[i] = (i - 1) / B + 1;
        pos2[i] = pos[a[i]].size(), pos[a[i]].push_back(i);
    }
    int now = 0;
    for (int i = 1; i <= bel[n]; i++) {
        memset(cnt, 0, sizeof(cnt)), now = 0;
        for (int j = (i - 1) * B + 1; j <= n; j++) {
            mode[i][bel[j]] = now = max(++cnt[a[j]], now);
        }
    }
    while (m--) {
        read(l, r), l ^= x, r ^= x;
        write(x = query(l, r)), puts("");
    }
    return 0;
}

```