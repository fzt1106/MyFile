# bitset优化

[bitset优化](https://www.luogu.com.cn/blog/221955/bitset-ying-yong-zong-jie)

## bitset优化01背包

### 思路



### 例题

#### [「LibreOJ β Round #2」贪心只能过样例]     (https://loj.ac/p/515) [代码](E:\Coding_Files\VScode\OI\algorithm\STL\bitset\bitset优化\bitset优化01背包：「LibreOJ β Round #2」贪心只能过样例.cpp)

```c++
// ! bitset优化01背包
// info https://loj.ac/p/515

/*
    bug 利用DP数组用int类型定义，只能通过for循环来转移，复杂度为n^5
    @ 由于dp数组存储的0与1，可以利用bitset优化，可以将一层for循环改为左移操作
*/

#include<bits/stdc++.h>
#define int long long
using namespace std;

const int N=105;
int n;
int l[N],r[N];
bitset<N*N*N>dp[2];


signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>l[i]>>r[i];
    }
    dp[0][0]=1;
    for(int i=1;i<=n;i++){
        int now=i&1,pre=!(i&1);
        dp[now].reset();
        for(int j=l[i];j<=r[i];j++){
            dp[now]|=(dp[pre]<<(j*j));
        }
    }
    cout<<dp[n&1].count()<<endl;
}

// #include <bits/stdc++.h>
// #define int long long
// using namespace std;

// const int N = 105;
// int n;
// int l[N], r[N];
// bitset<N * N * N> dp[2];

// signed main()
// {
//     ios::sync_with_stdio(false);
//     cin.tie(0);
//     cout.tie(0);
//     cin >> n;
//     for (int i = 1; i <= n; i++)
//     {
//         cin >> l[i] >> r[i];
//     }
//     dp[0][0] = 1;
//     for (int i = 1; i <= n; i++)
//     {
//         int now = i & 1, pre = !(i & 1);
//         dp[now].reset();
//         for (int j = l[i]; j <= r[i]; j++)
//         {
//             dp[now] |= (dp[pre] << (j * j));
//         }
//     }
//     cout << dp[n & 1].count() << endl;
// }
```

## bitset优化矩阵乘法

### 思路

两个矩阵相乘的复杂度为$O(n^3)$，但如果矩阵为两个**01方阵**，那么这两个矩阵相乘可以利用**bitset优化**，

假设两个01方阵`bitset<N>a[N],bitset<N>b[N]`相乘，计算结果矩阵`int c[N][N]`的`c[i][j]`值具体方法如下：

+ 利用a的第i行与b的第j列**相与**，得到一个行向量x：`bitset<N>x=a[i]&b[j]`
+ `x.count()`即为所求：`c[i][j]=x.count()`

### 模板

==条件：矩阵为01方针==

==复杂度：$O(\frac{n^3}{w})$==

```c++
// @ a b为两个01方针，返回a*b后[i,j]的值
int MatMul(bitset<N>a[N],bitset<N>b[N],int i,int j){
    return (a[i]&b[j]).count();
}
```

### 例题

#### [foreverlasting and fried-chicken](https://acm.hdu.edu.cn/showproblem.php?pid=7293)	[代码](E:\Coding_Files\VScode\OI\algorithm\STL\bitset\bitset优化\bitset优化矩阵乘法：foreverlasting and fried-chicken.cpp)

```c++
// ! bitset优化矩阵乘法（只能优化01矩阵相乘）
// info foreverlasting and fried-chicken https://acm.hdu.edu.cn/showproblem.php?pid=7293

/*
    @ 两个矩阵相乘的复杂度为O(n^3)
    @ 但如果矩阵为01矩阵时，可以利用bitset优化：利用行列相与并用count计算1的个数即可
*/


#include<bits/stdc++.h>
#define int long long
#define endl '\n'
using namespace std;

const int N=1e3+10;
const int mod=1e9+7;
int n,m;
int a[N][N];
int d[N];

int fact[N*N+10],infact[N*N+10];

//快速幂
int fastPower(int base,int power){
	int result=1;
    base%=mod;//防止最开始base*base时溢出
	while(power>0){
		if(power&1){//指数为奇数时特判，位运算判断奇数
			result=result*base%mod;//记录下指数为奇数时的底数,并应用乘法模运算 
		}
		power>>=1;//指数减半，位运算来除2
		base=base*base%mod;//底数平方,并应用指数模运算 
	}
	return result%mod;
}

//预处理：阶乘取模，阶乘取模的逆元
void init(){
    fact[0]=infact[0]=1;
    for(int i=1;i<=N*N+5;i++){
        fact[i]=(fact[i-1]*i)%mod;//阶乘取模
        infact[i]=(infact[i-1]*fastPower(i,mod-2))%mod;//阶乘取模逆元，利用了infact[i-1]以及费马小定理
    }
}

int get_C(int x,int y){
    return fact[x]*infact[y]%mod*infact[x-y]%mod;
}


void solve(){
    cin>>n>>m;
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            a[i][j]=0;
        }
    }
    bitset<N>a1[N],a2[N];
    for(int i=1;i<=n;i++)d[i]=0;
    for(int i=1;i<=m;i++){
        int x,y;
        cin>>x>>y;
        d[x]=(d[x]+1)%mod;
        d[y]=(d[y]+1)%mod;
        a1[x][y]=true;
        a1[y][x]=true;
        a2[x][y]=true;
        a2[y][x]=true;
    }
    // @ 此处预处理出ij之间距离为2的边数，利用矩阵的2次幂求，并且矩阵为01矩阵，可以利用bitset优化
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            a[i][j]=((a1[i]&a2[j]).count())%mod;
        }
    }
    int ans=0;
    for(int u=1;u<=n;u++){
        if(d[u]>=6){
            for(int v=1;v<=n;v++){
                if(u!=v){
                    int cnt=a[u][v]%mod;
                    int x=d[u]-(a1[u][v]?1:0);
                    if(cnt>=4){
                        ans=(ans+get_C(cnt,4)%mod*get_C(x-4,2)%mod)%mod;
                    }
                }
            }
        }
    }
    cout<<ans%mod<<endl;
}
signed main(){
    ios::sync_with_stdio(false);
    cin.tie(0);cout.tie(0);
    init();
    int t;
    cin>>t;
    while(t--){
        solve();
    }
}
```

#### [Flights for Regular Customers](https://www.luogu.com.cn/problem/CF576D)

