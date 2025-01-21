摘要：线段树

[传送门：https://www.luogu.com.cn/problem/P8969](https://www.luogu.com.cn/problem/P8969)

## 题意

维护一个长度为 $n$ 的序列，支持以下操作：

1. 区间加一个数；
2. 区间取 $\mathrm{popcount}$；
3. 单点查询值。

$n \le 3 \times 10^5, q \le 10^6, a_i, x \le 10^9$。

## 分析思路

考虑比较神秘的操作 $2$。本题中 $\mathrm{popcount}$ 是一个值域为 $[0, \log_2V]$ 的函数，因此一个线段树结点对应的区间要么只经过区间加法，要么值域只有 $O(\log V)$。

如果我们把所有的操作看作是映射，那么最终线段树的某个节点 $p$ 对应的映射 $F_p$ 一定是 $\mathrm{popcount}$ 和 $y = x + a$ 的复合，具体来说：

$$
F_p(x) = \mathrm{popcount}(\mathrm{popcount}(\dots\mathrm{popcount}(x + a) + b\dots) + c) + d
$$



## 代码

```cpp
```
