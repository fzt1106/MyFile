# Fence Job

## Information

> + Resource：[2020-2021 ICPC Southeastern European Regional Programming Contest (SEERC 2020：F)](https://codeforces.com/gym/103102)
> + Degree of difficulty：Hard- -
> + Tags：前缀和优化DP

## 题意

> ###### Descripition
>
> Fred the Farmer wants to redesign the fence of his house. Fred's fence is composed of $n$ vertical wooden planks of various heights. The $i$\-th plank's height is $h_i$ ($1 \leq h_i \leq n$). Initially, all heights are distinct.
>
> In order to redesign the fence, Fred will choose some contiguous segment $[l...r]$ of planks and "level" them, by cutting them in order to make all heights equal to the minimum height on that segment. More specifically, the new heights of the segment become $h'_{i} = \min \{ h_l, h_{l+1}, ..., h_r \}$ for all $l \leq i \leq r$.
>
> How many different designs can Fred obtain by applying this procedure several (possibly 0) times? Since the answer may be huge, you are required to output it modulo $10^9+7$.
>
> Two designs $A$ and $B$ are different if there is some plank that has a different height in $A$ than in $B$.
>
> ###### Input
>
> The first line of the input contains $n$ ($1 \leq n \leq 3\,000$), the number of planks of Fred's fence.
>
> The second line contains $n$ distinct integers $h_i$ ($1 \leq h_i \leq n, 1 \leq i \leq n$), the heights of each of the planks.
>
> ###### Output
>
> Output a single integer, the number of different possible fence designs that can be obtained, modulo $10^9+7$.

> 由$n$个高度不同的木板组成的栅栏，第$i$个木板高度为$h_i$，你可以下操作任意次：
>
> + 选择一个区间$[l,r]$
> + 将区间$[l,r]$的栅栏的高度都变为其中的最小值，即$h'_{i} = \min \{ h_l, h_{l+1}, ..., h_r \}$
>
> 问最终可能形成多少种不同的栅栏

## 思路

首先，可以观察到对于第$i$个栅栏，高度为$h_i$，其可以出现的位置区间为$[l,r]$则：

+ 其中$l-1$是$i$左边第1个高度小于$h_i$的木板
+ 其中$r+1$是$i$右边第1个高度小于$h_i$的木板

所以，结果序列一定是：**由若干数字组成的子序列（数字可以重复），并且数字的出现顺序是与给定的顺序相同**

构造如下动态规划：
$$
dp[i][j]:\text{在前i个位置中，最后一个位置为h[j]（即position[i]=h[j]）的方案数} \\
\text{状态转移方程}:
dp[i][j]=\begin{cases}\sum_{k=1}^{k=j}dp[i-1][k] & l[j]\leq i \leq r[j]\\
0,& i<l[j]或i>r[j]\end{cases}
$$


其中$l[k],r[k]$即为$k$可以出现的位置区间，下列为状态方程的解释

+ **注意不是前i个数，而是前i个位置**。~~若选择第一维状态为前i个数时，会有缺失的状态~~
+ 若$i<l[j]或i>r[j]$，那么$i$所在的位置$position[i]$不在$j$所能出现的位置区间，那么$position[i]\ne h[j]$，所以$dp[i][j]=0$
+ 若$l[j] \leq i \leq r[j]$，那么$i$所在的位置$position[i]$能出现$j$。又由于**结果序列数字出现的顺序是与给定输入顺序相同的**，所以$dp[i][j]$只能由$dp[i-1][j之前]$的状态转移而来，而不能从$dp[i-1][j之后]$的状态转移而来
+ 若暴力进行转移，那么需要$O(n)$，可以利用==前缀和来记录$i-1$状态的方案总数，利用前缀和来进行$O(1)$转移==

## 代码

复杂度$O(n^2)$

```c++
#include<bits/stdc++.h>
#define endl '\n'
#define int long long
using namespace std;

const int mod=1e9+7;
const int inf=0x3f3f3f3f3f3f3f3f;
const int N=3e3+5;
int n;
int h[N];
int dp[N][N];
int sum[N][N];
int l[N],r[N];


void solve(){
    cin>>n;
    for(int i=1;i<=n;i++)cin>>h[i];
    h[n+1]=-inf,h[0]=-1;
    for(int i=1;i<=n;i++){
        for(int j=i;j>=0;j--){
            if(h[j]<h[i]){
                l[i]=j+1;
                break;
            }
        }
        for(int j=i;j<=n+1;j++){
            if(h[j]<h[i]){
                r[i]=j-1;
                break;
            }
        }
    }
    dp[0][0]=1;
    for(int i=1;i<=n;i++)sum[0][i]=1;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            if(l[j]<=i&&i<=r[j])dp[i][j]=(dp[i][j]+sum[i-1][j])%mod;
        }
        for(int j=1;j<=n;j++)sum[i][j]=(sum[i][j-1]+dp[i][j])%mod;
    }

    int ans=0;
    for(int i=1;i<=n;i++)ans=(ans+dp[n][i])%mod;
    cout<<ans%mod<<endl;
    
    
}


signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    solve();
}

```

