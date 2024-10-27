# 离散的图
```c++
class DFS{
private:
	vector<vector<int edges>>;
	vector<int>visited;
public:
	void dfs(int u){
		visited[u] = 1;//代表已经访问了
		/*
		下面这部分是遍历所有与u相邻接的点
		因为我们用的是vector，所以只储存了相邻接的点
		直接访问容器里的点即可
		*/
		for(int v:edges[u]){
			if(!visited[v]){
				dfs(v);
			}
		}
	}
};

//注意使用的时候要初始化！！
 visited.resize(nodes.size(),0);
 edges.resize(nodes.size());
 //后面再插入边
如果是非二叉树
for(int i = 0;i<allnodesnum;i++){// 遍历所有节点
	if(!visited[i]){
		dfs(i);
	}
}
```

# 连续的迷宫类的图
> 对于连续的迷宫类的图，我们用dfs往往是访问四个方向
> 例如从i,j出发
> dfs(i-1,j);
> dfs(i+1,j);
> dfs(i,j-1);
> dfs(i,j+1);
> 
```c++

```

# 应用
## 检测有向图中是否有环
```c++
bool check = true;
void dfs(int u){
	visited[u]=1;
	for(int v:edges[u]){
		if(!visited[u]){
			dfs(v);
			if(!check){//如果check不通过，那么就return;说明有环存在
				return;
			}
		}
		/*
		为什么这里就可以判断有环了？
		因为我们从一个root点顺着一条链访问，回溯的时候，如果root点访问另外的点的时候
		发现被访问过了，那么就一定是上一个点的那一条链，连上了这个root点的邻接节点了。
		*/
		else if(visited[v]=1){
			check = false;
			return;
		}
	}
	visited[u]=2;
}
for(遍历图中的所有点&&!check){
	if(!visited[i]){
		dfs(i);
	}
}
return check;//返回有无环

```