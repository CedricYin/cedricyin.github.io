## 0 basics of class

- c++中，class和struct的区别
    
- include尖括号和双引号的区别
    
- 尖括号会从编译器默认的路径去找
    
- 双引号不仅会从编译器默认的路径找，**还会从当前文件所在的目录找**
    
- 成员函数定义在类内和类外的区别
    
- **内：inline的（即使不加inline）**
    
- 外：不是inline的
    
- 因此，通常**把简单的函数直接实现在类内**；把复杂的函数在类外实现
    

```C++
class Student {
private:
  string name;
  int age;

public:  
  // 默认是inline的
  void setName(string name) {
    this->name = name;
  }

  // 默认是inline的
  void setAge(int age) {
    this->age = age;
  }

  void printInfo();
};
```

- 一般把类的定义放在hpp文件，把定义在类外的函数放在cpp文件

```C++
// 默认是非inline的，如果要inline，需要手动加inline关键字
void Student::printInfo() {
    cout << name << ' ' << age << endl;
}
```

- 构造函数的写法
    
- 默认情况下，编译器会生成一个空的无参构造函数
    
- **如果自定义了有参构造，编译器就不会再生成无参构造了**
    
- 简洁写法（**初始化列表**）
    
    ```C++ Student(string n, int a) { this->name = n; this->age = a; cout << "Student(string name, int age)" << endl; }
    

// 简洁写法 Student(string n, int a): name(n), age(a) { cout << "Student(string name, int age)" << endl; }

```
- 创建对象的方法（**不管栈上堆上**，创建对象时都会调用对应的构造函数）
```

C++ // 在栈上创建对象，二者等价 Student st0{"asd", 1}; // 更简洁 Student st1 = Student{"asd", 2};

// 在堆上创建对象 Student *st2 = new Student{"asd", 3};

```
- 析构函数

  - **不能重载，没有形参，~函数名**

  - 用于释放资源

  - 在栈上的对象，**当离开作用域时，对象调用析构函数**，销毁；在堆上的对象需要**显式调用delete**来执行析构函数，销毁
```

C++ // … ~Student() { cout << "~Student()" << endl; } // …

int main() { {Student st0{"asd", 1};} // st0离开了作用域，析构函数被调用 Student st1 = Student{"asd", 2}; Student *st2 = new Student{"asd", 3}; delete st2; // st2被销毁 return 0; // 函数返回，st1被销毁 }

```
- 为什么要引入this指针？

  - 考虑一个情况：创建对象时，**每个对象所拥有的内存数据是否需要包含函数指令？**如果需要，那么创建多个对象时将会有许多重复的指令。因此所有对象应该共享函数的代码段，独自拥有自己的成员变量的数据。那么，**当执行成员函数时，该函数怎么知道是哪个对象调用了自己，该函数需要成员变量参与计算怎么办，该用哪个对象的成员变量数据？**由此我们需要this指针，指向当前调用该函数的对象

  - **非static成员函数隐含的第一个形参是this**

![](Pasted%20image%2020240516185927.png)

  - 还能消除歧义：当形参和成员变量名相同时，使用**this→成员变量可以消除歧义**

- const成员

  - const成员变量：**必须初始化**，无法被修改

  - const成员函数：**const放在函数签名后花括号前，无法修改成员变量**
```

C++ int func() const { age ++; // error }

```
- static成员

  - 创建

    ```C++
class S {
  // ...
  static int val;  // 不能在类内初始化（C++17以前）
  inline static int v = 10;  // C++17支持static成员变量在类内初始化，需要加inline关键字
  // ...
  static void getV();  // static成员函数既可以在类内初始化，也可以在类外初始化，这里以类外为例
  // ...
}

// static成员在类外初始化时，不能再带static
int S::val = 0;  // 静态成员变量必须在类外初始化（C++17以前）
void Student::getV() {
  cout << v << endl;
}
```

- 使用
    
    - 可以使用**类名::成员名**
        
    - 如果已经创建对象，**也可以通过对象名来访问**（.或→）
        
- 注意事项
    
    - static成员函数**不能访问非static成员**

## 1 advances of class

- 运算符重载
    
- 作用：将C++运算符作用于自定义类型
    
- 原理：重载的运算符本质上是有特殊名字的函数（**operator开头，运算符结尾**）
    
    ```C++ string s = "asd"; // 字符串字面量可以自动转成string // 下面二者等价 s += "a"; s.operator+=("a");
    

```
  - 以时间处理为例

    - 一般都需要**返回类的类型，以此来支持链式调用**

    ```C++
class MyTime {
private:
  int hour;
  int min;

public:
  MyTime(): hour(0), min(0) {}
  MyTime(int h, int m): hour(h), min(m) {}

  // 运算符重载
  MyTime operator+(const MyTime &t) const {  // 返回值为MyTime，可以实现链式调用a+b+c+d...
    MyTime sum;
    sum.hour = this->hour + t.hour;
    sum.min = this->min + t.min;
    sum.hour += (sum.min / 60);
    sum.min %= 60;
    return sum;
  }

  MyTime& operator+=(const MyTime &t) {  // 返回MyTime&可以实现：(a+=b)+c诸如此类的操作
    this->hour += t.hour;
    this->min += t.min;
    this->hour += this->min / 60;
    this->min %= 60;
    return *this;
  }

  void getTime() {
    cout << hour << ':' << min << endl;
  }
};

int main() {
  MyTime t1{4, 20}, t2{1, 10};
  (t1 + t2).getTime();
  t1 += t2;
  t1.getTime();
  return 0;
}
```

- 形参的类型也可以很灵活
  ```C++
MyTime operator+(const int min) const { // const 必须加，不然后续friend函数形参如果传进一个const MyTime类型，则无法使用该函数：因为const的this只能访问const成员 MyTime sum; sum.hour = this->hour ; sum.min = this->min + min; sum.hour += (sum.min / 60); sum.min %= 60; return sum;  
}

MyTime operator+(const string &h) const { MyTime sum; if (h == "one hour") sum.hour = this->hour + 1; sum.min = this->min ; return sum;  
}

t1 + 20; (t1 + "one hour").getTime();

```
- 友元函数

  - 可以解决**20+t1**无法调用的问题

  - **不是成员函数**；**可以在类内部定义，也可以只在类内部声明，在类外定义**；可以**访问类的所有成员**

    - 因为不是成员函数，因此在类外定义的时候不需要加类名的限定符::
```C++
friend MyTime operator+(int min, const MyTime& t) { return t + min; }

20 + t1;
```
  - **对重载运算符（函数）的理解：需要理解成函数，C++原意中有单目、双目、三目运算符，对应的，重载的运算符的形参有1个、2个、3个。上面的例子中，+是个双目运算符，因此可以有两个形参**

    - 而非友元函数的运算符重载中，为什么函数的形参会少一个？**this这个形参默认是第一个参数，被隐藏了**

    - **调用重载运算符函数时，实参的顺序要和形参顺序相同**。上面的例子中形参是int，MyTime，对应的，实参是20 + t1，对应位置的参数类型相同

  - 常用的场景

    - 当我们要**cout和cin**一个MyTime类型的对象时，**不可能去标准库中修改cout和cin的代码进行运算符<<和>>的重载，因此可以使用友元函数，在MyTime类中实现重载**

    ```C++
friend ostream& operator<<(ostream& os, const MyTime& t) {
  string s = to_string(t.hour) + ':' + to_string(t.min);
  os << s;
  return os;
}

cout << 20 + t1 << endl;  // 再也不用getTime()了
```

- 重载类型转换
    
- 隐式重载：支持隐式和显式类型转换
    
    ```C++ 
    operator int() const { return this->hour * 60 + this->min; }
    

// 结果全都相同 cout << (int) t1 << endl; cout << int(t1) << endl; int a = t1; // 这里发生了隐式类型转换

```
  - 显式重载：使用explicit关键字，**不允许隐式类型转换**

    ```C++
explicit operator int() const {
  return this->hour * 60 + this->min;
}

cout << (int) t1 << endl;
cout << int(t1) << endl;
int a = t1;  // 这里发生了隐式类型转换，error
```

- 当使用赋值运算符时，何时调用重载赋值运算符？何时调用构造函数？

```C++
MyTime(int n);  // 构造函数
MyTime& operator=(int n);  // 重载运算符

MyTime t1 = 30;  // 调用构造函数，因为这里不是赋值，这里算初始化
MyTime t2;  // 调用无参构造
t2 = 40;  // 调用重载运算符
```

- 重载自增和自减运算符
    
- 重载后缀自增或自减时，形参要加int，有点奇怪
    
    - gpt解答：**后缀自增运算符**（`operator++(int)`）：它接受一个额外的整数参数（通常是一个无用的整数0），这**使得编译器能够区分前缀和后缀版本。**

```C++
// 前缀++
MyTime& operator++() {
  this->min++;
  this->hour += this->min / 60;
  this->min %= 60;
  return *this;  
}

// 后缀++
MyTime operator++(int) {
  MyTime old = *this;
  operator++();
  return old;
}

MyTime t1{1, 59};
cout << t1++ << endl;  // 1::59
cout << ++t1 << endl;  // 2::1
```

- 所有可重载的运算符
    
    ![](Pasted%20image%2020240516190057.png)