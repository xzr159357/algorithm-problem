# 一、list介绍

- list是C++STL容器中的**顺序容器**，这里的顺序容器区别于关联容器，指的是**元素在容器中的位置与大小无关**。list和vector不同，vector是顺序存储的，内存是连续的；list底层实际上是**双向链表**，list的内存是分散的，内存可以在各个位置分布。
- 基于双向链表的数据结构，list具有array、vector、deque等不具备的优势：在任意位置**插入和删除**元素的时间复杂度O(1)，移动元素的效率也很高。因为元素之间是通过指针联系的，在某个元素间插入一个元素改变那指针的指向就可以了。
- 但是正因为底层是双向链表，无法像vector那样通过**下标[]**直接访问元素，需要从头（尾）遍历元素找到元素。



> 如果需要大量数据的插入和删除，而对数据的访问比较少，使用list是个不错的选择。

# 二、list创建

list的创建和vector类似，但要注意list存储的地址不是连续的，迭代器不可以偏移。

```cpp
//1.创建一个空的list
list<int> values1;
//2.创建10个空间的list，默认值为0
list<int> values2(10);
//3.创建10个值为2的list
list<int> values3(10, 2);
//4.值为[1,2,3,4,5]
list<int> values4 = {1, 2, 3, 4, 5};
//5.使用另一个list来初始化list
list<int> values5(values4);
//6.使用其他容器如vector来初始化list
vector<int> arr = { 1, 2, 3 };
list<int> values6(values5.begin() + 1, values5.end());
```



# 三、list方法

**iterators（迭代器）**

|  名字   |                             描述                             |
| :-----: | :----------------------------------------------------------: |
|  begin  |            返回指向容器中第一个元素的双向迭代器。            |
|   end   |    返回指向容器最后一个元素所在位置后一个位置的双向迭代器    |
| rbegin  |             返回容器逆序的第一个元素的双向迭代器             |
|  rend   |      返回容器逆序的最后一个元素的前一个位置的双向迭代器      |
| cbegin  | 和begin()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
|  cend   | 和end()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
| crbegin | 和rbegin()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
|  crend  | 和rend()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |



**Capacity（容量）**

|   名字   |                       描述                       |
| :------: | :----------------------------------------------: |
|   size   |                返回实际元素的个数                |
| max_size | 返回元素个数的最大值。这个值非常大，一般是2^32-1 |
|  empty   |    判断vector是否为空，为空返回true否则false     |
|  resize  |                  调整容器的大小                  |



**Element access（元素访问）**

| 名字  |           描述           |
| :---: | :----------------------: |
| front |   返回第一个元素的引用   |
| back  | 返回最后一个元素的引用。 |



**Modifiers（修改器）**

|       名字        |                             描述                             |
| :---------------: | :----------------------------------------------------------: |
|  **push_front**   |                      在容器头部插入元素                      |
|     push_back     |                      在容器尾部插入元素                      |
| **emplace_front** |          在容器头部生成元素。同push_front，效率更高          |
|   emplace_back    |          在容器尾部生成元素。同push_back，效率更高           |
|      insert       |                    在容器指定位置插入元素                    |
|      emplace      |         在容器指定位置擦插入元素。同insert，效率更高         |
|       erase       |                删除容器中一个或某区域内的元素                |
|      assign       |                 用新元素替换容器中的原有内容                 |
|       swap        | 交换**两个容器**中的元素。必须保证两个容器存储的是同一数据类型 |
|       clear       |              清除容器内容，size=0，存储空间不变              |



**list operations**：

|    名字     |                             描述                             |
| :---------: | :----------------------------------------------------------: |
|   splice    |        将一个list容器中的元素插入另一个容器的指定位置        |
| remove(val) |                删除容器中所有值等于val的元素                 |
|  remove_if  |                 删除容器中满足某个条件的元素                 |
|   unique    |           删除容器中**相邻**的重复元素，只保留一个           |
|    merge    | 合并两个实现已经排好序的list容器，并且合并后的list容器仍然是有序的 |
|    sort     |           将容器**排序**，通过更改容器中元素的位置           |
|   reverse   |                   **反转**容器中元素的位置                   |



## 对比vector

- vector有size和capacity，一个是当前容器大小，另一个是可以存储容器大小；而list只有size，因为list不需要扩容，所以不需要提前预留空间。
- vector访问容器可以通过下标[]和at，而list只能直接访问头部和尾部元素，访问某个具体元素需要遍历list。
- 因为list是双向链表，可以从头部加入元素，而vector只能尾部插入。
- list内置了一些**算法**中的方法，如：sort、reverse，并且可以轻松删除元素：remove、remoce_if；而vector并不具备。
- 总的来说，list相对于vector，插入和删除简单，访问难。



# 四、list的具体用法

## 4.1 iterators

- 使用迭代器访问元素，注意迭代器的名称。

- 当然用auto会很方便。

```cpp
list<int> values = {1, 2, 3, 4, 5};
for (list<int>::iterator it = values.begin(); it != values.end(); ++it)
{
    cout << *it << endl;
}
for (list<int>::reverse_iterator it = values.rbegin(); it != values.rend(); ++it)
{
    cout << *it << endl;
}
for (list<int>::const_iterator it = values.cbegin(); it != values.cend(); ++it)
{
    cout << *it << endl;
}
for (list<int>::const_reverse_iterator it = values.crbegin(); it != values.crend(); ++it)
{
    cout << *it << endl;
}
```



## 4.2 Capacity

和vector比起来少了capacity，所以只有对size进行判断和修改的方法。

```cpp
list<int> values = {1, 2, 3, 4, 5};
cout << values.size() << endl;                      //输出5
cout << values.max_size() << endl;                  //输出357913941
cout << values.empty() << endl;                     //输出0，因为不为空
values.resize(0);
cout << values.empty() << endl;                     //输出1，因为resize将元素size=0，所以为空
values.resize(5, 6);                                //重置list为5个值为6
for (auto value : values) cout << value << endl;    //输出5个6
```



## 4.3 Element access

不能够像vector、array那样对任意元素访问，只能够访问front、back。

```cpp
list<int> values = {1, 2, 3, 4, 5};
cout << values.front() << endl;     //输出1
cout << values.back() << endl;      //输出5
```



## 4.4 Modifiers

### push_front、push_back、emplace_front、emplace_back

list可以从头部插入，也可以从尾部插入

```cpp
list<int> values;
for (int i = 1; i <= 5; i++)
{
    values.push_front(i);
    values.emplace_front(i);
    values.push_back(i);
    values.emplace_back(i);
}
```



### insert、emplace

**insert**有四种用法：

- 在指定位置（迭代器）插入某个元素

  ```cpp
  //插入3和4，输出3 4
  list<int> values;
  values.insert(values.begin(), 3);
  auto it = values.begin();
  it++;
  values.insert(it, 4);
  ```

  注意，不可`values.insert(values.begin() + 1, 3);`，因为list的迭代器不可随意访问元素，没有重载`+`运算符，而且底层是双向链表，不具备随意访问性质。

  所以需要偏移时，定义一个迭代器it，it++就可以了。并且`it += 2`子类的操作也不可，因为`it++`并不是简单的加一，有点类似于指针的偏移，找到下一个元素所在位置的迭代器，是重载后的++。



- 在指定位置插入n个置为val的元素

  ```cpp
  list<int> values;
  values.insert(values.end(), 3, 10);
  ```

- 在指定位置插入一系列元素

  ```cpp
  list<int> values;
  values.insert(values.end(), {1, 2, 3});//使用{}
  ```

- 在指定位置插入**同一个类型容器**的元素

  ```cpp
  list<int> values;
  vector<int> arr = {4, 5, 6};
  values.insert(values.end(), arr.begin() + 1, arr.end());
  ```



**emplace**和insert类似都是插入元素，不过emplace只能插入**一个元素**。

```cpp
list<int> values;
values.emplace(values.end(), 3);
```



### erase

erase通过迭代器删除某个或某个范围的元素。

```cpp
list<int> values;
values.emplace(values.end(), 5, 1);
values.erase(values.begin());//某个元素
values.erase(values.begin(), values.end());//某个范围的元素
```



### assign

assign修改整个list，有两种用法：

- 修改为n个值为val

  ```cpp
  list<int> values;
  values.assign(5, 3);
  ```

- 修改为某个容器一定范围的值

  ```cpp
  list<int> values;
  list<int> values2 = { 1, 2, 3 };
  values.assign(values2.begin(), values2.end());
  ```



### swap

将list本身和另一个**相同数据类型的list**交换所有元素。

```cpp
list<int> values = {4, 5, 6};
list<int> values2 = { 1, 2, 3 };
values.swap(values2);
```





## 4.5 list operations

### splice

splice是list类容器独有的功能，可以将两个list容器的内容进行拼接，有三种用法：

- 从当前容器的某个迭代器开始，将另一个list容器的所有元素加入当前容器。

  ```cpp
  list<int> values = {4, 5, 6};
  list<int> values2 = { 1, 2, 3 };
  values.splice(values.begin(), values2);//最后values = {1,2,3,4,5,6}
  ```

- 从当前容器的某个迭代器开始，将另一个list容器的某个元素加入当前容器

  ```cpp
  list<int> values = {4, 5, 6};
  list<int> values2 = { 1, 2, 3 };
  values.splice(values.begin(), values2, values2.begin());//最后values = {1,4,5,6}
  ```

- 从当前容器的某个迭代器开始，将另一个list容器的某个范围内容的加入当前容器

  ```cpp
  list<int> values = {4, 5, 6};
  list<int> values2 = { 1, 2, 3 };
  values.splice(values.begin(), values2, values2.begin(), values2.end());//{1,2,3,4,5,6}
  ```

splice的用法其实和insert很像，不过splice只能操作list，而insert可以是其他类型的容器。



### remove、remove_if、unique

remove、remove_if、unique都是list容器独有的，用于删除元素的方法。

### remove

remove：删除值为val的所有元素。

```cpp
list<int> values = {1, 2, 3, 1, 2, 3};
values.remove(2);//values = {1,3,1,3}
```



### remove_if

成员函数 remove_if() 期望传入一个**一元断言**作为参数。一元断言接受一个和元素同类型的参数或引用，返回一个布尔值，断言返回 true 的所有元素都会被移除。

```cpp
list<int> values = {1, 2, 3, 1, 2, 3};
values.remove_if([&](int n) {return n == 1; });
```

如以上例子，就通过lambda表达式，将值n=1的元素删除。



### unique

unique可以删除list容器中相邻的相同元素，只留下一个。注意，是相邻的相同元素，不相邻的即使之前出现也不会删除的。

```cpp
list<int> values = {1, 1, 2, 2, 3, 4, 3};
values.unique();//values = {1,2,3,4,3}
```



## merge、sort、reverse

### sort

sort顾名思义就是排序，和算法中的sort一致。

用法，直接对整个list排序，默认升序；降序输入参数：`greater< int >()`

```cpp
list<int> values = {1, 9, 2, 2, 3, 4, 3};
values.sort();//升序
values.sort(greater<int>());//降序
```

### merge

合并两个**实现已经排好**序的list容器，两者合并后的list容器仍然是有序的。

```
list<int> values = { 1,3,5 };
list<int> values2 = { 2, 4, 6 };
values.merge(values2);
```

注意，两个list都要排序好才能够merge，否则会出错。可以用sort方法排序，并且排序方向需一致。

### reverse

将list容器中的元素翻转。

```cpp
list<int> values = { 1,3,5 };
values.reverse();//values = {5,3,1}
```



# 五、list和vector对比

|              |                            vector                            |                      list                      |
| :----------: | :----------------------------------------------------------: | :--------------------------------------------: |
| 底层实现原理 |                           动态数组                           |                    双向链表                    |
|  空间利用率  |       空间上是连续的，具有缓存和扩容机制，空间利用率高       |     空间上是离散的，内存分散，空间利用率低     |
|   随机访问   |                      支持随机访问，O(1)                      |        不支持随机访问，需要从头开始遍历        |
|  插入和删除  | 能在尾部实现O(1)的插入和删除；插入和删除其他部分需要O(n)的时间复杂度 | 可在任意位置进行插入和删除，时间复杂度都为O(1) |
|    迭代器    |                等同于指针，可以像指针一样偏移                |   在指针的基础上进行了疯转，迭代器不支持`+`    |
|   使用场景   |           对于随机访问要求高，不关心插入、删除效率           |     有大量的插入、删除操作，不关心随机访问     |





# 参考

- http://c.biancheng.net/view/6892.html
- http://c.biancheng.net/view/444.html

