# [J-Qu'est-ce Que C'est?\_2023牛客暑期多校训练营4](https://ac.nowcoder.com/acm/contest/57358/J)

## Information

> + Resource：[J-Qu'est-ce Que C'est?\_2023牛客暑期多校训练营4](https://ac.nowcoder.com/acm/contest/57358/J)
> + Degree of difficulty：Hard- -
> + Tags：前缀和优化DP

## 题意

> ###### Descripition
>
> ![image-20230728235430857](https://fzttypora.oss-cn-beijing.aliyuncs.com/typora_images/202307282354999.png)
>
> ###### Input
>
> The first line contains two positive integers n and m $(1\leq n,m\leq 5000)$, denoting the length of the report and the range of statements in the report, respectively.
>
> ###### Output
>
> Output an integer in one line, denoting the answer modulo$ 998244353$

> 你需要构造一个长度为$n$的序列，满足以下两个条件：
>
> + $-m\leq a_i \leq m \ \ \ \text{for all }\ 1\leq i \leq n$
> + $\sum_{i=l}^{r}a_i\geq 0\ \ \ \text{for all }1\leq l < r\leq n$
>
> 问可以构造多少种不同的序列？

## 思路

> 初始错误想法：
>
> $dp[i][j]:\text{选了i个数，最后一个数为j的方案数}$
>
> 但是发现状态转移会发送错误，需要维护前$i-1$个数的和才能判断其是否能从此状态转移到当前状态

其实不用维护前$i$个数的和，只需为维护**最小后缀和**即可，判断能否往序列后面增填数$x$是依据$x+最小后缀$是否大于等于0决定的。于是构造如下动态规划：
$$
dp[i][j]:\text{选了i个数，最小后缀和为j的方案数}\\
dp[i][j]=
\begin{cases}
dp[i-1][|j|]+dp[i-1][|j|+1]+...+dp[i-1][m]=\sum_{k=|j|}^m{dp[i-1][k]} & if(j\leq 0)\\
dp[i-1][j-m]+dp[i-1][j-m+1]+...+dp[i-1][m]=\sum_{k=j-m}^{m}{dp[i-1][k]}& if(j\geq 0)\\
\end{cases}
$$
可以利用==前缀和优化==实现$O(1)$的状态转移

> Notes：由于数组下标不能为负，需要对数组下标进行转换。同时**注意取模后两个前缀和相减不一定为正，需要+mod**

## 代码

复杂度$O(n^2)$

```c++
#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

const int mod=998244353;


signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    int n,m;
    cin>>n>>m;
    vector<vector<int>>dp(n+100,vector<int>(2*m+100));
    vector<int>sum(2*m+100);
    for(int j=1;j<=2*m+1;j++){
        dp[1][j]=1;
        sum[j]=(sum[j-1]+dp[1][j])%mod;
    }
    for(int i=2;i<=n;i++){
        for(int j=1;j<=2*m+1;j++){
            if(j<=m)dp[i][j]=(dp[i][j]+sum[2*m+1]-sum[2*(m+1)-j-1]+mod)%mod;
            else dp[i][j]=(dp[i][j]+sum[2*m+1]-sum[j-m-1]+mod)%mod;
        }
        for(int j=1;j<=2*m+1;j++){
            sum[j]=(sum[j-1]+dp[i][j])%mod;
        }
    }
    int ans=0;
    for(int j=1;j<=2*m+1;j++)ans=(ans+dp[n][j])%mod;
    cout<<ans%mod<<endl;
}
```

