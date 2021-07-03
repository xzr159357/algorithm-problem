# 一、std::string 简介

- string可以理解为C++的字符串，本质上是STL的一个类。

- 区别于java中的String是语言内置的字符串，C++的std::string需要包含头文件：`#include <string>`。

- std::string本质上是模板`basic_string`对于char的特例。

  ```cpp
  using string  = basic_string<char, char_traits<char>, allocator<char>>;
  ```

- std::string和STL容器很类似，可以通过迭代器、使用algorithm的算法。和`vector<char>`非常相似。

