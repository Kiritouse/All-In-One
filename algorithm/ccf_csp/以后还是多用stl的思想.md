[5720. 相似度计算 - AcWing题库](https://www.acwing.com/problem/content/5723/)
这道题,就是很简单的统计单词个数,既可以用set,也可以用map,所以这道题就是一道典型的stl题
获取每个文章的单词数后,找两个set/map中相同的单词,统计个数,然后输出
最后拿总的减去相同的就行
```c++
#include <bits/stdc++.h>
using namespace std;
set<string>article_1;
set<string>article_2;
int n, m;
int main() {
	cin >> n >> m;
	for (int i = 0; i < n; i++) {
		string temp;
		cin >> temp;
		//cout<<"当前输入的是"<<temp<<endl;
		for (int j = 0; j < temp.size(); j++) {
			if (temp[j] >= 'A' && temp[j] <= 'Z') {
				temp[j] += 32;
			}
		}
		//cout<<"转后的是"<<temp<<endl;
		article_1.insert(temp);
	}
	for (int i = 0; i < m; i++) {
		string temp;
		cin >> temp;
		for (int j = 0; j < temp.size(); j++) {
			if (temp[j] >= 'A' && temp[j] <= 'Z') {
				temp[j] += 32;
			}
		}
		article_2.insert(temp);
	}
	int cnt = 0;
	for(auto it1:article_1){ //这里之前做的时候总向
		if(article_2.find(it1)!=article_2.end()){
			cnt++;
		}
	}
	cout<<cnt<<endl;
	cout<<article_1.size()+article_2.size()-cnt<<endl;
	


}
```