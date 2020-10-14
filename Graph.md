# Graph

## 题目背景

$handsomer$在城市里游荡，由于他很懒他决定只走最短路，同时由于他人如其名怕被打，经过一条路还需要给驻守该路的恶霸交钱……

## 题目描述

给定一个每条边有两个参数 (长度$len$，价值$value$) 的无向图，请输出$\frac{n\times(n-1)}{2}$个数表示对于每一组点对$(i,j)$，他们之间所有长度最短的路径所经过的边构成了一个子图，求该子图的价值和$(i<j)$

注意：

定义子图的价值和，为该子图所有边的价值的和

如果对于点对$(i,j)$，没有任何一条路径可以使他们相互到达，则该点对的答案为$0$

## 输入格式

第一行两个正整数$n,m$表示无向图的点数，边数

此后$m$行，每行$4$个正整数，表示一条边的两端点，长度$len$，价值$value$

## 输出格式

一行$\frac{n\times(n-1)}{2}$个数依次表示点对$(1,2),(1,3)...(1,n)$，$(2,3),(2,4)...(2,n)$，...，$(n-1,n)$的答案

## 样例输入

```
5 7
1 5 1 1
1 4 2 1
5 4 1 1
5 2 2 1
4 2 1 1
3 4 3 1
2 1 3 1
```

## 样例输出

```
6 4 3 1 2 1 3 1 2 1
```

## 样例解释

![Xnip2020-10-10_10-50-08.jpg](https://i.loli.net/2020/10/10/Ko4x6XWPQinpM1k.jpg)

$(1,2)$的最短路径有：

$1->2$

$1->5->2$

$1->4->2$

$1->5->4->2$

构成子图的边有：$(1,2),(1,5),(1,4),(5,2),(4,2),(5,4)$，价值和为$6$

$(1,3)$的最短路径有：

$1->4->3$

$1->5->4->3$

构成子图的边有：$(1,4),(1,5),(5,4),(4,3)$，价值和为$4$

## 数据范围与约定

对于$20\%$的数据，保证$2\leq n\leq 50$

对于另外$10\%$的数据，保证是一棵树

对于另外$10\%$的数据，保证$0\leq m\leq 10^3$

对于$100\%$的数据，保证$2\leq n\leq 300$，$0\leq m\leq \frac{n\times(n-1)}{2}$，$1\leq len,value\leq 10^6$

保证没有重边和自环



## 题解

如果一条一条路算的话非常不好去重，不妨一条一条边算

$30pts:$先跑一遍Floyd预处理，枚举点对和每条边，$O(1)$判断边是否在点对的最短路上，总时间复杂度$O(n^2m)$

$100pts:$考虑优化上述解法，每次就不用枚举$n^2$条边：预处理以$k$为端点，且位于$(i,k)$最短路径上的边的价值和$cnt[i][k]$，这个预处理需要枚举$i,k$并检查以$k$为以端点的边的另一端点是否在$(i,k)$的最短路上，复杂度$O(n^3)$

这样$ans[i][j]=\sum\limits_kcnt[i][k]$ ($k$位于$(i,j)$最短路上)，这样对于每个点对只需要枚举$O(n)$个点，总时间复杂度$O(n^3)$

## STD

```cpp
//O(n^3)写法
#include <iostream>
#include <cstdio>
#include <cstring>
#include <ctime>
#define LL long long
using namespace std;
const int MAX=3e2+5;
int n,m;
LL len[MAX][MAX],value[MAX][MAX];
LL dis[MAX][MAX],cnt[MAX][MAX];
inline void pre(){
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++)if(dis[i][k]!=-1){
			for(int j=i+1;j<=n;j++)if(dis[k][j]!=-1){
				if(dis[i][j]==-1 || dis[i][j]>dis[i][k]+dis[k][j])dis[j][i]=dis[i][j]=dis[i][k]+dis[k][j];
			}
		}
	}
	for(int i=1;i<=n;i++){
		for(int k=1;k<=n;k++)if(dis[i][k]!=-1){
			for(int j=1;j<=n;j++)if(j!=k && dis[i][j]!=-1 && len[j][k]!=-1 && dis[i][j]+len[j][k]==dis[i][k])cnt[i][k]+=value[j][k];
		}
	}
}
LL ans;
signed main(){
	scanf("%d %d",&n,&m);
	memset(dis,-1,sizeof(dis));
	for(int x,y,i=1;i<=m;i++)scanf("%d %d",&x,&y),scanf("%lld %lld",&dis[x][y],&value[x][y]),dis[y][x]=dis[x][y],value[y][x]=value[x][y];
	for(int i=1;i<=n;i++)dis[i][i]=0;
	memcpy(len,dis,sizeof(dis));
	pre();
	for(int i=1;i<=n;i++){
		for(int j=i+1;j<=n;j++){
			ans=0;
			for(int k=1;k<=n;k++)if(dis[i][k]+dis[k][j]==dis[i][j])ans+=cnt[i][k];
			printf("%lld ",ans);
		}
	}
	return 0;
}
```

