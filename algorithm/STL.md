# 容器
## 顺序式容器

| 容器     | 说明  | 底层实现        |
| ------ | --- | ----------- |
| vector |     | 数组          |
| list   |     | 双向链表        |
| deque  |     | 中央控制器和多个缓冲区 |
其拥有的**成员函数**
```c++
begin()//获取指向开头的迭代器(地址)  
end() //结尾的迭代器,该位置是最后一个元素的下一位,不能解引用
rbegin()  
rend()  
front()  
back()  
erase()  
clear()  
push_back()  
pop_back()  
insert() // vector 和 deque 的 insert 复杂度较高
```
### vector
- 高效的随机访问和尾端插入  删除
- 其他位置的插入/删除可以看作数组
- 动态大小
> **超了提前分配**的空间会配置一块新的空间,再将原来的元素一一拷贝到新地址中,再释放掉原来的空间
> 复杂度太高

#### 声明及初始化
```c++
vector<int>vec; //声明一个空的vector,注意这里如果直接用vec[i]输入会报错
vector<int>vec(5); //声明一个大小为5的vector
vector<int>vec(10,1);//大小为10,初始值为1,这里可以用Vec[i]输入,类似于修改了原来的数值
vector<int>vec(oldvec);
vector<int>vec(oldvec.begin(),oldvec.begin()+3);
int arr[5] = {1,2,3,4,5};
vector<int>vec(arr,arr+5);//用数组初始化vec;
vector<int>vec(&arr[0],&arr[5]);//似乎迭代器就等于指向某一位置的地址
```

#### 插入数据
```c++
push_back();
emplace_back();
```
- push_back();接受一个已经构造好的对象
- emplace_back();是直接正在内部构造对象,传入构造对象所需的参数
```c++
std::vector<std::pair<int, int>> vec;
vec.push_back(std::make_pair(1, 2)); // 合法
vec.push_back({1, 2}); // 合法
vec.emplace_back(std::make_pair(1, 2)); // 合法
vec.emplace_back({1, 2}); // 不合法!!!!
应该是
vec.emplace_back(1,2);
```
#### 成员函数

### list
优点:

1. 不使用连续内存完成插入和删除操作, 使得能够在常数时间内插入新元素
2. 在内部方便的进行插入和删除操作
3. 可以在两端 push, pop

缺点:

1. 不能进行随机访问, 即不支持`[]`操作符和`.at()`访问函数
2. 相对于 vector 占用内存多

与vector的区别:
1. vector 为存储的对象分配一块连续的地址空间, `随机访问效率很高` 但是``插入和删除``需要移动大量的数据, ==效率较低==. 尤其当vector内部元素较复杂, 需要调用复制构造函数时, 效率更低.
2. list 中的对象是离散的, ``随机访问``需要遍历整个链表, 访问`效率比 vector 低`, 但是在 list 中==插入==元素, 尤其在首尾插入时, `效率很高`.
3. vector 是 ==单向的== 的, 而 ==list== 是双向的 (vector为什么单向???)
4. vector 中的 iterator 在使用后就释放了, 但是 list 不同, 它的迭代器在使用后还可以继续使用, 是链表所特有的.

#### 声明以初始化
```c++
list<int> l;
list<int> l(5); // 含有5个元素的list, 初始值为0
list<int> l(10, 1); // 含有10个元素的list, 初始值为1
list<int> l(oldL); // 复制构造
list<int> l(oldL.begin(), oldL.end());
int arr[5] = {1, 2, 3, 4, 5};
list<int> l(arr, arr+5); // 用数组初始化list
list<int> l(&arr[1], &arr[5]); // 用数组初始化list
```

#### 赋值和交换
```c++
1. lst1 = lst2;
    
2. lst1.assign(int n, elem); //赋值n个elem
    
3. lst1.assign(lst2.begin(), lst2.end());
    
4. lst1.swap(lst2);//交换链表
```
#### 空间大小操作和判定
```c++
list.size();//获得容器中元素的个数
list.empty();//判断list容器是否为空,为空返回true;
list.resize(8);//重新改变list大小,如果比原来的大则默认填充0
list.resize(int num,elem);//同上,默认填充为elem
```

#### 插入和删除
```c++
# 两端操作
list.push_front(T) // vector 没有该函数
list.pop_front(T) // vector 没有该函数
list.push_back(T)
list.pop_back(T)
# 其他位置进行操作
list.insert(pos,elem);//post为迭代器类型
list.insert(pos,begin,end);//三个都是迭代器,在pos处插入[begin,end)元素
list.erase(begin,end);//删除[begin,end)元素
list.erase(pos);//删除pos处位置
list.clear();//清除容器数据
list.remove(elem);//删除等于elem元素的数据

# 迭代器操作
## list的迭代器不支持随机访问,只能++或者--
lst1.assign(4, elem);
lst1.insert(lst1.begin()++, elem);
lst1.insert(lst1.begin()++, lst2.begin(), lst2.end());
	
lst1.erase(lst1.begin()++, lst1.end()--);
lst1.erase(lst1.begin()++);
lst1.clear();
lst1.remove(elem);

```
#### 访问数据
```c++
# 只访问开头或者结尾
list.front();//取出开头
list.back();//取出结尾
# 迭代器遍历
 // 使用迭代器遍历
  for (std::list<int>::iterator it = lst.begin(); it != lst.end(); ++it) {
    std::cout << *it << " ";
}
# 范围for循环
for(int val:lst){
	std::cout<<val<<" ";
}
# 常量迭代器访问(只读)
for(std::list<int>::const_iterator it = lst.begin();it!=lst.end();++it){
	std::cout<<*it<<" ";
}
```
#### 常用函数
```c++
list.merge(list2) // 合并两个list
list.remove(num) //移除所有等于num的元素
list.remove_if([](int x){return x>5}) // 按指定条件删除元素,移除所有大于5的元素
list.reverse() // 逆置list元素
list.sort() // 排序
list.sort(std::greater<int>());//使用自定义比较函数进行排序(降序)
list.unique() // 删除重复元素
list.splice() // 从另一个 list 中移动元素

```


### deque
双端队列
优点:

1. 随机访问方便, 支持`[]`操作符和`.at()`访问函数, 常数时间, 次于 vector, 因为有可能存在尾部的内存位置在头部之前的场景.
2. 可在两端进行push, pop操作, 效率都较高.

缺点:  
占用内存多

#### 常用函数
```c++
push_back()
pop_back()
push_front() // vector 没有该函数
pop_front() // vector 没有该函数
```

## 关联式容器
| 容器 | 说明 | 底层实现 | 操作复杂度 |
|------|------|----------|------------|
| map | 键值对的映射, 有序, 元素不可重复 | 红黑树 | O(logn) |
| unordered_map | 键值对的映射, 无序, 元素不可重复 | 哈希表 | O(1) |
| multimap | 键值对的映射, 有序, 元素可重复 | 红黑树 | O(logn) |
| unordered_multimap | 键值对的映射, 无序, 元素可重复 | 哈希表 | O(1) |
| set | 关键字集合, 有序, 元素不可重复 | 红黑树 | O(logn) |
| unordered_set | 关键字集合, 无序, 元素不可重复 | 哈希表 | O(1) |
| multiset | 关键字集合, 有序, 元素可重复 | 红黑树 | O(logn) |
| unordered_multiset | 关键字集合, 无序, 元素可重复 | 哈希表 | O(1) |
### map
- 相当于字典,==每个关键字只出现一次==
- 底部采用红黑树,会自动排序
#### 常用操作
```c++
// 数据插入, 复杂度为 logn
map.insert({key, value});
map[key] = value;
// 移除, 复杂度为 logn
map.erase(key)
// 搜索, 复杂度为 logn
map.find()
map[key]

map.count() // 返回匹配特定键的元素数量,要么0要么1,因为只出现一次, 对数复杂度
map.contains(key) //检查map中是否包含特定键,c++20引入
map.equal_range(key)//返回一个pair<iterator,iterator>,
//如果键存在，`pair.first` 会指向该键的元素，`pair.second` 会指向下一个元素(范围外)。如果键不存在，`pair.first` 和 `pair.second` 会相等，指向第一个大于给定键的元素。
/*
- **作用**：返回一个迭代器，指向第一个不小于给定键的元素。
- **用途**：用于查找第一个大于或等于给定键的元素，适用于需要查找某个范围起点的情况
*/
map.lower_bound(key)
map.upper_bound()
```

### unorder_map
##### 声明和初始化
```c++
unordered_map<type1,type2>u_map;//类似于vec[i] = value,即通过type1来查询type2
// 定义一个嵌套的unordered_map来存储坐标和值
 std::unordered_map<int, std::unordered_map<int, int>> pointMap;
 //可以快速查询大量的map[i][j] = value,即二维查询

```
##### 常用操作
```c++
// 构造函数
	map<string, int> dict;
	
	// 插入数据的三种方式
	dict.insert(pair<string,int>("apple",2));
	dict.insert(map<string, int>::value_type("orange",3));
	dict["banana"] = 6;
 
	// 判断是否有元素
	if(dict.empty())
		cout<<"该字典无元素"<<endl;
	else
		cout<<"该字典共有"<<dict.size()<<"个元素"<<endl;
 
	// 遍历
	map<string, int>::iterator iter;
	for(iter=dict.begin();iter!=dict.end();iter++)
		cout<<iter->first<<ends<<iter->second<<endl;
 
	// 查找  /*这个是最主要的用途*/
	if((auto iter=dict.find("banana"))!=dict.end()) //  返回一个迭代器指向键值为key的元素，如果没找到就返回end()
		cout<<"已找到banana,其value为"<<iter->second<<"."<<endl;
	else
//如果之间用dict[i]来进行查找,就还是会有问题,因为如果这样如果没有i,就会生成并且插入i,速度比较慢,用find会比较快


```

# 库函数
这里是搜集一些好用的库函数和使用注意事项

## unique()
> "去除"容器或数组中的重复元素
> 这里的去除实际上是指将重复的元素放到容器的末尾,返回的是去重后的数组的最后一个元素的地址

用法
```c++

    int a[8] = {2, 2, 2, 4, 4, 6, 7, 8};
    int c;
    std::sort(a, a + 8);  //对于无序的数组需要先排序
    c = (std::unique(a, a + 8) - a );//因为返回的是所构造的不重复数组的尾地址,所以这里要减去开头的
    std::cout<< "c = " << c << std::endl;
    //打印去重后的数组成员
    for (int i = 0; i < c; i++)
        std::cout<< "a = [" << i << "] = " << a[i] << std::endl;

```

