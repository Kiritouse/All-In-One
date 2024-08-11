[4281. 序列查询新解 - AcWing题库](https://www.acwing.com/problem/content/4284/)
差分的本质是对一个区间例如[l,r] 进行修改(加上或者减去同一个数),即diff[l]+=c,diff[r+1]-=r;
不难看出,g(i)-f(i)这是不是长得很像差分
与此同时,再样例中观察g(i)和f(i)有一段连续的区间是相同的数字,所以我们似乎可以考虑差分
分别对g(i)和f(i)进行差分,那么就需要找到 **l**,**r**,**c**的大小
- 对于g(i)的差分就是找到修改的区间(下标)和修改的大小,由g(i)的定义可知
l = g(i) * r ,r = (g(i)+1) * r-1  ,这个是**向下取整的性质**
因此diff[l]+=g(i),diff(r+1)-=g(i)这样就不难求出,g(i)的差分了,即g(i)的一段区间进行修改
- 对于f(i)的差分就是根据定义,A_i<=x<A_i+1,因此我们要修改的区间一定就是a[i],a[i]-1,其加入的值肯定就是原数组的下标中的一个
因此我们先对g(i)一段区间加入c,f(i)再对一段区间+ (-c),这样就构造出了
g(i)-f(i)的差分数组
接下来就考虑还原和求和了
- 先还原出当前的g(i)-f(i),即sum = sum+diff[i];
- 再求和
但注意,我们一个一个地求和,因为时间只有1s,并且N最大为1e9,会超时,并且也不能开这么大地数组
因从我们考虑**用unordered_map**来存修改边界的**两个端点l,r**
同时,优化最后求和的,根据序列查询的提示,相同的地方可以直接用最开始的数字 * 相同数字的个数的来进行优化
考虑到这一点,我们可以在修改区间的时候记录下所有使用过的边界点,只要g(i)和f(i)同时使用了两个相同的边界点,还原g(i)-f(i)就不会有影响
可以画图理解![](de1c644c0c6e930a681948fa0a529c69_720.jpg)
因为错开的部分的对g(i)-f(i)的贡献可能相同,也可能不相同,直接分段求就行
重叠部分的g(i)-f(i)一定为0
```c++
#include <bits/stdc++.h>

typedef long long LL;

using namespace std;

const int MAX = 100010;

int n,N,r;

LL a[MAX]={};//存储原始数组

unordered_map<LL,LL> diff;//差分数组对区间进行修改

unordered_map<LL,bool> is_used_index;//记录使用过的边界

vector<LL>exist_index;//存储使用过的下标

void __insert(LL l,LL r,LL c){

    diff[l]+=c;

    diff[r+1]-=c;

    if(is_used_index.find(l)==is_used_index.end()){

        is_used_index[l] = true;

        exist_index.push_back(l);

    }

    if(is_used_index.find(r+1)==is_used_index.end()){

        is_used_index[r+1] = true;

        exist_index.push_back(r+1);

    }

}

int main(){

    ios::sync_with_stdio(0),cin.tie(0),cout.tie(0);

    cin>>n>>N;

    r = N/(n+1);

    //计算g(i)的差分,对g(i)的区间进行修改,即遍历g(i),看可以对哪些下标进行修改

    //注意差分数组里diff[i] = c 中下标与数字的含义,即我们将对以下标为数字i开始的数加上c

    //我们遍历所有的g(i),根据g(i)定义可以获得对于同一个g(i)可以哪一段下标区间

    for(int g_i = 0;g_i*r<N;g_i++){

        __insert(g_i*r,min((g_i+1)*r-1,N-1),g_i);

    }

  

    //计算g(i)-f(i)的差分

    //因为f_i的值一定是在下标里,所以我们遍历下标

    for(int i = 1;i<=n;i++){

        cin>>a[i];

        __insert(a[i-1],a[i]-1,1-i);

    }

    __insert(a[n],N-1,-n);

    sort(exist_index.begin(),exist_index.end());//重新排序

    //还原g(i)-f(i)数组,同时求和

    LL error = 0,sum = 0;

    for(int i = 0;i<exist_index.size()-1;i++){

        sum=sum+diff[exist_index[i]];

        error =error+(exist_index[i+1]-exist_index[i])*abs(sum);

    }

    cout<<error<<"\n";

}
```