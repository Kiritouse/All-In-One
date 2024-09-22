[题目中的每个条件都是有用的---二维转一维](题目中的每个条件都是有用的---二维转一维.md)

之前在这里做过的一道题,学会了将二维转换为一维
x\*n(行数)+y = index
但要注意实际上是会有哈希冲突的,
因为存在x,y不同,但是index一样的情况
==如果你想要用一维数组来储存坐标对,并且实现O(1)的查询,请谨慎==
例如下面这道题
[4510. 寻宝！大冒险！ - AcWing题库](https://www.acwing.com/problem/content/description/4513/)
> 其实很简单,就是每次遍历藏宝图的坐标的值,查询对应的绿化图的坐标,是否之前有存在过,如果暴力做的话,就是每次都遍历一下输入的绿化图的坐标对.
> 但是如果用二维化为一维的时候要注意了,存在hash冲突

 ```c++
#include <bits/stdc++.h>

using namespace std;

const int LMAX = 1e9+3;

const int SMAX = 53;

vector<pair<int,int>>xy;

#define NEW

#ifdef NEW

unordered_map<long long,bool>xy_map;

#endif

int tr[SMAX][SMAX] = {};

int n,L,S;

bool check(int x,int y){

    for(int i = 0;i<=S;i++){

            for(int j = 0;j<=S;j++){ //遍历藏宝图

                int flag = 0;

                if(x+i>L||y+j>L){ //如果x+i越界了,说明这个坐标xy不满足

                    return false;

                }

                if(tr[i][j]==0){ //如果藏宝图上的(i,j)是0

                //这里可以优化,我们可以直接去找看对应出有么有点

                    #ifndef NEW

                    for(auto [k,v]:xy){ //遍历最开始储存的所有的点

                        if(x+i==k&&y+j==v){//看是否存在恰好对应上的点,但是我们储存的说有点都代表为1的点,自然说明x,y不满足,直接退出

                            return false;

                        }

                    }

                    #else

                    long long index = (long long)(x+i)*(long long)L+(long long)(y+j);//二维化为一维

                    if(xy_map.count(index)>0){  //如果恰好发现有那么一个点,直接就说明不满足,因为我们储存的是为1的点

                        return false;

                    }

                    #endif

                }

                else{ //藏宝图为1

                    #ifndef NEW

                    for(auto [k,v]:xy){

                        if((x+i==k&&y+j==v)){ //如果藏宝图为1,能找到原图为1,说明当前这个(i,j)满足,那么就不必寻找了,直接跳出,遍历藏宝图的下一个点

                            flag  =1;

                            break;

                        }

                    }

                    if(!flag){ //如果找完所有的点都找不到,说明(i,j)为1的时候,在原图对应不上

                        return false;

                    }

                    #else

                    long long index = (long long)a*(long long)(L+1)+(long long)b;

                    if(xy_map.count(index)>0){ //如果藏宝图此处的点为1,原图上的点也为1,说明我们遍历的(i,j)是符合要求的,就去遍历下一个(i,j)

                        continue;

                    }

                    else{

                        return false;

                    }

                    #endif

                }

            }

        }

        return true;

}

int main(){

    cin>>n>>L>>S;

    for(int i = 0;i<n;i++){

        int a,b;

        cin>>a>>b;

        xy.push_back({a,b});

        #ifdef NEW

        //long long index = (long long)a*(long long)L+(long long)b;
		long long index = (long long)a*(long long)(L+1)+(long long)b;
        if (xy_map.count(index)) {

            cout << "冲突检测: (" << a << ", " << b << ") 转换为 " << index << " 已存在!" << endl;

        }

        xy_map[index] = true;

        #endif      

    }

    for(int i = S;i>=0;i--){

        for(int j = 0;j<=S;j++){

            cin>>tr[i][j];

        }

    }

    int ans= 0;

    for(auto [x,y]:xy){

        if(check(x,y)){

            ans++;

        }

    }

    cout<<ans<<endl;

}
```

> PS:如果想要正确地二维化一维,最好的方法是例如 x,y的取值范围是[0,L]
> 那么系数的话就用L+1