# [Out of Control](https://acm.hdu.edu.cn/showproblem.php?pid=7304)

## Information

> + Resource：[2023“钉耙编程”中国大学生算法设计超级联赛（3）](https://acm.hdu.edu.cn/showproblem.php?pid=7304)
> + Degree of difficulty：Mid
> + Tags：前缀和优化DP（模板）

## 题意

> ###### Descripition
>
> There is a cloud service API to help user store history timestamps. The data structure for each user is initially an empty stack. Every time you post a request to the API with an integer $x$, denoting the current timestamp, the service will append $x$ to the end of the stack. When $x$ is less than the previous timestamp stored in the stack, the service will think the input is wrong, and will append the previous timestamp instead of $x$.
>
> You have posted the API for $n$ times, your request timestamp is $x_i$ in the $i$\-th call. However, the network is out of control. The service may receive your requests in any arbitrary order, and may even miss some requests. Knowing this issue, you are asking for the on-call engineer to have a look at your stack in the database. Assume the service received exactly $k$ requests, how many possible distinct stacks will it be?
>
> ###### Input
>
> The first line contains a single integer $T$ ($1 \leq T \leq 100$), the number of test cases. For each test case:
>
> The first line of the input contains a single integer $n$ ($1 \leq n \leq 3\,000$), denoting the total number of requests.
>
> The second line contains $n$ integers $x_1,x_2,\dots,x_n$ ($1\leq x_i\leq 10^9$), denoting the timestamp of each request.
>
> It is guaranteed that the sum of all $n$ is at most $30\,000$.
>
> ###### Output
>
> For each test case, output $n$ lines, the $i$\-th ($1\leq i\leq n$) of which containing an integer, denoting the number of distinct stacks when $k=i$. Note that the answer may be extremely large, so please print it modulo $10^9+7$ instead.

> 给你$n$个数$a_1,a_2,..,a_n$，从中选出$k$个数，进行以下操作：
>
> + 有一个栈stack，依次将$k$个数放入其中
> + 若放入的数$x$大于等于栈顶元素$y$，那么$x$正常被放入栈中
> + 若放入的数$x$小于栈顶元素$y$，那么$x$将被$y$替代放入栈中
>
> 问对于不同的$k=1,2,...,n$，由多少种类型的栈？

## 思路

构造如下动态规划：
$$
dp[i][j]:\text{放入的i个数，最后一个数为x[j]的方案数}\\
\text{状态转移方程}:dp[i][j]=dp[i-1][1]+dp[i-1][2]+...+dp[i-1][j]=\sum_{k=1}^{j}dp[i-1][k]
$$
利用==前缀和优化==可以以$O(1)$实现状态转移

## 代码

复杂度$O(n^2)$

```c++
#include<bits/stdc++.h>
#define endl '\n'
#define int long long
using namespace std;

const int mod=1e9+7;
const int N=1e4+5;
int n;
int a[N];

// @ dp[i][j]：选了i个数，以a[j]为结尾的方案数（只记录第一次以j结尾的情况）
// @ 状态转移：dp[i][j]=dp[i-1][1]+dp[i-1][2]+dp[i-1][3]+...+dp[i-1][j]=sum[i-1][j]
int dp[N][N]; 
// @ sum[i][j]：选了i个数，以a[1]...a[j]为结尾的方案数之和
int sum[N][N];


void solve(){
    cin>>n;
    for(int i=1;i<=n;i++)cin>>a[i];
    sort(a+1,a+1+n);
    for(int i=0;i<=n;i++){
        for(int j=0;j<=n;j++){
            dp[i][j]=0;
            sum[i][j]=0;
        }
    }

    //初始化
    dp[0][0]=1;
    for(int i=1;i<=n;i++)sum[0][i]=1;
    for(int i=1;i<=n;i++){
        for(int j=i;j<=n;j++){
            if(a[j]==a[j-1]&&j-1>=i)dp[i][j]==0;//只记录第一次以a[j]结尾的方案数
            else dp[i][j]=(dp[i][j]+sum[i-1][j])%mod;
        }
        for(int j=1;j<=n;j++)sum[i][j]=(sum[i][j-1]+dp[i][j])%mod;
        cout<<sum[i][n]%mod<<endl;
    }
}

signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    int t;
    t=1;
    cin>>t;
    while(t--){
        solve();
    }
}
```

