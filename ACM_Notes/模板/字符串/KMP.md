# KMP算法

```c++
char s[N],p[N];
int nx[N];
//获取kmp中的next数组 
void getnx(){
	int len=strlen(p+1);
	for(int i=2,j=0;i<=len;i++){
		while(j&&p[i]!=p[j+1])j=nx[j];
		if(p[i]==p[j+1])j++;
		nx[i]=j;
	}
}
//kmp算法 
void kmp(){
	int lens=strlen(s+1),lenp=strlen(p+1);
	for(int i=1,j=0;i<=lens;i++){
		while(j&&s[i]!=p[j+1])j=nx[j];
		if(s[i]==p[j+1])j++;
		if(j==lenp){
			//对匹配到的子串的操作 
			j=nx[j];
		}
	}
}
```

