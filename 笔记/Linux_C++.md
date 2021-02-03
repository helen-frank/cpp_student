# 基础

##  1 命名空间

### 1.1 名称空间的定义

基本格式

```c++
namespace 名称空间名
{
    int a;
    int add(int,int);
}
```

- 定义在名称空间中的变量或者函数都称为实体
- 名称空间中的实体作用域是全局的, 并不意味着其可见域是全局的
- 如果不使用作用域限定符和using机制，抛开名称空间嵌套和内部屏蔽的情况，实体的可见域是从实体创建到该名称空间结束
- 在名称空间外，该实体是不可见的

### 1.2 名称空间的使用方式

#### 1.2.1 作用限定符

在某个名称空间中定义或创建的程序实体，如果要在其他名称空间中或外部函数中访问，可以使用作用域限定符来使实体可见，如：

```c++
std::cout <<  1 << std::endl;
```

#### 1.2.2 using声明机制

- 如果不希望在每次使用名称空间中实体时都使用作用域限定符，可使用using声明机制扩展其可见域，using声明的基本格式为：

  ```c++
  using 名称空间::实体名;
  ```

- 如“using A::dispA;”，至于using声明语句将该实体的可见域扩展到什么程度，这取决于using语句的书写位置，换言之，这取决于using语句的可见域。

#### 1.2.3 using声明机制二义性

- 如果using声明使用不当，很容易引起多重声明错误，比如:
  已经定义了全局函数disp，却还使用全局using声明语句“using A::disp”，假设没有屏蔽发生，那么调用disp()时，编译器不确定是全局函数版本还是A::disp()，引发多重声明错误。

- 变量名同样存在这种问题
  假设有两个名称空间A、B中都定义了int型变量num，在程序的某处需要使用num，如果写出如下代码：

  using A::num;
  using B::num;
  num=5;

#### 1.2.4 using编译指令

using声明机制使得某个空间中的特定实体可见，using编译指令比using声明更进一步，通过      “using namespace 名称空间名;”的形式，使得名称空间中的所有实体都可见，不再需要作用域限定符。如

```c++
using namespace std;
```

### 1.3 名称空间的作用域和可见域

- 原则上讲，名称空间的作用域是全局的，但其可见域却并非如此，而且，不论使用限定符还是使用using声明语句，都要求名称空间可见
- 回头看以下前面给出的代码，如果将namespace B的定义放在namespace A定义之后，编译器将指出错误，using语句同样如此，如果在using声明时，namespace尚未定义，或者说namespace已经定义，但声明的实体尚未包含在此namespace中，编译器同样会指出错误，
- 因此，名称空间同样要先定义、后使用。

#### 1.3.1 名称空间的定义策略

- 假设想实现如下功能：A::dispA函数中要访问B::num，这要求B定义在A前；同时，在B中增加dispB函数，其中调用A::dispA()函数，这要求A定义在B之前，如此看来，上述功能似乎不太可能会实现。
- 实则不然，这取决于名称空间的定义策略，在前面提及，名称空间中函数的定义和实现可以分开进行，这是我们解决问题的突破口
- 另外，函数原型可以多次声明

1.3.2 名称空间嵌套

- 名称空间可以定义在另一个名称空间内，以单层嵌套为例，要访问内部名称空间中的实体，必须采用“外部名称空间::内部名称空间::实体名”的形式，如果是多层嵌套，还要多次使用作用域限定符。

1.3.3 匿名的名称空间

- 也可以通过省略名称空间的名称来创建匿名的名称空间，此时，该无名空间中的实体的可见性无法扩展（既不能采用“名称空间::实体”的形式，也不能采用using声明机制扩展），因此，该实体只能在本空间内使用。
- 在匿名空间中创建的全局变量，具有全局生存期，却只能被本空间内的函数等访问，是static变量的有效替代手段。

### 小结

作用域限定符::、using声明机制和using编译机制是3种常用的扩展实体可见域的方式，使名称空间中的特定实体或全部实体在声明可见域内可用。名称空间内实体的访问规则和原来介绍的没有名称空间时的情况类似，名称空间还支持嵌套层次结构，在外部使用内层空间时，必须使用多重作用域限定符的形式。

下面引用当前流行的名称空间使用指导原则：

1. 提倡在已命名的名称空间中定义变量，而不是直接定义外部全局变量或者静态全局变量。
2. 如果开发了一个函数库或者类库，提倡将其放在一个名称空间中。
3. 对于using 声明，首先将其作用域设置为局部而不是全局
4. 不要在头文件中使用using编译指令，这样，使得可用名称变得模糊，容易出现二义性，其次，包含头文件的顺序可能会影响程序的行为，如果非要使用using编译指令，建议放在所有#include预编译指令后。

## 2 const 



const 是 constant 的缩写，本意是不变的，不易改变的意思。在 C++ 中是用来修饰内置类型变量，自定义对象，成员函数，返回值，函数参数。

C++ const 允许指定一个语义约束，编译器会强制实施这个约束，允许程序员告诉编译器某值是保持不变的。如果在编程中确实有某个值保持不变，就应该明确使用const，这样可以获得编译器的帮助。

### 2.1 const修饰普通类型的变量

```c++
const int  a = 7; 
int  b = a; // 正确
a = 8;       // 错误，不能改变
```

a 被定义为一个常量，并且可以将 a 赋值给 b，但是不能给 a 再次赋值。对一个常量赋值是违法的事情，因为 a 被编译器认为是一个常量，其值不允许修改。

如果不想让编译器察觉到上面到对 const 的操作，我们可以在 const 前面加上 volatile 关键字。

volatile 关键字跟 const 对应相反，是易变的，容易改变的意思。所以不会被编译器优化，编译器也就不会改变对 a 变量的操作。

```c++
#include<iostream>
using namespace std;
int main(void)
{
    volatile const int  a = 7;
    int  *p = (int*)&a;
    *p = 8;
    cout<<a;//a=8
}
```



### 2.2 const 修饰指针变量

const 修饰指针变量有以下三种情况:

1.const 修饰指针指向的内容，则内容为不可变量。

```c++
const int *p = 8;
```

则指针指向的内容 8 不可改变。简称左定值，因为 const 位于 * 号的左边

2.const 修饰指针，则指针为不可变量

```c++
int a = 8;
int* const p = &a;
*p = 9; // 正确
int  b = 7;
p = &b; // 错误
```

对于 const 指针 p 其指向的内存地址不能够被改变，但其内容可以改变。简称，右定向。因为 const 位于 * 号的右边。

3.const 修饰指针和指针指向的内容，则指针和指针指向的内容都为不可变量

```c++
int a = 8;
const int * const  p = &a;
```

这时，const p 的指向的内容和指向的内存地址都已固定，不可改变。

总结：

**左定值，右定向，const修饰不变量**

### 2.3 const参数传递和函数返回值

对于 const 修饰函数参数可以分为三种情况：

1.值传递的 const 修饰传递，一般这种情况不需要 const 修饰，因为函数会自动产生临时变量复制实参值

```c++
#include<iostream>
 
using namespace std;
 
void Cpf(const int a)
{
    cout<<a;
    // ++a;  是错误的，a 不能被改变
}
 
int main(void)
 
{
    Cpf(8);
}
```

2.当 const 参数为指针时，可以防止指针被意外篡改

```c++
#include<iostream>
 
using namespace std;
 
void Cpf(int *const a)
{
    cout<<*a<<" ";
    *a = 9;
}
 
int main(void)
{
    int a = 8;
    Cpf(&a);
    cout<<a; // a 为 9
}
```

3.自定义类型的参数传递，需要临时对象复制参数，对于临时对象的构造，需要调用构造函数，比较浪费时间，因此我们采取 const 外加引用传递的方法

并且对于一般的 int、double 等内置类型，我们不采用引用的传递方式

```c++
#include<iostream>
 
using namespace std;
 
class Test
{
public:
    Test(){}
    Test(int _m):_cm(_m){}
    int get_cm()const
    {
       return _cm;
    }
 
private:
    int _cm;
};
 
 
 
void Cmf(const Test& _tt)
{
    cout<<_tt.get_cm();
}
 
int main(void)
{
    Test t(8);
    Cmf(t);
}
```

结果输出 **8**

**对于 const 修饰函数的返回值**

const 修饰返回值分三种情况:

1.const 修饰内置类型的返回值，修饰与不修饰返回值作用一样

```c++
#include<iostream>
 
using namespace std;
 
const int Cmf()
{
    return 1;
}
 
int Cpf()
{
    return 0;
}
 
int main(void)
{
    int _m = Cmf();
    int _n = Cpf();
 
    cout<<_m<<" "<<_n;
}
```

2.const 修饰自定义类型的作为返回值，此时返回的值不能作为左值使用，既不能被赋值，也不能被修改。

3. const 修饰返回的指针或者引用，是否返回一个指向 const 的指针，取决于我们想让用户干什么。

### 2.4 const修饰类成员函数

const 修饰类成员函数，其目的是防止成员函数修改被调用对象的值，如果我们不想修改一个调用对象的值，所有的成员函数都应当声明为 const 成员函数

**注意：**const 关键字不能与 static 关键字同时使用，因为 static 关键字修饰静态成员函数，静态成员函数不含有 this 指针，即不能实例化，const 成员函数必须具体到某一实例

下面的 get_cm()const; 函数用到了 const 成员函数:

```c++
#include<iostream>
 
using namespace std;
 
class Test
{
public:
    Test(){}
    Test(int _m):_cm(_m){}
    int get_cm()const
    {
       return _cm;
    }
 
private:
    int _cm;
};
 
 
 
void Cmf(const Test& _tt)
{
    cout<<_tt.get_cm();
}
 
int main(void)
{
    Test t(8);
    Cmf(t);
}
```

如果 get_cm() 去掉 const 修饰，则 Cmf 传递的 const _tt 即使没有改变对象的值，编译器也认为函数会改变对象的值，所以我们尽量按照要求将所有的不需要改变对象内容的函数都作为 const 成员函数。

如果有个成员函数想修改对象中的某一个成员怎么办？这时我们可以使用 mutable 关键字修饰这个成员，mutable 的意思也是易变的，容易改变的意思，被 mutable 关键字修饰的成员可以处于不断变化中，如下面的例子。

```c++
#include<iostream>
using namespace std;
class Test
{
public:
    Test(int _m,int _t):_cm(_m),_ct(_t){}
    void Kf()const
    {
        ++_cm; // 错误
        ++_ct; // 正确
    }
private:
    int _cm;
    mutable int _ct;
};
 
int main(void)
{
    Test t(8,7);
    return 0;
}
```

这里我们在 Kf()const 中通过 ++_ct; 修改 _ct 的值，但是通过 ++_cm 修改 _cm 则会报错。因为 ++_cm 没有用 mutable 修饰。

### 2.5 const vs #define

1. 编译器处理方式不同
      define宏是在预处理阶段展开。
      const常量是编译运行阶段使用。

2.  类型和安全检查不同
     define宏没有类型，不做任何类型检查，仅仅是展开。
     const常量有具体的类型，在编译阶段会执行类型检查。

   注意：尽量以const替换#define

## 3 引用

引用就是某一变量（目标）的一个别名，对引用的操作与对变量直接操作完全一样

```c++
int a ;
int &refa = a;
```

1. &在此不是求地址运算，而是起标识作用。
2. 类型标识符是指目标变量的类型
3. 声明引用时，必须同时对其进行初始化
4. 引用跟某一变量绑定之后，不能再绑定到其它变量之上

### 3.1 引用作为函数参数

C语言中函数参数传递是值传递，如果有大块数据作为参数传递的时候，采用的方案往往是指针，因为这样可以避免将整块数据全部压栈，可以提高程序的效率。

但是现在C++中又增加了一种同样有效率的选择，就是引用。
Example:
 swap(int &x, int &y)

- 传递引用给函数与传递指针的效果是一样的。这时，被调函数的形参就成为原来主调函数中的实参变量或对象的一个别名来使用，所以在被调函数中对形参变量的操作就是对其相应的目标对象的操作

- 使用引用传递函数的参数，在内存中并没有产生实参的副本，它是直接对实参操作；而使用一般变量传递函数的参数，当发生函数调用时，需要给 形参分配存储单元，形参变量是实参变量的副本；

- 使用指针作为函数的参数虽然也能达到与使用引用的效果，但是，在被调函数中同样要给形参分配存储单元，且需要重复使用"***指针变量名**"的 形式进行运算，这很容易产生错误且程序的阅读性较差；另一方面，在主调函数的调用点处，必须用变量的地址作为实参。

- 如果既要利用引用提高程序的效率，又要保护传递给函数的数据不在函数中被改变，就应使用常引用。

```c++
int a = 10;
const int &refa = a;
```

### 3.2 引用作为函数参数

- 使用指针作为函数的参数虽然也能达到与使用引用的效果，但是，在被调函数中同样要给形参分配存储单元，且需要重复使用"*指针变量名"的 形式进行运算，这很容易产生错误且程序的阅读性较差；另一方面，在主调函数的调用点处，必须用变量的地址作为实参。

- 如果既要利用引用提高程序的效率，又要保护传递给函数的数据不在函数中被改变，就应使用常引用。

```c++
int a = 10;
const int &refa = a;
```

### 3.3 引用作为函数返回值

- 以引用返回函数值，定义函数时需要在函数名前加&
- 用引用返回一个函数值的最大好处是，在内存中不产生被返回值的副本。

必须遵守的规则：
不能返回局部变量的引用。
不能返回函数内部new分配的内存的引用。例如，被函数返回的引用只是作为一个临时变量出现，而没有被赋予一个实际的变量，那么这个引用所指向的空间（由new分配）就无法释放，造成memory leak。

### 3.4 引用 vs 指针

**相同点**：都是地址的概念

**不同点**：

1. 指针是一个实体，而引用仅是个别名；	
2. 引用使用时无需解引用（*），指针需要解引用
3. 引用只能在定义时被初始化一次，之后不可变；指针可变
4. 引用不能为空，指针可以为空

## 4 string类

字符串处理在程序中应用广泛，C风格字符串是以'\0'（空字符）来结尾的字符数组，在使用时，程序员需要考虑字符数组大小的开辟，结尾空字符的处理，使用起来有诸多不便

之所以抛弃C风格字符串而选用C++标准程序库中的string类，是因为string和C风格字符串相比，不必担心内存是否足够、字符串长度，结尾的空白符等等。string作为一个类出现，其集成的成员操作函数功能强大，几乎能满足所有的需求，从另一个角度上说，完全可以把string当成是C++的内置数据类型，放在和int、double等同等位置上。

### 4.1 string类的构造函数

| string s();                                            | 生成一个空字符s                                       |
| ------------------------------------------------------ | ----------------------------------------------------- |
| string s(const string &str)                            | 复制拷贝构造函数                                      |
| string s(const string &str, size_type n, string::npos) | 将字符串str内“始于位置，npos”的部分当作字符串s的初值  |
| string s(const string &str, size_type n, size_type m)  | 将字符串str内“始于n且长度多于m”的部分作为字符串的初值 |
| string s(const char* cs)                               | 将C字符串cs作为s的初值                                |
| string s(const char *cs, size_type n)                  | 将C字符串cs前n个字符作为字符串s的初值，即使超过了cs尾 |
| string s(size_type num, char c)                        | 生成一个字符串，包含num个c字符                        |
| string s(iterator beg, iterator end)                   | 以区间[beg; end)内的字符作为字符串s的初值             |

```c++
string s1();  // s1 = ""
string s2("Hello");  // s2 = "Hello"
string s3(4, 'K');  // s3 = "KKKK"
string s4("12345", 1, 3);  //s4 = "234"，即 "12345" 的从下标 1 开始，长度为 3 的子串
```

string 类没有接收一个整型参数或一个字符型参数的构造函数。下面的两种写法是错误的：

```c++
string s1('K');
string s2(123);
```

### 4.2 对string对象赋值

可以用 char* 类型的变量、常量，以及 char 类型的变量、常量对 string 对象进行赋值。例如：

```c++
string s1;
s1 = "Hello";  // s1 = "Hello"
s2 = 'K';  // s2 = "K”
```

string 类还有 assign 成员函数，可以用来对 string 对象赋值。assign 成员函数返回对象自身的引用。例如：

```c++
string s1("12345"), s2;
s3.assign(s1);  // s3 = s1
s2.assign(s1, 1, 2);  // s2 = "23"，即 s1 的子串(1, 2)
s2.assign(4, 'K');  // s2 = "KKKK"
s2.assign("abcde", 2, 3);  // s2 = "cde"，即 "abcde" 的子串(2, 3)
```

### 4.3 求字符串的长度

length 成员函数返回字符串的长度。size 成员函数可以实现同样的功能。 

### 4.4 string对象中字符串的连接

除了可以使用`+`和`+=`运算符对 string 对象执行字符串的连接操作外，string 类还有 append 成员函数，可以用来向字符串后面添加内容。append 成员函数返回对象自身的引用。例如：

```c++
string s1("123"), s2("abc");
s1.append(s2);  // s1 = "123abc"
s1.append(s2, 1, 2);  // s1 = "123abcbc"
s1.append(3, 'K');  // s1 = "123abcbcKKK"
s1.append("ABCDE", 2, 3);  // s1 = "123abcbcKKKCDE"，添加 "ABCDE" 的子串(2, 3)
```

### 4.5 string对象的比较

除了可以用 <、<=、==、!=、>=、> 运算符比较 string 对象外，string 类还有 compare 成员函数，可用于比较字符串。compare 成员函数有以下返回值：

- 小于 0 表示当前的字符串小；
- 等于 0 表示两个字符串相等；
- 大于 0 表示另一个字符串小。

```c++
string s1("hello"), s2("hello, world");
int n = s1.compare(s2);
n = s1.compare(1, 2, s2, 0, 3);  //比较s1的子串 (1,2) 和s2的子串 (0,3)
n = s1.compare(0, 2, s2);  // 比较s1的子串 (0,2) 和 s2
n = s1.compare("Hello");
n = s1.compare(1, 2, "Hello");  //比较 s1 的子串(1,2)和"Hello”
n = s1.compare(1, 2, "Hello", 1, 2);  //比较 s1 的子串(1,2)和 "Hello" 的子串(1,2)
```

### 4.6 求 string 对象的子串

substr 成员函数可以用于求子串 (n, m)，原型如下：

```c++
string substr(int n = 0, int m = string::npos) const;
```

调用时，如果省略 m 或 m 超过了字符串的长度，则求出来的子串就是从下标 n 开始一直到字符串结束的部分。例如：

```c++
string s1 = "this is ok";
string s2 = s1.substr(2, 4);  // s2 = "is i"
s2 = s1.substr(2);  // s2 = "is is ok"
```

### 4.7 交换两个string对象的内容

swap 成员函数可以交换两个 string 对象的内容。例如：

```c++
string s1("West”), s2("East");
s1.swap(s2);  // s1 = "East"，s2 = "West"
```

### 4.8 查找子串和字符

string 类有一些查找子串和字符的成员函数，它们的返回值都是子串或字符在 string 对象字符串中的位置（即下标）。如果查不到，则返回 string::npos。string: :npos 是在 string 类中定义的一个静态常量。这些函数如下：

- find：从前往后查找子串或字符出现的位置。
- rfind：从后往前查找子串或字符出现的位置。
- find_first_of：从前往后查找何处出现另一个字符串中包含的字符。例如：
- s1.find_first_of("abc"); //查找s1中第一次出现"abc"中任一字符的位置
- find_last_of：从后往前查找何处出现另一个字符串中包含的字符。
- find_first_not_of：从前往后查找何处出现另一个字符串中没有包含的字符。
- find_last_not_of：从后往前查找何处出现另一个字符串中没有包含的字符。

```c++
#include <iostream>
#include <string>
using namespace std;
int main()
{
    string s1("Source Code");
    int n;
    if ((n = s1.find('u')) != string::npos) //查找 u 出现的位置
        cout << "1) " << n << "," << s1.substr(n) << endl;
    //输出 l)2,urce Code
    if ((n = s1.find("Source", 3)) == string::npos)
        //从下标3开始查找"Source"，找不到
        cout << "2) " << "Not Found" << endl;  //输出 2) Not Found
    if ((n = s1.find("Co")) != string::npos)
        //查找子串"Co"。能找到，返回"Co"的位置
        cout << "3) " << n << ", " << s1.substr(n) << endl;
    //输出 3) 7, Code
    if ((n = s1.find_first_of("ceo")) != string::npos)
        //查找第一次出现或 'c'、'e'或'o'的位置
        cout << "4) " << n << ", " << s1.substr(n) << endl;
    //输出 4) l, ource Code
    if ((n = s1.find_last_of('e')) != string::npos)
        //查找最后一个 'e' 的位置
        cout << "5) " << n << ", " << s1.substr(n) << endl;  //输出 5) 10, e
    if ((n = s1.find_first_not_of("eou", 1)) != string::npos)
        //从下标1开始查找第一次出现非 'e'、'o' 或 'u' 字符的位置
        cout << "6) " << n << ", " << s1.substr(n) << endl;
    //输出 6) 3, rce Code
    return 0;
}
```

### 4.9 替换子串

replace 成员函数可以对 string 对象中的子串进行替换，返回值为对象自身的引用。例如：

```c++
string s1("Real Steel");
s1.replace(1, 3, "123456", 2, 4);  //用 "123456" 的子串(2,4) 替换 s1 的子串(1,3)
cout << s1 << endl;  //输出 R3456 Steel
string s2("Harry Potter");
s2.replace(2, 3, 5, '0');  //用 5 个 '0' 替换子串(2,3)
cout << s2 << endl;  //输出 HaOOOOO Potter
int n = s2.find("OOOOO");  //查找子串 "00000" 的位置，n=2
s2.replace(n, 5, "XXX");  //将子串(n,5)替换为"XXX"
cout << s2 < < endl;  //输出 HaXXX Potter
```

### 4.10 删除子串

erase 成员函数可以删除 string 对象中的子串，返回值为对象自身的引用。例如:

```c++
string s1("Real Steel");
s1.erase(1, 3);  //删除子串(1, 3)，此后 s1 = "R Steel"
s1.erase(5);  //删除下标5及其后面的所有字符，此后 s1 = "R Ste"
```

### 4.11 插入字符串

insert 成员函数可以在 string 对象中插入另一个字符串，返回值为对象自身的引用。例如：

```c++
string s1("Limitless"), s2("00");
s1.insert(2, "123");  //在下标 2 处插入字符串"123"，s1 = "Li123mitless"
s1.insert(3, s2);  //在下标 2 处插入 s2 , s1 = "Li10023mitless"
s1.insert(3, 5, 'X');  //在下标 3 处插入 5 个 'X'，s1 = "Li1XXXXX0023mitless"
```

### 4.12 将 string 对象作为流处理

使用流对象 istringstream 和 ostringstream，可以将 string 对象当作一个流进行输入输出。使用这两个类需要包含头文件 sstream。

```c++
#include <iostream>
#include <sstream>
#include <string>
using namespace std;
int main()
{
    string src("Avatar 123 5.2 Titanic K");
    istringstream istrStream(src); //建立src到istrStream的联系
    string s1, s2;
    int n;  double d;  char c;
    istrStream >> s1 >> n >> d >> s2 >> c; //把src的内容当做输入流进行读取
    ostringstream ostrStream;
    ostrStream << s1 << endl << s2 << endl << n << endl << d << endl << c <<endl;
    cout << ostrStream.str();
    return 0;
}
```

程序的输出结果是：
Avatar
Titanic
123
5.2
K

第 11 行，从输入流 istrStream 进行读取，过程和从 cin 读取一样，只不过输入的来源由键盘变成了 string 对象 src。因此，"Avatar" 被读取到 s1，123 被读取到 n，5.2 被读取到 d，"Titanic" 被读取到s2，'K' 被读取到 c。

第 12 行，将变量的值输出到流 ostrStream。输出结果不会出现在屏幕上，而是被保存在 ostrStream 对象管理的某处。用 ostringstream 类的 str 成员函数能将输出到 ostringstream 对象中的内容提取出来。

### 4.13 用 STL 算法操作 string 对象

string 对象也可以看作一个顺序容器，它支持随机访问迭代器，也有 begin 和 end 等成员函数。STL 中的许多算法也适用于 string 对象。下面是用 STL 算法操作 string 对象的程序示例。

```c++
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;
int main()
{
    string s("afgcbed");
    string::iterator p = find(s.begin(), s.end(), 'c');
    if (p!= s.end())
        cout << p - s.begin() << endl;  //输出 3
    sort(s.begin(), s.end());
    cout << s << endl;  //输出 abcdefg
    next_permutation(s.begin(), s.end());
    cout << s << endl; //输出 abcdegf
    return 0;
}
```

## 5 程序内存分配方式

### 5.1程序的内存布局

1. **栈区（stack）**：由编译器自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。 
2. **堆区（heap）**：一般由程序员分配释放，若程序员不释放，程序结束时可能由OS回收。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表。
3. **全局/静态区(static）**：全局变量和静态变量的存储是放在一块的，在程序编译时分配
4. **文字常量区**：存放常量字符串
5. **程序代码区**：存放函数体（类的成员函数、全局函数）的二进制代码

```c++
int a = 0; //全局初始化区
char *p1;  //全局未初始化区
int main()
{
    int b;                //栈
    char s[] = "abc";     //栈
    char *p2;             //栈
    char *p3 = "123456";  //字符串位于常量区，p3位于栈
    static int c = 0;     //全局(静态)初始化区
    p1 = new char[10];    //p1位于全局区，p1指向的对象位于堆
    strcpy(p1, "123456"); //
}
```

### 5.2 栈和堆的比较

#### 5.2.1 申请方式  

​	**stack:**  
​	由系统自动分配。例如，声明在函数中一个局部变量int b; 系统自动在栈中为b开辟空间  	**heap:**  
​	需要程序员自己申请，并指明大小，在c中malloc函数，如

```c++
char* p1 = (char *)malloc(10);
```

​	在C++中用new运算符，如

```c++
int *p2 = new int (10); 
```

​    **注意p1、p2本身是在栈中的。**

#### 5.2.2 申请后系统的响应 

- 栈：只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。

- 堆：首先应该知道操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序。另外，对于大多数系统，首地址处会记录这块内存空间中本次分配的大小，这样，代码中的delete语句才能正确的释放本内存空间。另外，由于找到的堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。

#### 5.2.3 申请大小的限制 

- 栈：在Windows下,栈是向低地址扩展的数据结构，是一块连续的内存的区域。这句话的意思是栈顶的地址和栈的最大容量是系统预先规定好的，在WINDOWS下，栈的大小是2M（也有的说是1M，总之是一个编译时就确定的常数），如果申请的空间超过栈的剩余空间时，将提示overflow。因此，能从栈获得的空间较小。
- 堆：堆是向高地址扩展的数据结构，是不连续的内存区域。这是由于系统是用链表来存储的空闲内存地址的，自然是不连续的，而链表的遍历方向是由低地址向高地址。。由此可见，堆获得的空间比堆的大小受限于计算机系统中有效的虚拟内存较灵活，也比较大。

#### 5.2.4 申请效率的比较

- 栈由系统自动分配，速度较快。但程序员无法控制。
- 堆是由new分配的内存，一般速度比较慢，而且容易产生内存碎片,不过用起来最方便。

#### 5.2.5 堆和栈中的存储内容

- 栈： 在函数调用时，第一个进栈的是主函数的下一条指令（函数调用语句的下一条可执行语句）的地址，然后是函数的各个参数，在大多数的C编译器中，参数是由右往左入栈的，然后是函数中的局部变量。注意静态变量是不入栈的。当本次函数调用结束后，局部变量先出栈，然后是参数，最后栈顶指针指向最开始存的地址，也就是主函数中的下一条指令，程序由该点继续运行。
- 堆：一般是在堆的头部用一个字节存放堆的大小。堆中的具体内容由程序员安排。

#### 5.2.6 栈与堆小结

区别：

- 管理方式不同

  对于栈来讲，是由编译器自动管理，无需我们手工控制；对于堆来说，释放工作由程序员控制，容易产生memory leak

- 空间大小不同

  一般来讲在32位系统下，堆内存可以达到4G的空间，从这个角度来看堆内存几乎是没有什么限制的。但是对于栈来讲，一般都是有一定的空间大小的，例如，在VC6下面，默认的栈空间大小是1M

- 能否产生碎片不同

  对于堆来讲，频繁的new/ delete势必会造成内存空间的不连续，从而造成大量的碎片，使程序效率降低。对于栈来讲，则不会存在这个问题，因为栈是先进后出的，他们是如此的一一对应，以至于永远都不可能有一个内存块从栈中间弹出，在他弹出之前，在它上面的后进的栈内容已经被弹出

- 生长方向不同

  对于堆来讲，生长方向是向上的，也就是向着内存地址增加的方向；对于栈来讲，它的生长方向是向下的，是向着内存地址减小的方向增长。

- 分配方式不同

  堆都是动态分配的，没有静态分配的堆。内存有2种分配方式：静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由malloc,calloc函数进行分配，但是栈的动态分配和堆是不同的，他的动态分配是由编译器进行释放，无需我们手工实现。

- 分配效率不同

## 6 类和对象

### 6.1 结构化程序设计

- 解决问题的方式
  	复杂的大问题
  		层层分解/模块化
  			若干子问题
  	自顶向下，逐步求精
  		程序  ＝   变量  +  函数
  	（程序 ＝ 数据结构  +  算法）
- 问题：数据结构和算法之间没有直观的联系
- 不得不面对几个问题：
  理解难  修改难  重用难

### 6.2 面向对象程序设计

- 认为现实世界是由对象组成的
- 用程序来模拟现实世界中对象与对象之间的交互
- 对象由相关的数据和操作组成，对象具有相对独立性，对外提供一定的服务
          程序 ＝ 对象 + 对象 + … + 对象

**小结**

- 在C语言中，程序是由一个个函数组成的，是结构化的面向过程的编程方法。
- C++语言进行面向对象的程序设计，编写的程序是由对象组成的。
- 面向对象的四大特性：抽象、封装、继承、多态

### 6.3 面向对象基本概念

- 类是怎么来的？
  		从哲学角度说，先有对象，然后才有类，类和对象是“一般和特殊”这一哲学原理在程序世界中的具体体现。
    		类其实是抽象认知能力作用于程序世界的基本元素——对象后所衍生出来的抽象概念，是抽象思维在程序世界中物化后的产物。
- 类的提取往往是从两个方面来考虑的，
  一是特征（C++常称为“属性”）、
  另一个是功能（C++中常称为“行为”），具备类中定义的“属性”和“行为”的对象都是该类的对象，因此，可以说，电动车也是“车”类的对象。

#### 6.3.1 类的概念

- C++用类来描述对象，类是对现实世界中相似事物的抽象，同是“双轮车”的摩托车和自行车，有共同点，也有许多不同点。“车”类是对摩托车、自行车、汽车等相同点的提取与抽象，如所示。
- 类的定义分为两个部分：数据（相当于属性）和对数据的操作（相当于行为）。
- 从程序设计的观点来说，类就是数据类型，是用户定义的数据类型，对象可以看成某个类的实例（某个类的变量）

#### 6.3.2 类和对象的关系

类是对象的封装，对象是类的实例。
		类需要从属性和行为两个方面进行描述 。类的使用主要有以下几个步骤：

- 定义类：C++中，分别用数据成员和成员函数来表现对象的属性和行为。类的定义强调“信息隐藏”，将实现细节和不允许外部随意访问的部分屏蔽起来，保证了对象的数据安全。
- 实现类：即进一步定义类的成员函数，使各个成员函数相互配合以实现接口对外提供的功能，类的定义和实现是由类设计者完成的。
- 使用类：通过该类声明一个属于该类的变量（即对象），并调用其接口（即public型的数据成员或函数成员），这是使用者的工作

### 6.4 类的定义

#### 6.4.1 类定义的基本形式

C++中使用关键字class定义一个类，其基本形式如下：

```c++
class 类名
{
private:
	   私有成员变量和函数
protected:
	   保护成员变量和函数
public:
	   公共成员变量和函数
};	//不要漏写了这个分号;
```

#### 6.4.2 类定义示例

对一台计算机来说，它有如下特征：
属性：品牌、价格。
方法：输出计算机的属性。

```c++
//注意：
//C++规定，类成员的访问权限默认是private，不加申明的成员默认是private，因此上述代码中的第一个private可以省略。
//数据成员的类型前面不可使用auto、extern和register等，也不能在定义时对变量进行初始化，如果将float price写成float price = 0，编译器会报错。
//类定义中提供的成员函数是函数的原型申明。
//关键字private和public出现的顺序和次数可以是任意的，如上述代码也可以写成如下形式：
class computer
{
private: //私有成员列表，这里的private可以省略
    char brand[20];
    float price; //不能在这里初始化，如float price=0是错误的
public:          //公共成员列表（接口）
    void print();
    void SetBrand(char *sz);
    void SetPrice(float pr);
};
//private数据成员只能由本类的函数访问，protected数据成员只能在派生类中访问，而public数据成员在派生类和类外均可访问。
```

#### 6.4.3 class和struct

- class的定义看上去很像struct定义的扩展，事实上，类定义时的关键字class完全可以替换成struct，也就是说，结构体变量也可以有成员函数。**class和struct的唯一区别在于**：struct的默认访问方式是public，而class为private。
- 提示：通常使用class来定义类，而把struct用于只表示数据对象、没有成员函数的类。

### 6.5 C++类的实现

类的实现就是定义其成员函数的过程，类的实现有两种方式：

- 在类定义时同时完成成员函数的定义。
- 在类定义的外部定义其成员函数。

#### 6.5.1 在类定义时定义成员函数

成员函数的实现可以在类定义时同时完成

```c++
//example802.h
#include <cstring>
#include <iostream>
using namespace std;

class computer
{
private:
    char brand[20];
    float price;

public:
    //在类定义的同时实现了3个成员函数
    void print()
    {
        cout << "品牌：" << brand << endl;
        cout << "价格：" << price << endl;
    }

    void SetBrand(char *sz)
    {
        strcpy(brand, sz); //字符串复制
    }

    void SetPrice(float pr)
    {
        price = pr;
    }
};
```

```c++
// Main.cpp
#include "example802.h" //包含了computer类的定义
int main()
{
    computer com1;           //声明创建一个类对象
    com1.SetPrice(5000);     //调用public成员函数SetPrice设置price
    com1.SetBrand("Lenovo"); //调用public成员函数SetBrand设置Brand
    com1.print();            //调用print()函数输出信息
    return 0;
}
```

#### 6.5.2 在类定义的外部定义成员函数

在类定义的外部定义成员函数时，应使用作用域操作符（::）来标识函数所属的类，即有如下形式

```c++
返回类型 类名::成员函数名(参数列表)
{
	函数体
}
```

其中，返回类型、成员函数名和参数列表必须与类定义时的函数原型一致。

```c++
// example803.h
class computer //类定义，起到接口作用
{
private:
    char brand[20];
    float price;

public:
    //3个public成员函数的原型声明
    void print();
    void SetBrand(char *sz);
    void SetPrice(float pr);
};
```

```c++
// Main.cpp
#include "example803.h" //包含computer类定义
#include <cstring>
#include <iostream>
using namespace std;

void computer::print() //成员函数的实现，注意作用域限定符的使用
{
    cout << "品牌：" << brand << endl;
    cout << "价格：" << price << endl;
}

void computer::SetBrand(char *sz)
{
    strcpy(brand, sz); //字符串复制
}

void computer::SetPrice(float pr)
{
    price = pr;
}

int main()
{
    computer com1;           //声明创建一个类对象
    com1.SetPrice(5000);     //调用public成员函数SetPrice设置price
    com1.SetBrand("Lenovo"); //调用public成员函数SetBrand设置Brand
    com1.print();            //调用print()函数输出信息
    return 0;
}
```

### 6.6 类的使用

定义了一个类之后，便可以如同用int、double等类型符声明简单变量一样，创建该类的对象，称为类的实例化。由此看来，类的定义实际上是定义了一种类型，类不接收或存储具体的值，只作为生成具体对象的“蓝图”，只有将类实例化，创建对象（声明类的变量）后，系统才为对象分配存储空间。

#### 6.6.1 声明一个对象

使用类定义声明了一个对象，并利用对象名实现了public成员函数的调用

```c++
class computer //类定义
{
private:
    char brand[20];
    float price;

public:
    void print();
    void SetBrand(char *sz);
    void SetPrice(float pr);
};

#include "computer.h" //包含类定义
#include <iostream>
using namespace std;

void computer::print() //成员函数的实现
{
    cout << "品牌：" << brand << endl;
    cout << "价格：" << price << endl;
}

void computer::SetBrand(char *sz)
{
    strcpy(brand, sz); //字符串复制
}

void computer::SetPrice(float pr)
{
    price = pr;
}

#include "computer.h" //定义了类computer
int main()            //主函数
{
    computer com1;           //声明了computer类对象（或说类变量）com1
    com1.SetBrand("Lenovo"); //调用public成员函数SetBrand设置品牌brand
    com1.SetPrice(8000);     //调用public成员函数SetPrice设置品牌price
    com1.print();            //信息输出
    return 0;
}
```

#### 6.6.2 对象的作用域、可见域和生存期

- 对象的作用域、可见域和生存期与普通变量，如int型变量的作用域、可见域和生存期并无不同，对象同样有局部、全局和类内（稍后就将对对象成员进行介绍）之分，对于在代码块中声明的局部对象，在代码块执行结束退出时，对象会被自动撤销，对应的内存会自动释放（当然，如果对象的成员函数中使用了new或malloc申请了动态内存，却没有使用delete或free命令释放，对象撤销时，这部分动态内存不会自动释放，造成内存泄露）。
- 跟踪调试，查看同一个类的不同对象的成员变量和成员函数在内存中的地址分配情况。结论：成员变量占据不同的内存区域(堆、栈)；成员函数共用同一内存区域(代码段)。

### 6.7 对象的创建和撤销

在之前的Computer类中，通过自定义的公共成员函数SetBrand和SetPrice实现对数据成员的初始化，实际上，C++为类提供了两种特殊的成员函数来完成同样的工作：

- 一是**构造函数**，在对象**创建时自动调用**，用以完成对象成员变量等的初始化及其他操作（如为指针成员动态申请内存空间等）；如果程序员没有显式的定义它，系统会提供一个默认的构造函数。
- 另一个是析构函数，在对象**撤销时自动调用**，用以执行一些清理任务，如释放成员函数中动态申请的内存等。如果程序员没有显式的定义它，系统也会提供一个默认的析构函数。

#### 6.7.1 构造函数的作用

- 当对象被创建时，构造函数自动被调用。构造函数有一些独特的地方：函数的名字与类名相同，没有返回类型和返回值，即使是void也不能有。其主要工作有：
  给对象一个标识符。
  为对象数据成员开辟内存空间。
  完成对象数据成员的初始化（函数体内的工作，由程序员完成）。
- 上述3点也说明了构造函数的执行顺序，在执行函数体之前，构造函数已经为对象的数据成员开辟了内存空间，这时，在函数体内对数据成员的初始化便是顺理成章了。

```c++
class point
{
private:
	int xPos;
	int yPos;
public:
	point();
};

point::point()
{
	xPos = 0;
	yPos = 0;
}
```

#### 6.7.2 构造函数可以有参数

- 编译器自动生成的缺省(默认)构造函数是无参的，实际上，构造函数可以接收参数，在对象创建时提供更大的自由度
- 一旦用户定义了构造函数，系统便不再提供默认构造函数。
- 跟踪执行，理解构造函数的执行顺序。

```c++
// point.h
#include <iostream>
using namespace std;
class point //point类定义，在定义同时实现其成员函数
{
private: //私有成员，分别代表x轴和y轴坐标
	int xPos;
	int yPos;
public:
	point(int x, int y) //有参构造函数
	{
		cout << "对象创建时构造函数被自动调用" << endl;
		xPos = x;
		yPos = y;
	}
	void print() //输出信息
	{
		cout << "xPos: " << xPos << ",yPos: " << yPos << endl;
	}
};

#include "point.h"
int main()
{
//	point pt0;			//错误的调用，因为我们已经显示的定义了一个带参数的构造函数
//	pt0.print();		//输出pt0的信息
	point pt1(3, 4);	//调用有参构造函数声明point类变量（类对象）pt1
	pt1.print();		//输出pt1的信息
	return 0;
}
```

#### 6.7.3 构造函数支持重载

一旦程序员为一个类定义了构造函数，编译器便不会为类自动生成缺省构造函数，因此，如果还想使用无参的构造函数，如“point pt1;”的形式必须在类定义中显式定义一个无参构造函数。这样，构造函数就会出现两个，会不会有问题呢？不会，构造函数支持重载，在创建对象时，根据传递的具体参数决定采用哪个构造函数。

```c++
// point.h
#include <iostream>
using namespace std;
class point //point类定义，在定义同时实现其成员函数
{
private: //私有成员，分泌诶代表x轴和y轴坐标
	int xPos;
	int yPos;
public:
	point(int x, int y)//有参构造函数
	{
		cout << "有参构造函数的调用" << endl;
		xPos = x;
		yPos = y;
	}
	point() //无参构造函数
	{
		cout << "无参构造函数的调用" << endl;
		xPos = 0;
		yPos = 0;
	}
	void print()//输出信息
	{
		cout << "xPos: " << xPos << ",yPos: " << yPos << endl;
	}
};

#include "point.h"
int main()
{
	point pt1(3, 4);	//调用有参构造函数声明point类变量（类对象）pt1
	pt1.print();		//输出pt1的信息
	point pt2;			//调用无参构造函数声明point类变量（类对象）pt2
	pt2.print();		//输出pt2的信息
	return 0;
}
```

#### 6.7.4 构造函数允许按参数缺省方式调用

```c++
#include <iostream>
using namespace std;
class point //point类定义，在定义同时实现其成员函数
{
private: //私有成员，分别代表x轴和y轴坐标
	int xPos;
	int yPos;
public:
	point(int x = 0, int y = 0) //有参构造函数
	{
		cout << "对象创建时构造函数被自动调用" << endl;
		xPos = x;
		yPos = y;
	}
	void print() //输出信息
	{
		cout << "xPos: " << xPos << ",yPos: " << yPos << endl;
	}
};

#include "point.h"
int main()
{
	point pt0;		//两个值都采用默认值
	pt0.print();		
	point pt1(3, 4);	//两个参数都不采用默认值
	pt1.print();		//输出pt1的信息
	return 0;
}
```

代码中的构造函数可以作如下定义：

```c++
point(int x＝0,int y＝0)
{
	cout<<"对象创建时构造函数被自动调用"<<endl;
	xPos=x;
	yPos=y;
}
```

此时，可在创建对象时省略参数，下列语句都是合法的：

```c++
point pt;	//x和y都采用默认值0
point pt(3);	//x为3，y采用默认值0
point pt(3,4);//x为3，y为4，两个参数都不采用默认值
```

#### 6.7.5 初始化表达式1

- 除了在构造函数体内初始化数据成员外，还可以通过成员初始化表达式来完成。
- 成员初始化表可用于初始化类的任意数据成员（后面要介绍的static数据成员除外），该表达式由逗号分隔的数据成员表组成，初值放在一对圆括号中。
- 只要将成员初始化表达式放在构造函数的头和体之间，并用冒号将其与构造函数的头分隔开，便可实现数据成员表中元素的初始化

```c++
point(int x,int y)
{
	cout<<"有参构造函数的调用"<<endl;
	xPos=x;
	yPos=y;
}
//等价于：
point(int x,int y):xPos(x),yPos(y)
{
	cout<<"有参构造函数的调用"<<endl;
}
```

#### 6.7.6 初始化表达式2

每个成员在初始化表中只能出现一次

初始化的顺序不是由成员变量在初始化表中的顺序决定的，而是由成员变量在类中被申明时的顺序决定的。理解这一点有助于避免意想不到的错误。

```c++
#include <iostream>
using namespace std;
class point
{
private:
	int yPos;	//先定义
	int xPos;	//后定义
public:
	point(int x):xPos(x), yPos(xPos) //初始化表取决于成员声明的顺序
	{
	}
	void print()
	{
		cout << "xPos: " << xPos << ", yPos: " << yPos << endl;
	}
};

#include "point.h"
int main()
{
	point pt1(3);		//调用有参构造函数声明变量pt1
	pt1.print();	
	return 0;
}
```

#### 6.7.7 析构函数

- 构造函数在创建对象时被系统自动调用，而析构函数在对象被撤销时被自动调用，相比构造函数，析构函数要简单的多。析构函数有如下特点：
  - 与类同名，之前冠以波浪号，以区别于构造函数。
  - 析构函数没有返回类型，也不能指定参数，因此，析构函数只能有一个，不能被重载。
  - 对象超出其作用域被销毁时，析构函数会被自动调用。
- 如果用户没有显式地定义析构函数，编译器将为类生成“缺省析构函数”，缺省析构函数是个空的函数体，只清除类的数据成员所占据的空间，但对类的成员变量通过new和malloc动态申请的内存无能为力，因此，对于动态申请的内存，应在类的析构函数中通过delete或free进行释放，这样能有效避免对象撤销造成的内存泄漏。

```c++
#include <iostream>
#include <cstring>
using namespace std;
class computer
{
private:
	char * brand; //指针成员
	float price;
public:
	computer(const char * sz, float p)
	{
		brand = new char[strlen(sz) + 1]; //对象创建时为brand分配一块动态内存
		strcpy(brand, sz); //字符串复制
		price = p;
	}
	~computer()
	{
		delete[] brand; //对象别撤销时，释放内存，避免泄露
		brand = NULL;
		cout << "清理现场" << endl;
	}
	void print() //信息输出
	{
		cout << "品牌：" << brand << endl;
		cout << "价格：" << price << endl;
	}
};

#include "computer.h"
int main()
{
	computer comp("Dell", 7000); //调用构造函数声明computer变量comp
	comp.print(); //信息输出
	return 0;
}
```

#### 6.7.8 析构函数在什么时候被调用

- 对于全局定义的对象，每当程序开始运行，在主函数main(或WinMain)接受程序控制权之前，就调用全局对象的构造函数。整个程序结束时调用析构函数。
- 对于局部定义的对象，每当程序流程到达该对象的定义处就调用构造函数，在程序离开局部对象的作用域时调用对象的析构函数。{}
- 对于关键字static定义的静态局部变量，当程序流程第一次到达该对象定义处调用构造函数，在整个程序结束时调用析构函数。
- 对于用new运算符创建的对象，每当创建该对象时调用构造函数，当用delete删除该对象时，调用析构函数。

#### 6.7.9 显式调用析构函数

程序员不能显式调用构造函数，但却可以调用析构函数控制对象的撤销，释放对象所占据的内存空间，以更高效地利用内存，如：

```c++
#include "computer.h"
int main()
{
    computer comp("Dell",7000);
    comp.print();
    comp.~computer();//显式调用析构函数，comp被撤销
    return 0;
}	//注意这里comp对象释放时析构函数也被隐式调用了一次
```

虽然可以显式调用析构函数，但不推荐这样做，因为可能带来重复释放指针类型变量所指向的内存等问题。

```c++
#include <iostream>
#include <cstring>
using namespace std;

class computer
{
private:
	char * brand; //指针成员
	float price;
public:
	computer(const char * sz, float p)
	{
		brand = new char[strlen(sz) + 1]; //对象创建时为brand分配一块动态内存
		strcpy(brand, sz); //字符串复制
		price = p;
	}

	~computer()
	{
//		if(brand != NULL)
//		{
			delete[] brand; //对象别撤销时，释放内存，避免泄露
//			brand = NULL;	//如果注释掉本句，则在显式调用析构
						//函数后，退出时会因为重复释放而报内存错误
//		}
		cout << "清理现场" << endl;
	}

	void print() //信息输出
	{
		cout << "品牌：" << brand << endl;
		cout << "价格：" << price << endl;
	}
};

int main()
{
	computer comp("Dell",7000);
	comp.print();
	comp.~computer();//显式调用析构函数，合法，comp被撤销
	return 0;
}	//注意这里comp对象释放时析构函数也被隐式调用了一次

```

### 6.8 复制构造函数

C++中经常使用一个常量或变量初始化另一个变量，例如：

```c++
double x＝5.0； 
double y＝x;
```

使用类创建对象时，构造函数被自动调用以完成对象的初始化，那么能否象简单变量的初始化一样，直接用一个对象来初始化另一个对象呢？答案是肯定的，以point类为例：

```c++
point pt1(2,3);  
point pt2=pt1;
```

后一个语句也可写成：

```c++
point pt2( pt1);
```

上述语句用pt1初始化pt2，相当于将pt1中每个数据成员的值复制到pt2中，这是表面现象。实际上，系统调用了一个复制构造函数。如果类定义中没有显式定义该复制构造函数时，编译器会隐式定义一个缺省的复制构造函数，它是一个inline、public的成员函数，其原型形式为：

```c++
//类名::类名 (const 类名 &);
point::point (const point &);
```

#### 6.8.1 复制构造函数调用机制

复制构造函数的调用示例：

```c++
point pt1(3,4); 	//构造函数
point pt2(pt1);	//复制构造函数
point pt3 = pt1;//复制构造函数
```

```c++
#include <iostream>
using namespace std;
class point
{
private:
    int xPos;
    int yPos;

public:
    point(int x = 0, int y = 0)
    {
        cout << "调用构造函数" << endl;
        xPos = x;
        yPos = y;
    }
    point(const point &pt) //复制构造函数的定义及实现
    {
        cout << "调用复制构造函数" << endl;
        xPos = pt.xPos;
        yPos = pt.yPos;
    }
    void print()
    {
        cout << "xPos: " << xPos << ",yPos: " << yPos << endl;
    }
};
#include "point.h"

int main()
{
    point pt1(3, 4);
    pt1.print();

    point pt2 = pt1; //等价于point pt2(pt1)，调用复制构造函数
    pt2.print();

    point pt3;
    pt3.print();

    point pt4(pt3); //等价于point pt4＝pt3，调用复制构造函数
    pt4.print();

    //	pt2 = pt1;		//调用默认的赋值运算符重载函数
    //	pt2.print();
    return 0;
}

#include <iostream>
using namespace std;

class CPoint
{
private:
    int x;
    int y;

public:
    //缺省构造函数,如果定义类时未指定任何构造函数，
    //系统将自动生成不带参数的缺省构造函数
    CPoint()
    {
        cout << "默认构造函数 " << this << " " << endl;
        x = 0;
        y = 0;
    }

    //带一个参数的可用于类型转换的构造函数
    CPoint(int ix)
    {
        cout << "1参数构造函数 " << this << " " << endl;
        x = ix;
        y = 0;
    }

    //带参数的构造函数
    CPoint(int ix, int iy)
    {
        cout << "2参数构造函数 " << this << " " << endl;
        x = ix;
        y = iy;
    }

    //拷贝构造函数,如果此函数不定义，系统将生成缺省拷贝构造函数功能,
    //缺省拷贝构造函数的行为是：用传入的对象参数的成员初始化正要建立的对象的相应成员
    CPoint(const CPoint &cp)
    {
        cout << "拷贝构造函数 " << this << " " << endl;
        x = cp.x;
        y = cp.y;
    }

    CPoint &operator=(const CPoint &cp)
    {
        cout << "赋值重载函数 " << this << " " << endl;
        if (this != &cp)
        {
            x = cp.x;
            y = cp.y;
        }
        return (*this);
    }

    //析构函数，一个类中只能有一个析构函数,如果用户没有定义析构函数，
    //系统会自动未类生成一个缺省的析构函数
    ~CPoint()
    {
        cout << "析构函数 " << this << " " << endl;
    }
};

void fun1(CPoint pt)
{
}

CPoint fun2()
{
    CPoint a;
    return a;
}

CPoint fun(CPoint a)
{
    return a;
}

int main(int argc, char *argv[])
{
    //第1类
    //	CPoint pt1 = CPoint();
    //当有析构函数的时候，CPoint()不会生成调用构造函数生成临时的匿名对象。
    //当没有析构函数的时候，CPoint()会生成一个临时的匿名对象，等价于CPoint pt1;这句话只会调用无参构造函数，不会调用拷贝构造函数

    //	CPoint pt2 = CPoint(1);
    //当有析构函数的时候，CPoint(1)不会生成调用构造函数生成临时的匿名对象。
    //当没有析构函数的时候，CPoint()会生成一个临时的匿名对象，等价于CPoint pt(1);这句话只会调用一个参数的构造函数，不会调用拷贝构造函数

    //	CPoint pt3 = 1;
    //普通数据类型转换为类类型，利用相应的构造函数就可以实现。等价于CPoint pt(1);

    //第2类
    /*拷贝构造函数与赋值运算符重载函数的区别:
		1. 拷贝构造函数是用已经存在的对象的各成员的当前值来创建一个相同的新对象。
		   在下述3种情况中，系统会自动调用所属类的拷贝构造函数。
			 1.1  当说明新的类对象的同时，要给它赋值另一个已经存在对象的各成员当前值。
			 1.2  当对象作为函数的赋值参数而对函数进行调用要进行实参和形参的结合时。
			 1.3  当函数的返回值是类的对象，在函数调用结束后返回主调函数处的时候。
		2. 赋值运算符重载函数要把一个已经存在对象的各成员当前值赋值给另一个已经存在的同类对象
	*/
    CPoint pt4; //调用无参构造函数
                //	CPoint pt5 = pt4;	//调用拷贝构造函数  属于1.1
                //	fun1(pt4);			//调用拷贝构造函数  属于1.2
                //	fun2();				//调用拷贝构造函数  属于1.3

    //	CPoint pt6 = fun2();//调用无参构造函数，拷贝构造函数，此处如果没有写析构函数，则还会调用一次拷贝构造函数
    //因为函数返回会生成一个临时对象，然后再将这个临时对象赋值给pt6，所以多调用一次拷贝构造函数；如果有析构函数
    //则不会生成中间的临时变量，所以少一次拷贝构造函数的调用

    //还可以通过下面函数验证
    //	CPoint pt7 = fun(pt4);
    //如果没有析构函数，会调用3次拷贝构造函数

    return 0;
}
```

拷贝构造函数在以下三种情况会自动调用：

- 当把一个已经存在的对象赋值给另一个新的对象时。
- 当实参和形参都是对象，进行形参和实参的结合时。
- 当函数的返回值是对象，函数调用完成返回时。

#### 6.8.2 缺省复制构造函数带来的问题

缺省的复制构造函数并非万金油，在一些情况下，必须由程序员显式定义缺省复制构造函数，先来看一段错误代码示例。

```c++
#include <iostream>
using namespace std;

class computer
{
private:
	char * brand;
	float price;

public:
	computer(const char * sz, float p)
	{
		brand = new char[strlen(sz) +1]; //构造函数中为brand指针动态分配内存
		strcpy(brand, sz);
		price = p;
	}
	
//	computer(const computer &cp)	//默认的复制构造函数的实现如下:
//	{
//		brand = cp.brand;	//对指针变量不能这样直接赋值，这是引起错误的根源
//		price = cp.price;
//	}

	~computer()
	{
		delete[] brand; //析构函数中释放申请到的动态内存
		cout << "清理现场" << endl;
	}
	
	void print()
	{
		cout << "品牌：" << brand << endl;
		cout << "价格：" << price << endl;
	}
};

#include "computer.h"
int main()
{
	computer comp1("Dell", 7000); //声明computer类对象comp，并初始化
	comp1.print();
	computer comp2(comp1); //调用缺省的复制构造函数
	comp2.print();
	return 0;
}
```

其中语句computer comp2(comp1)等价于：

```c++
comp2.brand = comp1.brand;
comp2.price = comp1.price;
```

后一句没有问题，但comp2.brand = comp1.brand却有问题：经过这样赋值后，两个对象的brand指针都指向了同一块内存，当两个对象释放时，其析构函数都要delete[]同一内存块，便造成了2次delete[]，从而引发了错误。

#### 6.8.3 解决方案――显式定义复制构造函数

如果类中含有**指针型的数据成员、需要使用动态内存**，程序员最好显式定义自己的复制构造函数，避免各种可能出现的内存错误，见代码。

```c++
computer(const computer &cp) //自定义复制构造函数
{
    //重新为brand开辟和cp.brand同等大小的动态内存
    brand = new char[strlen(cp.brand) + 1];
    strcpy(brand, cp.brand); //字符串复制
    price = cp.price;
}
```

```c++
#include <cstring>
#include <iostream>
using namespace std;
class computer
{
private:
    char *brand;
    float price;

public:
    computer(const char *sz, float p)
    {
        brand = new char[strlen(sz) + 1];
        strcpy(brand, sz);
        price = p;
    }
    
    //1.形参的引用符号&不能删，如果删除，根据复制构造函数的调用规则，会进入无穷递归的循环中
    //2形参的const不能删，如果删除，当传递过来的实参是一个临时对象或右值，就不能正常调用
    computer(const computer &cp)//自定义复制构造函数
    {
        brand = new char[strlen(cp.brand) + 1];//重新为brand开辟于cp.brand同等大小的动态内存
        strcpy(brand, cp.brand);//字符串复制
        price = cp.price;
    }
    ~computer()
    {
        delete[] brand;
        cout << "清理现场" << endl;
    }
    void print()
    {
        cout << "品牌：" << brand << endl;
        cout << "价格：" << price << endl;
    }
};

#include "computer.h"
int main()
{
    computer comp1("Dell", 7000); //调用有参构造函数声明computer类对象comp1，并初始化
    comp1.print();
    computer comp2(comp1); //调用复制构造函数声明computer类对象comp2，并用comp1为其初始化
    comp2.print();
    return 0;
}
```

#### 6.8.4 关于构造函数和复制构造函数

- 复制构造函数可以看成是一种特殊的构造函数，这里姑且区分为“复制构造函数”和“普通构造函数”，因此，它也支持初始化表达式。
- 创建对象时，只有一个构造函数会被系统自动调用，具体调用哪个取决于创建对象时的参数和调用方式。C++对编译器何时提供缺省构造函数和缺省复制构造函数有着独特的规定

| 用户是否自定义了一个普通构造函数 | 用户是否自定义了复制构造函数 | 编译器是否自动提供缺省构造函数 | 编译器是否自动提供缺省复制构造函数 |
| -------------------------------- | ---------------------------- | ------------------------------ | ---------------------------------- |
| 否                               | 否                           | 是                             | 是                                 |
| 否                               | 是                           | 否                             | 否                                 |
| 是                               | 否                           | 否                             | 是                                 |
| 是                               | 是                           | 否                             | 否                                 |

#### 6.8.5 构造函数调用示例

```c++
CTest t0();	//这是函数的声明，不是实例化类
CTest t1;	//缺省构造函数
CTest t2(1);	//一个参数的构造函数
CTest t3(1, 2);	//两个参数的构造函数
CTest t4 = 1;	//等价于CTest t4(1);	//explicit
CTest t5 = t1;	//CTest(t1);
CTest t6 = CTest();//CTest(1); CTest(1,2);
t6 = CTest(1);
t6 = 1;		//首先调用单个参数的构造函数，生成临时
			//对象CTest(1), 然后调用赋值运算符函数
t6 = t1;		//调用赋值运算符函数
```

```c++
//为了防止CPoint pt = 2;和CPoint pt2 = pt1;这种隐性转换，可以在相应的构造函数前增加explicit标识符
#include <iostream>
using namespace std;

class CPoint
{
protected:
	int x;
	int y;

public:
	//缺省构造函数,如果定义类时未指定任何构造函数，
	//系统将自动生成不带参数的缺省构造函数
	CPoint()
	{
		cout << "默认构造函数 " << this << " ";
		x = 0;
		y = 0;
	}

	//带一个参数的可用于类型转换的构造函数
//	explicit	//加上 explicit 可防止 CPoint pt1 = 1; 这种隐性转换
	CPoint(int ix)
	{
		cout << "1参数构造函数 " << this << " ";
		x = ix;
		y = 0;
	}

	//带参数的构造函数
	CPoint(int ix, int iy)
	{
		cout << "2参数构造函数 " << this << " ";
		x = ix;
		y = iy;
	}

	//拷贝构造函数,如果此函数不定义，系统将生成缺省拷贝构造函数功能,
	//缺省拷贝构造函数的行为是：用传入的对象参数的成员初始化正要建立的对象的相应成员
//	explicit	//加上 explicit 可防止 CPoint pt2 = pt1; 这种隐性转换
	CPoint(const CPoint &cp)
	{
		cout << "拷贝构造函数 " << this << " ";
		x = cp.x;
		y = cp.y;
	}

	CPoint &operator=(const CPoint &cp)
	{
		cout << "赋值重载函数 " << this << " ";
		if (this != &cp)
		{
			x = cp.x;
			y = cp.y;
		}
		return (*this);
	}

	//析构函数，一个类中只能有一个析构函数,如果用户没有定义析构函数，
	//系统会自动未类生成一个缺省的析构函数
	~CPoint()
	{
		cout << "析构函数 " << this << " ";
	}
};

int main(int argc, char* argv[])
{
	CPoint p0();		//这是函数的声明，不是实例化类

	cout << endl << "CPoint pt1;\t\t";
	CPoint pt1;			//缺省构造函数
	
	cout << endl << "CPoint pt2(1);\t\t";
	CPoint pt2(1);		//一个参数的构造函数

	cout << endl << "CPoint pt3(1, 2);\t";
	CPoint pt3(1, 2);	//两个参数的构造函数

	cout << endl << "CPoint pt4 = 1;\t\t";
	CPoint pt4 = 1;		//等价于CPoint t4(1);	//explicit

	cout << endl << "CPoint pt5 = t1;\t";
	CPoint pt5 = pt1;	//CPoint(t1);

	cout << endl << "CPoint pt6 = CPoint();\t";
	CPoint pt6 = CPoint();	//CPoint(1); CPoint(1,2);

	cout << endl << "pt6 = CPoint(1);\t";
	pt6 = CPoint(1);

	cout << endl << "pt6 = 1;\t\t";
	pt6 = 1;			//首先调用单个参数的构造函数，生成临时对象CPoint(1), 然后调用赋值运算符函数
	
	cout << endl << "pt6 = t1;\t\t";
	pt6 = pt1;			//调用赋值运算符函数

	cout << endl << endl;
	return 0;
}
```

#### 6.8.6 赋值运算符 =

- 成员函数形式的运算符声明和实现与成员函数类似，首先应当在类定义中声明该运算符，声明的具体形式为：

  ```c++
  返回类型  operator 运算符（参数列表）;
  ```

- 既可以在类定义的同时定义运算符函数使其称为inline型，也可以在类定义之外定义运算符函数，但要使用作用域限定符“::”，类外定义的基本格式为：

```c++
返回类型  类名::operator 运算符（参数列表）
{
…
}
```

```c++
//成员函数形式 的 运算符重载
#include <iostream>
using namespace std;

class complex					//定义复数类 complex
{
private:
	double real, imag;			//private 成员，分别代表实部和虚部

public:
	complex(double r = 0.0, double i = 0.0)	//构造函数，带缺省参数值
	{
		real = r;
		imag = i;
	}

	complex operator+= (const complex &);	//成员函数形式重载加+=

	complex operator+(const complex &);	//成员函数形式重载加+
	complex operator-(const complex &);	//成员函数形式重载减－
	complex operator-();					//成员函数形式重载一元－（取反）
	complex operator*(const complex &);	//成员函数形式重载乘*
	complex operator/(const complex &);	//成员函数形式重载除*

	complex& operator++();					//成员函数形式重载前置++
	complex operator++(int);				//成员函数形式重载后置++

	void disp()								//成员函数，输出复数
	{
		cout << real << " + " << "i*" << imag << endl;
	}
};

complex complex::operator+=(const complex& CC)		//加+=的实现
{
	real += CC.real;
	imag += CC.imag;
	return (*this);
}

complex complex::operator+(const complex& CC)		//加+的实现
{
	return complex(real + CC.real, imag + CC.imag);
}

complex complex::operator-(const complex& CC)		//减-的实现
{
	return complex(real - CC.real, imag - CC.imag);
}

complex complex::operator*(const complex& CC)		//乘*的实现
{
	return complex(real * CC.real - imag * CC.imag, real * CC.imag + imag * CC.real);
}

complex complex::operator/(const complex& CC)		//除/的实现
{
	return complex((real * CC.real + imag + CC.imag) / (CC.real * CC.real + CC.imag * CC.imag),
		(imag * CC.real - real * CC.imag) / (CC.real * CC.real + CC.imag * CC.imag));
}

complex complex::operator-()						//单目-，即取反的实现
{
	return complex(-real, -imag);
}

complex& complex::operator++()						//前置++的实现
{
	cout << "前置++" << endl;
	++real;
	++imag;
	return (*this);
}

complex complex::operator++(int)	//后置++的实现，体会和前置++的区别
{
	cout << "后置++" << endl;
	complex cTemp = (*this);	//最终的返回值的是原来的值，因此需要先保存原来的值
	++(*this);					//返回后原来的值需要加1
	return cTemp;
}

int main()
{
	complex cx1(1.0, 2.0), cx2(3.0, 4.0), cxRes;

	cxRes += cx2;		//相当于cxRes.operator+=(cx2)
	cxRes.disp();
	
	cxRes = cx1 + cx2;	//相当于cx1.operator+(cx2)
	cxRes.disp();
	
	cxRes = cx1 - cx2;	//相当于cx1.operator-(cx2)
	cxRes.disp();
	
	cxRes = cx1 * cx2;	//相当于cx1.operator*(cx2)
	cxRes.disp();
	
	cxRes = cx1 / cx2;	//相当于cx1.operator/(cx2)
	cxRes.disp();
	
	cxRes = -cx1;		//相当于cx1.operator-()
	cxRes.disp();
	
	cout << endl;
	
	complex cx3(1.0, 1.0), cx4(5.0, 5.0);
	
	cxRes = ++cx3;		//相当于cx3.operator++()
	cxRes.disp();
	cx3.disp();

	cout << endl;
	
	cxRes = cx4++;		//相当于cx4.operator++(0)
	cxRes.disp();
	cx4.disp();

	cout << endl;
	
	//注意下述语句在友元函数形式和成员函数形式中的对比。
	cxRes = cx1 + 5;	//相当于cx1.operator+(5) 或 cx1.operator+(complex(5))
	cxRes.disp();

//	cxRes = 5 + cx1;	//错误. 相当于5.operator+(cx1);
//	cxRes.disp();

	return 0;
}
```

- 赋值运算是一种很常见的运算，如果不重载赋值运算符，编译器会自动为每个类生成一个缺省的赋值运算符重载函数，先看下面的语句：

```c++
对象1＝对象2;
```

- 实际上是完成了由对象2各个成员到对象1相应成员的复制，其中包括指针成员，这和第8章中复制构造函数和缺省复制构造函数有些类似，如果对象1中含指针成员，并且牵扯到类内指针成员动态申请内存时，问题就会出现。
- 注意下述两个代码的不同：

```c++
类名 对象1＝对象2;	//复制构造函数
```

```c++
类名 对象1;	//默认构造函数
对象1＝对象2;	//赋值运算符函数
Int a = 3; int b; b = a; 
```

```c++
//9-9 赋值运算符重载。如果没有，执行时会出错.
#include <iostream>
using namespace std;

class computer
{
private:
	char *brand;						//字符指针brand
	float price;

public:
	computer()							//无参构造函数
	{
		brand = NULL;					//brand初始化为NULL
		price = 0;
		cout << "无参构造函数被调用" << endl;
	}

	computer(const char* sz,float p)
	{
		brand = new char[strlen(sz)+1];	//构造函数中为brand分配一块动态内存
		strcpy(brand, sz);				//字符串复制
		price = p;
		cout << "带参构造函数被调用" << endl;
	}

	computer(const computer& cp)		//复制构造函数
	{
		brand = new char[strlen(cp.brand) + 1];	//为brand分配动态内存
		strcpy(brand, cp.brand);		//字符串复制
		price = cp.price;
		cout << "复制构造函数被调用" << endl;
	}

//	//如果我们没有重载=，则系统会隐式的重载成如下形式:
//	computer &operator=(const computer &cp)
//	{
//		price = cp.price;
//		brand = cp.brand;
//		cout<<"系统默认赋值函数被调用"<<endl;
//		return (*this);
//	}
//
//	//应该使用下述函数取代上述系统隐式的定义
//	computer &operator=(const computer &cp)		//成员函数形式重载赋值运算符
//	{
//		if (this==&cp)							//首先判断是否是自赋值，是的话返回当前对象
//			return  (*this);
//
//		price=cp.price;							//如果不是自赋值，先对price赋值
//		delete[] brand;							//防止内存泄露，先释放brand指向的内容
//		brand=new char[strlen(cp.brand)+1];		//为brand重新开辟一块内存空间
//		if (brand!=NULL)						//如果开辟成功
//		{
//			strcpy(brand,cp.brand);				//复制字符串
//		}
//		cout<<"赋值运算符重载函数被调用"<<endl;
//
//		return  (*this);										//返回当前对象的引用，为的是实现链式赋值
//	}

	~computer()							//析构函数，释放动态内存，delete[] NULL不会出错
	{
		delete[] brand;
		cout << "析构函数被调用" << endl;
	}

	void print()						//成员函数，输出信息
	{
		cout << "品牌：" << brand << endl;
		cout << "价格：" << price << endl;
	}
};

int main()
{
	computer com1("Dell", 2000);	//调用含参构造函数声明对象com1
	computer com2 = com1;			//调用复制构造函数

	if (true)
	{
		computer com3;				//调用无参构造函数
		com3 = com1;				//调用赋值运算符重载函数
	}

	return 0;
}
```

### 6.9 特殊数据成员的初始化

有4类特殊的数据成员（常量成员、引用成员、类对象成员、静态成员），其初始化及使用方式与前面介绍的普通数据数据成员有所不同

#### 6.9.1 const数据成员的初始化

- 数据成员可以由const修饰，这样，一经初始化，该数据成员便具有“只读属性”，在程序中无法对其值修改。
- 事实上，在构造函数体内或复制构造函数体内初始化const数据成员是非法的

```c++
// const成员变量的初始化不能在构造函数体内进行
#include <iostream>
using namespace std;

class point
{
private:
	const int xPos; //符号常量成员xPos和yPos
	const int yPos;

public:
	point(int x, int y)
	{
		xPos = x; //错误，无法直接赋值
		yPos = y;
	}

	point(const point & pt)
	{
		xPos = pt.xPos;
		yPos = pt.yPos;
	}

	void print()
	{
		cout << "xPos: " << xPos << ",yPos: " << yPos << endl;
	}
};

int main()
{
	point pt1(3, 4);//调用有参构造函数
	pt1.print();

	point pt2(pt1);//调用复制构造函数
	pt2.print();

	return 0;
}
```

- const数据成员只能通过成员初始化表进行初始化

```c++
// const数据成员只能在初始化表中进行初始化
#include <iostream>
using namespace std;

class point
{
private:
	const int xPos;
	const int yPos;

public:
	point(int x, int y):xPos(x), yPos(y) //const数据成员只能在初始化表中进行初始化
	{
	}

	point(const point & pt):xPos(pt.xPos), yPos(pt.yPos) //const数据成员只能在初始化表中进行初始化
	{
	}

	void print()
	{
		cout << "xPos: " << xPos << ",yPos: " << yPos << endl;
	}
};

int main()
{
	point pt1(3, 4);
	pt1.print();

	point pt2(pt1);
	pt2.print();

	return 0;
}
```

#### 6.9.2 引用成员的初始化

对于引用类型的数据成员，同样只能通过成员初始化表达式进行初始化

```c++
#include <iostream>
using namespace std;
class point
{
private:
	int xPos;
	int yPos;
	int &ref1;
	double &ref2;
public:
	point(int x, int y, double &z):ref1(xPos), ref2(z) //引用成员的初始化同样要放在初始化表中
	{
		xPos = x;
		yPos = y;
	}
	//复制构造函数与此一致：)引用成员的初始化同样要放在初始化表中
	point(const point &pt):ref1(pt.ref1), ref2(pt.ref2
	{
		xPos = pt.xPos;
		yPos = pt.yPos;
	}
	void print()
	{
		cout << "xPos: " << xPos << ", yPos: " << yPos;
		cout << ", ref1:  " << ref1 << ", ref2:  " << ref2 << endl;
	}
	void SetX(int x)
	{
		xPos = x;
	}
};

#include "point.h"
int main()
{
	double outInt = 5.0;
	point pt1(3, 4, outInt);
	pt1.print();

	point pt2(pt1);
	pt2.print();

	cout << "改变pt1中的x后" << endl;
	pt1.SetX(7);
	pt1.print();
	pt2.print();

	outInt = 6;
	cout << "outInt变化后：" << endl;
	pt1.print();
	pt2.print();

	return 0;
}
```

#### 6.9.3 类对象成员的初始化

类数据成员也可以是另一个类的对象，比如，一个直线类对象中包含两个point类对象，在直线类对象创建时可以在初始化列表中初始化两个point对象。

```c++
#include <iostream>
using namespace std;
class point //点类的定义
{
private:
	int xPos;
	int yPos;
public:
	point(int x = 0, int y = 0) //带缺省调用的构造函数
	{
		cout << "点的构造函数被执行" << endl;
		xPos = x;
		yPos = y;
	}
	point(const point & pt) //复制构造函数
	{
		cout << "点的复制构造函数被执行" << endl;
		xPos = pt.xPos;
		yPos = pt.yPos;
	}
	void print()
	{
		cout << "( " << xPos << ", " << yPos << ")";
	}
};

class line //line类的定义
{
private:
	point pt1; //point类对象作为line类成员，此处若写成point pt1(3,4)，错
	point pt2;
public:
	line(int x1, int y1, int x2, int y2): pt1(x1, y1), pt2(x2, y2) //line对象的有参构造函数
	{
		cout << "线的构造函数被执行" << endl;
	}
	line(const line &ll):pt1(ll.pt1), pt2(ll.pt2) //line对象的复制构造函数
	{
		cout << "线的复制构造函数被执行" << endl;
	}
	void draw()
	{
		pt1.print();
		cout << "  to  ";
		pt2.print();
		cout << endl;
	}
};

#include "PointAndLine.h"
int main()
{
	line l1(1, 2, 3, 4); //调用有参构造函数
	l1.draw();
	line l2(l1); //调用复制构造函数
	l2.draw();
	return 0;
}
```

#### 6.9.4 特别说明

对复制构造函数来说，一旦给出了自己定义的形式，**编译器便不会提供缺省的复制构造函数**，因此，确保自定义的复制构造函数的有效性很重要。因此，在一些必须使用自定义复制构造函数的场合，掌握特殊成员的用法很必要。所举例子中，尽管有些复制构造函数纯属“画蛇添足”，用系统提供的缺省复制构造函数足以实现想要的功能，但还是给出了完整的书写形式，这就是原因所在

#### 6.9.5 static数据成员的初始化

- C++允许使用static（静态存储）修饰数据成员，这样的成员在编译时就被创建并初始化的（与之相比，对象是在运行时被创建的），且其实例只有一个，被所有该类的对象共享，就像住在同一宿舍里的同学共享一个房间号一样。静态数据成员和第6章中介绍的静态变量一样，程序执行时，该成员已经存在，一直到程序结束，任何对象都可对其进行访问。
- 静态数据成员的初始化必须在类申明之外进行，且不再包含static关键字，格式如下：

```c++
类型 类名::变量名 = 初始化表达式; //普通变量
类型 类名::对象名(构造参数);	 //对象变量
float computer::total_price = 0;
```

```c++
#include <iostream>
using namespace std;
class computer
{
private:
	float price; //float型数据成员price，表示价格
	static float total_price; //statci成员，总价，不依附于某个对象
public:
	computer(const float p) //构造函数，模拟买电脑的操作，对total_price进行累加	
	{
		price = p;
		total_price += p;
	}
	~computer() //析构函数，模拟退还电脑的操作，从total_price中减去所退电脑的price
	{
		total_price -= price;
	}
	void print() //输出函数
	{
		cout << "总价：" << total_price << endl;
	}
};

#include "computer.h"
float computer::total_price = 0;//初始化
int main()
{
	computer comp1(7000); //买入电脑1
	cout << "购买电脑1后" << endl;
	comp1.print();
	computer comp2(4999); //买入电脑2
	cout << "购买电脑2后" << endl;
	comp2.print();
	computer comp3(2500); //买入电脑3
	cout << "购买电脑3后" << endl;
	comp1.print(); //此处调用comp1.print()、comp2.print()和comp3.print()输出结果相同
	comp2.~ computer(); //退掉电脑2
	cout << "退掉电脑2后" << endl;
	comp3.print();
	return 0;
}
```

### 6.10 特殊函数成员

除了构造函数、复制构造函数和析构函数外，其他成员函数被用来提供特定的功能，一般来说，提供给外部访问的函数称为接口，访问权限为public，而一些不供外部访问，仅仅作为内部功能实现的函数，访问权限设为private。主要讨论函数成员的一些特殊用法。

#### 6.10.1 静态成员函数

- 成员函数也可以定义成静态的，与静态成员变量一样，系统对每个类只建立一个函数实体，该实体为该类的所有对象共享。
- 静态成员函数体内不能使用非静态的成员变量和非静态的成员函数；只能调用静态成员数据和函数
- 静态成员函数的参数列表中不含有this指针

```c++
#include <iostream>
using namespace std;
class computer
{
private:
	char *name;
	float price;
	static float total_price; //静态数据成员
public:
	computer(const char *chr, const float p) //构造函数，模拟买电脑操作
	{
		name = new char[strlen(chr) + 1];
		strcpy(name, chr);
		price = p;
		total_price += p;
	}
	~computer() //析构函数，模拟退掉电脑的操作
	{
		if (name)
		{
			delete[] name;
			name = NULL;
		}
		total_price -= price;
	}
	static void print_total() //静态成员函数，原则上只能访问静态数据成员
	{
		cout << "总价：" << total_price << endl;
	}
	static void print(computer &com); //静态成员函数print()原型，如果要访问某具体对象，必须传递参数
};
void computer::print(computer &com) //静态成员函数print()实现
{
	cout << "名称" << com.name << endl;
	cout << "价格" << com.price << endl;
}

#include "computer.h"
float computer::total_price = 0; //初始化
int main()
{
	computer comp1("IBM", 7000); //声明类对象comp1，初始化，买入
	computer::print(comp1); //类名加作用域限定符访问statci成员函数，传递参数comp1
	computer::print_total(); //类名加作用域限定符访问statci成员函数

	computer comp2("ASUS", 4999); //声明类对象comp2，初始化，买入
	computer::print(comp2); //类名加作用域限定符访问statci成员函数，传递参数comp2
	computer::print_total();

	comp2.~computer(); //析构函数调用，退还电脑
	computer::print_total();

	return 0;
}


```

#### 6.10.2 const成员函数

之前已经介绍了const在函数中的应用，实际上，const在类成员函数中还有种特殊的用法，把const关键字放在函数的参数表和函数体之间（与之前介绍的const放在函数前修饰返回值不同），称为const成员函数，其特点有二：

- 只能读取类数据成员，而不能修改之
- 只能调用const成员函数，不能调用非const成员函数

如Point类中的打印函数：

```c++
void print(Point * const this)
void print() const 
//		=>
void print(const Point * const this)  const
void print();
//      => 
void print(Point * const this);
```

其基本定义格式为：

1. 类内定义时：

```c++
类型 函数名(参数列表) const
{
	函数体
}
```

2. 类外定义时，共分两步：

```c++
类内声明：
	类型 函数名(参数列表) const；
类外定义
	类型 类名::函数名(参数列表) const
	{
		函数体
	}
```

```c++
#include <iostream>
using namespace std;

class point
{
	int x; //默认为private的数据成员x和y
	int y;

public:
	point(int xp = 0, int yp = 0) //构造函数
	{
		x = xp;
		y = yp;
	}

	void print() const //const成员函数内无法修改数据成员，否则编译器报错
	{
		x = 5; //1. 试图修改x将引发编译器报错
		set(); //2. 试图调用非const函数将引发编译器报错
		cout << "x: " << x << " ,y: " << y << endl;
	}

	void set() //将set()定义成const函数就能解决问题
	{
	}
};

int main()
{
	point pt;		//声明类对象，以缺省参数形式调用构造函数
	pt.print();		//调用const成员函数
}
```

### 6.11 对象的组织

有了自己定义的类，或者使用别人定义好的类创建对象，其机制与使用int等创建普通变量几乎完全一致，同样可以const对象、创建指向对象的指针，创建对象数组，还可使用new和delete等创建动态对象。

#### 6.11.1 const对象

类对象也可以声明为const对象，一般来说，能作用于const对象的成员函数除了构造函数和析构函数，便只有const成员函数了，因为const对象只能被创建、撤销以及只读访问，改写是不允许的。

```c++
#include <iostream>
using namespace std;

class point //类定义
{
    int x; //默认private型成员变量x和y
    int y;

public:
    point(int xp = 0, int yp = 0) //构造函数
    {
        x = xp;
        y = yp;
    }

    ~point() //析构函数
    {
        x = -1;
    }

    void SetX(int xp) //非const成员函数SetX，设置x
    {
        x = xp;
    }

    void SetY(int yp) //非const成员函数SetY，设置y
    {
        y = yp;
    }

    void print() const //const成员函数print，不能修改x和y
    {
        cout << "x: " << x << " ,y: " << y << endl;
    }
};

#include "point.h"
using namespace std;

int main()
{
    point pt(3, 4); //声明一个普通类变量pt
    pt.SetX(5);     //使用pt可调用非const成员函数
    pt.SetY(6);
    pt.print(); //pt也可调用const成员函数

    const point ptC(1, 2); //声明一个const对象（类变量）
    //ptC.SetX(8);			//错误，ptC是const对象，只能调用const成员函数
    //ptC.SetY(9);			//错误，ptC是const对象，只能调用const成员函数
    ptC.~point(); //正确，const对象也能调用非const类型的析构函数
    ptC.print();  //正确，const对象只能调用const成员函数

    return 0;
}
```

#### 6.11.2 指向对象的指针

- 对象占据一定的内存空间，和普通变量一致，C++程序中采用如下形式声明指向对象的指针：

```c++
类名 *  指针名 [=初始化表达式];
```

- 初始化表达式是可选的，既可以通过取地址（&对象名）给指针初始化，也可以通过申请动态内存给指针初始化，或者干脆不初始化（比如置为NULL），在程序中再对该指针赋值。 
- 指针中存储的是对象所占内存空间的首地址。
- 针对上述定义，则下列形式都是合法的：

```c++
point pt;			//默认构造函数
point *ptr = NULL;		//空指针
point *ptr = &pt;		//取某个对象的地址
point *ptr = new point(1, 2.2);//动态分配内存并初始化
point *ptr = new point[5];	//动态分配5个对象的数组空间
```

- 使用指针对象：ptr->print();(*ptr).print();都是合法的。

```c++
#include <iostream>
using namespace std;

class point
{
	int x;
	int y;

public:
	point(int xp = 0, int yp = 0)
	{
		x = xp;
		y = yp;
	}
	
	void print()
	{
		cout << "x: " << x << ", y: " << y << endl;
	}
};
```

#### 6.11.3 对象的大小(sizeof)

- 对象占据一定大小的内存空间。总的来说，对象在内存中是以结构形式（只包括非static数据成员）存储在数据段或堆中，类对象的大小（sizeof）一般是类中所有非static成员的大小之和。在程序编译期间，就已经为static变量在静态存储区域分配了内存空间，并且这块内存在程序的整个运行期间都存在。而类中的成员函数存在于代码段中，不管多少个对象都只有一个副本。
- 对象的大小，有一些特殊之处需要强调：
  - C++将类中的引用成员当成“**指针**”来维护，占据4个内存字节。
  - 如果类中有虚函数时，虚析构函数除外，还会额外分配一个指针
  - 用来指向虚函数表（vtable），因此，这个时候对象的大小还要加4。
    指针成员和引用成员属于“最宽基本数据类型”的考虑范畴。

```c++
#include <iostream>
using namespace std;

class cex
{
private:
	int a;	//int型，在一般系统上占据4个内存字节	4
	char b;	//char型，占1个内存字节					1 + 3 (3浪费)
	float c;	//单精度浮点型，占4个内存字节	4 + 4 (4浪费)
	double d;	//double型，占8个内存字节		8
	short e[5]; //short型数组，每个元素占2个内存字节	8 + 2
	char & f;	//引用，当成指针维护	2 + 4 (2浪费)
	double & g; //引用，当成指针维护	4 + 4 (后4浪费)
	static int h;	//static成员，公共内存，不影响单个对象的大小 0
	
public:
	cex():f(b), g(d) //和构造函数，引用成员必须在初始化表中初始化
	{
	}
	void print()	//成员函数的定义，普通成员函数不影响对象大小
	{
		cout << "Hello" << endl;
	}
};

int cex::h = 0;		//static成员的初始化

int main()
{
	cex c;
	cout << "sizeof(cex):  " << sizeof(cex) << endl; //输出类对象的大小sizeof(cex) = 48
	return 0;
}
```

#### 6.11.4 this指针

- 一个类的所有对象共用成员函数代码段，不管有多少个对象，每个成员函数在内存中只有一个版本，那编译器是如何知道是哪个对象在执行操作呢，答案就是“this指针”。

- this指针是隐含在成员函数内的一种指针，称为**指向本对象的指针**，可以采用诸如“this->数据成员”的方式来存取类数据成员。

```c++
class Ex
{
private:
    int x;
    int y;

public:
    void Set()
    {
        this->x = 1;
        this->y = 2;
    }
};
```

- 对于类成员函数而言，并不是一个对象对应一个单独的成员函数体，而是此类的所有对象共用这个成员函数体。 当程序被编译之后，此成员函数地址即已确定。而成员函数之所以能把属于此类的各个对象的数据区别开, 就是靠这个this指针。函数体内所有对类数据成员的访问， 都会被转化为this->数据成员的方式。
- this作用域是在类内部，当在类的非静态成员函数中访问类的非静态成员的时候，编译器会自动将对象本身的地址作为一个隐含参数传递给函数

```c++
class CNullPointCall
{
public:
        static void Test1();
        void Test2();
        void Test3(int iTest);
        void Test4();

private:
        static int m_iStatic;
        int m_iTest;
};
int CNullPointCall::m_iStatic = 0;
void CNullPointCall::Test1() {    cout << m_iStatic << endl; }
void CNullPointCall::Test2()
{
        cout << "Very Cool!" << endl;
     
}
void CNullPointCall::Test3(int iTest)
{
        cout << iTest << endl;
     
}
void CNullPointCall::Test4()
{
        cout << m_iTest << endl;
     
}
```

#### 6.11.5 对象数组

对象数组和标准类型数组的使用方法并没有什么不同，也有声明、初始化和使用3个步骤。

1. 对象数组的声明：

```c++
类名 数组名[对象个数];
```

这种格式会自动调用无参或所有参数都有缺省值的构造函数，类定义要符合该要求，否则编译报错。

2. 对象数组的初始化：可以在声明时初始化。
   对于point(int ix,int iy){}这种没有缺省参数值的构造函数：

```c++
point pt[2]={point(1,2), point(3,4)}; // 正确
point pt[ ]={point(1,2), point(3,4)};  // 正确
point pt[5]={point(1,2), point(3,4)}; // 错误
//没有写出来的都是采用point()的形式，需要调用无参构造函数
```

语句#1和#2是正确的，但语句#3错误，因为pt的后3个元素会自动调用无参的或者所有参数都有缺省值的构造函数，但这样的构造函数不存在。

```c++
//对象数组的声明
#include <iostream>
using namespace std;

class point
{
private:
	int x;
	int y;

public:
//	point(int ix = 0, int iy = 0)
	point(int ix, int iy)
	{
		static int iCount = 0;
		iCount++;
		cout << iCount << " .构造函数被调用" << endl;
		x = ix;
		y = iy;
	}
};

int main()
{
	//错误: 没有合适的构造函数
	point pt1[2];

	//正确
	point pt2[2] = {point(1,2), point(3,4)};

	//正确, 自动确定数组的元素个数
	point pt3[] = {point(1,2), point(3,4)};

	//错误: 后3个元素会自动调用无参的构造函数，但这样的构造函数不存在
	point pt4[5] = {point(1,2), point(3,4)};

	//解决方法: 给上述构造函数的2个参数定义缺省值
	return 0;
}
```

### 6.12 为对象动态分配内存

和把一个简单变量创建在动态存储区一样，可以用new和delete为对象分配动态存储区，在复制构造函数一节中已经介绍了为类内的指针成员分配动态内存的相关范例，本节主要讨论如何为对象和对象数组动态分配内存。

#### 6.12.1 使用new和 delete 为单个对象分配/释放动态内存

```c++
//使用new/delete为类指针分配/释放内存
#include <iostream>
using namespace std;

class point
{
private: //private数据成员列表
    int x;
    int y;

public:
    point(int xp = 0, int yp = 0) //构造函数，带缺省参数值
    {
        x = xp;
        y = yp;
        cout << "构造函数被调用" << endl;
    }

    ~point() //析构函数
    {
        cout << "析构函数被调用" << endl;
    }

    void print() //成员函数，类内部实现
    {
        cout << "x:  " << x << ", y:  " << y << endl;
    }
};

int main()
{
    point *p = new point(4, 5); //动态申请一块内存，存储point类对象，并将地址赋值给point型指针p
    p->print();                 //使用指针加－>调用成员函数
    delete p;                   //释放动态申请的内存，防止内存泄露
    p = NULL;                   //养成良好习惯，防止野指针

    point *pp = new point[3]; //此时你会发现分配了28个字节的内存空间，多出的最开始的4个字节的内存空间，存放3
    delete[] pp;
    return 0;
}
```

此时，如果写了析构函数，并且是用new动态分配数组的内存空间的话，那么还会多开辟4个字节的内存空间，用来存放分配数组的大小。

#### 6.12.2 使用new和delete[]为对象数组分配/释放动态空间

使用new为对象数组分配动态空间时，不能显式调用对象的构造函数，因此，对象要么没有定义任何形式的构造函数（由编译器缺省提供），要么显式定义了一个（且只能有一个）所有参数都有缺省值的构造函数。

```c++
// 使用new和delete[]为对象数组分配/释放动态空间
#include <iostream>
using namespace std;

class point
{
private: //private数据成员列表
    int x;
    int y;

public:
    point(int xp = 0, int yp = 0) //构造函数，带缺省参数值
    {
        x = xp;
        y = yp;
        cout << "构造函数被调用" << endl;
    }

    ~point() //析构函数
    {
        cout << "析构函数被调用" << endl;
    }

    void print() //成员函数，类内部实现
    {
        cout << "x:  " << x << ", y:  " << y << endl;
    }

    void Set(int xp, int yp) //成员函数，类内部实现，用来修改成员x和y
    {
        x = xp;
        y = yp;
    }
};

int main()
{
    point *p = new point[2]; //申请一块动态内存，连续存放连个point对象，将首地址赋值给point指针p
    p[0].print();            //可以将指针当成数组名，使用下标运算符访问对应对象，等价性
    p[1].Set(4, 5);          //调用数据元素（对象）的成员函数Set
    p[1].print();
    delete[] p; //释放申请的动态内存
    p = NULL;   //养成良好习惯，防止野指针
    return 0;
}
```

#### 6.12.3 malloc和free不能为对象动态申请内存

malloc/free无法满足动态对象的要求，因为malloc和free无法像new/delete及new/delete[]那样自动调用对象的构造函数和析构函数。

#### 6.12.4 单例设计模式（singleton)

设计模式：singleton
要求：内存中只能有一份对象，即类在内存中只能有1个实例
实现步骤：

1. 将构造函数私有化。
2. 在类中定义一个静态的指针对象（一般设为私有），并在类外初始化为空

3. 定义一个返回值为类指针的静态成员函数，如果2中的指针对象为空，则初始化对象，以后再有对象调用该静态成员函数的时候，不再初始化对象，而是直接返回对象，保证类在内存中只有一个实例。

```c++
#include <iostream>
using namespace std;
class CA
{
private:
    static CA *pA;
    int x;
    int y;
    CA(int x = 0, int y = 0)
    {
        this->x = x;
        this->y = y;
        cout << "构造函数被调用" << this << endl;
    }

public:
    static CA *GetInstance(int x = 0, int y = 0)
    {
        if (pA == NULL)
        {
            pA = new CA(x, y);
        }
        return pA;
    }
    void disp() const
    {
        cout << "(" << this->x << "," << this->y << ")" << endl;
    }
};
CA *CA::pA = NULL; //静态成员要在外面初始化
int main()
{
    CA *p1 = CA::GetInstance(3, 4);
    CA *p2 = CA::GetInstance(2, 1);
    CA *p3 = CA::GetInstance(6, 2);
    cout << p1 << endl;
    cout << p2 << endl;
    cout << p3 << endl;
    p1->disp();
    p2->disp();
    p3->disp();
}
//此时p1、p2、p3是同一个，输出来的值相等，所以调用disp都输出同样的内容
```

