# 字符串Hash

```c++
const int base=131;//base=13331 // hash
const int N=2e6+5;
int n;
char s[N];
ull h[N],p[N];//自然溢出
//字符串hash，自然溢出
void Hash(){
    p[0]=1;
    int len=strlen(s+1);
    for(int i=1;i<=len;i++){
        h[i]=h[i-1]*base+s[i];
        p[i]=p[i-1]*base;
    }
}
//获得子串的Hash
int gethash(int l,int r){
    return h[r]-h[l-1]*p[r-l+1];
}

```

