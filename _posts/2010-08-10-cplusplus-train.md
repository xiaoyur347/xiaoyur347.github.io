---
title: C++培训练习题
layout: post
category: 嵌入式
tags: [培训]
---
{% include JB/setup %}
# C++培训练习题
---

<!--break-->

# 1. 构造和静态构造

```C++
#include <iostream>
using namespace std;
class A
{
public :
     A(int a = 0)
          : m_a(a)
     {
          cout << "A() : m_a = " << m_a << endl;
     }
     ~A()
     {
          cout << "~A() : m_a = " << m_a << endl;
     }
protected:
     int   m_a;
};

A     a1(10);
void fun()
{
     static A     a2(20);//第一次调用时初始化
     A     a3(30);
}
A     a4(40);
void fun1()
{
     A a5(50);
     return;
}
int main(int argc, char *argv[])
{
     fun1();
     fun();
     fun();
     return 0;
} 
```

构造顺序:
初始化全局变量（静态全局变量）->开始执行。
静态全局变量，在第一次调用时构造。

1. 全局变量a1，a4
2. fun1->a5（然后就析构）
3. fun->静态变量a2
4. fun->a3（然后就析构）
5. fun->a3（然后就析构）

```shell
A() : m_a = 10
A() : m_a = 40
A() : m_a = 50
~A() : m_a = 50
A() : m_a = 20
A() : m_a = 30
~A() : m_a = 30
A() : m_a = 30
~A() : m_a = 30
~A() : m_a = 20
~A() : m_a = 40
~A() : m_a = 10
```

# 2. 构造和成员构造

```C++
#include <iostream>
using namespace std;
class C
{
public :
     C(int   c) : m_c(c)
     {
          cout << "C(int c)" << endl;
     }
     ~C()
     {
          cout << "~C()" << endl;
     }
private:
     int  m_c;
};

class B
{
public :
     B(int b) : m_b(b)
     {
          cout << "B(int b)" << endl;
     }
     ~B()
     {
          cout << "~B()" << endl;
     }
private:
     int   m_b;
};

class A
{
public:
     A(int b, int c) :m_c_obj(c), m_b_obj(b)//不是根据此处的顺序初始化的，而是根据定义顺序
     {
          cout << "A(int b)" << endl;
     }

     ~A()
     {
          cout << "~A()" << endl;
     }
private:
     B    m_b_obj;
     C    m_c_obj;
};

int main(int argc, char *argv[])
{
     A    a(10, 20);
     return 0;
} 
```

new：先调用基类构造函数，再按定义的顺序依次构造成员变量，再调用当前类的构造函数。

delete：与new相反。

虚函数的作用是为了用父类的指针来操作子类！！

# 3. 类指针

```C++
class B
{
public:
      B(int b = 0) : m_b(b)
      {
            cout << "B(int b)" << endl;
      }
      ~B()
      {
            cout << "~B()" << endl;
      }
      void print()
      {
             cout << "m_b = " << m_b << endl;
      }
private:
      int   m_b;
};
 
class A
{
public:
      A(int a = 0) : m_a(a)
      {
            cout << "A(int a)" << endl;
      }
      ~A()
      {
            cout << "~A()" << endl;
      }
      void print()
      {
            cout << "m_a = " << m_a << endl;
      }
private:
      int    m_a;
};
 
void print(A *athis)
{
      athis->print();
}
 
void print(B *bthis)
{
      bthis->print();
}
 
int main(int argc, char *argv[])
{
      B    b(10);
      b.print();
      print(&b);
      
      A    *p = (A *)&b;
      p->print();//调用A类的打印函数，实际上使用B中的数据（一定要有指针的概念）
      print(p);
      
      return 0;
}
```

使用类的指针不会初始化类，类的大小=类中成员变量的大小（+4（如果存在虚函数，增加一个指向虚函数表的指针））。

对于非虚函数，调用哪个成员函数取决于其指针。对于虚函数，则取决于虚函数表中存放的指针。

# 4. 类指针

```C++
#include <iostream>
using namespace std;
class A
{
public:
     void Print();
public:
     int x[8188];
};
void A::Print()
{
     cout << x << endl;
}

class B: public A
{
public:
     void Print1();
     int z[40960];
     int y;
};
void B::Print1()
{
     cout << "B::Print1()" << endl;
     cout << y << endl;
}
int main(int argc, char *argv[])
{
     B *pB = (B*)new A();
     pB->Print1();//调用B类的函数，会导致崩溃
     return 0;
}
```

对于非虚函数，调用哪个成员函数取决于其指针。对于虚函数，则取决于虚函数表中存放的指针。

# 5. 类指针

```C++
class A
{
public:
      A()
      {
            cout << "A()" << endl;
      }
      ~A()
      {
            cout << "~A()" << endl;
      }
};
 
class B : public A
{
public:
      B()
      {
            cout << "B()" << endl;
      }
      ~B()
      {
            cout << "~B()" << endl;
      }
};
 
int main(int argc, char *argv[])
{
      A    *p = new B;
      delete p;//非虚函数，调用~A()
      return 0;
}
```

对于非虚函数，调用哪个成员函数取决于其指针。对于虚函数，则取决于虚函数表中存放的指针。