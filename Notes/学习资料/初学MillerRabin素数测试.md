# 初学MillerRabin素数测试

### 前言

$MillerRabin$素数测试是一种很实用的素数判定方法。

它只针对单个数字进行判定，因而可以对较大的乃至于$long\ long$范围内的数进行判定，而且速度也很快，是个十分优秀的算法。

### 前置定理

-   **费马小定理**：$a^{p-1}\equiv1(mod\ p)$（详见此博客：[费马小定理](https://www.cnblogs.com/chenxiaoran666/p/Fermat_little.html)）
-   **二次探测定理**：若$p$为奇素数且$x^2\equiv1(mod\ p)$，则$x\equiv1(mod\ p)$或$x\equiv p-1(mod\ p)$。

### 大致思路

假设我们要验证$x$是否为素数，则我们应先找一个质数$p$来对其进行测试（$p$可以选取多个依次进行测试，只要有一个不满足就可以确定其不是质数）。

首先，我们先判断如果$x=p$，则$x$必为质数（因为$p$为质数）。如果$x$是$p$的倍数，则$x$必为合数。

然后，由于费马小定理，我们先测试$p^{x-1}\%x$是否等于$1$，如果不是，则它必然不是质数（这一步也叫作**费马测试**）。

否则，我们根据二次探测定理，先用一个$k$记录下$x-1$，然后只要$k$为偶数就持续操作：

-   先将$k$除以$2$，然后用一个$t$记录下$p^k\%x$的值。
-   如果$t$不等于$1$且不等于$p-1$，则根据二次探测定理，$x$非质数。
-   如果$t=p-1$，则无法继续套用二次探测定理，因此直接返回$true$。

如果一直操作到$k$为奇数仍然无法确定$x$非质数，就返回$true$。

这一过程应该还是比较容易理解的。

### 代码

```cpp
class MillerRabin//MR测试
{
    private:
        #define Pcnt 10
        Con int P[Pcnt]={2,3,5,7,11,13,19,61,2333,24251};//用于测试的质数
        I int Qpow(RI x,RI y,CI X) {RI t=1;W(y) y&1&&(t=1LL*t*x%X),x=1LL*x*x%X,y>>=1;return t;}//快速幂
        I bool Check(CI x,CI p)//测试
        {
            if(!(x%p)||Qpow(p%x,x-1,x)^1) return false;//判断x是否为p的倍数，然后费马测试
            RI k=x-1,t;W(!(k&1))//持续操作直至k为奇数
            {
                if((t=Qpow(p%x,k>>=1,x))^1&&t^(x-1)) return false;//如果p^k不是1也不是-1，说明x不是质数
                if(!(t^(x-1))) return true;//如果p^k已为-1，无法用二次探测定理，因此返回true
            }return true;
        }
    public:
        I bool IsPrime(CI x)//判断一个数是否为质数
        {
            if(x<2) return false;
            for(RI i=0;i^Pcnt;++i) {if(!(x^P[i])) return true;if(!Check(x,P[i])) return false;}//枚举质数进行测试
            return true;
        }
}MR;
```