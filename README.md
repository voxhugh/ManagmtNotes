# C++

- 记录学习过程的核心事项总结

[TOC]



## 基础

`while(condition)`							//表达式是执行条件

`for(int i=0;i<len;i++)`			   		//遍历语句条件<长度

**总结：**

- 在数组索引中不是具体变量就是地址
- 数组在函数中传参用数组名会比指针更为直观
- %描述的是一种相对位置，0作左操作数无意义
- 三目运算返回的是变量
- true本质是1，false是0
- \t：%8为0位置输出切入点
- 数组在内存中是连续存放的，相邻差一个sizeof
- NULL是宏常量，代表0号内存空间，[0,255]为系统占用
- C++声明结构体变量时struct关键字可省略
- 数据的清除只要将记录的数量置0，做逻辑清空即可
- C++中main函数和类并列
- 类与结构体的区别在于成员的默认访问权限，class私有，struct公共
- void也可以主动return;规避后续代码



## 分文件编写

`#pragma once`					//防止头文件重复包含

`#include <algorithm>`			//标准算法头文件

- 头文件声明，源文件实现（需要指明作用域）

**作用域：**

`Person::`						//成员函数

`Person<int>::`					//类模板

**类模板：**

1. 包含.cpp源文件
2. 将声明和实现写在一个以.hpp为后缀名的文件中						//.hpp是约定俗成的，并非规范



## 指针

- 指针即地址
- *解引用，&取址
- 指针变量所占大小为一个字长
- int * 不是指针标志，是一种数据类型
- 空指针和野指针都不是主动申请的空间，忌访问
- 指针作为函数形参可以节省内存，const 指针可以防止误写操作

**区分：**

- 常量指针：常指针，常量的地址

- 指针常量：常量的值是指针

  

`p++`			//地址后移，引用内存指向下一个相邻内存空间

|  数组  |  地址(D)  |  值  | 大小(B) |
| :----: | :-------: | :--: | :-----: |
| arr[0] | 366278536 |  1   |    4    |
| arr[1] | 366278540 |  2   |    4    |



## const

- 修饰什么什么就是常量
- const和*谁在后面本质就是什么



## 内存四区

- **注意：**不要返回本函数的局部变量地址

`new`				//在堆区开辟内存，返回该数据类型的指针

`delete`			//直接接地址，释放堆区单个内存空间，释放数组还要加[]

| 代码区 |                 机器指令                 | 编译后运行前 |  生命周期  |
| :----: | :--------------------------------------: | :----------: | :--------: |
| 全局区 | 全局变量，静态变量，字符串常量，全局常量 | 编译后运行前 |   OS控制   |
|  栈区  |            局部变量，局部常量            |    运行后    | 编译器控制 |
|  堆区  |                                          |    运行后    | 程序员控制 |



## 引用

`int &nickname = name;`				//本质是指针常量实现

- 可以作为形参、返回值
- 作为函数的返回值是一个变量，可以作为左值
- 给常量引用赋常量值合法，编译器隐式在栈中分配空间初始化并把指针带回

**重载：**

const可以区分重载的版本

- `func(a);`					//调用无const

- `func(10);`					//调用有const

  

## 形参默认值

`void func(int a,int b=1)`			//可以有默认值，但必须在签名结尾

`template<class NameType, class AgeType = int>`		//类模板在模板参数列表可以有默认参数

- 声明和实现不能同时有默认参数
- 重载碰到默认参数容易产生歧义

`void func(int a,int)`			//占位参数,用纯数据类型表示



## 构造函数



**调用：**

`Person p`                                                   				     		 		//默认构造

- **注意：**不要加()，编译器会认为是函数的声明

`Person p(参)` 														//有参构造

 ⇔ `Person p = Person(参)`  ⇔ `Person p = 参`



 **匿名对象：**

`Person([参])`					//省略对象名，当前行执行结束马上析构

- **注意：**不能用拷贝构造初始化匿名对象，编译器认为`Person(p) === Person p`



## 拷贝构造

`Person (const Person &p)`					//是一种构造函数，而且必然存在



**浅拷贝：**

- 浅拷贝：简单的赋值拷贝操作                          	//默认拷贝操作

- 深拷贝：在堆区重新申请空间，进行拷贝                //重写拷贝操作

  

**情景：**

（以下情况务必重写拷贝构造）

- 属性有在堆区开辟的

- 涉及对象的值传递或值返回时

  

**彻底解决浅拷贝问题：**重写拷贝构造，重载`operator=`



## 析构

`~Person()`			//在构造函数前加~号，不可重载

**释放堆区开辟的内存：**

```c++
if(m_Ptr != NULL)
{
	delete m_Ptr;
	m_Ptr = NULL;
}
```

- 销毁后指针本身仍在栈中，由编译器控制释放
- [纯]虚析构能解决父类指针释放子类对象，都需要有具体函数实现



## 对象模型



- 只有 **非静态成员变量** ∈类的对象			//子类会继承父类所有非静态成员属性
- 静态成员变量类内声明，类外初始化
- 空对象：1B

**开发人员命令提示工具：**

`cl /d1 reportSingleClassLayout类名 文件名`		//跳转到文件所在路径，可以用命令查看类的对象模型



## this指针

**本质：**指针常量



- 隐含在每一个非静态成员函数内，指向 被调用的成员函数 所属的对象

`*this` 			//返回对象本身，链式编程思想

`this->`			//成员属性前默认添加



## 常函数

`void func() const{}`		//const修饰this指针

`mutable int m_a;`			//mutable修饰的属性常函数里仍可写

- 常对象只能调用常函数



## 友元

**意义：**作友元可以访问类内私有成员

`friend void func();`					//全局函数作友元，在类内写声明并用friend关键字修饰

`friend void GoodGay::func();`			//成员函数作友元还需要加作用域



## 运算符重载

`operator_`				//_为重载的运算符

**意义：**能简化自定义数据类型间的运算，支持动态绑定

```c++
/*左移配合友元可以实现输出自定义数据类型，但只能全局函数重载，成员函数实现不了对象本身在cout右侧*/
ostream & operator<<(ostream &cout,Person &p)			//左移运算符重载
```

- `int& operator++()`			    //前置递增
- `int operator++(int)`			//后置递增

`operator=`			//配合深拷贝

`operator[]`			//对象[索引]直接访问某成员指针维护的数组元素

`operator()`			//仿函数，对象()调用



## 继承

`class Son:public Base`			//class 子类:继承方式 父类

- 继承方式：决定子类继承后的最大访问权限
- 父类所有非静态成员属性都会被继承
- 父类私有成员继承后被编译器隐藏，访问不到



**当子类与父类拥有同名的成员：**

- 编译器会隐藏父类中所有同名成员函数

`s.Base::xxx`		//访问父类成员需要加作用域

```c++
Son::Base::m_Age				//静态成员通过子类类名访问父类成员
/*	第一个:: 类名方式访问
	第二个:: 父类作用域下	*/
```



- C++允许多继承，但实际开发不建议用

**菱形继承：**（🦙）

- 子类继承两份相同数据，导致资源浪费，用虚继承来解决

```C++
//vbptr：虚基类指针     vbtable：虚基类表

class Sheep : virtual public Animal{}

/*此时Animal变为虚基类

SheepTuo继承Sheep和Tuo的vbptr

vbptr指向各自的vbtable

vbtable里含有相对偏移，可以定位到唯一的m_Age*/
```



## 多态

- 一般指动态多态，子类重写父类虚函数实现函数地址晚绑定
- 虚⇔可重写，纯虚⇔声明

**实现：**

```C++
//vfptr：虚函数指针     vftable：虚函数表

/*当Animal中多了虚函数会生成一个vfptr

vfptr指向vftable，vftable存有 &Animal::function

Cat继承Animal，各自的vfptr指向各自vftable

Cat 虚函数表内也存有 &Animal::function

当发生函数重写，&Cat::function会覆盖Cat表内容*/
```

- 开发提倡开闭原则：对扩展进行开放，对修改进行关闭

`virtual void func()=0;`			//纯虚函数，实质是一条声明

- 抽象类：含纯虚函数的类，无法实例化对象，一般不写构造函数
- 案例：零件抽象类，电脑类里写组装函数，维护指针来调用接口，零件厂商实现



## 文件操作

`#include <fstream>`

**操作文件三大类：**

- `ofstream` ：写操作

- `ifstream` ：读操作

- `fstream` ：读写操作

**文件打开方式：**（多选：| ）

- `ios::in`———读文件

- `ios::out`———写文件

- `ios::app`———追加文件

- `ios::ate`———初始位置文末

- `ios::trunc`———[存在则删除]创建

- `ios::binary`———二进制方式

**步骤：**

```C++
ofstream ofs;
ofs.open(“文件路径”,打开方式);
ofs << “写入的数据”;
ofs.close();
```

`while(ifs >> buf)`		//读入字符数组

```C++
ifs >> ch				//判断空文件
ifs.eof()
```

- 尽量不用c++字符串而是用字符数组，底层是c写的

  

## 模板

**意义：**类型参数化

**语法：**

```c++
template<typename/class T,…>        	//模板声明
//函数/类
```

```
func<int>(a,b)                		//显式指定类型
func(a,b)                   		//自动类型推导（不发生自动类型转换，只适用函数模板）
```

**注意：**必须确定T的类型才能使用



**调用规则：**

1. 函数模板和普通函数相同，优先普通函数

2. 函数模板匹配更好时，优先函数模板

3. `func<>(a,b)`		//空模板参数列表，能强制调用函数模板

4. 函数模板可以重载

   

- `template<> void func(Person p1,Person p2)`		//具体化的模板可以解决自定义类型的通用化

- 学模板不是为了写，而是为了在STL中运用系统提供的模板
- 类模板中的成员函数在调用时创建

**类模板实例化的对象当函数形参时：**

1. 指定传入类型：`func(Person<string,int>&p)`
2. 参数模板化：`func(Person<T1,T2>&p)`
3. 整个类模板化：`func(T &p)`

`typeid(T).name()` 			//以字符串形式返回T的具体数据类型

- 全局函数配合友元在类模板外实现，需要先声明类模板再实现全局函数



## STL - 容器

- 容器、算法、迭代器、仿函数、适配器、空间配置器

- 迭代器：指针

`for(vector<Person>::iterator it=v.begin();it!=v.end();it++)`			//迭代器遍历容器

`#include <algorithm>`		//所有不支持随机访问迭代器的容器，不可以用标准算法

`vector<Person>`				//*it是<>里面的Person



tips：让迭代器`++,--,it = it +1`，观察编译器是否报错就能验证容器支持的迭代器了

### string容器

- string是一个类，char *是一个指针

- string类内封装了char*，是一个char*型的容器


**构造：**

​	`string();`

​	`string(const char* s);`

​	`string(const string &str);`

​	`string(int n,char c);`

**赋值：**

​	`string& operator=(const char* s);`

​	`string& operator=(const string& s);`

​	`string& operator=(char c);`

​	`string& assign=(const char* s);`

​	`string& assign=(const char* s,int n);`

​	`string& assign=(const string &s);`

​	`string& assign=(int n,char c);`

**字符串拼接：**

​	`string& operator+=(const char* str);`

​	`string& operator+=(const char c);`

​	`string& operator+=(const string& str);`

​	`string& append(const char* s);`

​	`string& append(const char* s,int n);`

​	`string& append(const string &s);`

​	`string& append(const string &s,int pos,int n);`

**查找和替换：**

​	`int find(const string& str,int pos=0) const;`

​	`int find(const char* s,int pos=0) const;`

​	`int find(const char* s,int pos,int n) const;`

​	`int find(const char c,int pos=0) const;`

​	`int rfind(const string& str,int pos=npos) const;`		//  rfind是从右向左查

​	`int rfind(const char* s,int pos=npos) const;`

​	`int rfind(const char* s,int pos,int n) const;`

​	`int rfind(const char c,int pos=0) const;`

​	`string& replace(int pos,int n,const string& str);`

​	`string& replace(int pos,int n,const char* s);`

**字符串比较：**

​	`int compare(const string &s) const;`			// 相等返回0

​	`int compare(const char * s) const;`

**字符存取：**

​	`char& operator[](int n);`

​	`char& at(int n);`

**字符串插入和删除：**

​	`string& insert(int pos,const char* s);`

​	`string& insert(int pos,const string& str);`

​	`string& insert(int pos,int n,char c);`

​	`string& erase(int pos,int n=npos);`

**子串获取：**

​	`string substr(int pos=0,int n=npos) const;`

### ==vector容器==

<img src="./settings/vector容器.jpg" alt="vector容器" style="zoom:130%;" />

**构造：**

​	`vector<T> v;`

​	`vector(v.begin(), v.end());`

​	`vector(n, elem);`

​	`vector(const vector &vec);`

**赋值：**

​	`vector& operator=(const vector &vec);`

​	`assign(beg, end);`

​	`assign(n, elem);`

**容量和大小：**

​	`empty(); `

​	`capacity();`

​	`size();`  

​	`resize(int num);` 				//将元素批量搬入容器时须提前开辟空间

​	`resize(int num, elem);` 

**插入和删除：**

​	`push_back(ele);`  

​	`pop_back();`   

​	`insert(const_iterator pos, ele);`  

​	`insert(const_iterator pos, int count,ele);`

​	`erase(const_iterator pos);` 

​	`erase(const_iterator start, const_iterator end);`

​	`clear();`

**数据存取：**

​	`at(int idx); `

​	`operator[]; ` 

​	`front(); `

​	`back();` 

**互换容器**：

​	`swap(vec);` 				//可以使两个容器互换，达到实用的收缩内存效果

**预留空间：**

​	`reserve(int len);`		//预留len个元素长度，预留位置不初始化，元素不可访问

```c++
/*利用动态扩展机制，统计开辟次数*/
int num = 0;
int* p = NULL;
for (int i = 0; i < 100000; i++) {
    v.push_back(i);
    if (p != &v[0]) {
        p = &v[0];
        num++;
    }
}
```

### deque容器

双端数组，头部增删速度比vector快，但元素访问速度要慢

<img src="./settings/deque容器.jpg" alt="deque容器" style="zoom:130%;" />

![deque内部工作原理](./settings/中控器.jpg)

**构造：**

​	`deque<T> deqT;`

​	`deque(beg, end);` 

​	`deque(n, elem);` 

​	`deque(const deque &deq);`

**赋值：**

​	`deque& operator=(const deque &deq); `

​	`assign(beg, end);`

​	`assign(n, elem);` 

```c++
void printDeque(const deque<int>& d) 
{			/*限制容器为只读状态时，相应的迭代器也要用const_iterator*/
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}
```

**大小：**

​	`deque.empty();`

​	`deque.size();` 				//deque没有容量的概念

​	`deque.resize(num);` 

​	`deque.resize(num, elem);`

**插入和删除：**

​	`push_back(elem);`

​	`push_front(elem);`

​	`pop_back();`

​	`pop_front();`

​	`insert(pos,elem);`

​	`insert(pos,n,elem);`

​	`insert(pos,beg,end);`

​	`clear();`

​	`erase(beg,end);`

​	`erase(pos);`

**数据存取：**

​	`at(int idx); `

​	`operator[]; `

​	`front(); `

​	`back();`

**排序：**

​	`sort(iterator beg, iterator end)`		//默认升序，属于标准算法

### stack容器

栈中只有顶端的元素才可以被外界使用，因此栈不允许有遍历行为（遍历是非质变算法）

<img src="./settings/stack容器.jpg" alt="stack容器"  />

**构造：**

​	`stack<T> stk;`

​	`stack(const stack &stk);`

**赋值：**

​	`stack& operator=(const stack &stk);`

**数据存取：**

​	`push(elem);`

​	`pop();` 

​	`top();`

**大小：**

​	`empty();`

​	`size();`

### queue容器

队列中只有队头和队尾才可以被外界使用，因此队列不允许有遍历行为

<img src="./settings/queue容器.jpg" alt="queue容器"  />

**构造：**

​	`queue<T> que;`

​	`queue(const queue &que);`

**赋值：**

​	`queue& operator=(const queue &que);`

**数据存取：**

​	`push(elem);`

​	`pop();` 

​	`back();` 

​	`front();` 

**大小：**

​	`empty();` 

​	`size();` 

### ==list容器==

双向循环链表，list中的迭代器只支持前移和后移，属于**双向迭代器**

<img src="./settings/list容器.jpg" alt="list容器"  />

插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的

**构造：**

​	`list<T> lst;` 

​	`list(beg,end);` 

​	`list(n,elem);` 

​	`list(const list &lst);`

**赋值和交换：**

​	`assign(beg, end);`

​	`assign(n, elem);`

​	`list& operator=(const list &lst);`

​	`swap(lst);` 

**大小：**

​	`size();`

​	`empty();`

​	`resize(num);` 

​	`resize(num, elem);` 

**插入和删除：**

​	`push_back(elem);`

​	`pop_back();`

​	`push_front(elem);`

​	`pop_front();`

​	`insert(pos,elem);`

​	`insert(pos,n,elem);`

​	`insert(pos,beg,end);`

​	`clear();`

​	`erase(beg,end);`

​	`erase(pos);`

​	`remove(elem);`

**数据存取：**

​	`front();`

​	`back();`

**反转和排序：**

​	`reverse();`

​	`sort();` 		//默认升序，是list容器的成员算法

### set/multiset 容器

属于**关联式容器**，底层结构是用**二叉树**实现，所有元素都会在插入时自动被排序

`#include <set>`			//set/multiset共用同一个头文件



**构造和赋值：**

​	`set<T> st;`

​	`set(const set &st);`

​	`set& operator=(const set &st);`

**大小和交换：**

​	`size();`

​	`empty();`

​	`swap(st);` 

**插入和删除：**

​	`insert(elem);`

​	`clear();`

​	`erase(pos);`

​	`erase(beg, end);` 

​	`erase(elem);`

**查找和统计：**

​	`find(key);`                  //查找key是否存在：存在返回该键的元素的迭代器；不存在，返回`set.end();`

​	`count(key);`



***set和multiset区别：***

```c++
pair<iterator, bool> insert(value_type&& _Val);			//set插入数据时返回bool
iterator insert(value_type&& _Val);						//multiset可以插入重复数据
```

***pair对组：***

​	成对出现的数据，可以返回两个数据	通过p.first、p.second访问

​	`pair<type, type> p ( value1, value2 );`
​	`pair<type, type> p = make_pair( value1, value2 );`

***容器排序：***

​	用仿函数可以指定set容器的排序规则

```c++
class Compare
{
public:
	bool operator()(const Person& p1, const Person &p2)
	{
		return p1.m_Age > p2.m_Age;			//按照年龄降序
	}
};
void test01(){	set<Person,Compare> s;}
```

- **注意：**自定义数据类型必须指定规则

### map/ multimap容器

属于**关联式容器**，底层结构是用**二叉树**实现，所有元素都会在插入时自动被排序



所有元素都是`pair<key,value>`，可以根据key值快速找到value值



**构造和赋值：**

​	`map<T1, T2> mp;`

​	`map(const map &mp);`

​	`map& operator=(const map &mp);`

**大小和交换：**

​	`size();`

​	`empty();`

​	`swap(st);`

**插入和删除：**

​	`insert(elem);`

```c++
	/*四种插入方式*/
m.insert(pair<int, int>(1, 10));
m.insert(make_pair(2, 20));
m.insert(map<int, int>::value_type(3, 30));
m[4] = 40; 			//建议获取某个key的value，而非修改
```

​	`clear();`

​	`erase(pos);`

​	`erase(beg, end);`

​	`erase(key);`

**查找和统计：**

​	`find(key);`                 //用法同set

​	`count(key);`

## STL - 函数对象



### 函数对象

重载**函数调用操作符**的类的对象，也叫**仿函数**

- **本质：**是一个**类**，不是一个函数



* 使用时像普通函数：`p()`
* 可以有自己的状态：`int m_count`
* 可以作为参数传递

### 谓词

返回bool类型的仿函数

```c++
class Predicate
{
public:				/*一个参数叫一元谓词，两个叫二元谓词*/
    bool operator()(int num1,int num2){	return num1>num2}
}
```

### 内建函数对象

`#include<functional>`



**算数仿函数：**

​	`template<class T> T plus<T>`             	   //加法

​	`template<class T> T minus<T>`        	      //减法

​	`template<class T> T multiplies<T>`  	  //乘法

​	`template<class T> T divides<T>`      	   //除法

​	`template<class T> T modulus<T>`     	    //取模

​	`template<class T> T negate<T>`       	    //取反

**关系仿函数：**

​	`template<class T> bool equal_to<T>`                    //等于

​	`template<class T> bool not_equal_to<T>`            //不等于

​	`template<class T> bool greater<T>`      		//大于

​	`template<class T> bool greater_equal<T>` 	//大于等于

​	`template<class T> bool less<T>` 			 //小于

​	`template<class T> bool less_equal<T>`     	 //小于等于

**逻辑仿函数：**

​	`template<class T> bool logical_and<T>`	    	//与

​	`template<class T> bool logical_or<T>`                	//或

​	`template<class T> bool logical_not<T>`              	//非

## STL - 算法

`#include <algorithm>`			   //比较、 交换、查找、遍历操作、复制、修改等等

 `#include <functional>` 			//定义了一些模板类,用以声明函数对象

`#include <numeric>`				//只包括几个在序列上面进行简单数学运算的模板函数



### 遍历



`for_each(iterator beg, iterator end, _func);`

-  遍历容器，尾参为函数指针或者函数对象



`transform(iterator beg1, iterator end1, iterator beg2, _func);`

- 容器间搬运，必须提前开辟空间

### 查找



`find(iterator beg, iterator end, value);`

-  按值查找元素	【重载operator==】



`find_if(iterator beg, iterator end, _Pred);`

- 按条件查找元素



`adjacent_find(iterator beg, iterator end);`

- 查找相邻重复元素



`bool binary_search(iterator beg, iterator end, value);`

- 查找指定元素是否存在，只适用**有序序列**



`count(iterator beg, iterator end, value);`

- 统计元素个数	【重载operator==】



`count_if(iterator beg, iterator end, _Pred);`

- 按条件统计元素个数

### 排序



`sort(iterator beg, iterator end, _Pred);`

-  按值查找元素



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



`replace_if(iterator beg, iterator end, _pred, newvalue);`

- 按条件替换元素，满足条件的替换成指定元素



`swap(container c1, container c2);`

- 互换两个容器的元素

### 算术生成算法

属于小型算法，使用时包含的头文件 `#include <numeric>`



`accumulate(iterator beg, iterator end, value);`

- 计算区间内 容器元素累计总和



`fill(iterator beg, iterator end, value);`

- 向容器中填充指定的元素

### 集合算法

- **注意：**初始集合必须是**有序序列**，返回值是目标容器集合最后一个元素的迭代器



`set_intersection(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator dest);`

- 求两个容器的交集	目标容器开辟空间需要从**两个容器中取小值**



`set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

- 求两个容器的并集	目标容器开辟空间需要**两个容器相加**



`set_difference(iterator beg1,iterator end1,iterator beg2,iterator end2, iterator dest);`

- 求两个容器的差集	目标容器开辟空间需要从**两个容器取较大值**
