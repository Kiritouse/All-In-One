BFS板子
```c++ ()
q.push(start);
visited[start] = 1;
while(!q.empty()){
	int nowstate = q.front();
	if(nowstate = targetstate) break;
	q.pop();
	int next[n] .....
	for(int i = 0;i<n;i++){
		if(visited[next[i]]==1||其他条件) continue;
		visited[next[i]]=1;
		q.push(next[i]);
	}
}
```