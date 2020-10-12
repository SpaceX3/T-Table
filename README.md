# T-Table
Just a Table about my mind and solution to those problems i've solved in the past few month

Hope it can be helpful

GL & HF

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <queue>
#define int long long
using namespace std;
const int MAX=5e2+5;
const int inf=1061109567;
int n,m;
int len[MAX][MAX],value[MAX][MAX];
int dis[MAX][MAX];
inline void pre(){
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++)if(dis[i][k]!=-1 && dis[k][j]!=-1){
				if(dis[i][j]==-1 || dis[i][j]>dis[i][k]+dis[k][j])dis[i][j]=dis[i][k]+dis[k][j];
			}
		}
	}
}
struct node{
	int num,d;
}now;
signed main(){
	scanf("%lld %lld",&n,&m);
	memset(dis,-1,sizeof(dis));
	for(int x,y,i=1;i<=m;i++)scanf("%lld %lld",&x,&y),scanf("%lld",&dis[x][y]),dis[y][x]=dis[x][y],value[y][x]=value[x][y]=1;
	for(int i=1;i<=n;i++)dis[i][i]=0;
	memcpy(len,dis,sizeof(dis));
	pre();
	for(int i=1;i<=n;i++){
		for(int ans,j=i+1;j<=n;j++){
			ans=0;
			for(int k=1;k<=n;k++)if(dis[i][k]!=-1 && dis[k][j]!=-1 && dis[i][k]+dis[k][j]==dis[i][j]){
				for(int l=k+1;l<=n;l++)if(dis[i][l]!=-1 && dis[l][j]!=-1 && dis[i][l]+dis[l][j]==dis[i][j]){
					if(len[k][l]!=-1 && dis[i][k]+len[k][l]==dis[i][l])ans+=value[k][l];
				}
			}
			printf("%lld ",ans);
		}
	}
	return 0;
}
```

