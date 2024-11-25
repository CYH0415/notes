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