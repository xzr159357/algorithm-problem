> 本文将详细介绍STL容器之vector的用法，并对相关底层原理说明。

# 一、介绍

- vector是STL容器中的一种常用的容器，和数组类似，由于其大小(size)可变，常用于数组大小不可知的情况下来替代数组。
- vector是为了实现**动态数组**而产生的容器，然而**向量**这个名字是STL编写者取名没区好，因为在数学上的向量在几何中是矢量，两者名字相同而意义大相径庭。
- vector也是一种顺序容器，在内存中连续排列，因此可以通过下标快速访问，时间复杂度为O(1)。然而，连续排列也意味着大小固定，数据超过vector的预定值时vector将自动扩容。



# 二、vector的创建和方法

首先，使用vector时需包含头文件：

```
#include <vector>
```



## 创建vector

vector本质是类模板，可以存储任何类型的数据。数组在声明前需要加上数据类型，而vector则通过模板参量设定类型。

比如，声明一个int型的vector数组。

```
vector<int> arr1;								//一个空数组
vector<int> arr2 {1, 2, 3, 4, 5};				//包含1、2、3、4、5五个变量
vector<int> arr3(4);							//开辟4个空间，值默认为0
vector<int> arr4(5, 3);							//5个值为3的数组
vector<int> arr5(arr4);							//将arr4的所有值复制进去，和arr4一样
vector<int> arr6(arr4.begin(), arr4.end());		//将arr4的值从头开始到尾复制
vector<int> arr7(arr4.rbegin(), arr4.rend());	//将arr4的值从尾到头复制
```



## 方法

**iterators（迭代器）**

|  名字   |                             描述                             |
| :-----: | :----------------------------------------------------------: |
|  begin  |              返回指向容器中第一个元素的迭代器。              |
|   end   |      返回指向容器最后一个元素所在位置后一个位置的迭代器      |
| rbegin  |               返回容器逆序的第一个元素的迭代器               |
|  rend   |        返回容器逆序的最后一个元素的前一个位置的迭代器        |
| cbegin  | 和begin()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
|  cend   | 和end()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
| crbegin | 和rbegin()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |
|  crend  | 和rend()功能相同，在其基础上增加了 const 属性，不能用于修改元素。 |



**Capacity（容量）**

|     名字      |                       描述                       |
| :-----------: | :----------------------------------------------: |
|     size      |                返回实际元素的个数                |
|   capacity    |            返回总共可以容纳的元素个数            |
|   max_size    | 返回元素个数的最大值。这个值非常大，一般是2^32-1 |
|     empty     |    判断vector是否为空，为空返回true否则false     |
|    resize     |          改变实际元素的个数，对应于size          |
|    reserve    |       增加容器的容量，控制vector的预留空间       |
| shrink_to_fit |   减少capacity到size的大小，即减小到size的大小   |



**Element access（元素访问）**

|    名字    |                      描述                       |
| :--------: | :---------------------------------------------: |
| operator[] |        vector可以和数组一样用[]访问元素         |
|     at     | vector.at(i)等同于vector[i]，访问数组下表的元素 |
|   front    |                 返回第一个元素                  |
|    back    |                返回最后一个元素                 |
|    data    |       返回指向容器中第一个元素的**指针**        |



**Modifiers（修改器）**

|     名字     |                   描述                   |
| :----------: | :--------------------------------------: |
|  push_back   |           在容器的尾部插入元素           |
|   pop_back   |             删除最后一个元素             |
|    insert    |                 插入元素                 |
|    erase     |                 删除元素                 |
|    clear     |    清除容器内容，size=0，存储空间不变    |
|     swap     |          交换两个元素的所有内容          |
|    assign    |          用新元素替换原有内容。          |
|   emplace    | 插入元素，和insert实现原理不同，速度更快 |
| emplace_back |  在容器的尾部插入元素，和push_back不同   |



# 三、vector的具体用法

下面将讲解vector的具体用法

## 3.1 遍历vector

### 3.1.1 迭代器访问

- 通过迭代器访问从begin()到end()，需要定义iterator，当然可以用auto替代。
- begin()表示第一个元素，而end()不是最后一个元素，end()是最后一个元素的前一个位置。

```cpp
//迭代器：vector<int>::iterator
for (vector<int>::iterator it = arr.begin(); it != arr.end(); it++)
{
    cout << *it << endl;
}
//迭代器：vector<int>::reverse_iterator
for (vector<int>::reverse_iterator it = arr.rbegin(); it != arr.rend(); it++)
{
    cout << *it << endl;
}
```



### 3.1.2 下标访问

和数组类似，从下标0开始遍历，而不到size的大小。

```cpp
for (int i = 0; i < arr.size(); i++)
{
	cout << arr[i] << endl;
}
```



### 3.1.3 范围for循环

C++11的特性，范围for，遍历元素十分方便。

```cpp
for (auto num : arr)
{
	cout << num << endl;
}
```





## 3.2 vector 容量和大小

- 顾名思义，**size**表示当前有多少个元素，**capacity**是可容纳的大小。因为vector是顺序存储的，那么和数组一样，有一个初始容量，在vector里就是capacity。capacity必然大于等于size，每次扩容时会改变，具体大小和vector底层实现机制有关。

- **max_size**是可存储的最大容量，和实际的编译器、系统有关，使用的比较少。

- **empty**很好理解，判断vector是否为空，其实就是判断size是否等于0。定义vector时设定了大小、resize修改大小等操作，vector都不为空；clear后，size=0，那么empty判断就为空。

- **resize**改变size的大小，而**reserve**改变capacity的大小，**shrink_to_fit**减小capacity到size

  ```cpp
  vector<int> arr;
  arr.resize(4);
  arr.reserve(6);
  cout << arr.size() << " " << arr.capacity() << endl;
  cout << "##########################" << endl;
  arr.shrink_to_fit();
  cout << arr.size() << " " << arr.capacity() << endl;
  ```

  

## 3.3 vector 常用算法

### 3.3.1 push_back、pop_back 和 emplace_back

- push_back和pop_back用法简单

```cpp
vector<int> arr;
for (int i = 0; i < 5; i++)
{
    arr.push_back(i);
}
for (int i = 0; i < 5; i++)
{
    arr.pop_back();
}
```

- emplace_back的效果和push_back一样，都是尾部插入元素

```cpp
arr.emplace(10);
```

两者的差别在于**底层实现的机制不同**：`push_back`将这个元素**拷贝或者移动**到容器中（如果是拷贝的话，事后会自行销毁先前创建的这个元素）；而 `emplace_back` 在实现时，则是直接在容器尾部**创建**这个元素，省去了拷贝或移动元素的过程。所以emplace_back的速度更快。

### 3.3.2 insert 和 emplace

insert有三种用法：

- 在指定位置插入值为val的元素。

  ```cpp
  //在arr的头部插入值为10的元素
  vector<int> arr;
  arr.insert(arr.begin(), 10);
  ```

- 在指定位置插入n个值为val的元素

  ```cpp
  //从arr的头部开始，连续插入3个值为10的元素
  vector<int> arr;
  arr.insert(arr.begin(), 3, 10);
  ```

- 在指定位置插入区间[start, end]的所有元素

  ```cpp
  //从arr的头部开始，连续插入arrs区间[begin, end]的所有元素
  vector<int> arr;
  vector<int> arrs = { 1, 2, 3, 4, 5 };
  arr.insert(arr.begin(), arrs.begin(), arrs.end());
  ```



emplace和insert同为插入元素，不过emplace只能插入一个元素：

```cpp
//在arr的头部插入值为10的元素
vector<int> arr;
arr.emplace(arr.begin(), 10);
```

insert和emplace的区别和上面类似，就是一个是拷贝和复制的过程，而另一个则是直接创建一个新元素。



### 3.3.3 erase

erase通过迭代器删除某个或某个范围的元素，并**返回下一个元素的迭代器**。

```cpp

vector<int> arr{1, 2, 3, 4, 5};
//删除arr开头往后偏移两个位置的元素，即arr的第三个元素，3
arr.erase(arr.begin() + 2);
//删除arr.begin()到arr.begin()+2之间的元素，删除两个;即删除arr.begin()而不到arr.begin()+2的元素
arr.erase(arr.begin(), arr.begin() + 2);
```



### 3.3.4 assign

assign修改vector，和insert操作类似，不过insert是从尾部插入，而assign则将整个vector改变。

- 将整个vector修改为n个值为val的容器

  ```cpp
  //将arr修改为3个值为5的vector。
  vector<int> arr = {5, 4, 3, 2, 1};
  arr.assign(3, 10);
  ```

- 将整个vector修改为某个容器[start, end]范围内的元素

  ```cpp
  //将arr修改为范围[arrs.begin, arrs.end]内的元素
  vector<int> arr = {5, 4, 3, 2, 1};
  vector<int> arrs = { 1, 2, 3, 4, 5 };
  arr.assign(arrs.begin(), arrs.end());
  ```

- 用数组的值进行范围修改

  ```cpp
  //将arr替换为数组arrs
  vector<int> arr = {5, 4, 3, 2, 1};
  int arrs[5] = { 1, 2, 3, 4, 5 };
  arr.assign(arrs, arrs + 5);
  ```



### 3.3.5 swap 和 clear

swap将两个vector进行交换。

```cpp
vector<int> arr = {5, 4, 3, 2, 1};
vector<int> arrs = { 1, 2, 3, 4, 5 };
arr.swap(arrs);
```

clear清空整个vector，size变为0，但空间仍然存在。

```cpp
arr.clear();
```



## 3.4 vector二维操作

实际上，二维vector其实就是嵌套定义vector，那么对其进行操作我们可以从嵌套的vector得到单层的vector，就可以调用其方法了。

### 定义

```
vector<vector<int>> arr;						//定义一个空的二维vector
vector<vector<int>> arr(5, vector<int>(3, 1));	//定义一个5行3列值全为1的二维vector
```

### 访问

和二维数组一样通过 [] [] 访问即可。

```cpp
for (int i = 0; i < arr.size(); i++)
{
	for (int j = 0; j < arr[0].size(); j++)//注意如果arr为空不可直接arr[0]
	{
		cout << arr[i][j] << endl;
	}
}
```

或者用范围for：

```cpp
for (auto nums : arr)
{
    for (auto num : nums)
    {
    	cout << num << endl;
    }
}
```



### resize操作

```cpp
vector<vector<int>> arr;
arr.resize(5);
for (auto num : arr)
{
    num.resize(3);
}
```





# 四、vector扩容原理

前面我们提到，vector作为容器有着动态数组的功能，当加入的数据大于vector容量(capacity)时会**自动扩容**，系统会自动申请一片更大的空间，把原来的数据拷贝过去，释放原来的内存空间。

看以下一段代码：

```cpp
vector<int> arr;
for (int i = 0; i < 20; i++)
{
    arr.push_back(i);
    cout << arr.size() << " " << arr.capacity() << endl;
}
```

![image-20210528210140455](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210528210140455.png)

在VS中运行以上代码测试扩容，发现：

- 初始时capacity和size都是零；
- 开始capacity和size大小一致，在size=5时，capacity从4 -> 6，即发生了扩容：4 * 1.5 = 6，以1.5倍开始扩容。同样，在9、13、19时均是以1.5倍的方式扩容，向下取整。
- 其实，在capacity等于size时，下一次插入操作时vector就以1.5倍开始扩容。开始时，0 * 1.5 = 1（需要）, 1 * 1.5 = 2(此时需要)，2 * 1.5 = 3, 3 * 1.5 = 4，4 * 1.5 = 6,6 * 1.5 = 9 。。。



可以看到，理论上每次都是1.5扩容，但是遇到一些特殊情况如：0、1或者一次性插入多个元素时，也许1.5扩容就无法满足了。其实很简单，按照我们自己的思路，这无非是程序健壮性的体现，加一句判断语句即可。

看如下VS中vector扩容的源码：

```cpp
size_type _Calculate_growth(const size_type _Newsize) const {
    
    const size_type _Oldcapacity = capacity();
    const auto _Max              = max_size();

    //扩容后将超出max_size，返回max_size
    if (_Oldcapacity > _Max - _Oldcapacity / 2) {
        return _Max; 
    }
	//采取1.5倍扩容
    const size_type _Geometric = _Oldcapacity + _Oldcapacity / 2;
	//扩容后仍然小于新加入元素后的大小，以新加入元素后的大小为准
    if (_Geometric < _Newsize) {
        return _Newsize; 
    }

    return _Geometric;
}
```

由此可见，确实是以1.5倍扩容，并且还有需要判断是否超过max_size，已经是否小于newsize。

而其实，扩容时在插入时元素需要进行判断的，所以在vector的方法如：push_back、insert中都有用到扩容。



## push_back

1. 首先，在VS中，push_back有两个重载函数。_Ty就是vector模板的类型，如vector< int >中的int。发现push_back其实是调用了emplace_back成员函数。	

   ```cpp
   void push_back(const _Ty& _Val) { 
   	emplace_back(_Val);
   }
   
   void push_back(_Ty&& _Val) { 
   	emplace_back(_STD move(_Val));
   }
   ```

2. 接着，进入emplace_back函数。判断capacity和size是否相等，如果相等就进入_Emplace_reallocate函数。

   **tips**:对于size和capacity，代码里通过这三个指针实现内存管理。

   ![image-20210528221347665](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210528221347665.png)

   ```cpp
   template <class... _Valty>
       decltype(auto) emplace_back(_Valty&&... _Val) {
           
           auto& _My_data   = _Mypair._Myval2;
           pointer& _Mylast = _My_data._Mylast;
           if (_Mylast != _My_data._Myend) {
               return _Emplace_back_with_unused_capacity(_STD forward<_Valty>(_Val)...);
           }
   		
           _Ty& _Result = *_Emplace_reallocate(_Mylast, _STD forward<_Valty>(_Val)...);
   #if _HAS_CXX17
           return _Result;
   #else // ^^^ _HAS_CXX17 ^^^ // vvv !_HAS_CXX17 vvv
           (void) _Result;
   #endif // _HAS_CXX17
       }
   ```

3. 我们进入_Emplace_reallocate函数。在这函数里，首先会检查size是否等于max_size，超过最大值时触发错误。接着，就看到了之前提到的扩容函数 _Calculate_growth，修改capacity的值。

   ```cpp
   template <class... _Valty>
   pointer _Emplace_reallocate(const pointer _Whereptr, _Valty&&... _Val) {
       
       _Alty& _Al        = _Getal();
       auto& _My_data    = _Mypair._Myval2;
       pointer& _Myfirst = _My_data._Myfirst;
       pointer& _Mylast  = _My_data._Mylast;
   	
       _STL_INTERNAL_CHECK(_Mylast == _My_data._Myend);
   
       const auto _Whereoff = static_cast<size_type>(_Whereptr - _Myfirst);
       const auto _Oldsize  = static_cast<size_type>(_Mylast - _Myfirst);
   	
       if (_Oldsize == max_size()) {
           _Xlength();
       }
   	
       const size_type _Newsize     = _Oldsize + 1;
       const size_type _Newcapacity = _Calculate_growth(_Newsize);
   
       const pointer _Newvec           = _Al.allocate(_Newcapacity);
       const pointer _Constructed_last = _Newvec + _Whereoff + 1;
       pointer _Constructed_first      = _Constructed_last;
   
       _TRY_BEGIN
       _Alty_traits::construct(_Al, _Unfancy(_Newvec + _Whereoff), _STD forward<_Valty>(_Val)...);
       _Constructed_first = _Newvec + _Whereoff;
   
       if (_Whereptr == _Mylast) { // at back, provide strong guarantee
           _Umove_if_noexcept(_Myfirst, _Mylast, _Newvec);
       } else { // provide basic guarantee
           _Umove(_Myfirst, _Whereptr, _Newvec);
           _Constructed_first = _Newvec;
           _Umove(_Whereptr, _Mylast, _Newvec + _Whereoff + 1);
       }
       _CATCH_ALL
       _Destroy(_Constructed_first, _Constructed_last);
       _Al.deallocate(_Newvec, _Newcapacity);
       _RERAISE;
       _CATCH_END
   	
       _Change_array(_Newvec, _Newsize, _Newcapacity);
       return _Newvec + _Whereoff;
   }
   ```

   



