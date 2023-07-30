# [Problem - C - Ntarsis' Set - Codeforces](https://codeforces.com/contest/1853/problem/C)

## Information

> + Resource：[Codeforces Round 887 (Div. 2) - Problem - C - Ntarsis' Set- Codeforces](https://codeforces.com/contest/1853/problem/C)
> + Degree of difficulty：Mid++
> + Tags：数学、二分（非完全单调）

## 题意

> ###### Description
>
> Ntarsis has been given a set $S$, initially containing integers $1, 2, 3, \ldots, 10^{1000}$ in sorted order. Every day, he will remove the $a_1$\-th, $a_2$\-th, $\ldots$, $a_n$\-th smallest numbers in $S$ simultaneously.
>
> What is the smallest element in $S$ after $k$ days?
>
> ###### Input
>
> Each test contains multiple test cases. The first line contains the number of test cases $t$ ($1 \le t \le 10^5$). The description of the test cases follows.
>
> The first line of each test case consists of two integers $n$ and $k$ ($1 \leq n,k \leq 2 \cdot 10^5$) — the length of $a$ and the number of days.
>
> The following line of each test case consists of $n$ integers $a_1, a_2, \ldots, a_n$ ($1 \leq a_i \leq 10^9$) — the elements of array $a$.
>
> It is guaranteed that:
>
> -   The sum of $n$ over all test cases won't exceed $2 \cdot 10^5$;
> -   The sum of $k$ over all test cases won't exceed $2 \cdot 10^5$;
> -   $a_1 < a_2 < \cdots < a_n$ for all test cases.
>
> ###### Output
>
> For each test case, print an integer that is the smallest element in $S$ after $k$ days.
>
> ###### Note
>
> For the first test case, each day the $1$\-st, $2$\-nd, $4$\-th, $5$\-th, and $6$\-th smallest elements need to be removed from $S$. So after the first day, $S$ will become $\{\cancel 1, \cancel 2, 3, \cancel 4, \cancel 5, \cancel 6, 7, 8, 9, \ldots\} = \{3, 7, 8, 9, \ldots\}$. The smallest element is $3$.
>
> For the second case, each day the $1$\-st, $3$\-rd, $5$\-th, $6$\-th and $7$\-th smallest elements need to be removed from $S$. $S$ will be changed as follows:
>
> <table><tbody><tr><td>Day</td><td><span></span><span id="MathJax-Element-44-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>S</mi></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mi>S</mi></math></span></span> before</td><td></td><td><span></span><span id="MathJax-Element-45-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>S</mi></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mi>S</mi></math></span></span> after</td></tr><tr><td>1</td><td><span></span><span id="MathJax-Element-46-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>{</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>1</mn></menclose><mo>,</mo><mn>2</mn><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>3</mn></menclose><mo>,</mo><mn>4</mn><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>5</mn></menclose><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>6</mn></menclose><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>7</mn></menclose><mo>,</mo><mn>8</mn><mo>,</mo><mn>9</mn><mo>,</mo><mn>10</mn><mo>,</mo><mo>&amp;#x2026;</mo><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>}</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo fence="false" stretchy="false">{</mo><menclose notation="updiagonalstrike"><mn>1</mn></menclose><mo>,</mo><mn>2</mn><mo>,</mo><menclose notation="updiagonalstrike"><mn>3</mn></menclose><mo>,</mo><mn>4</mn><mo>,</mo><menclose notation="updiagonalstrike"><mn>5</mn></menclose><mo>,</mo><menclose notation="updiagonalstrike"><mn>6</mn></menclose><mo>,</mo><menclose notation="updiagonalstrike"><mn>7</mn></menclose><mo>,</mo><mn>8</mn><mo>,</mo><mn>9</mn><mo>,</mo><mn>10</mn><mo>,</mo><mo>…</mo><mo fence="false" stretchy="false">}</mo></math></span></span></td><td><span></span><span id="MathJax-Element-47-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo stretchy=&quot;false&quot;>&amp;#x2192;</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo stretchy="false">→</mo></math></span></span></td><td><span></span><span id="MathJax-Element-48-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>{</mo><mn>2</mn><mo>,</mo><mn>4</mn><mo>,</mo><mn>8</mn><mo>,</mo><mn>9</mn><mo>,</mo><mn>10</mn><mo>,</mo><mo>&amp;#x2026;</mo><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>}</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo fence="false" stretchy="false">{</mo><mn>2</mn><mo>,</mo><mn>4</mn><mo>,</mo><mn>8</mn><mo>,</mo><mn>9</mn><mo>,</mo><mn>10</mn><mo>,</mo><mo>…</mo><mo fence="false" stretchy="false">}</mo></math></span></span></td></tr><tr><td>2</td><td><span></span><span id="MathJax-Element-49-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>{</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>2</mn></menclose><mo>,</mo><mn>4</mn><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>8</mn></menclose><mo>,</mo><mn>9</mn><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>10</mn></menclose><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>11</mn></menclose><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>12</mn></menclose><mo>,</mo><mn>13</mn><mo>,</mo><mn>14</mn><mo>,</mo><mn>15</mn><mo>,</mo><mo>&amp;#x2026;</mo><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>}</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo fence="false" stretchy="false">{</mo><menclose notation="updiagonalstrike"><mn>2</mn></menclose><mo>,</mo><mn>4</mn><mo>,</mo><menclose notation="updiagonalstrike"><mn>8</mn></menclose><mo>,</mo><mn>9</mn><mo>,</mo><menclose notation="updiagonalstrike"><mn>10</mn></menclose><mo>,</mo><menclose notation="updiagonalstrike"><mn>11</mn></menclose><mo>,</mo><menclose notation="updiagonalstrike"><mn>12</mn></menclose><mo>,</mo><mn>13</mn><mo>,</mo><mn>14</mn><mo>,</mo><mn>15</mn><mo>,</mo><mo>…</mo><mo fence="false" stretchy="false">}</mo></math></span></span></td><td><span></span><span id="MathJax-Element-50-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo stretchy=&quot;false&quot;>&amp;#x2192;</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo stretchy="false">→</mo></math></span></span></td><td><span></span><span id="MathJax-Element-51-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>{</mo><mn>4</mn><mo>,</mo><mn>9</mn><mo>,</mo><mn>13</mn><mo>,</mo><mn>14</mn><mo>,</mo><mn>15</mn><mo>,</mo><mo>&amp;#x2026;</mo><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>}</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo fence="false" stretchy="false">{</mo><mn>4</mn><mo>,</mo><mn>9</mn><mo>,</mo><mn>13</mn><mo>,</mo><mn>14</mn><mo>,</mo><mn>15</mn><mo>,</mo><mo>…</mo><mo fence="false" stretchy="false">}</mo></math></span></span></td></tr><tr><td>3</td><td><span></span><span id="MathJax-Element-52-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>{</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>4</mn></menclose><mo>,</mo><mn>9</mn><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>13</mn></menclose><mo>,</mo><mn>14</mn><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>15</mn></menclose><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>16</mn></menclose><mo>,</mo><menclose notation=&quot;updiagonalstrike&quot;><mn>17</mn></menclose><mo>,</mo><mn>18</mn><mo>,</mo><mn>19</mn><mo>,</mo><mn>20</mn><mo>,</mo><mo>&amp;#x2026;</mo><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>}</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo fence="false" stretchy="false">{</mo><menclose notation="updiagonalstrike"><mn>4</mn></menclose><mo>,</mo><mn>9</mn><mo>,</mo><menclose notation="updiagonalstrike"><mn>13</mn></menclose><mo>,</mo><mn>14</mn><mo>,</mo><menclose notation="updiagonalstrike"><mn>15</mn></menclose><mo>,</mo><menclose notation="updiagonalstrike"><mn>16</mn></menclose><mo>,</mo><menclose notation="updiagonalstrike"><mn>17</mn></menclose><mo>,</mo><mn>18</mn><mo>,</mo><mn>19</mn><mo>,</mo><mn>20</mn><mo>,</mo><mo>…</mo><mo fence="false" stretchy="false">}</mo></math></span></span></td><td><span></span><span id="MathJax-Element-53-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo stretchy=&quot;false&quot;>&amp;#x2192;</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo stretchy="false">→</mo></math></span></span></td><td><span></span><span id="MathJax-Element-54-Frame" tabindex="0" data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>{</mo><mn>9</mn><mo>,</mo><mn>14</mn><mo>,</mo><mn>18</mn><mo>,</mo><mn>19</mn><mo>,</mo><mn>20</mn><mo>,</mo><mo>&amp;#x2026;</mo><mo fence=&quot;false&quot; stretchy=&quot;false&quot;>}</mo></math>" role="presentation"><span role="presentation"><math xmlns="http://www.w3.org/1998/Math/MathML"><mo fence="false" stretchy="false">{</mo><mn>9</mn><mo>,</mo><mn>14</mn><mo>,</mo><mn>18</mn><mo>,</mo><mn>19</mn><mo>,</mo><mn>20</mn><mo>,</mo><mo>…</mo><mo fence="false" stretchy="false">}</mo></math></span></span></td></tr></tbody></table>
>
> The smallest element left after $k = 3$ days is $9$.

> 对于$1,2,3,...,100^{10}$的数列，给你$n$个数$a_1,a_2...,a_n$，你可以操作$k$次，每一次将第$a_1,a_2,...,a_n$的数删除，问最后剩下的数中最小的是多少？

## 思路

### 二分

可以发现对于$a_i\to a_{i+1}$之间的数$x$，操作一次其坐标就会变成$x-i$。

对于**最后剩余最小的数**进行==二分搜索==，`check(int mid)`函数中判断其经过$k$操作后是否满足条件：

+ 若经过$k$次操作后结果$\leq 0$，那么mid之前的的数经过$k$次操作一定也$\leq 0$，将l置为mid+1
+ 若经过$k$次操作后结果$>0$，那么mid之后（包括mid）的数可能是答案，将r置为mid

需要在check中进行$k$次二分来找mid的位置，总时间复杂度为$O(k\log {n} \log {nk})$

### 逆向模拟

对于$a_i\to a_{i+1}$之间的数$x$，操作一次其坐标就会变成$x-i$。

对于$k$次delete操作，进行==逆向模拟==，进行$k$次insert操作：从最后（经过$k$次delete操作后）的状态开始，此时第1个位置（计为ans=1）即为最终结果。为了得到其初始位置，需要进行$k$次insert，具体方法如下：

+ 在$a_1-1,a_2-2,...,a_n-n$处添加一个0
+ 若添加的0在ans前，那么ans的位置将后移相应的数量
+ 最终经过$k$次insert操作后，ans的位置即为所求

进行$k$次insert操作，每次操作需要找ans的偏移量，复杂度为$O(k+n)$

## 代码

### 二分

复杂度：$O(k\log {n} \log {nk})$

```c++
#include<bits/stdc++.h>
#define endl '\n'
#define int long long
using namespace std;

const int mod=1e9+7;
const int inf=0x3f3f3f3f3f3f3f3f;
const int N=2e5+5;
int n,k;
int a[N];


void solve(){
    cin>>n>>k;
    for(int i=1;i<=n;i++)cin>>a[i];
    if(a[1]!=1){
        cout<<1<<endl;
        return;
    }
    auto check=[&](int x)->bool{
        for(int i=1;i<=k;i++){
            int id=lower_bound(a+1,a+1+n,x)-a;
            if(id==n+1)id=n;
            if(a[id]>x)id--;
            x-=id;
            if(x<=0)return false;
        }
        return true;
    };
    int l=0,r=1e18;
    while(l<r){
        int mid=l+r>>1;
        if(check(mid))r=mid;
        else l=mid+1;
    }
    cout<<l<<endl;
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

### 逆向模拟

复杂度：$O(k+n)$

```c++
#include<bits/stdc++.h>
#define endl '\n'
#define int long long
using namespace std;

const int mod=1e9+7;
const int inf=0x3f3f3f3f3f3f3f3f;
const int N=2e5+5;

void solve(){
    int n,k;
    cin>>n>>k;
    vector<int>a(n+1);
    for(int i=1;i<=n;i++)cin>>a[i];
    if(a[1]!=1){
        cout<<1<<"\n";
        return;
    }
    int ans=1,id=1;
    while(k--){
        while(id<=n&&ans>a[id]-id)id++;
        ans+=id-1;
    }
    cout<<ans<<endl;
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

