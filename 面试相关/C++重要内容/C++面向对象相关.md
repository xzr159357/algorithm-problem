# 一、面向对象

面向对象的三大特点：**封装**、**继承**、**多态**。当然讲四大特点可以加上**抽象**。



# 二、封装

**定义**：将数据和信息包装在单个单元中。在面向对象的编程中，封装被定义为将数据和操纵它们的功能绑定在一起。

比如，一家公司有销售部门、技术部门、财务部门，销售部门有对应销售数据，而如果财务部门想要查看销售部门的数据时，需要先通过销售部门的人员才能得到相应的数据。而这就是**封装**，外部人员访问相关部门时需要通过某种渠道、接口才能够获得数据。

而在代码中，具体体现在**类**的编写过程中，有private、public、protected修饰符。定义在private中的成员变量，外部只能够通过友元或类对外提供的接口访问。其中，友元函数、友元类会破会类的封装性，一般不使用。



# 三、继承

- 定义：一个类（子类）从另一个类（父类）获得成员函数和成员变量的过程。

- C++有三种继承：public、private、protected，如果不写参数默认为private。一般在工程中只采用public继承。

- C++支持多继承，多继承功能强大，使得C++不需要像java、C#引入接口。**接口**：一个不能实例化，且只能拥有抽象方法的类型。接口为没有亲缘关系的类提供通用功能。当然多继承使用起来也比较麻烦，可能带来几个问题：

  - 菱形继承。D继承B和C，而B和C同时继承A。如果A中有个成员变量a，那么D无法知道是从A->B->D来的a还是A->C->D来的a。当然可在前面用域运算符(::)指明是哪个元素：B::a,C::a。

    ![image-20210520185016984](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210520185016984.png)

  - 为了解决多继承时的命名冲突和冗余数据，C++提出了虚继承，使得在派生类中只保留一份间接基类的成员。

    ```cpp
    class A
    {
    protected:
        int m_a;
    };
    
    class B : virtual public A	//虚继承
    {
    protected:
        int m_b;
    };
    
    class C : virtual public A	//虚继承
    {
    protected:
        int m_c;
    };
    
    class D : public B, public C
    {
    protected:
        void seta(int x) {m_a = x;}
        void setb(int x) {m_b = x;}
        void setc(int x) {m_c = x;}
        void setd(int x) {m_d = x;}
    private:
        int m_d;
    };
    ```

    使用虚继承实现上述的菱形继承，就不会出现多份A的成员变量了，访问a时也不用指定某个类了。

  

  - 虚继承的目的是让某个类做出声明，承诺愿意共享它的基类。这个被共享的基类就称之为**虚基类**，本例中的A就是个虚基类。在这种机制下，不论虚基类在后面的继承中出现多少次，继承的类都只包含一份虚基类的成员。

    ![image-20210520190541519](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210520190541519.png)







# 四、虚函数和纯虚函数

## 虚函数

​		**多态**是面向对象编程的一大特点，而虚函数是实现多态的机制。多态使得程序调用的函数是在**运行时**确定的，而不是编译时静态确定的。使用基类对象或引用指向子类对象，通过虚函数实现对子类的访问，就是实现多态的一个案例。

- 虚函数：在函数**声明**前面加上**virtual**，如：virtual void func();

- 纯虚函数：在虚函数的基础上加上=0，如：virtual void func() = 0;
- 虚函数，实现了运行时多态；重载，实现了编译时多态。
- 子类不实现虚函数，默认调用基类的虚函数。



## 普通函数内存

```cpp
class Base
{
public:
    virtual void vir_show()
    {
        cout << "vir_show Base" << endl;
    }
    void show()
    {
        cout << "show Base" << endl;
    }
};

class A : public Base
{
public:
    virtual void vir_show()
    {
        cout << "vir_show A" << endl;
    }
    void show()
    {
        cout << "show A" << endl;
    }
};

class B : public Base
{
public:
    virtual void vir_show()
    {
        cout << "vir_show B" << endl;
    }
    void show()
    {
        cout << "show B" << endl;
    }
};


int main()
{
    Base* base = new Base();
    Base* a = new A();
    Base* b = new B();
    base->show(); a->show(); b->show();
    cout << "###########################" << endl;
    base->vir_show(); a->vir_show(); b->vir_show();
    cout << "###########################" << endl;
    ((A*)b)->show(); ((A*)b)->vir_show();

    return 0;
}
```

​		通过以上代码的运行结果，可以看到，**当使用类的指针调用成员函数时，普通函数由指针类型决定，而虚函数由指针指向的实际类型决定**。将b强转为a，调用普通函数显示A，而虚函数仍然显示B。

​		这是因为，普通函数放在代码区，为该类所有对象共用，而成员变量则在堆区，为各个对象私有。在调用成员函数时，程序会根据类的类型，在对应的代码区找到对应的函数并调用。其实就是根据指针的类型调用对应的函数，如强转为A，就调用类A的函数。

## 

![image-20210520195622176](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210520195622176.png)

## 虚函数内存

​		那么虚函数的内存分配又是如何的？如以下代码：

```cpp
class C
{
public:
    void fun1() {}
    void fun2() {}
    int x;
};
class D
{
public:
    void fun1() {}
    virtual void fun2() {}
    int x;
};
```

​		定义了C和D，其中D的函数fun2()是虚函数，这时候我们使用sizeof计算C和D的大小时会发现，D比C大四个字节。首先来看内存分配：

![image-20210520200948280](C:\Users\DELL\AppData\Roaming\Typora\typora-user-images\image-20210520200948280.png)

​		类D多出的四个字节是虚函数表指针vptr，这个指针指向一张名为“虚函数表”(vtbl)的表，表中的数据为函数指针，存储了虚函数fun2()具体实现对应函数的位置，即地址。

- 普通函数、虚函数、虚函数表都是同一个类的所有对象公有的。

- 成员变量、虚函数表指针是每个对象私有的。

- sizeof()计算的值只包括成员变量和虚函数表指针，所以D有虚函数表指针所以比C多四个字节。

- 即使类有**多个**虚函数，也**只有一个虚函数表指针**，同时虚函数表里会有多个函数指针，指向对应的虚函数实现位置。

- 虚函数实现的过程是：**通过对象内存中的虚函数指针vptr找到虚函数表vtbl，再通过vtbl中的函数指针找到对应虚函数的实现区域并进行调用。**

  所以，虚函数的调用不由指针的类型决定，而由虚函数指针指向的位置所决定。



## 纯虚函数

- 纯虚函数可以看做一个抽象的个体，例如动物可能有猫、狗、老虎等，但动物本身就是一个抽象的概念，不存在诸如吃、叫等方法的实现，所以可以将吃、叫等方法定义为纯虚函数，基类不实现其方法，由派生类实现。
- 含有纯虚函数的类称之为**抽象类**，它不能生成对象（创建实例），只能创建它的派生类的实例。
- 子类实现了纯虚函数，该纯虚函数在子类中就变成了虚函数。
- 如果派生类中没有重新定义实现纯虚函数，而只是继承基类的纯虚函数，则这个派生类仍然还是一个抽象类。
- 纯虚函数可以理解为提供一个接口，基类无法实现该接口，由继承的派生类实现对应的操作。







# 五、参考文章

- [C++继承](https://zhuanlan.zhihu.com/p/29581695)
- [C++虚函数多态](https://zhuanlan.zhihu.com/p/37331092)

