>STL在项目中或者在竞赛中，都有大量的使用。下面将讲解STL的入门，包括历史、STL组件等内容。



# 一、STL历史

- 1994 年 7 月，美国国家标准与技术研究院通过投票决定，将 STL 纳入 C++ 标准，使之成为 C++ 库的重要组成部分。1997 年，C++ 标准完成了最近一次的修改，官方名称为 ISO/IEC 14882。

- STL 是 C++ 的一部分，不用额外安装，被内建在支持 C++ 的编译器中。

- STL 是 C++ 标准程序库的核心。STL 内的所有组件都由模板构成，其元素可以是任意型别。



# 二、STL组成

STL 是 C++ 通用库，由迭代器、算法、容器、仿函数、配接器和配置器（即内存配置器）组成。

## 2.1 容器

容器是用来存储并管理某类对象的集合。容器都是**模板类**，可存储各种类型的数据。

容器分为两类：

### （1）顺序容器

顺序容器有以下三种：

- vector，可变长动态数组
- deque，双端队列
- list，双向链表

顺序容器，**元素在容器内的位置同元素大小无关**，插入（尾部、头部、中间）后元素的位置就已固定。

### （2）关联容器

关联容器有以下四种：

- set
- multiset
- map
- multimap

关联容器，容器内的元素是按一定规则排序的。插入元素后，容器会根据一定的规则将元素插入合适的位置。

### （3）容器配接器

```
stack <Container>;
queue <Container>;
deque <Container>;
```

stack实现栈的功能，queue实现队列的功能，deque为双端队列。



### （4）C++11新增容器

- array

  - 先回答第一个问题，`std::vecotr` 太强大了，以至于我们没有必要为了去敲碎一个鸡蛋而用一个钉锤。使用 `std::array` 保存在**栈内存**中，相比堆内存中的 `std::vector`，我们就能够灵活的访问这里面的元素，从而获得更高的性能；同时正式由于其堆内存存储的特性，有些时候我们还需要自己负责释放这些资源。

  - 而第二个问题就更加简单，使用`std::array`能够让代码变得更加现代，且封装了一些操作函数，同时还能够友好的使用标准库中的容器算法等等，比如 `std::sort`。
  - `std::array` 会在编译时创建一个固定大小的数组，`std::array` 不能够被隐式的转换成指针，使用 `std::array` 很简单，只需指定其类型和大小即可：

- forward_list

  需要知道的是，和 `std::list` 的双向链表的实现不同，`std::forward_list` 使用单向链表进行实现，提供了 `O(1)` 复杂度的元素插入，不支持快速随机访问（这也是链表的特点），也是标准库容器中唯一一个不提供 `size()` 方法的容器。当不需要双向迭代时，具有比 `std::list` 更高的空间利用率。

- unordered_set/unordered_multiset

- unordered_map/unordered_multimap

  - 我们已经熟知了传统 C++ 中的有序容器 `std::map`/`std::set`，这些元素内部通过红黑树进行实现，插入和搜索的平均复杂度均为 `O(log(size))`。在插入元素时候，会根据 `<` 操作符比较元素大小并判断元素是否相同，并选择合适的位置插入到容器中。当对这个容器中的元素进行遍历时，输出结果会按照 `<` 操作符的顺序来逐个遍历。
  - 而无序容器中的元素是不进行排序的，内部通过**哈希表**实现，插入和搜索元素的平均复杂度为 `O(constant)`，在不关心容器内部元素顺序时，能够获得显著的性能提升。
  - 它们的用法和原有的 `std::map`/`std::multimap`/`std::set`/`set::multiset` 基本类似

- tuple

  - 了解过 Python 的程序员应该知道元组的概念，纵观传统 C++ 中的容器，除了 `std::pair` 外，似乎没有现成的结构能够用来存放不同类型的数据（通常我们会自己定义结构）。但 `std::pair` 的缺陷是显而易见的，只能保存两个元素。

## 成员函数

**顺序容器和关联容器**都有的成员函数：

- int size()：返回容器对象中元素的个数。
- bool empty()：判断容器对象是否为空。
- begin()：返回指向容器中第一个元素的迭代器。
- end()：返回指向容器中最后一个元素后面的位置的迭代器。
- rbegin()：返回指向容器中最后一个元素的反向迭代器。
- rend()：返回指向容器中第一个元素前面的位置的反向迭代器。
- erase()：从容器中删除一个或几个元素。
- clear()：从容器中删除所有元素。



**顺序容器**拥有的成员函数：

- front()：返回容器中第一个元素的引用。
- back()：返回容器中最后一个元素的引用。
- push_back()：在容器末尾增加新元素。
- pop_back()：删除容器末尾的元素。
- insert()：插入一个或多个元素。

## 2.2 迭代器

- 迭代器（Iterator）是一种检查容器内元素并遍历元素的数据类型。

- 迭代器是一个变量，相当于容器和操纵容器的算法之间的中介。
- 迭代器可以访问容器中的某个元素，如：map< int, int >::iterator it;那么`it->first`和`it->second`分别访问第一、二个参量。
- 迭代器的操作有点类似于指针。

## 2.3 算法

STL提供了很多数据结构的算法。头文件 <algorithm>。

常用算法如下：

- for_each()；
- find()；
- find_if()；
- count()；
- count_if()；
- replace()；
- replace_if()；
- copy()；
- unique_copy()；
- sort()；
- equal_range()；
- merge()；

## 2.4 仿函数

仿函数(functor)，就是使一个类的使用看上去像一个函数。其实现就是在类中实现一个`operator()`，这个类就有了类似函数的行为，就是一个仿函数类了。

例子：

我们需要计算vector< string >中字符串长度小于3的个数，通过算法中的count_if算法，可以这么写：

```
bool isLess3Word(const string& str)
{
    return str.size() < 3;
}

int main()
{
    vector<string> arr{ "ab", "abc", "b", "abcd" };
    cout << count_if(arr.begin(), arr.end(), isLess3Word);
}
```

那么问题是，如果我们要计算长度小于5的个数，我们就需要3改成5，这样代码的封装性不很好。或者将3改为宏、全局变量等，都不是很好的解决方案。



所以我们可以利用仿函数：

```
class LessNWord
{
public:
    bool operator () (const string& str)
    {
        return str.size() < len;
    }
    LessNWord(int size) : len(size) {}
private:
    int len;
};

int main()
{
    vector<string> arr{ "ab", "abc", "b", "abcd" };
    cout << count_if(arr.begin(), arr.end(), LessNWord(3));
}
```

可以看到，仿函数通过实现operator()函数，通过访问类而访问该函数。从而解决len参数的问题，因为count_if中第三个参量函数指针是只能够有1个参数，而无法传入len的长度。



当然，对于本题，使用lambda表达式是个不错的选择：

```cpp
int main()
{
    vector<string> arr{ "ab", "abc", "b", "abcd" };
    cout << count_if(arr.begin(), arr.end(), [&](const string& str) {return str.size() < 3; });
}
```



## 2.5 配接器

配接器可以实现不同类之间的数据转换。最常用的配接器有 istream_temtor，它提供了函数复制的接口。配接器对于 STL 技术来说非常重要。







## 2.6 配置器

配置器，allocators，主要负责空间的配置和管理。STL容器需要配置一定的空间，而程序员只要调用容器即可，容器的空间配置就由配置器完成。







# 三、参考文章

- http://c.biancheng.net/view/1436.html
- http://c.biancheng.net/view/331.html











