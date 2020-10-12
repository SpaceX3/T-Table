# T-Table
Just a Table about my mind and solution to those problems i've solved in the past few month

Hope it can be helpful

GL & HF



```
//给定一有边权的无向图 求出所有点对间最短路所经过的路径条数 多条最短路经过同一条边不会重复计数
//如果一条一条路算的话非常不好去重，不妨一个一个点算
//对于点对$(i,j)$枚举位于其最短路上的点$k$，统计以$k$为端点，且位于$(i,k)$最短路径上的边的个数$cnt[i][k]$，则$ans[i][j]=\sum\limits_kcnt[i][k]$，只要$cnt[i][k]$没有重复，最终的$dp[i][j]$也不会重复统计
//先跑一遍Floyd以方便检查$k$，预处理$cnt[i][k]$只需要枚举$i,k$并检查以$k$为以端点的边的另一端点是否在$(i,k)$的最短路上
//复杂度$O(n^3)$
#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;
const int MAX=5e2+5;
const int inf=1061109567;
int n,m;
int map[MAX][MAX];
int dis[MAX][MAX],cnt[MAX][MAX];
inline void pre(){
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++)if(dis[i][k]!=-1 && dis[k][j]!=-1){
				if(dis[i][j]==-1 || dis[i][j]>dis[i][k]+dis[k][j])dis[i][j]=dis[i][k]+dis[k][j];
			}
		}
	}
	for(int i=1;i<=n;i++){
		for(int k=1;k<=n;k++)if(dis[i][k]!=-1){
			for(int j=1;j<=n;j++)if(j!=k && dis[i][j]!=-1 && map[j][k]!=-1 && dis[i][j]+map[j][k]==dis[i][k])cnt[i][k]++;
			//cout<<cnt[i][k]<<" ";
		}
		//cout<<endl;
	}
}
int main(){
	freopen("D.in","r",stdin);
	scanf("%d %d",&n,&m);
	memset(dis,-1,sizeof(dis));
	for(int x,y,i=1;i<=m;i++)scanf("%d %d",&x,&y),scanf("%d",&dis[x][y]),dis[y][x]=dis[x][y];
	for(int i=1;i<=n;i++)dis[i][i]=0;
	memcpy(map,dis,sizeof(dis));
	pre();
	for(int i=1;i<=n;i++){
		for(int ans,j=i+1;j<=n;j++){
			ans=0;
			for(int k=1;k<=n;k++)if(dis[i][k]+dis[k][j]==dis[i][j])ans+=cnt[i][k];
			printf("%d ",ans);
		}
	}
	return 0;
}
```

