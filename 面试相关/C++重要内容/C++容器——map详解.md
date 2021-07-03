# 一、map介绍

- std::map是一种关联容器，即以**key-value**键值对的形式存储数据。
- map底层实现是**红黑树**，而C++11新增容器unordered_map与map用法基本一致，但底层是：**哈希表**。
- 红黑树是一种平衡二叉树，而平衡二叉树在二叉排序树基础上的，所以红黑树也就有排序的特性（unordered_map没有排序）。可以做到在O(log n)时间内完成查找，插入和删除，在对单次时间敏感的场景下比较建议使用map做为容器。



# 二、map的属性和方法

## iterators

|  名字   |                             描述                             |
| :-----: | :----------------------------------------------------------: |
|  begin  |    返回指向容器中第一个（经过排序后）键值对的双向迭代器。    |
|   end   | 返回指向容器最后一个元素（排序后）所在位置后一个位置的键值对的双向迭代器 |
| rbegin  |            返回容器逆序的第一个键值对的双向迭代器            |
|  rend   |   返回容器逆序的最后一个元素的前一个位置的键值对双向迭代器   |
| cbegin  | 和begin()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
|  cend   | 和end()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
| crbegin | 和rbegin()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
|  crend  | 和rend()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |



## capacity

|   名字   |                       描述                       |
| :------: | :----------------------------------------------: |
|   size   |               返回存在键值对的个数               |
| max_size | 返回元素个数的最大值。这个值非常大，一般是2^32-1 |
|  empty   |     判断容器是否为空，为空返回true否则false      |



## Element access

|    名字    |                             描述                             |
| :--------: | :----------------------------------------------------------: |
| operator[] | map容器重载了 [] 运算符，只要知道 map 容器中某个键值对的键的值，就可以向获取数组中元素那样，通过键直接获取对应的值。 |
|     at     | 找到 map 容器中 key 键对应的值，如果找不到，该函数会引发 out_of_range 异常。 |



## Modifiers

|     名字     |                             描述                             |
| :----------: | :----------------------------------------------------------: |
|    insert    |                      向map中插入键值对                       |
|    erase     |                  删除map容器键值或者键值对                   |
|     swap     |                 交换2个map容器中的所有键值对                 |
|    clear     |                  清除map容器中的所有键值对                   |
|   emplace    | 在当前 map 容器中的指定位置处构造新键值对。其效果和插入键值对一样，但效率更高。 |
| emplace_hint | 在本质上和 emplace() 在 map 容器中构造新键值对的方式是一样的，不同之处在于，使用者必须为该方法提供一个指示键值对生成位置的迭代器，并作为该方法的第一个参数。 |



## Operations

|    名字     |                             描述                             |
| :---------: | :----------------------------------------------------------: |
|    find     | 在 map 容器中查找键为 key 的键值对，如果成功找到，则返回指向该键值对的双向迭代器；反之，则返回和 end() 方法一样的迭代器。另外，如果 map 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
|    count    | 在当前 map 容器中，查找键为 key 的键值对的个数并返回。注意，由于 map 容器中各键值对的键的值是唯一的，因此该函数的返回值最大为 1。 |
| lower_bound | 返回一个指向当前 map 容器中第一个大于或等于 key 的键值对的双向迭代器。如果 map 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
| upper_bound | 返回一个指向当前 map 容器中第一个大于 key 的键值对的迭代器。如果 map 容器用 const 限定，则该方法返回的是 const 类型的双向迭代器。 |
| equal_range | 该方法返回一个 pair 对象（包含 2 个双向迭代器），其中 pair.first 和 lower_bound() 方法的返回值等价，pair.second 和 upper_bound() 方法的返回值等价。也就是说，该方法将返回一个范围，该范围中包含的键为 key 的键值对（map 容器键值对唯一，因此该范围最多包含一个键值对）。 |



# 三、map的具体用法

## 3.1 iterator（迭代器访问）

```cpp
map<string, int> myMap;
myMap["张三"] = 20;
myMap["李四"] = 22;
myMap["孙钱"] = 18;
for (map<string, int>::iterator it = myMap.begin(); it != myMap.end(); it++)
{
    cout << it->first << " ：" << it->second << endl;
}
/* 输出
李四 ：22
孙钱 ：18
张三 ：20
*/
```

由以上代码可以看到，map会对元素进行排序，"李四"开头字母`L`、"孙钱"开头字母`S`、"张三"开头字母`Z`，`L < S < Z`，可见map默认以key值从小到大排序。

```cpp
map<string, int> myMap;
myMap["张三"] = 20;
myMap["李四"] = 22;
myMap["孙钱"] = 18;
for (auto it = myMap.rbegin(); it != myMap.rend(); it++)
{
    cout << it->first << " ：" << it->second << endl;
}
```

rbegin()和rend()则可从尾到头遍历。



## 3.2 capacity（容量）

- `size()`返回map中键值对的个数
- `max_size()`map中能存储的最大值
- `empty()`判断map是否为空：为空返回true，不为空返回false

```cpp
map<string, int> myMap;
myMap["张三"] = 20;
myMap["李四"] = 22;
myMap["孙钱"] = 18;
cout << myMap.size() << endl;//大小为3
cout << myMap.max_size() << endl;//最大容量：89478485
cout << myMap.empty() << endl;//非空即为0
```



## 3.3 Element access（下标访问）

- map重载了`[]`运算符，可以通过map[]通过key值访问其value。
- at作用和[]用法一致，实现效果一致。
- 如果某个key值不存在，即没有该键值对，返回0。（可见各个位置初始值为0）。

```cpp
map<string, int> myMap;
myMap["张三"] = 20;
myMap["李四"] = 22;
myMap["孙钱"] = 18;
cout << myMap["张三"] << endl;//20
cout << myMap.at("张三") << endl;//20
cout << myMap["王五"] << endl;//0
```



## 3.4 Modifiers（修改器）

这部分主要用于修改元素，如**增删改查**等操作。

### 3.4.1 insert

因为map中存储的元素时键值对类型（pair），所以insert可以有两种插入方式：

（1）使用pair、value_type、make_pair()函数，插入一个键值对

```cpp
map<string, int> myMap;
myMap.insert(pair<string, int>("李四", 22));
myMap.insert(map<string, int>::value_type("张三", 20));
pair<map<string, int>::iterator, bool> ans;//定义返回值类型
ans = myMap.insert(make_pair("孙钱", 18));
cout << ans.first->first << ":" << ans.first->second << endl;
myMap.insert(myMap.begin(), make_pair("赵六", 28));
```

注意，插入后的返回值是一个pair类型，pair.first是map的迭代器，pair.second为是否插入成功的返回值。

（2）在指定位置插入元素

```cpp
map<string, int> myMap;
myMap.insert(pair<string, int>("李四", 22));
myMap.insert(map<string, int>::value_type("张三", 20));
myMap.insert(make_pair("孙钱", 18));
myMap.insert(myMap.begin(), make_pair("赵六", 28));//在map头部后面插入
for (auto it = myMap.begin(); it != myMap.end(); it++)
{
	cout << it->first << ":" << it->second << endl;
}
```

但是通过显示的结果可知，即使指定在某个位置后插入，但是实际上map还是会根据**key值**将其排序。

注意，和前一种用法不一样的是，**返回值是迭代器**，而不是pair。



（3）在某个范围内插入元素，[first, last]

```cpp
map<string, int> myMap;
myMap.insert(pair<string, int>("李四", 22));
myMap.insert(map<string, int>::value_type("张三", 20));
myMap.insert(make_pair("孙钱", 18));

map<string, int> newMap;
newMap.insert(myMap.begin(), myMap.end());
for (auto it : newMap)
cout << it.first << ":" << it.second << endl;
```



（4）一次性插入多个键值对

```cpp
map<string, int> myMap;
myMap.insert({ {"李四", 22}, {"张三", 20} });//一次性插入两个键值对
myMap.insert(make_pair("孙钱", 18));
```



### 3.4.2 erase

删除元素，输入参数为迭代器，有以下两种遍历删除元素的方法：

（1）通过迭代器删除元素

- 因为erase的返回值是下一个迭代器的指针，所以可以用返回值做下一次删除的元素

  ```cpp
  map<string, int> myMap;
  myMap.insert(map<string, int>::value_type("张三", 20));
  myMap.insert(map<string, int>::value_type("李四", 22));
  myMap.insert(map<string, int>::value_type("孙钱", 18));
  for (auto it = myMap.begin(); it != myMap.end(); )
  {
  it = myMap.erase(it);
  }
  cout << myMap.empty() << endl;
  ```

- 根据后`++`的特点，先删除后it偏移。

  ```cpp
  map<string, int> myMap;
  myMap.insert(map<string, int>::value_type("张三", 20));
  myMap.insert(map<string, int>::value_type("李四", 22));
  myMap.insert(map<string, int>::value_type("孙钱", 18));
  for (auto it = myMap.begin(); it != myMap.end(); )
  {
  	myMap.erase(it++);
  }
  cout << myMap.empty() << endl;
  ```




（2）通过key值删除元素

```cpp
map<string, int> myMap;
myMap.insert({ {"李四", 22}, {"张三", 20} });
myMap.emplace_hint(myMap.begin(), make_pair("孙钱", 18));
myMap.erase("李四");//删除key值为李四的元素
```



### 3.4.3 swap

swap交换两个map的内容，两个map的所有内容均交换。

```cpp
map<string, int> myMap;
myMap.insert(map<string, int>::value_type("张三", 20));
myMap.insert(map<string, int>::value_type("李四", 22));
myMap.insert(map<string, int>::value_type("孙钱", 18));
map<string, int> myMap2;
myMap2["王五"] = 30;
myMap2["赵六"] = 50;
myMap.swap(myMap2);
for (auto it = myMap.begin(); it != myMap.end(); it++)
{
cout << it->first << ":" << it->second << endl;
}
```



### 3.4.4 clear

clear清空map的键值对，size = 0。

```cpp
map<string, int> myMap;
myMap.insert(map<string, int>::value_type("张三", 20));
myMap.insert(map<string, int>::value_type("李四", 22));
myMap.insert(map<string, int>::value_type("孙钱", 18));
myMap.clear();
cout << myMap.empty() << endl;
```



### 3.4.5 emplace

用法和insert一样，只是内部实现机制不同，emplace更快，但是只能插入一个键值对。

emplace的插入可以，将创建新键值对所需的数据作为参数直接传入即可，此方法可以自行利用这些数据构建出指定的键值对。

```cpp
map<string, int> myMap;
myMap.emplace(map<string, int>::value_type("张三", 20));//法1
myMap.emplace(pair<string, int>("李四", 22));//法2
myMap.emplace("赵六", 30);
```



### 3.4.6 emplace_hint

emplace_hint有点类似于insert的第二种用法，在指定位置插入。

但是，emplace_hint和emplace一样，可以将数据参数直接输入。

第一个参数是要插入的位置，接下来第二、三个参数分别是key 和 value。

```cpp
map<string, int> myMap;
myMap.insert({ {"李四", 22}, {"张三", 20} });
myMap.emplace_hint(myMap.begin(), "孙钱", 18);
```

当然，和insert一样，指定位置插入，但是map仍然会根据key值大小进行排序。



## 3.5 Operations（操作）

这部分操作用于查找元素，计算元素大小。



### 3.5.1 find

map有find方法，需和algorithm库的find区别。map的find方法寻找键值对中的键值，返回一个迭代器。如果迭代器不是`map.end()`，那么说明找到元素，返回该元素的迭代器。

```cpp
map<string, int> myMap;
myMap.insert({ {"李四", 22}, {"张三", 20} });
myMap.emplace_hint(myMap.begin(), make_pair("孙钱", 18));
auto it = myMap.find("李四");//寻找是否有名为"赵四"的人
if (it != myMap.end())
{
cout << it->first << ":" << it->second << endl;
}
```



### 3.5.2 count

map和set两种容器的底层结构都是**红黑树**，所以容器中不会出现相同的元素，因此count()的结果只能为0和1。

所以可以用`count`判断map是否有某键值对。

```cpp
map<string, int> myMap;
myMap.insert({ {"李四", 22}, {"张三", 20} });
myMap.emplace_hint(myMap.begin(), make_pair("孙钱", 18));
cout << myMap.count("张三") << endl;//输出1
```



### 3.5.3 lower_bound

在STL里有lower_bound函数，用于在有序容器中找到第一个大于val值的迭代器。

同样的，在map中有该方法，返回**大于或等于**某个键值`key`的迭代器。

```cpp
map<int, int> myMap;
myMap[1] = 2;
myMap[2] = 4;
myMap[3] = 7;
myMap[5] = 8;
myMap[6] = 9;
auto it = myMap.lower_bound(3);
if (it != myMap.end())
{
    cout << it->first << ":" << it->second << endl;//3:7
}
```



### 3.5.4 upper_bound

和lower_bound类似，不过upper_bound返回的是**大于**某个键值`key`的迭代器。

```cpp
map<int, int> myMap;
myMap[1] = 2;
myMap[2] = 4;
myMap[3] = 7;
myMap[5] = 8;
myMap[6] = 9;
auto it = myMap.upper_bound(3);
if (it != myMap.end())
{
    cout << it->first << ":" << it->second << endl;//5:8
}
```



### 3.5.5 equal_range

```
pair<const_iterator,const_iterator> equal_range (const key_type& k) const;
pair<iterator,iterator>             equal_range (const key_type& k);
```

如上是map中equal_range的原形，返回值是一个pair类型，pair类型里是两个迭代器，pair::first返回的值等于lower_bound的返回值，即大于等于某个值的第一个值的迭代器；pair::second返回的值等于upper_bound返回的值，即大于某个值的第一个值的迭代器。

所以，equal_range用于计算某个值的所有插入的元素。因为我们可能插入（insert）了很多元素，但是实际上只有一个值有效，也就是第一个插入的值。equal_range就可以为`multimap`计算所有插入的元素。

而`map`**不论传入多少个都只有一个值**，equal_range得到也只有一个值。

以下是multimap的`equal_range`：

```cpp
multimap<int, int> myMap;
myMap.insert(make_pair(1, 5));
myMap.insert(make_pair(2, 4));
myMap.insert(make_pair(3, 6));
myMap.insert(make_pair(3, 5));
myMap.insert(make_pair(3, 4));


auto ret = myMap.equal_range(3);
for (auto it = ret.first; it != ret.second; it++)
{
	cout << it->first << ":" << it->second << endl;
}
//输出：
/*
3:6
3:5
3:4
*/
```



# 参考

- http://www.cplusplus.com/
- http://c.biancheng.net/view/7173.html



