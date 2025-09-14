# 后缀数组入门（二）——Height数组与LCP

### 前言

看这篇博客前，先去了解一下**后缀数组**的基本操作吧：[后缀数组入门（一）——后缀排序](https://www.cnblogs.com/chenxiaoran666/p/SuffixSort.html)。

这篇博客的内容，主要建立于**后缀排序**的基础之上，进一步研究一个$Height$数组以及如何求$LCP$。

### 什么是$LCP$

$LCP$，即$Longest\ Common\ Prefix$，是**最长公共前缀**的意思。

而在后缀数组中，$LCP(i,j)$表示**后缀$_{SA_i}$与后缀$_{SA_j}$的最长公共前缀的长度**，注意是$SA_i$和$SA_j$，而不是$i$和$j$。

### $LCP$的性质

先是几个比较简单的基本性质：

-   $LCP(i,j)=LCP(j,i)$
    
    这应该是比较显然的。
    
-   $LCP(i,i)=n-SA_i+1$
    
    这个性质非常重要，因为在求$LCP$的过程中要特判该情况，~~不然会死得特别惨~~。
    

接下来，是一些比较复杂的性质：

-   $LCP(i,j)=min(LCP(i,k),LCP(j,k))$（对于任意$1\le i\le k\le j$）
    
    首先，设$x=min(LCP(i,k),LCP(j,k))$，则可得$LCP(i,k)\ge x,LCP(j,k)\ge x$。
    
    因此我们可以知道**后缀$_{SA_i}$，后缀$_{SA_j}$的前$x$个字符分别与后缀$_{SA_k}$的前$x$个字符相等**。
    
    则**后缀$_{SA_i}$，后缀$_{SA_j}$的前$x$个字符相等**，即$LCP(i,j)\ge x$。
    
    而由于后缀$_{SA_i}<$后缀$_{SA_k}<$后缀$_{SA_{j}}$，且由$x=min(LCP(i,k),LCP(j,k))$可得，$LCP(i,j)\le x$。
    
    故$LCP(i,j)=x$。
    
-   $LCP(i,j)=min_{k=i+1}^jLCP(k,k-1)$
    
    由$LCP(i,j)=min(LCP(i,k),LCP(j,k))$这个性质，我们可以把$LCP(i,j)$拆成$j-i$个部分，分别为$LCP(i+1,i),LCP(i+2,i+1),...,LCP(j,j-1)$。
    
    然后再取$min$即可。
    

这两个性质虽然看似令人匪夷所思，但仔细理解其实还是能看懂的。

这两个性质在$LCP$的求解过程中发挥着十分重要的作用。

### $Height$数组

为了方便求解$LCP$，我们需要在定义一个新的数组：$Height$数组。

$Height_i$表示的是$LCP(i,i-1)$。

因此$LCP(i,j)$的结果就是$min_{k=i+1}^jHeight_i$，这似乎可以在知道$Height$数组的情况下用$RMQ$实现$O(1)$求解。

于是关键来了：如何求出$Height$数组。

### 如何求$Height$数组

首先我们要知道一个性质$Height_{i}\ge Height_{i-1}-1$。

这个性质我也不会证，~~反正它还是挺简单的，背一下就好了~~。

这样一来，我们每次可以把$Height_{i}$初始化为$Height_{i-1}-1$，然后每次尽量向外延长即可，这一过程似乎与$Manacher$算法有点类似。

### 代码

放一份求$Height$数组及$LCP$的模板代码：

```cpp
class Class_SuffixArray
{
    private:
        int n,SA[N+5],Height[N+5],rk[N+5],pos[N+5],tot[N+5];
        inline void RadixSort(int S)//基数排序
        {
            register int i;
            for(i=0;i<=S;++i) tot[i]=0;
            for(i=1;i<=n;++i) ++tot[rk[i]];
            for(i=1;i<=S;++i) tot[i]+=tot[i-1];
            for(i=n;i;--i) SA[tot[rk[pos[i]]]--]=pos[i];
        }
        inline void GetSA(char *s)//后缀排序，求SA数组
        {
            register int i,k,Size=122,cnt=0;
            for(i=1;i<=n;++i) rk[pos[i]=i]=s[i-1];
            for(RadixSort(Size),k=1;cnt<n;k<<=1)
            {
                for(Size=cnt,cnt=0,i=1;i<=k;++i) pos[++cnt]=n-k+i;
                for(i=1;i<=n;++i) SA[i]>k&&(pos[++cnt]=SA[i]-k);
                for(RadixSort(Size),i=1;i<=n;++i) pos[i]=rk[i];
                for(rk[SA[1]]=cnt=1,i=2;i<=n;++i) rk[SA[i]]=(pos[SA[i-1]]^pos[SA[i]]||pos[SA[i-1]+k]^pos[SA[i]+k])?++cnt:cnt;
            }
        }
        inline void GetHeight(char *s)//求Height数组
        {
            register int i,j,k=0;
            for(i=1;i<=n;++i) rk[SA[i]]=i;//更新rk数组
            for(i=1;i<=n;++i)
            {
                if(k&&--k,!(rk[i]^1)) continue;//对于rk[i]=1的情况直接跳过
                j=SA[rk[i]-1];//找到上一个后缀的坐标
                while(i+k<=n&&j+k<=n&&!(s[i+k-1]^s[j+k-1])) ++k;//尽量拓展
                Height[rk[i]]=k;//存值
            }
        }
        class Class_RMQ//RMQ求区间最值
        {
            private:
                #define LogN 15
                int Log2[N+5],Min[N+5][LogN+5];
            public:
                inline void Init(int len,int *data)
                {
                    register int i,j;
                    for(i=2;i<=len;++i) Log2[i]=Log2[i>>1]+1;
                    for(i=1;i<=len;++i) Min[i][0]=data[i];
                    for(j=1;(1<<j-1)<=len;++j) for(i=1;i+(1<<j-1)<=len;++i) Min[i][j]=min(Min[i][j-1],Min[i+(1<<j-1)][j-1]);
                }
                inline int GetMin(int l,int r) {register int k=Log2[r-l+1];return min(Min[l][k],Min[r-(1<<k)+1][k]);}
        }RMQ;
    public:
        inline void Init(int len,char *s) {n=len,GetSA(s),GetHeight(s),RMQ.Init(n,Height);}//初始化
        inline int LCP(int x,int y) {return x^y?(rk[x]>rk[y]&&swap(x,y),RMQ.GetMin(rk[x]+1,rk[y])):n-x+1;}//求LCP，注意特判x=y的情况
};
```