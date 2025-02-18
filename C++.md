我操了，东西都没了
# override
## virtual function
overload的函数，需要重新写，否则只能继承父类中第一个函数
非虚函数，编译器执行静态绑定，通过base类型引用调用非虚函数，即调用base类型中的函数
虚函数，执行动态绑定，通过base类型引用derived类型调用虚函数，则调用derived类型中的函数
- 然而二者的参数都是静态绑定，即使用base类型中的参数
```cpp
class Base {
public:
    void func() { std::cout << "Base::func()" << std::endl; }
};

class Derived : public Base {
public:
    void func() { std::cout << "Derived::func()" << std::endl; }
};

Base* b = new Derived();
b->func();  // "Base::func()"
```

```cpp
class Base {
public:
    virtual void func() { std::cout << "Base::func()" << std::endl; }
};

class Derived : public Base {
public:
    void func() override { std::cout << "Derived::func()" << std::endl; }
};

Base* b = new Derived();
b->func();  // "Derived::func()"
```

## 输入输出操作符重载
```cpp
friend ostream &operator<<( ostream &output, const Distance &D ) { 
	output << "F : " << D.feet << " I : " << D.inches;
	return output; 
} 
	friend istream &operator>>( istream &input, Distance &D ) { 
	input >> D.feet >> D.inches; return input; 
}
```
- 如果不使用友元函数，会变成`Distance<<cout`
### 自定义输出
```cpp
ostream &tab( ostream &output ) {
	output << “\t”;
	return output;
}
```
## Prevent implicit conversions
*隐式转换*
```cpp
	explicit A();
```
### `static_cast`
显式类型转换
`static_cast<type>(expression)`
### `dynamic_cast`
向下转换时会做类型检查
### `const_cast`
Modify the const or volatile property.
### `reinterpret_cast`
Convert ptr or reference to int
# Template
Generic Programing
Reuse source code
- Function template
	
- Class template
	Typical: containers `stack <int>`
- Can use multiple types: 
```cpp
template <class Key, class Value>
class Table { … }
```
- 模板嵌套时，尖括号之间加空格
- Template parameters:
```cpp
template <class T, int bound = 100>
class List {
public:
	T element[bound];
}

List <int, 20>;
```
- 不同数据类型模板的静态成员函数不互通
## Exception 
`try` and `catch`
# Smart Pointer
- `unique_ptr`
    - 不能复制，可以移动
    - 某个对象只能被一个 `unique_ptr` 指向
- `shared_ptr`
    - 记录对象被指向的次数
    - 当这个次数减少到 0，对象会被销毁
    - 内存管理更方便
- `weak_ptr`
    - 指向 `shared_ptr` 指向的对象，而不增加它的计数
## 实现
- `UCObject`：有计数的对象
```cpp
#include <assert.h>
class UCObject {
public:
  UCObject() : m_refCount(0) { }
  virtual ~UCObject() { assert(m_refCount == 0);};
  UCObject(const UCObject&) : m_refCount(0) { }
  void incr() { m_refCount++; }
  void decr();
  int references() { return m_refCount; }
private:
  int m_refCount;
};

inline void UCObject::decr(){ 
  m_refCount -= 1;
  if (m_refCount == 0) { 
    delete this;
  }
}
```
- `UCPointer`：使用模板的智能指针
    - 重载 `->` 和 `*` 运算符
```cpp
template <class T>
class UCPointer {
private:
  T* m_pObj;
  void increment() { if (m_pObj) m_pObj->incr(); }
  void decrement() { if (m_pObj) m_pObj->decr(); }
public:
  UCPointer(T* r = 0): m_pObj(r) { increment();}
  ~UCPointer() { decrement(); };
  UCPointer(const UCPointer<T> & p);
  UCPointer& operator=(const UCPointer<T> &);
  T* operator->() const {
      return m_pObj;
  };
  T& operator*() const { return *m_pObj; };
};
```
- Copy Ctor 增加计数：
```cpp
template <class T>
UCPointer<T>::UCPointer(const UCPointer<T> & p){ 
  m_pObj = p.m_pObj;
  increment();
}
```
- 赋值会检查是否本来就指向同一对象：
```cpp
template <class T>
UCPointer<T>&
UCPointer<T>::operator=(const UCPointer<T>& p){
    if (m_pObj != p.m_pObj) {
        decrement();  
        m_pObj = p.m_pObj;  
        increment();
    }
    return *this;
}
```
# Multiple Inheritance
```cpp
class B1 { int m_i; };
class D1 : public B1 {};
class D2 : public B1 {};
class M : public D1, public D2 {};

int main() {
   M m;  // OK
   B1* p = &m; // ERROR: which B1
   B1* p2 = static_cast<D1*>(&m); // OK
   return 0;
}
```
- 父类转化为子类用 `dynamic_cast`
- 显式地指定使用 `D1` 的路径
- 逻辑错误：
```cpp
m.m_i++; //ERROR: D1::B1.m_i or D2::B1.m_i?
```
- 解决方式：双冒号或 `protocal/interface class`：虚基类/抽象基类
    - 提供定义数据，但不提供具体实现
    - Non-static 成员函数：除了析构函数都是虚函数
    - Static 成员函数属于类，因此不作限制
在基类中声明成员函数为 `virtual`，使用虚函数表间接访问。将基类声明为 `virtual`，它的子类们会共享基类的一份单独的拷贝。
- 编译器会做额外的工作
```cpp
class B1 { int m_i; };
class D1 : virtual public B1 {};
class D2 : virtual public B1 {};
class M : public D1, public D2 {};

int main() {
   M m;  // OK
   m.m_i++;
   B1* p = new M; // OK
   return 0;
}
```
- 在创建 `M` 对象时，只会调用一次 `B1` 的构造函数，然后分别调用 `D1` 和 `D2` 的，最后是 `M` 的
- 虚基类构造函数会在非基类构造函数之前被调用
    - 然而根据环境不同仍可能有变化，所以要避免菱形继承
# Class Design

# Exam
## C++特性
Const 位置
封装
继承
多肽
- 继承、多肽、异常（catch、throw）、模板
- 重载
Static member
## 