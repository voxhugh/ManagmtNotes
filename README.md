<h1 align="center">C++ 指南</h1>

<div align="center">
  <img src="https://img.shields.io/badge/C++-Advanced-blue?style=for-the-badge&logo=cplusplus" alt="C++ Level">
  <img src="https://img.shields.io/badge/Note-Tips-green?style=for-the-badge" alt="Note Type">
</div>

## 📚 核心概念

| [基础](#基础)         | [关键字](#关键字) | [内存四区](#内存四区) | [指针](#指针) | [引用](#引用) |
| --------------------- | ----------------- | --------------------- | ------------- | ------------- |
| [构造函数](#构造函数) | [析构](#析构)     | [对象模型](#对象模型) | [多态](#多态) | [模板](#模板) |

---

## 🎯 目录索引

### 🧐 基础

- [字符与数值](#基础)
- [作用域与生命周期](#基础)
- [类与结构体](#基础)
- [运算符特性](#基础)
- [值类别](#基础)

### 💡 关键字

- [类型转换](#关键字)
- [迭代器操作](#关键字)
- [函数修饰符](#关键字)
- [常量与类型推导](#关键字)

### 🧠 内存管理

- [内存四区模型](#内存四区)
- [new/delete 操作](#内存四区)
- [内存对齐](#内存四区)

### ➡️ 指针与引用

- [栈帧与函数调用](#指针)
- [指针操作](#指针)
- [常量指针 vs 指针常量](#指针)
- [右值引用](#引用)
- [完美转发](#引用)

### 🏗️ 面向对象

- [初始化](#初始化)
- [构造函数重载](#构造函数)
- [移动构造与深拷贝](#拷贝构造)
- [虚函数表机制](#多态)
- [抽象类与接口](#多态)

### 📦 STL 容器

| [vector](#vector) | [deque](#deque) | [list](#list)     | [set/map](#setmultiset) |
| ----------------- | --------------- | ----------------- | ----------------------- |
| [stack](#stack)   | [queue](#queue) | [string](#string) | [智能指针](#智能指针)   |

### ⚙️ 高级特性

- [模板元编程](#模板)
- [Lambda 表达式](#匿名函数)
- [并发与线程](#多线程)
- [时间日期库](#日期时间)

### 🛠️ 工具与技巧

- [文件读写](#文件操作)
- [STL 算法](#stl---算法)
- [函数对象与绑定](#可调用对象)
- [POD 类型优化](#pod类型)
- [编程惯用法](#惯用法)
- [分文件编写](#分文件编写)

---

<div align="center">
  <a href="#基础" style="font-size: 1.2em; margin: 0 10px">🚀 开始阅读</a> |
  <a href="#stl---容器" style="font-size: 1.2em; margin: 0 10px">🔍 STL 专题</a> |
  <a href="#多线程" style="font-size: 1.2em; margin: 0 10px">⚡ 并发编程</a>
</div>




## 基础

- `'0'`  48
- %size：[0,size-1]
- true本质是1，false是0
- `?:`  具有短路求值特性
- `\t`  %8为0位置输出切入点
- break：搭配 `if` 退出循环；搭配 `switch` 退出开关
- C++中main函数和类并列
- void也可以主动return，规避后续代码
- 数据的清除只要将记录的数量置0，做逻辑清空即可
- 在数组索引中不是具体变量就是地址
- 数组在函数中传参用数组名会比指针更为直观
- 数组在内存中是连续存放的，相邻差一个sizeof
- 优先队列的谓词和通常逻辑相反
- 哈希集合、映射 使用须包含各自独立的头文件
- `13!` ,`10^10` ,`cout` 爆int；务必对 **结果范围** 敏感
- 哑结点是链表中常用的优化技巧
- 尾后指针 `&arr[size]` 是惯用技巧
- 传输出引用，而非依赖成员属性
- C++声明结构体变量时struct关键字可省略
- 类与结构体的区别在于成员的默认访问权限，class私有，struct公共
- 聚合类：**全公无构的平凡**类，支持列表初始化
- 成员变量初始化先于构造函数体执行
- 构造析构自动调用
- 联合体大小取最大成员，含非平凡类型则默认构造隐式删除
- 静态局部变量初始化线程安全

| 运算符（优先级）                                            |      |
| :---------------------------------------------------------- | :--: |
| `::`  `a++` `a--` `a()` `a[]` `.` `->`                      |  →   |
| `++a` `--a` `+a` `-a` `!` `(type)` `*a` `&a` `new` `delete` |  ←   |
| `*` `/` `%` `+` `-`                                         |  →   |
| `<<` `>>`                                                   |  →   |
| `<` `<=` `>` `>=` `==` `!=`                                 |  →   |
| `&` `^` `\|` `&&` `\|\|`                                    |  →   |
| `a?b:c` `=` `+=` `-=` `*=` `/=` `%=`                        |  ←   |
| `,`                                                         |  →   |

|  值&nbsp;类&nbsp;别  | 表达式                                                       |
| :---: | :----------------------------------------------------------- |
| **左值** | 变量/函数名、字符串字面量、赋值表达式、前置自增、解引用、数组下标、对象数据成员、左值引用返回/转型 |
| **右值** | **纯右**：`T()`、非引用函数返回、非字符串字面量、后置自增、算术/逻辑/比较表达式、取地址、成员函数调用、非引用转型<br>**亡值**：右值引用 |



## 关键字

|     static_cast      | 定义明确的标准转换             |
| :------------------: | :----------------------------- |
|   **dynamic_cast**   | **多态类型安全的下行转换**     |
|    **const_cast**    | **cv限定转换**                 |
| **reinterpret_cast** | **底层实现依赖的位模式重解释** |

`(void)a`				                                                           // 显式弃值，消除未使用警告

`NULL` , `nullptr`		  	                                                   // NULL：宏常量0

`uint64_t`, `uint32_t`, `uint16_t`, `uint8_t`                             // 无符号类型

`memset`, `memcpy`, `memcmp`                                                            // C 库内存操作函数

`using` , `typedef`			                                                   // using与typedef类似，但using能定义模板别名

`using namespace` 						                        // 后续代码使用指定命名空间

`::std`                                                                                              // 全局命名空间查找 std

`while(expr)`					                                         // 表达式是合法条件

`for(int i=0;i<len;++i)`			   	                        // 条件<长度，使用更高效的前置自增

`for(const auto& it : v)`				                         // 基于范围的for循环，先确定迭代范围，高效遍历

`void func() final{}`			                                         // final修饰虚函数不可被重写，修饰类不可被继承

`void func() override{}`		                                           // override显式表明重写

`void func() noexcept{}`		                                           // noexcept修饰的函数不会抛出异常

`void func() const{}`		                                                 // 常函数，const修饰this指针	常对象只能调用常函数

`mutable int m_a;`			                                               // mutable修饰的属性常函数里仍可写

`const` , `constexpr`			                                              // const ⇔ constexpr，“只读”const，“常量”constexpr 

`R"(D:\Steam\kanon.exe)"`		                                        // 原始字面量，表示字符串的实际含义

`static_assert(sizeof(long) == 8, "not 64!")`	      // 静态断言，编译期检查，false警告

`enum class Colors :char { Red, Green, Blue }`             // 定义强类型枚举，指定底层类型为char



## 内存四区

`new`				   // 堆区开辟内存，返回对应类型指针（*`operator new()`分配内存→构造初始化*）

`delete`		    	// 接地址释放堆区单个内存，数组加 []（*析构销毁→`operator delete()`释放内存*）

|  分区  |                      内容                      | 阶段 | 存储期 |
| :----: | :--------------------------------------------: | :--: | :----: |
| 代码区 |                      函数                      | 编译 |   OS   |
| 全局区 | 全局变量，静态变量，字符串常量，全局常量，虚表 | 编译 |   OS   |
|  栈区  |               局部变量，局部常量               | 运行 | 编译器 |
|  堆区  |                                                | 运行 |  手动  |

> **注意**：不要返回本函数的局部变量地址

---

**内存对齐**:

类型的内存布局属性，规定对象地址必为其对齐值的整数倍

- *align*: 内置类型为自身大小，复合类型为成员最大对齐值
- *size*: 成员偏移与整体大小为相应对齐值的整数倍，不足则填充

```c++
// case
struct stu2 { char x; int y; double z; char v[6]; };
struct stu1 { union { int a1; char a2[5]; }a; struct stu2 b; int c; };
```

|  ◾   |  ◾   |  ◾   |  ◾   |  ◽   |  ◽   |  ◽   |  ◽   |  ◾   |  ◾   |  ◾   |  ◾   |  ◽   |  ◽   |  ◽   |  ◽   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  🟡   |  🟡   |  🟡   |  🟡   |  🟡   |  -   |  -   |  -   |  🟠   |  -   |  -   |  -   |  🟢   |  🟢   |  🟢   |  🟢   |
|  🔵   |  🔵   |  🔵   |  🔵   |  🔵   |  🔵   |  🔵   |  🔵   |  🟤   |  🟤   |  🟤   |  🟤   |  🟤   |  🟤   |  -   |  -   |
|  🔴   |  🔴   |  🔴   |  🔴   |  -   |  -   |  -   |  -   |      |      |      |      |      |      |      |      |

> 虚表指针参与内存对齐



## 指针

**栈帧**：函数调用的上下文存储单元，包含参数/返回地址/局部变量

- *溢出/爆栈*：通常是 **递归层次太深**，**无垃圾回收**

**指针即地址**，其操作与栈帧、堆区等内存结构密切相关

- *解引用，&取址
- `&` 升级，`*/[]` 降级
- 指针的作用是间址
- 指针变量所占大小为一个字长
- int * 不是指针标志，是一种数据类型
- 空指针和野指针都不是主动申请的空间，忌访问
- 指针作为函数形参可以节省内存，const 指针可以防止误写操作

**区分**：

- 常量指针：指向常量的指针

- 指针常量：常指针




`p + n ⇔ p + n * sizeof(T)`			// T* p，即 p 的 offset 加 n*T



## 引用

`int& a = b`				// 引用变量是一个别名，本质是指针常量实现

- 可作为函数形参和返回值，且返回值是左值
- const左值引用可绑定右值

------

**右值引用**：

`int&& a = 520`			// 延长右值生命周期

**转发引用**：

`T&&` 					    // 右值 得到 `int&&`，否则 `int&`

- `move(p)`				     // 转实参为亡值，转移对象的所有权
- `forward<T>(t)`			// 完美转发，`<int&>`转左值，否则转右值

**引用包装**：

`ref(t)` 					// 包装引用为可复制对象，隐式转换为 T&



## 形参默认值

`void func(int a,int b=1)`			// 可以有默认值，但必须在签名结尾

`template<class NameType, class AgeType = int>`		// 类模板在模板参数列表可以有默认参数

- 声明和实现不能同时有默认参数
- 重载碰到默认参数容易产生歧义

`void func(int a,int)`			// 占位参数,用纯数据类型表示



## 初始化

**初始化器**&nbsp;&nbsp;&nbsp; `=`,  `()`,  `{}`

**初始化列表**：

`T(V1 a,V2 b) :m_a(a), m_b(b) {}`		// 按声明顺序，显式初始化类成员

`T t{arg1, arg2...}`					   // 列表初始化，作 **实参**、**return** 简写 `{...}`

`initializer_list<T>` , `size()` , `begin()` , `end()`	// 动态初始化列表容器

---

**效果**：

- 内置类型默认不初始化
- 空参对 **内置** 或 **隐式构造** 类型先清零
- 类类型纯右值 复制消除

**机制**：

*临时量实质化*：通过初始化临时对象，将纯右值转换为同类型亡值

*复制消除*：无拷贝与移动构造，零复制传值

> 标准库默认值初始化



## 构造函数

**调用**：

`Person p`                                                   				     		 		// 默认构造，零原则自动生成

- **注意**：不要加()，编译器会认为是函数的声明；堆区推荐 `new Person()`

`Person p(参)` 														// 有参构造

 ⇔ `Person p = Person(参)`  ⇔ `Person p = 参`

------

**委托构造**：

`Person(string name, int age):Person(age) {}`		// 调用重载的其他构造函数

**继承构造**：

`using Base::Base`			// 子类声明使用父类构造函数

**显式指定**：

`Person() = default`				// 显式指定编译器生成默认构造，只能修饰六大函数

`void func(char c) = delete`		// 显式删除函数，可有效禁用重载时的隐式类型转换

**匿名对象**：

`Person([参])`					// 省略对象名，当前行执行结束马上析构

- **注意**：不能用拷贝构造初始化匿名对象，编译器认为`Person(p) === Person p`



## 拷贝构造

`Person (const Person &p)`



- 浅拷贝：简单的赋值拷贝操作                          	// 默认拷贝操作

- 深拷贝：在堆区重新申请空间，进行拷贝                // 重写拷贝操作


（以下情况务必重写拷贝构造）

1. 属性有在堆区开辟的

2. 涉及对象的值传递或值返回时


- **彻底解决浅拷贝**：重写拷贝构造，重载`operator=`



**移动构造**：

`Person(Person&& p) : m_P(p.m_P) {p.m_P = nullptr;}`		// 赋右值时会优先调用来转移属性所有权

- **注意**：自定义拷贝或析构会抑制隐式移动操作



## 析构

`~Person()`			// 在构造函数前加~号，不可重载

**释放堆区开辟的内存**：

```c++
if(m_Ptr != NULL)
{
	delete m_Ptr;
	m_Ptr = NULL;
}
```

- 销毁后指针本身仍在栈中，由编译器控制释放
- [纯]虚析构能解决父类指针释放子类对象时调用完整的析构链，都需要有具体函数实现



## 对象模型

**数据**：非静态成员变量

**布局**：

```c++
class A { virtual void a(); int x_0x8; };							// A_vtable* vtable_0x0;
class B { virtual void b(); int y_0x8; };							// B_vtable* vtable_0x0;
class C : public A, public B { virtual void c(); int z_0x20; };		// A inherit__0x0; B inherit__0x10;
```

- 空类 1B，空基类优化
- 访问控制对内存布局无影响

**开发人员命令提示工具**：

`cl /d1 reportSingleClassLayout类名 文件名`		// 跳转到文件所在路径，可以用命令查看类的对象模型



## this指针

`C* const`

**本质**：指针常量


- 非静态成员函数签名隐含 `func(C* const this, ...)`，this指向其所属对象

`*this` 			// 返回对象本身，链式编程思想

`this->`			// 成员属性前默认添加



## 函数指针

`int (*fp)(int a)`			// 定义函数指针fp

`typedef int (*F)(int a)`	  // typedef简化定义，F是别名

`using F = int (*)(int a)`	// using简化定义，直观清晰

- 函数名就是地址			// 成员函数需要&
- 可以作为函数形参

**类成员函数指针**：

- 声明必须加作用域
- 只能指向非静态成员函数
- 使用 `.*` 或 `->*` 调用指向的函数



## 友元

**意义**：作友元可以访问类内私有成员

`friend void func();`					   // 全局函数作友元，在类内写声明并用friend关键字修饰

`friend void GoodGay::func();`			// 成员函数作友元还需要加作用域

`template<typename T> class Person{ friend T; }`		// 为类模板声明友元类



## 运算符重载

`operator_`				// _为重载的运算符

**意义**：能简化自定义数据类型间的运算，支持动态绑定

```c++
/*左移运算符重载通常声明为友元全局函数，因为对象需要作为右操作数*/
ostream & operator<<(ostream &cout,Person &p)			// 左移运算符重载
```

- `int& operator++()`			    // 前置递增
- `int operator++(int)`			// 后置递增

`operator=`			// 配合深拷贝

`operator[]`			// 对象[索引]直接访问某成员指针维护的数组元素

`operator()`			// 仿函数，对象()调用



## 自动类型推导

`auto x = 3.14`			// x为double

`auto it = v.begin()`		// it为迭代器

- **注意**：只有变量为 **指针** 或 **引用** 时推导结果才保留const、volatile关键字



`decltype(b) a = 10`				// a：int

`decltype((Person.m_Age)) a = 0`	// a：int&

- **注意**：当表达式为 **左值** 或 **()** 时推导结果是一个引用，且保留const、volatile关键字



`auto func(T& t) -> decltype(test(t))`		// 返回类型后置，auto会追踪decltype推导的类型



## 匿名函数

`auto f = [](int a){ return a+10 }`			// 匿名函数定义

| 捕获列表 | 作用              |
| -------- | ----------------- |
| []       | 不捕捉            |
| [&]      | 按引用捕捉        |
| [=]      | 按值捕捉          |
| [=, &f]  | 按值捕捉，f按引用 |

- **注意**：Lambda表达式通常被看作仿函数，[]时可转换成函数指针



## 继承

`class Derived:public Base`			// class 派生类:继承方式 基类

- 继承方式：决定派生类继承后的最大访问权限
- 基类所有非静态成员属性都会被继承
- 基类私有成员继承后被编译器隐藏，访问不到

> C++允许多继承，但实际开发不建议用

---

**隐藏**：

*当派生类拥有与基类同名的成员且非重写：*

- 编译器会隐藏基类中所有同名成员函数

`s.Base::xxx`		// 访问基类成员需要加作用域

```c++
Son::Base::m_Age				// 静态成员通过派生类类名访问基类成员
/*	第一个:: 类名方式访问
	第二个:: 基类作用域下	*/
```

> C++默认采用静态绑定，编译期确定

---

**菱形继承**：（💎）

派生类继承两份相同数据，导致资源浪费，中间类使用虚继承

```c++
==================================================
   Itanium ABI: Diamond Virtual Inheritance
==================================================

[ D3 ]
├─ D1 (0x00) — main
│   ├─ vptr1 (0x00) ─────────>  [ VTable for D1-in-D3 ]
│   │                              ├─ [-2]: vbase_offset = 0x28
│   │                              └─ [-1]: typeinfo for D3
│   └─ d1 (0x08)
├─ D2 (0x10) — sec
│   ├─ vptr2 (0x10) ─────────>  [ VTable for D2-in-D3 ]
│   │                              ├─ [-2]: vbase_offset = 0x18
│   │                              └─ [-1]: typeinfo for D3
│   └─ d2 (0x18)
├─ d3 (0x20)
└─ Base (0x28) — virtual (unique)
    ├─ vptr_base (0x28) ─────>  [ VTable for Base-in-D3 ]
    └─ b (0x30)

sizeof(D3) = 0x38 (56 bytes)
```

- 虚基类最先构造
- 虚基类非主基类
- 无普通基类则自身为主基类



## 多态

**分类**：

- 静态：重载、模板
- 动态：重写

**实现**：

- **虚函数表**：编译时生成的静态数组，存储虚函数指针，按声明顺序排列
- **虚表指针**：位于对象（基类子对象）起始位置，初始化指向当前类虚表

**规则**：

- 基派生类虚表物理独立
- 虚析构成对出现
- 派生类重写虚函数覆盖基类对应位置
- 派生类新增虚函数追加至虚表末尾
- 多继承多vptr，新增函数追加至主虚表
- 非主基类调用虚函数时通过thunk调整this指针

```c++
==================================================
      C++ Multiple Inheritance VTable Layout
==================================================

[Derived Object]
├─ vptr1 (0x00) → Base1 vtable (main)
│   ├─ vtable1[0]: Derived::f1()        // override
│   ├─ vtable1[1]: __cxa_pure_virtual   // PURE VIRTUAL STUB
│   ├─ vtable1[2]: ~Derived() [complete]
│   ├─ vtable1[3]: ~Derived() [deleting]
│   └─ vtable1[4]: Derived::dv_func()   // new
│
└─ vptr2 (0x10) → Base2 vtable
    ├─ vtable2[0]: thunk to Derived::f2() // override
    ├─ vtable2[1]: thunk to ~Derived() [complete]
    └─ vtable2[2]: thunk to ~Derived() [deleting]
```

**纯虚**：

`virtual void func()=0;`			// 声明，虚表中以专用占位指针表示，调用触发运行时错误

- 抽象类：含纯虚函数的类，无法实例化对象，一般不写构造函数

> 开发提倡开闭原则：对扩展进行开放，对修改进行关闭



## 文件操作

`#include <fstream>`

**操作文件三大类**：

- `ofstream` ：写操作

- `ifstream` ：读操作

- `fstream` ：读写操作

**文件打开方式**：（多选：| ）

- `ios::in`———读文件

- `ios::out`———写文件

- `ios::app`———追加文件

- `ios::ate`———初始位置文末

- `ios::trunc`———[存在则删除]创建

- `ios::binary`———二进制方式

**步骤**：

```C++
ofstream ofs;
ofs.open(“文件路径”,打开方式);
ofs << “写入的数据”;
ofs.close();
```

`while(ifs >> buf)`		// 读入字符数组

```C++
ifs >> ch				// 判断空文件
ifs.eof()
```



## 模板

基于类型参数化，实现零开销安全复用

**语法**：

```c++
template <typename, auto, template <typename> typename>         // 主模板
/* 声明 */;
```

**规则**：

&nbsp;&nbsp;*特化*：为特定实参定制模板实现&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*实例化*：编译器从模板生成具体实体

```cpp
template <>                                                     // 全特化
class HSTVector<X, 5, std::vector>;
```

- 嵌套模板特化，依赖外层实例化
- 实例化时，按偏序选择最优匹配：全特化 > 偏特化 > 主模板
- 类模板仅实例化被使用的成员，相同实例链接时合并
- 函数模板支持重载，优先普通函数，除非模板匹配更佳
- `f<>()` 强制调用函数模板

**工具**：

```c++
// 类型萃取
template <typename> class TypeTraits;
template <template <typename, typename> typename C, typename U, typename V>
class TypeTraits<C<U, V>> { public: using First = U; using Second = V; using Full = C<U, V>; };

// 别名模板
template <typename C> using First_t = typename TypeTraits<C>::First;

// 类模板占位符
template<std::array> void f(); f<std::array<double, 8>{}>();
```



## STL - 容器

- 容器、算法、迭代器、仿函数、适配器、空间配置器

- 迭代器：aka 指针

`for(vector<Person>::iterator it=v.begin(); it!=v.end(); ++it)`			// 迭代器遍历容器

`#include <algorithm>`		// 所有不支持随机访问迭代器的容器，不可以用标准算法



tips：尝试 `it + 1`，观察编译是否通过即可判断迭代器类别

### string

封装的 const char*，物理上以ASCII 0结尾

### vector

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/vector容器.jpg" style="zoom:130%;" />

​	`resize(int num);` 		    // 调整个数，保证有效性，必要时重分配

​	`push_back(ele);`  		     // *emplace_back()*，原地构造，性能更高

​	`pop_back();`   

​	`swap(vec);` 				// 互换容器，达到实用的收缩内存效果

​	`reserve(int len);`		   // 扩容

```c++
/*利用动态扩展机制，统计开辟次数*/
int num = 0;
int* p = NULL;
for (int i = 0; i < 100000; ++i) {
    v.push_back(i);
    if (p != &v[0]) {
        p = &v[0];
        num++;
    }
}
```

### deque

双端数组，头部增删速度比vector快，但元素访问速度要慢

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/deque容器.jpg" alt="deque容器" style="zoom:130%;" />

![deque内部工作原理](https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/中控器.jpg)

```c++
void printDeque(const deque<int>& d) 
{			/*限制容器为只读状态时，相应的迭代器也要用const_iterator*/
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}
```

​	`push_front(elem);`

​	`pop_front();`

### stack

栈中只有顶端的元素才可以被外界使用，因此栈不允许有遍历行为（遍历是非质变算法）

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/stack容器.jpg"  />

​	`push(elem);`

​	`pop();` 

​	`top();`

### queue

队列中只有队头和队尾才可以被外界使用，因此队列不允许有遍历行为

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/queue容器.jpg" alt="queue容器"  />

​	`push(elem);`

​	`pop();` 

​	`back();` 

​	`front();` 

`priority_queue<T, fd_Container, cmp_type> q(cmp)`		 优先队列，默认大堆

### list

双向循环链表，list中的迭代器只支持前移和后移，属于**双向迭代器**

<img src="https://github.com/voxhugh/Appendix/blob/main/Cpp_IMGs/list容器.jpg" alt="list容器"  />

插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的

​	`remove(elem);`

​	`reverse();`

​	`sort();` 		// 默认升序，是list容器的成员算法

### set/multiset

属于**关联式容器**，底层结构是用**二叉树**实现，所有元素都会在插入时自动被排序



​	`insert(elem);`

​	`erase(pos);`

​	`find(key);`                  // 查找key是否存在：存在返回该键的元素的迭代器；不存在，返回`set.end();`

​	`count(key);`



***set和multiset区别**：*

```c++
pair<iterator, bool> insert(value_type&& _Val);			// set插入数据时返回bool
iterator insert(value_type&& _Val);						// multiset可以插入重复数据
```

------

***pair对组**：*

​	成对出现的数据，可以返回两个数据	通过p.first、p.second访问

​	`pair<type, type> p ( value1, value2 );`
​	`pair<type, type> p = make_pair( value1, value2 );`

***tuple元组**：*   固定大小的异质值的汇集，是 [std::pair](https://zh.cppreference.com/w/cpp/utility/pair) 的泛化

**结构化绑定**： 解包聚合对象为独立变量

```c++
auto [x, y, z] = std::tuple<int, double, char>(1, 2.5, 'Y')			// 默认值拷贝，可指定auto&
std::tie(x, y, z) = std::make_tuple(3, 5.2, 'C')					// tie 解包聚合对象至已有变量
```

------

***容器排序**：*

​	用仿函数可以指定set容器的排序规则

```c++
class Compare
{
public:
	bool operator()(const Person& p1, const Person &p2)
	{
		return p1.m_Age > p2.m_Age;			// 按照年龄降序
	}
};
void test01(){	set<Person,Compare> s;}
```

- **注意**：自定义数据类型必须指定规则

### map/ multimap

属于**关联式容器**，底层结构是用**二叉树**实现，所有元素都会在插入时自动被排序

- 所有元素都是`pair<key,value>`，可以根据key值快速找到value值

​	`operator[](key);`           // 在key不存在时插入

### unordered_set/map

哈希表， 平均时间复杂度O(1)

## STL - 函数对象

重载**函数调用操作符**的类对象，也叫**仿函数**

- 本质是一个**类实例**

* 使用时像普通函数：`p()`
* 可以有自己的状态：`int m_count`
* 可以作为参数传递

### 谓词

返回bool类型的可调用对象

```c++
class Predicate
{
public:				/*一个参数叫一元谓词，两个叫二元谓词*/
    bool operator()(int num1,int num2){	return num1>num2}
}
```

### 内建函数对象

​	`template<class T> bool equal_to<T>`                    // 等于

​	`template<class T> bool less<T>` 			 // 小于

## STL - 算法

`#include <algorithm>`			   // 比较、 交换、查找、遍历操作、复制、修改等等

`#include <functional>` 			// 内建函数对象

`#include <numeric>`				// 序列上的简单数学运算函数



### 遍历



`for_each(iterator beg, iterator end, _func);`

-  遍历容器



`transform(iterator beg1, iterator end1, iterator beg2, _func);`

- 容器间搬运，必须提前开辟空间

### 查找



`find(iterator beg, iterator end, value);`

-  按值查找元素	【重载operator==】



`adjacent_find(iterator beg, iterator end);`

- 查找相邻重复元素



`bool binary_search(iterator beg, iterator end, value);`

- 查找指定元素是否存在，只适用**有序序列**



`count(iterator beg, iterator end, value);`

- 统计元素个数	【重载operator==】



`lower_bound(iterator beg, iterator end, value, _Pred);`

- 二分查找指定元素起始位置



`max_element(iterator beg, iterator end, _Pred)`

- 查找最大元素位置

### 排序



`sort(iterator beg, iterator end, _Pred);`

-  正序排列



`random_shuffle(iterator beg, iterator end);`

- 洗牌   指定范围内的元素随机调整次序	使用时记得加随机数种子



`merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

* 两个容器元素合并，并存储到另一容器中	所有容器必须**有序**



`reverse(iterator beg, iterator end);`

- 反转容器内的元素

### 拷贝和替换



`copy(iterator beg, iterator end, iterator dest);`

- 容器内指定范围的元素拷贝到另一容器中



`replace(iterator beg, iterator end, oldvalue, newvalue);`

- 将区间内旧元素 替换成 新元素	全部替换



`swap(container c1, container c2);`

- 互换两个容器的元素

### 算术生成算法



`accumulate(iterator beg, iterator end, value);`

- 计算区间内 容器元素累计总和



`fill(iterator beg, iterator end, value);`

- 向容器中填充指定的元素

### 集合算法

- **注意**：初始集合必须是**有序序列**，返回值是目标容器集合最后一个元素的迭代器



`set_intersection(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator dest);`

- 求两个容器的交集	目标容器开辟空间需要从**两个容器中取小值**



`set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

- 求两个容器的并集	目标容器开辟空间需要**两个容器相加**



`set_difference(iterator beg1,iterator end1,iterator beg2,iterator end2, iterator dest);`

- 求两个容器的差集	目标容器开辟空间需要从**两个容器取较大值**



## 可调用对象

**函数指针**，**仿函数**，可转换为函数指针的类对象，类成员（函数）指针



**包装器**：

`function<int(int, double)> f = add`			// 包装成一个对象，可直接调用

**绑定器**：

`auto f = bind(func, arg1, ...)`			// 绑定函参并返回一个仿函数，实现降元

`bind(func, 2, placeholders::_1)(10)`			// 为调用时第一个实参占位



```c++
// 搭配绑定器包装 类成员函数
function<void(int, int)> f = bind(&Person::add, &p, placeholders::_1, placeholders::_2)
// 搭配绑定器包装 类成员变量
function<int&(void)> f = bind(&Person::m_Age, &p)
```

- **注意**：function不能包装类成员（函数）指针



## 智能指针

维护指针的资源，自动销毁堆区对象



`get()`			    // 获取原指针地址

`reset()`			// reset解除管理，有参可初始化智能指针

`use_count()`		// 获取管理当前对象的引用计数

`make_shared<int>(10)`			// make_shared创建内存对象，可直接初始化智能指针

- **注意**：`.` 调用智能指针api，`->`调用间址内存api；不能用同一原指针初始化多个共享指针

**共享指针**

`shared_ptr<int> sp(new int(10))`			// 初始化，多个智能指针可管理同一块内存

**独占指针**

`unique_ptr<int> up(new int(10))`			// 初始化独占型，仅允许构造或move

**弱引用指针**

`weak_ptr<int> wp(sp)`			// 监视共享指针管理的资源

`weak_ptr::expired()`			  // 判断观测资源是否已经被释放

`weak_ptr::lock()`				// 获取监视的共享指针对象

```c++
// 1.解决 返回管理this的共享指针
struct Person : public enable_shared_from_this<Person>
{
    shared_ptr<Person> func()
    {
        return shared_from_this();
    }
};
// 2.解决 循环引用
shared_ptr<A> a(new A);
shared_ptr<B> b(new B);
a->bp = b;		// A::shared_ptr<B> bp
b->ap = a;		// B::shared_ptr<A> ap
/*此时引用计数无法减为零，会造成内存泄漏，需要将ap或bp改为weak_ptr，因为weak_ptr不占强引用计数*/
```



## POD类型

汇编层原生连续内存块



**“平凡”**

- 无自定义特殊成员函数
- 无虚函数、虚继承

**“标准布局”**

- 同段非静态成员权限统一
- 成员不跨层级，首个成员非基类
- 无虚函数、虚继承
- 所有成员递归满足以上

```cpp
bool pod = is_trivial<Person>::value && is_standard_layout<Person>::value;
```



## 惯用法

**CRTP**&nbsp;&nbsp;&nbsp;奇特重现模板模式

派生类以自身实例化基类模板，实现编译期多态。

```cpp
template<typename D> struct B {
    void name() { static_cast<D*>(this)->impl(); }  // 推导 this 可以简化 CRTP
protected:
    B() = default;
};
struct D1 : B<D1> { void impl() { puts("D1"); } };
struct D2 : B<D2> { void impl() { puts("D2"); } };
```



**PImpl**&nbsp;&nbsp;&nbsp;指向实现的指针

用不透明指针构筑编译防火墙。

```cpp
// widget.h
class widget {
    struct impl;
    std::unique_ptr<impl> pImpl;
public:
    widget();
    ~widget();
};

// widget.cpp
struct widget::impl {};
widget::widget() : pImpl(std::make_unique<impl>()) {}
widget::~widget() = default;
```



**RAII**&nbsp;&nbsp;&nbsp;资源获取即初始化

将资源生命周期严格绑定于对象生命周期，通过构造时获取（可能抛异常）、析构时无条件释放（绝不抛异常）来实现自动化及异常安全的资源管理。



**<span style="color: red">零原则</span>**

类要么为管理资源而完整定义特殊成员函数，要么完全不定义。



**零开销原则**

未使用的抽象不产生运行时开销，且其实现效率与最优手写代码等同。



## 分文件编写

头文件声明，源文件实现



**头文件**：预处理的源码文本复用，`#pragma once` 防止重复包含

**前向声明**：无类体的不完全类型声明，适用于指针、引用、函数声明、模板实参

静态成员变量类内声明，类外初始化

- **注意**：源文件需指明作用域 `Person::`, `Person<int>::`

---

**类模板**：

- **Header-Only**: 约定俗成 .hpp
- **Separate**: 显式实例化 `template class HSTVector<int>`



## 日期时间

```c++
#include <chrono>
#include <iostream>
using namespace std;
using namespace std::chrono;
//  时间间隔
void interval()
{
    seconds s(1);							//  一秒
    minutes min(1);							//  一分钟
	hours h(1);								//  一小时
	duration<double, ratio<9, 7>> f1(3);	//  T：9/7s  f：3
	duration<double, ratio<6, 5>> f2(1);	//  T：6/5s  f：1
	//  f1 和 f2 统一时钟周期：	9,6取最大公约数	7,5取最小公倍数
	duration<double, ratio<3, 35>> hz = f1 - f2;
    hz.count();								// 获取f
}
//  时钟
void clocks()
{
	//  新纪元1970.1.1时间 + 1天
	duration<int, ratio<60*60*24>> day(1);
	system_clock::time_point ppt(day);
	//  系统当前时间
	system_clock::time_point today = system_clock::now();
	//  转换为time_t时间类型
	time_t tm = system_clock::to_time_t(today+day);
	cout << "明天的日期是:	" << ctime(&tm);

	//  获取开始时间点
	steady_clock::time_point start = steady_clock::now();
	//  执行业务流程...
	//  获取结束时间点
	steady_clock::time_point last = steady_clock::now();
	//  计算差值
	auto dt = last - start;
	cout << "总共耗时: " << dt.count() << "纳秒" << endl;
}
//  转换
void cast()
{
    using Clock = chrono::steady_clock;
	using Ms = chrono::milliseconds;
	using Sec = chrono::seconds;
	template<class Duration>
	using TimePoint = chrono::time_point<Clock, Duration>;
    //  整数时长：时钟周期纳秒转毫秒，需要 duration_cast 显式转换
	auto int_ms = duration_cast<Ms>(last - start);
    //  毫秒转秒，损失精度使用 time_point_cast 显示转换
	TimePoint<Sec> time_point_sec(Sec(6));
    TimePoint<Ms> time_point_ms(Ms(6789));
	time_point_sec = time_point_cast<Sec>(time_point_ms); //  6000 ms
}
```



## 多线程

`#include <thread>`				 // 线程

`#include <mutex>`				   // 互斥量

`#include <atomic>`				 // 原子变量

`#include <condition_variable>`	// 条件变量

`#include <semaphore>`			   // 信号量

`#include <future>`				 // 未来



**线程**

`thread t(func,arg1,arg2...)`			   // 创建线程，执行任务

`this_thread::get_id()`					// 获取当前线程ID

`this_thread::sleep_for()`				 // 休眠指定时间，参数是一个时间段

`this_thread::sleep_until()`			    // 休眠至指定时刻，参数是一个时间点

`this_thread::yield()`					// 主动放弃已抢到的CPU资源一次

`get_id()`								 // 获取子线程ID

`join()`								     // 阻塞当前线程并等待子线程执行完毕

`detach()`								 // 分离子线程，当前线程退出会一并销毁所有子线程

`static hardware_concurrency()`		     // 获取计算机的CPU核心数

`call_once(once_flag,func,args)`		   // 函数只被调用一次，once_flag对象须多线程可见

**互斥量**

`mutex mx`								      // 声明互斥量

`lock()`									  // 对临界区加锁，加锁失败被阻塞

`unlock()`								      // 解锁

`try_lock()`								  // 加锁，失败返回false

`lock(mtx1,mtx2,...)`						// 同时加锁多个互斥量

`lock_guard<mutex> lock(mx)`				 // 自动上锁，析构解锁

`unique_lock<mutex> locker(mx)`			   // 自动上锁，灵活解锁

`recursive_mutex`							// 声明递归互斥量（允许一个线程对同一互斥量获取多次）

`timed_mutex`								// 声明超时互斥量（超过指定时间解除阻塞）

`try_lock_for()` , `try_lock_until()`		    // 加锁，失败阻塞指定时间后返回false，t_mtx下的api

**原子变量**

`atomic<int> a = 0`						// 声明原子变量，只能封装整形数据

`atomic_int a = 0`						    // 使用模板特化声明

**条件变量**

`condition_variable cv`				      // 声明条件变量

`wait(locker)`							 // 阻塞线程

`wait_for()` , `wait_until()`				// 阻塞线程指定时间

`notify_one()` , `notify_all()`			    // 唤醒一个或多个被阻塞的线程

`condition_variable_any`				    // 声明通用条件变量

**信号量**

`counting_semaphore<6> csem(0)` 			// 创建信号量 （LeastMaxValue为6）

`binary__semaphore bsem(0)` 				// LeastMaxValue为1时的模板特化

`acquire()` 								 // 阻塞等待

`release()` 								 // 唤醒

**未来**

`future<int> f`						  // 声明一个未来

`get()`								  // 获取数据，阻塞至子线程数据就绪

`wait()`								// 阻塞当前线程

------

`get_future()`						// 得到 *promise* 中管理的future对象

`set_value()`						 // 存储传出数据，立即让状态就绪

------

`get_future()`						// 得到 *packaged_task* 中管理的future对象

------

`async(func,arg1,arg2...)`			// 创建线程执行任务并返回一个future对象

**总结**：

- *LeastMaxValue*：允许同时访问临界资源的线程的最大数目

- 一个互斥量维护一个临界资源，线程传入类成员函数时用法同绑定器
- 信号量的release能使内部计数器增加，bsem++，csem+n（n<=LeastMaxValue）
- 主线程promise对象作为子线程任务函数形参，packaged_task对象包装并充当任务函数，都须传引用ref(obj)
- 获取数据：*packaged_task* 和 *async* 是任务结束return，*promise* 可以随时set_value
- async可以指定任务执行策略
  - *launch::async* 创建线程并执行任务
  - *launch::deferred* 延迟调用执行任务



