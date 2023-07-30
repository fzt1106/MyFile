# 状压DP

## 思路

将某一**状态**压缩为**数字**，通常状压DP用于表格（棋盘）问题，而每一个格子有*两种状态：0与1*，因此对于一行的**所有状态**，可以用**二进制枚举**暴力枚举出，其**状态**压缩为二进制对于的**十进制数**。

所以说，状压DP**非常暴力**，需要枚举$2^n-1$种状态，==有时可以通过预处理或离散化去掉不合法状态来降低复杂度==

状压DP需要通常需要通过[位运算](E:\Software_Files\Typora\Files\ACM笔记\算法\数学\位运算\位运算)来判断两状态是否兼容。

==状态x的所有子集为`for(int i=x;i!=0;i=(i-1)&x)`==

## 例题

[P2704 [NOI2001] 炮兵阵地](https://www.luogu.com.cn/problem/P2704)

1. dfs预处理去除不合法状态
2. 若在dp数组中存状态对于的十进制数，会溢出，于是通过对所有状态离散化以使数组不会溢出
3. 利用动态规划，当前状态由上一状态而来，但是前提要考虑两状态的**兼容性**

```c++
#include<bits/stdc++.h>
#define ll long long
using namespace std;

int n,m;
int a[105][15];
struct node{
    int num,val;
};
vector<node>v;
int b[105];
int dp[105][105][105];

void dfs(int cur,int num,int ans){
    if(cur>=m){
        v.push_back(node{num,ans});
        return;
    }
    dfs(cur+1,num,ans);
    dfs(cur+3,num+1,ans+(1<<cur));
}

void solve(){
    cin>>n>>m;
    dfs(0,0,0);
    // for(int i=0;i<v.size();i++)cout<<v[i].val<<' '<<v[i].num<<endl;
    for(int i=1;i<=n;i++){
        int temp=0;
        for(int j=1;j<=m;j++){
            char x;
            cin>>x;
            if(x=='H')temp+=(1<<(m-j));
        }
        b[i]=temp;
    }
    // for(int i=1;i<=n;i++)cout<<b[i]<<endl;
    for(int i=0;i<v.size();i++){
        if(v[i].val&b[1])continue;
        dp[1][i][0]=v[i].num;
        // cout<<v[i].val<<' '<<v[i].num<<endl;
    }
    for(int i=0;i<v.size();i++){
        if(v[i].val&b[2])continue;
        for(int j=0;j<v.size();j++){
            if(v[j].val&b[1]||v[j].val&v[i].val)continue;
            dp[2][i][j]=max(dp[2][i][j],dp[1][j][0]+v[i].num);
        }
    }
    for(int i=3;i<=n;i++){
        for(int cur=0;cur<v.size();cur++){
            if(v[cur].val&b[i])continue;
            for(int pre1=0;pre1<v.size();pre1++){
                if(v[pre1].val&b[i-1]||v[pre1].val&v[cur].val)continue;
                for(int pre2=0;pre2<v.size();pre2++){
                    if(v[pre2].val&b[i-2]||v[pre2].val&v[pre1].val||v[pre2].val&v[cur].val)continue;
                    dp[i][cur][pre1]=max(dp[i][cur][pre1],dp[i-1][pre1][pre2]+v[cur].num);
                }
            }
        }
    }
    int mx=0;
    for(int cur=0;cur<v.size();cur++){
        for(int pre=0;pre<v.size();pre++){
            mx=max(mx,dp[n][cur][pre]);
        }
    }
    cout<<mx<<'\n';
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    solve();
    return 0;
}

```

## 状压DP解决哈密顿通路（回路）问题

> 例题：
>
> [最短Hamilton路径](https://www.acwing.com/problem/content/description/93/)
>
> [E. Routing](https://codeforces.com/contest/1804/problem/E)

`dp[i][j]：经过状态i，到达终点j`

### 求哈密顿通路：

```c++
const ll maxn=100;
ll n;
ll a[maxn][maxn];
ll dp[1<<20][25];

void solve(){
    cin>>n;
    for(ll i=0;i<n;i++){
        for(ll j=0;j<n;j++){
            cin>>a[i][j];
        }
    }
    memset(dp,0x3f,sizeof(dp));
    for(ll i=0;i<n;i++)dp[1<<i][i]=0;
    for(ll i=0;i<(1<<n);i++){//状态
        for(ll j=0;j<n;j++){//终点
            if(i&(1<<j)){
                for(ll k=__builtin_ctz(i)+1;k<n;k++){//规定起点为__builtin_ctz(i)，继续在j后面加点k（当起点没有要求时可以去掉）
                    if(i&(1<<k))continue;
                    dp[i+(1<<k)][k]=min(dp[i+(1<<k)][k],dp[i][j]+a[j][k]);
                }
            }
        }
    }
    cout<<dp[(1<<n)-1][n-1]<<endl;//此处求起点0到终点n-1的最短哈密顿路径
}
```

`dp[i][j]：经过状态i中所有点，可以从以__builtin_ctz(i)为起点，到达以j为终点`

### 求哈密顿回路：

```c++
void solve(){
    cin>>n>>m;
    for(int i=1;i<=m;i++){
        int x,y;
        cin>>x>>y;
        x--,y--;//从0开始
        g[x][y]=1;
        g[y][x]=1;
    }
    
    //dp[i][j]：，经过状态i中所有点，可以从以__builtin_ctz(i)为起点，到达以j为终点
    memset(dp,-1,sizeof(dp));
    for(int i=0;i<n;i++)dp[1<<i][i]=0;
    for(int i=0;i<(1<<n);i++){//状态i
        for(int j=0;j<n;j++){//终点j
            if(i&(1<<j)){
                for(int k=__builtin_ctz(i)+1;k<n;k++){//从起点__builtin_ctz(i)后开始加入点
                    if(i&(1<<k))continue;
                    if(dp[i][j]!=-1&&g[j][k])dp[i+(1<<k)][k]=j;//k的前一个点为j
                }
            }
        }
    }
    for(int i=0;i<(1<<n);i++){
        //判断状态i是否可以成为环，及起点__builtin_ctz(i)与终点j可以直接相连
        for(int j=0;j<n;j++){
            if((i&(1<<j))&&g[j][__builtin_ctz(i)]&&dp[i][j]!=-1){
                cout<<"Yes"<<endl;
                //路径还原
                int st=__builtin_ctz(i),end=j,now=i;
                while(st!=end){
                    int temp=dp[now][end];
                    v[temp]=end;
                    now-=(1<<end);
                    end=temp;
                }
                v[j]=st;
                for(int i=0;i<n;i++)cout<<v[i]+1<<' ';
                cout<<endl;
                return;
            }
        }
    }
    cout<<"No"<<endl;
}
```

