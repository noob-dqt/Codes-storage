#### Dijstra：（单源起点V到其余顶点的最短距离）
##### 方法一：适用于稠密图的Dijstra算法，复杂度$O(N^2)$，N是节点数
```
vector<vector<pair<int,int>>>mp(105);
int n,m;
int sht[105];	//最短路径
int path[105];	//path[i]表示i的上一个点的值，path[i]=-1表示i是起点
void dijstra(int k){
    memset(sht,0x3f,sizeof sht);
    bool vis[105]={0};
    vis[k]=1;
    for(int i=1;i<=n;i++) path[i]=-1;
    for(auto &v:mp[k]){
        sht[v.second]=v.first;
        path[v.second]=k;
    }
    for(int i=1;i<=n;i++){
        int key=-1,minn=inf;
        for(int j=1;j<=n;j++){
            if(!vis[j]&&sht[j]<minn){
                key=j;
	minn=sht[j];
            }
        }
        if(key==-1) return ;
        vis[key]=1;
        for(auto &v:mp[key]){
            if(!vis[v.second]&&sht[v.second]>sht[key]+v.first){
                sht[v.second]=sht[key]+v.first;
                path[v.second]=key;
            }

        }
    }
}
```
##### 方法二：堆优化，适用于稀疏图，复杂度$O(MlogM)$，M是边的数量
```
void dijstra(int k){
    vector<vector<pair<int,int>>>mp(n+1); //n为节点数
    for(auto &x:edges) mp[x[0]].push_back({x[1],x[2]});
    vector<int>dis(n+1,0x3f3f3f3f);
    priority_queue<pair<int,int>,vector<pair<int,int>>,greater<>>q;
    dis[k]=0;
    q.push({0,k});
    while (!q.empty())
    {
        auto [dix,x]=q.top();
        q.pop();
        if(dix>dis[x]) continue;
        for(auto &[y,diy]:mp[x]){
            int tdis=dix+diy;
            if(tdis<dis[y]){
                dis[y]=tdis;
                q.push({tdis,y});
            }
        }
    }
}
```

#### Floyd（任意两点间的最短距离）
vector<vector<pair<int,int>>>mp(105);
int n,m;
int sht[105][105];
int path[105][105];		//path[i][j]的值表示以i为起点，路径中距离j最近的点
void floyd(){
    memset(sht,0x3f,sizeof sht);
    for(int i=1;i<=n;i++){
        for(auto &v:mp[i]){
            sht[i][v.second]=v.first;
            path[i][v.second]=i;
        }
    }    
    for(int k=1;k<=n;k++){
        for(int i=1;i<=n;i++){
            for(int j=1;j<=n;j++){
                if(i==j) {sht[i][i]=0;continue;}
                if(sht[i][k]+sht[k][j]<sht[i][j]){
                    sht[i][j]=sht[i][k]+sht[k][j];
                    path[i][j]=path[k][j]
                }
            }
        }
    }
}