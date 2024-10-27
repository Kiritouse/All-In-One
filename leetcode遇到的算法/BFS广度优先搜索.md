# 离散的图
# 连续的迷宫类的图
此题是[. - 力扣（LeetCode）](https://leetcode.cn/problems/number-of-islands/description/?envType=problem-list-v2&envId=2cktkvj)
leetcode200的岛屿问题，其中一个解法，用的BFS
```c++
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int rn = grid.size();
        if(rn==0)return 0;
        int cn = grid[0].size();
        if(cn==0)return 0;
        int ans = 0;
        for(int i = 0;i<rn;i++){
            for(int j = 0;j<cn;j++){
                if(grid[i][j]=='1'){
                    queue<pair<int,int>>q;
                    ans++;
                    q.push({i,j});
                    grid[i][j]='0'; //这里替换可以替换为visited(i,j);
                    //注意这上面这部分，什么时候叫做访问了？这里就说明了
                    //！！！push的时候就代表已经访问了，一定要搞清楚这个逻辑
                    while(!q.empty()){
                        auto rc = q.front();
                        q.pop();
                        if(rc.first-1>=0&&grid[rc.first-1][rc.second]=='1' ){
                            q.push({rc.first-1,rc.second});
                            grid[rc.first-1][rc.second] = '0';
                        }
                        if(rc.first+1<rn&&grid[rc.first+1][rc.second]=='1' ){
                            q.push({rc.first+1,rc.second});
                            grid[rc.first+1][rc.second] = '0';
                        }
                        if(rc.second-1>=0&&grid[rc.first][rc.second-1]=='1' ){
                            q.push({rc.first,rc.second0});
                            grid[rc.first][rc.second-1] = '0';
                        }
                        if(rc.second+1<cn&&grid[rc.first][rc.second+1]=='1' ){
                            q.push({rc.first,rc.second+1});
                            grid[rc.first][rc.second+1] = '0';
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```