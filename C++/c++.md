# 基础

### 数据类型

sizeof()查看判断



### 跳转

goto

无条件跳转

```c++
#include<iostream>
using namespace std;
#include<ctime>
int main(void) {

	for (int i = 1; i < 10; i++) {
		for (int j = 1; j <= i; j++) {
			cout << i << "*" << j << "=" << i * j<<"\t";
			if (j == 6) {
				goto Flag;
			}
		}
		cout << endl;
	}
Flag:cout << "跳转" << endl;
	system("pause");
	return 0;
}
```



### 数组

数据初始化没填完会用0填补剩余数据

```c++
int arr[] = {1,2,3};
```



数组名：

+ 可以统计整个数组在内存中的长度：sizeof(arr)
+ 可以获取数组在内存中的首地址
+ 数组名是常量，不可以进行修改

```c++
int a[] = { 1,2,3 };
cout <<(long long) a << endl;
cout << (long long) & a[1] << endl;
cout << sizeof(a)/sizeof(a[0]) << endl;
```



排序

```c++
int temp;
int a[] = { 1,2,3,5,6,7,4,8};
int length = sizeof(a) / sizeof(a[0]);
for (int i = length-1; i >0; i--) {
	for (int j = 0; j < i; j++) {
		if (a[j] > a[j + 1]) {
			temp = a[j];
			a[j] = a[j + 1];
			a[j + 1] = temp;
		}
	}
}
```





### 分文件编写

头文件函数声明

源文件函数定义  ----------指的是封装函数的源文件需要引入#include “自定义头文件.h”

其他文件（主文件等）使用也需要引入，但只要头文件即可     #include “自定义头文件.h”







# 指针 int* p

用来保存地址

&取址符

*p 解引用找到指针指向的内存



> 在32位操作系统下，指针是占4个字节空间，不管什么数据类型
>
> 在64位操作系统下，指针是占8个字节空间



##### 空指针

指针变量指向内存中编号为0的空间

初始化指针变量，不可以进行访问

（0~255内存编号）系统占用内存，不可以进行访问



##### 野指针

指向非法的内存空间





### const

##### 常量指针

const修饰指针

```c++
const int* p = &a;

//////或者////////

int const* p = &a;
```

指向可改，但指针指向的值不可以改



##### 指针常量

const修饰常量

```c++
int* const p = &a;
```

指针的指向不可以改，但是指针指向的值可以改



##### 即修饰指针，又修饰常量

```c++
const int* const p = &a;
```

什么都不能改



### 结构体

```c++
	struct Student
{
	string name;
	int age;
	int score;
};
	
	
	// struct可省略
	struct Student s1;
	s1.age = 18;


////////////////////////////////
	struct Student s1 = {};
```



##### 结构体数组



##### 结构体指针

->

```c++

struct Student
{
	string name;
	int age;
	int score;
};


	struct Student s;
	struct Student* p = &s;
	p->age = 10;

```



##### 结构体嵌套

. .

##### 结构体函数

- 值传递（会赋值一份新的，增加容量）
- 地址传递（一个指针只占4/8个字节内存）



##### const使用

将函数的形参改成指针，可以减少内存空间，而且不会复制新的副本，只会创建一个指针

但是通过地址可能会误改本体值，因此需要const防止误改





### 与变量创建区别

![image-20230328213115736](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230328213115736.png)

指针需要指定空间，变量不需要手动分配空间







# 引用

必须要有初始化，无法更改

相当于指针常量 

```c++
int a = 5;
int& ref = a;
```

相当于别名，但是本体是一致的，上面：本体都是a；

编译器不需要实际创建一个新的变量

不是一个真正的变量，语法糖



**const &** 

**没有& const这种写法**

因为，在 C++ 中，`const` 关键字必须出现在类型名称之前，用于指定常量类型修饰符。如果你需要同时指定常量类型和引用类型，应该先写 `const` 关键字，再加上 `&` 引用运算符，例如：`int const &` 或者 `const int &`。





### 注意

![image-20230328213310020](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230328213310020.png)

函数结束single结束，引用也没了

解决

![image-20230328213703740](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230328213703740.png)

返回的是一个class类







# 函数指针

void(*funcname)(params);

没有括号

![image-20230404114135872](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230404114135872.png)

  



### lambda函数

[=]  所有参数

[&] 所有参数的引用

[a] a参数



#include<functional>

std::function<函数声明>

**非捕获lambda可以隐式转换为函数指针，而有捕获lambda不可以**

```c++
auto a = std::find_if(data.begin(), data.end(), [](int value) {return value > 4; });
Log(*a);
```







# 类

默认都是私有的

和struct区别：默认归属（可见性）不同

建议：

struct：数据

类：继承、数据、处理





### 创建

![image-20230407160415144](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407160415144.png)

因为通过字符串字面量创建，不会触发拷贝构造。

![image-20230407162128221](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407162128221.png)

类型转化











### 静态（static）

需要再次定义

```
类型 类名::静态属性；
```

使用

```
类名::静态属性 = ...;
```



静态方法不能访问静态变量：不知道该调用哪个实例的变量







### 注意

```c++
class Singleton {
public:
	int a;
	Singleton* create() {
		int a = 1;
		std::cout << &a <<  std::endl;
		Singleton single;
		return &single;
	}
	void ss(int pa) {
		int c = pa;
		std::cout << &c << std::endl;
		std::cout << "Single" << std::endl;
	}
};
int main() {
	Singleton s1;
	Singleton s2;
	s1.ss(2);
	s1.ss(3);

	std::cin.get();
}
```

在你的代码中，`c` 的地址看起来虽然相同，但实际上这是一种假象。这是因为在 `ss` 函数中，`c` 是局部变量，它存储在栈上。每次你调用该函数时，都会为 `c` 分配一块新的内存空间。当函数返回时，这块内存就被释放了。

因此，尽管 `c` 的地址在两次调用 `ss` 函数时看起来相同，但实际上这只是因为程序运行时为其分配的内存空间恰好是相同的，并不是由于这些变量实际上位于相同的内存位置。

另外需要注意的是，在 `create` 函数中，你创建了一个局部对象 `single`，然后将其地址作为返回值返回。但是，在该函数返回后，`single` 就会被销毁，因此不能保证该返回值是有效的。这是因为 `single` 对象是在 `create` 函数的栈帧中创建的，当函数返回时，该栈帧就被销毁，从而导致 `single` 对象在内存中被清除。因此，如果你使用 `create` 函数返回的指针，将会导致未定义的行为。

**禁止返回局部的栈引用或者指针。**

> 这种现象的原理主要涉及到两个方面：编译器优化和栈帧布局。
>
> 对于编译器优化，当函数内部定义了一个局部变量时，编译器会选择在堆栈中为该变量分配一段空间。但是，在某些情况下，为了提高程序性能和内存使用效率，编译器可能会采用寄存器来存储局部变量，而不是在堆栈上分配空间。这样做可以避免不必要的内存读写操作，从而加快程序运行速度。
>
> 另一个因素是栈帧布局。在函数被调用时，系统会为该函数在调用栈中创建一个新的栈帧，该栈帧包括函数的参数、局部变量和返回地址等信息。每次函数被调用时，系统都会为该函数创建一个新的栈帧，从而在堆栈上分配一块新的内存空间。然而，由于栈帧大小通常是固定的，因此在某些情况下，相邻的栈帧可能会在堆栈上使用相同的内存空间，导致局部变量的地址看起来相同。
>
> 需要注意的是，这种优化行为并不是由编程语言或操作系统规范所定义的，因此不能保证在所有情况下都会发生。因此，在编写代码时，不能依赖于这种行为来实现任何功能，而应该使用正式的内存管理和指针操作方法。





### 构造函数

与对象同名，主要用于初始化

如果不实例化对象，不会运行

没有参数，实例构造不要用()

**使用类的静态方法不会运行**



![image-20230329103100168](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230329103100168.png)

删除构造函数





### 析构函数

销毁，释放内存

函数栈，数据超出函数范围就会自动销毁，对象超出销毁，析构函数被调用

```c++
class AA {
public:
	AA() {
		//构造函数
	};
	~AA() {
		//析构函数
	};
};
```







### 继承

子类拥有父类的一切

![image-20230411164306672](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230411164306672.png)





### 虚函数

子类和父类有**同名函数**时，使用父类的类型时去调用函数，会只能产生父类效果

![image-20230329112119121](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230329112119121.png)

为了在**子类**中能够实现复写同名基函数的功能

将基类中的基函数标记为虚函数

```c++
virtual  void functionvir(){};
```

告诉编译器为这个函数生成v表；如果它被重写了，就指向正确的函数

最好在子类上复写函数也标上override记号

```c++
void functionvir() override {};
```

![image-20230329112229604](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230329112229604.png)





缺点：

- 额外内存存储v表
- 基类要有一个成员指针指向v表
- 调用虚函数时，需要遍历这个表来确定要映射到哪个函数

 影响小







### 纯虚函数

基类只声明函数，没有定义(无法创建实例)。强制由子类实现。这通常被称为接口（interface）

如果子类也不定义纯虚函数，子类也无法创建实例。

纯虚函数必须被创建才能创建实例。

子类共有的函数的类。

确保类有一个共同的特定方法。

```c++
class Template
{
public:
	virtual std::string PrintName() = 0;
};
```







### 虚析构函数

**一定要有**

子类派生于父类，子类实例化—>delete：会先调用父类构造，然后子类构造；delete时会先调用子类析构，然后父类析构。

如果父类类型定义子类，子类会先父类构造，然后子类构造，delete是只会父类析构。因为父类析构没有virtual标志。

virtual  父类析构函数   

要加上子类析构







### move构造函数

移动语义：减少拷贝次数。

利用右值实现：只是个临时的，接管。

减少新的内存块分配。



##### 一般情况

第一次destroyed,临时对象的销毁。

![image-20230407142749546](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407142749546.png)

##### 移动

​		把旧实例的地址替换给新的指针，然后把旧的置为空或者0，这样删除也不会删两次而报错。

**只接管，不拷贝**

![image-20230407144617456](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407144617456.png)

因为右值引用在进入函数体内的之后,参数类型会变为左值
也就是在函数体内你可以对 String&& val中的 val取&
所以要触发移动语义必须让他变为右值引用,std::move就是干这个转换的
你看std::move的代码 就是干了一个找到源参数类型的static_cast转换
把参数换成了&&类型
所以就能触发后面的移动构造函数



##### 最终move实现

![image-20230407144734983](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407144734983.png)

String&& 使用 std::move代替

![image-20230407154016341](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407154016341.png)









### 可见性

private

protected

public









### 成员初始化列表

![image-20230329192737590](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230329192737590.png)



在对象实例化后，调用构造函数首先会调用初始化列表中的成员变量，初始化时没有在列表中的成员也初始化，然后再执行构造函数花括号里的。

为此以防初始化两次，要使用初始化列表。

![image-20230329194701220](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230329194701220.png)



##### 多态情况

初始化列表初始化时，一定是先初始化话父类，再初始化子类，这点可以通过在父类和子类中的构造函数加打印语句就可以证明。在子类Center的构造函数初始化列表中初始化m_name变量，首先m_name是继承自父类的变量，初始化该变量前必须保证其父类已经完成初始化，可以这么理解

```c++
Center::Center(std::string &name)
	:Player(name)
{
	//m_name = name;
}
```

但前提是Player需要提供一个含参构造函数。故这种修改在这里不适用，但是可以帮助我能理解，在初始化列表中，初始化继承自父类的成员变量，必须通过父类的构造函数去实现，否则可以选择在构造函数函数体内进行初始化

```c++
Center::Center(std::string &name)
{
	m_name = name;
}
```

这样才能保证其父类已经完成初始化，然后才能使用继承自父类的变量。











### this

类型

```c++
 Entity* a = this;
 Entity& b = *this;
```





### 成员函数

类的成员函数可以访问**同一类**的其他对象的**私有成员**，即使这些对象是不同的实例。





### const对象

const指针只能调用const方法







# static

类内 所有实例共享

在类和结构体外，链接只在内部有效（局部）

类似类的private

name.cpp

```c++
int s_age = 15;
```

main.cpp

```cpp
#include<iostream>

extern int s_age;
```



### 类实例

```c++
class Singleton {
public:
	int a = 0;
	static Singleton& create() {
		static Singleton single;  //实现函数销毁，实例仍然在
		return single;
	}
	void ss(int pa) {
		int c = pa;
		a = pa;
		std::cout << &c << std::endl;
		std::cout << "Single" << std::endl;
	}
};
int main() {
	Singleton s1;

	s1.create().ss(2);
	Singleton::create().ss(1);

	std::cout << s1.create().a << std::endl;  // 1
	std::cout << s1.a << std::endl;   //0


	std::cin.get();
}
```









# 枚举

整数类型

数值集合

```c++
#include<iostream>

enum Example
{
	A = 0, B = 3, C
};


int main() {
	std::cout << A << std::endl;

	Example cout = B;			//枚举这样使用就能限制在枚举定义的内容
	std::cout << cout << std::endl;

	std::cout << C << std::endl;
	std::cin.get();
}
```



```C++
std::cout << Example::A << std::endl;
```



# new

创建内容

堆上创建，不会随作用域消失而销毁

步骤：

​		new 加上数据类型，决定了必要的分配大小，以字节为单位。

​		告诉c语言标准库，需要大小，找到适合字节内存的连续块。

​		找到内存后返回指向地址的**指针**。

返回给你的是一个指向分配地址的指针，需要指针变量



### new对象

不仅在堆上创建空间，还给对象初始化

类似分配空间，但malloc不会调用构造函数

```c++
Entity* En = new Entity;

Entity* En = (Entity*)malloc(sizeof(Entity));
```



### delete

必须要记得



### placement new

```c++
int p;
new (&p) int('a')
```

你在new后面紧接着提供一个 (参数...) ，这些参数就构成了new表达式中的placement params，它们将被传递给内存分配函数，而标准内存分配函数的行为是直接把这个多出来的参数返回回来。效果就是使用你预先分配的内存地址而不是内存分配器新分配出来的地址，在你提供的内存上就地构造对象，从而实现不额外分配堆内存、只调用构造函数的效果。

前面的例子，就是首先开一个**栈上**变量p（同时获取了1个int大小的内存），然后在这块内存上就地构造一个int对象，初始值为int('a')也就是97。终状态是p=97，没有堆内存被分配。



覆盖









# 数组

```c++
int e[5];
int* ptr = e;
*(int*)((char*)ptr + 4) = 1;

///////////////////e[1] = 1
```

指针------------地址-----------字节数区分





### new

堆上创建，不会随作用域消失而销毁

需要手动销毁：delete

函数栈，数据超出函数范围就会自动销毁,通过new实现不会随函数运行结束就消失的数组

```c++
int* example = new int[5];


delete[] example;
//////////////////////////
//example指向数组的地址
```







# 动态数组

当push_back，往数组中添加元素，如果数组容量不够，数组就需要分配新的内存，把旧数组内容从内存旧的位置复制到内存中新的位置，然后删除旧位置的内存。拖慢进度。

### std::vector

连续一块，一排上。

定义尽量不用指针。

![image-20230331135954447](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331135954447.png)



### 多次拷贝

![image-20230331145031122](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331145031122.png)



![image-20230331145046471](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331145046471.png)



![image-20230331145206400](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331145206400.png)



### 优化

- 初始为数组长度为0会使得一开始就复制——————可以定一个初始长度（.reserve(50);）
- 每次创建实例是在main函数栈中创建，然后复制给vector类———————可以直接让vector帮着创建，通过（.emplace_back(参数)）

![image-20230331150115753](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331150115753.png)

**把值给vector**







# 静态数组

std::array<type,size>

栈上

数组使用这种定义：有着调试，也帮你记录了长度









# 多维数组

```c++
int** a2d = new int*[50];
```



50 * 50;



i:0~50;

[50 + 50 * i]

转到内存同一块区域速度更快

**转成一维数组**







# 字符串

std::string

+= 进行增加，因为这个操作符在string类中被重载了

```c++
	const wchar_t* ll = L"ssss";
	const char16_t* name2 = u"llll";
	const char32_t* name4 = U"TTT";
```



### 字符串字面量

永远只保存在内存的只读区域内



### 字符指针

char*

常量，不可更改



### 字符数组

char[]

可以更改



### 取消转义

R"()"

```c++
	const char* names = R"(s
		s
		dd
		dd
		)";
```





### 复制

memcpy(destination,original,size)









# const表现

> 在编译时，编译器会将程序中的常量直接替换为字面量。这样可以减少程序运行时的计算量，提高程序的执行效率。例如，在以下代码中：
>
> ```c++
> const int a = 5;
> int b = a + 3;
> ```
>
> 在编译时，常量 `a` 的值 `5` 会被直接替换为字面量 `5`，因此代码实际上等价于：
>
> ```c++
> int b = 5 + 3;
> ```
>
> 这种优化技术在许多编程语言中都得到了广泛应用。





### 对象中

方法：

```c++
void getter() const {
	//不能修改对象中的值，只读
}
```





```c++
const int* const get() const{
////只读，返回一个不能修改值和地址的内容
}
```



##### 限制

const对象，方法也要有const

```c++
class Tem
{
public:
	int X, Y;
public:
	int get() const
	{
		return X;
	}
};

//常量对象时注意
void Print(const Tem& tl) {
	tl.get();
}
```

const需要对应



如果想要打破限制

可以（mutable）：允许函数是常量的方法，但可以修改变量

![image-20230329190620634](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230329190620634.png)

mutable使用场景：

+ 基本是类 const中
+ lambda





##### 注意

**如果对象中方法没有修改值，就要加上const**







# 堆、栈

**创建对象推荐栈**

栈随作用域消失而释放（空间小，速度快）

堆手动（new）创建，需要手动回收（空间大，但会内存泄漏）





### 栈上创建函数

```c++
int* CreateArray() {
	int array[50];
	return array;
}
```

无效的创建，一旦函数结束，内存就被释放，没有东西



对象自动释放

![image-20230330154449020](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330154449020.png)



### 堆上实现自动释放

通过一个对象的构造函数创建，对象的析构函数删除。

![image-20230330155004259](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330155004259.png)



解决直接通过->实现效果

![image-20230330164743623](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330164743623.png)





### 内存比较

应用程序启动后，操作系统把整个程序加载到内存，并分配一大堆物理ram，以便实际应用程序运行。堆和栈是ram中实际存在的两个区域。

栈：预定大小的内存区域，通常约2M字节左右。

堆：虽然也预定了大小，但会生长，并随着应用程序的进行而改变。

**都在内存中**



栈：内存相互叠加存储，反向增长，邻近区域（一条cpu指令）

堆：free list 空闲列表（一堆事情：检查空闲列表，请求内存，记录）cache miss



尽量栈分配









# 智能指针

#include<memory>

### unique_ptr

作用域指针：超出作用域会被销毁，然后调用delete

必须是唯一的，不能被复制

explicit 显式的

![image-20230330161421939](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330161421939.png)

最好的定义方式

```c++
std::unique_ptr<Entity> entity = std::make_unique<Entity>();
entity->Show();
```

为了异常安全

因为这样如果构造函数碰巧抛出异常，会稍微安全一点，不会得到一个没有引用的悬空指针从而造成内存泄漏。

![image-20230330185324776](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330185324776.png)



unique独一无二不能复制

实现原理

![image-20230330203154380](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330203154380.png)













### shared_ptr

```c++
std::shared_ptr<Entity> sharedEntity = std::make_shared<Entity>();
```

用这种定义更高效。

因为shared_ptr需要分配一块内存——控制块，用来存储引用计数

​			如果使用std::shared_ptr<Entity> sharedEntity(new Entity);

​						会先创建一个new Entity，然后传递给shared_ptr构造函数

​			这样会做两次内存分配。（一次new Entity,一次shared_ptr内存块）

可用于对象之间共享

> 只有计数的引用变为0才会释放







### weak_ptr

shared_ptr复制过来不会增加shared_ptr引用次数

可以被复制，但不会增加额外的控制块来控制计数，仅声明这个指针还活着

![image-20230330191231804](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330191231804.png)









# 隐式转换

编译过程可以允许有一次隐式转换

隐式构造函数

![image-20230329210237045](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230329210237045.png)



### explicit

取消隐式转化，只能按规定创建









# 运算符(operator)



### 重载

类内只能有一个参数

在C++中，类内运算符重载函数只能有一个隐含的对象指针参数, 而不仅仅是指向当前对象的指针。

对于类内部定义的运算符重载函数，包括"+"，"-"，"*"等等，C++规定它们必须只接受一个参数，而这个参数就是需要参与运算的第二个对象。然而，由于运算符重载函数本质上是一种成员函数，因此它们还有一个隐含的指向当前对象的指针参数，即"this"指针。

所以，对于之前我提到的代码：

```c++
Copy Codeclass Vector {
public:
    //...
    Vector operator+(const Vector& other) const {
        Vector result;
        //执行加法操作
        return result;
    }
};
```

虽然运算符重载函数看起来有两个参数，但实际上只有一个显式传入的参数 "other"，而第二个参数则是隐含的指向当前对象的指针 "this"。

因此，C++规定了类内部的运算符重载函数只能有一个显式传入的参数，并且第一个参数通常为“const 引用”，代表需要参与运算的第二个对象。同时，由于该函数也是成员函数，它还有一个隐含的指向当前对象的指针参数。

> C++规定了类内运算符重载函数只能有一个显式参数，这是因为运算符重载函数本质上是一种成员函数，必须遵守C++对成员函数的规范。在定义成员函数时，第一个参数通常是指向当前对象的指针，而其他参数则根据需要自行添加。
>
> 对于运算符重载函数来说，它必须与原始的运算符语义保持一致，同时需要满足成员函数的特殊要求。另外，为了方便使用，C++还规定运算符重载函数必须只有一个显式参数，并且该参数通常是const引用。这样做的好处是：
>
> 1. 保证运算符重载函数的调用方式与普通成员函数相同，即通过对象名加运算符号进行调用。
> 2. 简化运算符重载函数的定义和使用，使代码更加简洁易懂，同时也避免了编译器产生歧义的情况。





推荐：

​	先写一个函数，再在重载函数里引用这个函数



### 顺序

在重载运算符时，我们必须关注参数和返回值类型以及函数名称，但是对于二元运算符，操作数的顺序也很重要，而对于一元运算符则不太重要。





# 复制和拷贝构造函数

![image-20230330193126888](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330193126888.png)



### 复制

##### 浅拷贝

对象内 char* ,对象外给新对象赋值为旧对象，两者的char*指向一样

浅拷贝：复制了指针，两个指针指向同一个完全相同的内存缓存区。

不会去到指针的内容或者指针所指的地方，也不会去复制它。

![image-20230330201852779](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330201852779.png)



### 拷贝构造函数

实现深拷贝

所有传形参的过程都是在拷贝,所以拷贝构造函数不能传形参,因为会无限递归,Notice(Notice other)这样是不行的

对象间复制，实现复制具体的值

![image-20230330203139578](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330203139578.png)

实现

![image-20230330203904104](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230330203904104.png)

传对象到函数参数也会触发拷贝构造函数，因此用引用(&)最合适。

通过const引用传对象









# 箭头操作符

### 偏移量操作

```c++
#include<iostream>


struct Vector
{
	int x, y, z;
};

int main() {
	//int* s = &((Vector*)nullptr)->y;
	int s =(int) & ((Vector*)0)->y;
	std::cout << s << std::endl;
	std::cin.get();
}
```

先创建指向0的Vector，对内部值分布进行查看



















# 库

静态库，库会被放到可执行文件中。库文件被编译到exe文件。lib

动态库，运行时被链接。库文件链接到exe文件。dll

 

![image-20230331185522884](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331185522884.png)



### 配置

##### 静态库

静态链接，编译时发生，是可执行文件的一部分。

引头文件，再引实际内容

include 头文件 函数声明

lib库  函数定义

![image-20230331194802732](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331194802732.png)



![image-20230331194834186](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331194834186.png)

![image-20230331152958899](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331152958899.png)



调用c库

![image-20230331195028709](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230331195028709.png)



##### 动态库

动态链接，链接发生在运行（exe）时，不是可执行文件的一部分。

.dll

.dll.lib（lib帮你指向了不需要_declspec(dllimport)，就已经能够使用dll库了。lib就是起到定位dll的作用，所以用不用这个_declspec(dllimport)都无所谓。）

需要把dll文件放在可执行文件一起，相同的位置。





### 创建和使用库

![image-20230403102521417](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230403102521417.png)

主文件配置引入目录

![image-20230403102601982](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230403102601982.png)



库文件配置类型（命名空间写法）

![image-20230403102648923](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230403102648923.png)

![image-20230403102754906](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230403102754906.png)



主文件引用

![image-20230403102729899](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230403102729899.png)











# 多返回值

元组tuple

pair

结构体yyds









# template

功能上>泛型

模板

```C++
template<typename T>
```

复制、粘贴、替换填空

函数模板：只有在调用了才会使用，没有调用只是个模板并不是实际存在的。

```c++
#include<iostream>

template<typename T>

void Print(T value) {
	std::cout << value << std::endl;
}

int main() {

	Print(1.1f);
	Print("saa");

	std::cin.get();
}
```



类模板：

```c++
template<typename T,int N>
class Array
{
private:
	T array[N];
public:
	int getSize()
	{
		return N;
	}
};


int main() {

	Array<int,10> aa;
	std::cout << aa.getSize() << std::endl;
	std::cin.get();
}
```





让编译器自己编代码

禁止滥用，不然可读性差







# 宏

预处理阶段

只是文本替换

debug模式和release模式切换

![image-20230404095725778](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230404095725778.png)

配置

![image-20230404095841462](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230404095841462.png)











# auto

自定判断类型

两面性

能够减少修改类型次数，但会破坏特定类型的使用

推荐：类型过长在使用

> auto不出来引用









# 命名空间

namespace



直接使用函数

using apple::print；



改名称

namespace a = apple;



尽可能在小作用域使用











# 线程

并行

![image-20230404184406213](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230404184406213.png)









# 基准测试

### 计时

std::chrono::high_resolution_clock::



对象作用域实现

```c++
struct Timer
{
	std::chrono::steady_clock::time_point begin,end;
	std::chrono::duration<float> duration;

	Timer()
	{
		begin = std::chrono::high_resolution_clock::now();
	}
	~Timer()
	{
		end = std::chrono::high_resolution_clock::now();
		duration = end - begin;
		std::cout <<"输出：" << duration.count() / 1000.0f << std::endl;
	}
};

```







### 可视化

chrome://tracing

json文件







# 排序

sort

#include<algorithm>

```
std::sort()
```

![image-20230404201002604](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230404201002604.png)





# 类型双关

int -> double

最好不使用



只是把指针解释成别的东西



结构体类似数组





# union

联合体

内部同等级数据内存相同

```c++
union
{
	struct
	{
		float x,y,z,w;	
	};
	struct
	{
		float a,b;
	}
}
```

a -> x;

b -> z;







# 类型转换（cast）

c风格：(int) 参数 ---------强制转换

c++风格：

+ static_cast
+ reinterpret_cast（类似双关：只是把指针解释成别的东西）
+ const_cast
+ dynamic_cast（检查类型转换是否成功）



### dynamic_cast

判断多态类型是否转换成功，

基类需要有virtual

运行时类型信息————rtti，增加了一些开销

![image-20230405130118207](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230405130118207.png)













# 条件与操作断点

实用技巧

可以实现在程序运行时调式

断点基础上选择条件和操作











# 预编译头文件(PCH)

每次编译都要编译头文件，不同文件相同头文件也要编译。很麻烦。

预编译头文件作用：接收一堆告诉它要接受的头文件，  基本上是一段代码，只编译一次，以二进制格式存储，这对编译器来说比单纯的文本处理要快的多。大大加快编译速度。



vs:将含有pch.h的cpp文件设置成预编译头文件，pch.h里有要的所有库的.h文件。主函数调用.h文件。



**推荐使用**











# **c++17后可用**

### 结构化绑定



以前：

元组tuple：std::tuple< int,std::string >  person = {1,"ma"}——————std::get<0>(person) 值为1；

对组pair

tie：类似解构      int age;std::string name;    std::tie(age,name)  = person;



现在：

结构化绑定：

​			auto[age,name] = person;

![image-20230406114452128](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230406114452128.png)







### optional

**c++17后可用**

判断数据有**没有存在**，可看作内部会产生一个bool数据。

.value_or("自定义返回内容")；有值就返回，没有返回自定义内容。

```c++
#include<tuple>
#include<string>
#include<optional>
#include<iostream>
std::optional<std::tuple<std::string,int>> CreatePerson(int param)
{
	if(param)
	return std::tuple<std::string,int> {"sasa",20 };
	return {};
}

int main()
{
	std::optional a = CreatePerson(1);
	//a == a.has_value()
	std::cout << a.has_value() << std::endl;
	if (a)
	{
		auto [name, age] = a.value();
		std::cout << name << std::endl;
	}
	std::optional b = CreatePerson(0);
	//返回空值，值判断为0
	//空值设置自定义值
	auto [names, ages] = b.value_or(std::tuple<std::string,int>{ "Adad", 2});
	std::cout << names << std::endl;

	std::cin.get();
}

```







### any（useless）

不需要列出类型，如果需要更多存储空间，any会**动态分配**

取值：

```c++
typename1 left = std::any_cast<typename1>(variability)；
```



推荐还是variant更安全。





### **string_view**









# 单一变量存多类型（variant）

variant

需要列出类型。如果需要更多存储空间，variant不会动态分配。

**对比any更优秀**。

检查是否是要的类型数方法：

.index()   返回类型的索引

std::get_if<类型>(&data);————返回的是地址

```c++
std::variant<std::string,int> data;
data = 2;
if(auto value = std::get_if<std::string>(&data))
{
    std::string name = std::get<0>(data);
}

data = "2323";
```



可以使用在错误等级提示，比optional只返回0，1更个性化。















# 运行更快



### 多线程

#include<future>

std::ref传引用进入并行函数

async 不能往里面传非常量左值引用，如果需要传递的话，自己要明晰可能的安全问题，使用std::ref 包装即可传递

```
std::async(sta::launch::async,任务函数，参数...)
```

需要互斥锁(mutex),RAII,类似计时器使用。

```c++
static std::mutex s_names;



//锁（析构函数会自动解锁）
std::lock_guard<std::mutex> lock(s_names);
//当期线程执行任务，其他线程运行到这里等待。
```













### 字符串复制

尽管使用引用也会有一次分配内存

字符串视图

**string_view**

指针+size

```c++
std::string name ="sdadrt";

//c.str()转换成const char*类型
//从0开始，两个
std::string_view(name.c_str(),2);

//从3开始，3个
std::string_view(name.c_str()+3,3);
```



![image-20230406165718766](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230406165718766.png)



















# 单例模式

对象实例化，只能有一个。

实际上是一个类

例如随机数对象不需要实例化，只通过单例模式便可实现。

![image-20230406195932772](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230406195932772.png)

改进

![image-20230406200222753](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230406200222753.png)







# 小字符串优化

buffer   16

![image-20230406202605161](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230406202605161.png)



![image-20230406202717045](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230406202717045.png)



release 模式下 小于15个字符都不会分配

​							一旦超过15个字符，新开辟16个字节

debug模式下 会先分配16给字节，不够再分配16个字节，堆分配。











# 内存跟踪释放

new和delete的重载操作符

实现可视化创建释放。



void*

**当void\*指针\**作为函数的输入和输出\**时，表示可以\**接受任意类型的输入指针和输出任意类型的指针\****

malloc函数只关注你要多大的内存，你需要把它怎么划分是你的事情，但是你需要显式的表明你是怎么划分的。这里语法要求是必须的，void *类型转为其他类型必须强制类型转换。

只能单向类型转换。void*可以转化成其他类型，但是有类型的不能转化成void*。









# 左值和右值

左值 非const常量

```c++
//左值引用
int& GetValue
{
    static int value = 10;
    return value;
}

int main()
{
	GetValue() = 20;
}
```





const std::string name+“asa”——————>加const的原因，name+“asa”是右值

### 左值引用&

int& a;





### 右值引用&&

int&& a;



左值不能引用非const右值。

需要const int& a = 100;





### 区别

- 左值：某种存储支持的变量，只能用左值引用

- 右值：临时存在的量，能用加了const 的左值引用，也能用右值引用









# 静态分析















# 参数计算顺序

不同编译器顺序可能不同，未定义

![image-20230407133615194](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407133615194.png)







# 移动语义(move)

窃取

std::move

移动赋值操作符重载：

​				防止内存泄漏（当前对象可能有内存分配着），需要先删去当前对象的内存，然后再赋值。

其他类似移动拷贝构造函数

判断是否是同一对象可以增加if判断

```c++
if(this != &other)
{
	//不是同一对象
}
else{
	//是同一对象
    return *this；
}
```





### 转移赋值操作符

```c++
String& operator=(String&& other)
	{
		//是否是同一个，不是则转移，是直接返回
		if (this != &other)
		{
			//先删除当前实例可能存在的内存
			delete[] m_Data;

			//转移操作
			m_Size = other.m_Size;
			m_Data = other.m_Data;

			//置空
			other.m_Size = 0;
			other.m_Data = nullptr;
		}
		return *this;
		
	}
```



结果发生转化

![image-20230407164436490](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407164436490.png)









# 常量表达式(constexpr)

`constexpr` 是 C++11 中引入的一个关键字，用于声明**能在编译期计算得出结果的常量表达式**。这样可以在编译时进行优化，提高程序的执行效率。

使用 `constexpr` 关键字声明的变量或函数必须满足以下条件：

1. 变量必须是一个常量表达式，即在编译期间就可以确定其值；
2. 函数必须返回一个常量表达式，并且函数体中只包含常量表达式和其他 `constexpr` 函数调用。

例如，下面的代码使用了 `constexpr` 来定义一个常量：

```c++
constexpr int factorial(int n) {
    return (n == 0) ? 1 : n * factorial(n - 1);
}

int main() {
    constexpr int res = factorial(5); // 在编译期计算
    std::cout << res << std::endl;
    return 0;
}
```

在上面的代码中，`factorial` 函数被声明为 `constexpr`，它可以在编译期求出给定参数的阶乘，并返回一个常量表达式。然后，我们在 `main` 函数中使用 `constexpr` 定义一个常量 `res` 并初始化为 `factorial(5)`，这个计算过程也会在编译期进行，最终输出结果 `120`。

需要注意的是，虽然 `constexpr` 声明的变量或函数必须是常量表达式，但并不是所有常量表达式都能被声明为 `constexpr`。一些常量表达式可能会因为过于复杂而不能在编译期计算，这时候就无法使用 `constexpr` 进行优化。



### const区别

`const` 和 `constexpr` 都可以用来声明常量，但二者有以下几个区别：

1. `const` 声明的常量可以是运行时常量，也可以是编译时常量；而 `constexpr` 声明的常量必须是编译时常量，即在编译期间就能确定其值。
2. `const` 声明的常量可以是任意类型，包括用户自定义类型；而 `constexpr` 声明的常量必须具备字面值类型（literal type）的特征，即不能包含运行时变量、动态分配内存等。通俗点说，就是只有简单类型和符合条件的复合类型才能用于 `constexpr` 常量的计算。
3. `const` 声明的变量可以通过函数调用进行初始化；而 `constexpr` 声明的变量只能使用常量表达式进行初始化。因为 `constexpr` 变量必须在编译时被计算，而函数调用可能涉及到运行时操作，所以不能用于 `constexpr` 常量的初始化。

例如，下面的代码演示了 `const` 和 `constexpr` 的不同之处：

```c++
const int a = 10;           // const 常量
constexpr int b = 20;       // constexpr 常量

class MyClass {};
const MyClass c;             // const 对象
// constexpr MyClass d;      // 错误：MyClass 不满足字面值类型的要求

const int func() { return 30; }
constexpr int sum(int x, int y) { return x + y; }

const int d = func();       // 可以使用函数调用初始化 const 常量
constexpr int e = sum(a, b);// 可以使用常量表达式计算初始化 constexpr 常量
```

在上面的代码中，`a` 和 `b` 都是声明为常量的，但 `a` 是一个运行时常量，而 `b` 是一个编译时常量。类 `MyClass` 不满足字面值类型的要求，所以不能声明为 `constexpr` 对象。函数 `func()` 返回一个运行时常量，在 `const` 常量的初始化中可以使用；而 `sum()` 函数返回一个编译时常量，在 `constexpr` 常量的初始化中可以使用。









# 静态断言(static_assert)

`static_assert` 是 C++11 中引入的一个关键字，用于在编译时对表达式进行静态断言。其语法为：

```c++
static_assert(expr, message);
```

其中 `expr` 是要进行断言的表达式，必须是一个**常量表达式**（在编译时就能计算出结果的表达式），如果 `expr` 的值为 `false`，则会在编译时产生一个错误，并输出 `message` 作为错误信息。

例如，假设我们有一个函数，需要确保传入的参数类型为整数，可以使用 `static_assert` 进行断言：

```c++
template<typename T>
void foo(T t) {
    static_assert(std::is_integral<T>::value, "T must be an integer type");
    // ...
}
```

这里使用了 `std::is_integral` 类型特征来检查模板参数 `T` 是否为整数类型，如果不是，则会在编译时产生一个错误，并输出错误信息 `"T must be an integer type"`。



![image-20230407202545562](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230407202545562.png)

 





# 自定义

### 数组Array

```c++
#include<iostream>


template<typename T,size_t S>
class Array
{
private:
	T m_array[S];
public:
	T& operator[](size_t index) const {
		return m_array[index]; 
	}
	T& operator[](size_t index)  {
		static_assert( S,"the index is out of the range");
		return m_array[index];
	}


	constexpr size_t size()const { return S; }

	const T* Data()const{ return m_array; }
	T* Data() { return m_array; }
};


int main()
{

	Array<std::string, 2> b;
	b[0] = "asdads";

	Array<int, 5> a;
	memset(a.Data(),0,a.size()*sizeof(int));
	a[1] = 2;


	for (size_t i = 0; i < a.size(); i++)
		std::cout << a[i] << std::endl;
	
	std::cin.get();
}
```











### Vector

![image-20230410140046281](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230410140046281.png)

emplace_back实现在类里帮我们创建，使用到了newplacement。

```c++
	//emplacement
	template<typename... Args>
	T& emplace_back(Args&&... args)
	{
		if (m_size >= m_capacity)
			ReAlloc(m_capacity + m_capacity / 2);

		//new placement,直接在类内部创建，覆盖
		new(&m_data[m_size]) T(std::forward<Args>(args)...);
		return m_data[m_size++];
	}

```



本体类会析构delete，出了作用域大类会触发析构delete（也会调用本体类的delete），触发了两次，会报错。

new即会创建空间也会调用构造函数；delete就会删除空间，也会调用析构函数。

![image-20230410162141721](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230410162141721.png)

解决方法：`::operator new/delete`

只增加/删,是一对的。不调用构造/析构函数。

created：

```c++
//分配新空间
// allocate space for new block
//T* capacity = new T[newCapacity];
T* capacity =(T*)::operator new(newCapacity*sizeof(T));
```

delete：

```c++
//每一个元素自己调用析构
void clear()
	{
		for (size_t i = 0; i < m_size; i++)
			m_data[i].~T();
		m_size = 0;
	}


//delete[] m_data;
//修改
clear();
::operator delete(m_data, m_capacity * sizeof(T));
```



















# iterator

```c++
for(int v:vector)
    
//c++17  
for(auto [key,value]:map)
```











# map  unordered_map

map: #include < map >
unordered_map: #include < unordered_map >

运行效率：unordered_map最高，而map效率较低但提供了稳定效率和有序的序列。

占用内存：map内存占用略低，unordered_map内存占用略高,而且是线性成比例的。

什么时候使用哪个？ 需要无序容器，快速查找删除，不担心略高的内存时用unordered_map；有序容器稳定查找删除效率，内存很在意时候用map。

```c++
#include <iostream>  
#include <unordered_map>  
#include <map>
#include <string>  
using namespace std;  
int main()  
{  
	//注意：C++11才开始支持括号初始化
    unordered_map<int, string> myMap={{ 5, "张大" },{ 6, "李五" }};//使用{}赋值
    myMap[2] = "李四";  //使用[ ]进行单个插入，若已存在键值2，则赋值修改，若无则插入。
    myMap.insert(pair<int, string>(3, "陈二"));//使用insert和pair插入
  
	//遍历输出+迭代器的使用
    auto iter = myMap.begin();//auto自动识别为迭代器类型unordered_map<int,string>::iterator
    while (iter!= myMap.end())
    {  
        cout << iter->first << "," << iter->second << endl;  
        ++iter;  
    }  
	
	//查找元素并输出+迭代器的使用
    auto iterator = myMap.find(2);//find()返回一个指向2的迭代器
    if (iterator != myMap.end())
	    cout << endl<< iterator->first << "," << iterator->second << endl;  
    system("pause");  
    return 0;  
}  
```



### 遍历方式

- 迭代器

  ```c++
  for(unordered_map<int,int>::iterator it=map.begin();it!=map.end();it++){
          cout<<it->first<<it->second<<endl;
      }
  //auto
  for(auto it=map.begin();it!=map.end();it++){
          cout<<it->first<<it->second<<endl;
      }
  ```

- 值遍历

  ```c++
  for(pair<int,int> kv:map){
          cout<<kv.first<<kv.second<<endl;
      }
  ```

- 结构化绑定（c++17）

  ```c++
  //值传递
  for(auto [k,v]:map){
          cout<<k<<v<<endl;
      }
  
  //引用传递
  for(auto& [k,v]:map){
          cout<<k<<v<<endl;
      }
  
  //只要一个结果
  for(auto& [k,_]:map){
          cout<<k<<endl;
      }
  
  for(auto& [_,v]:map){
          cout<<v<<endl;
      }
  ```

- 引用传递

  ```c++
  for(const pair<int,int>& kv:map){
          cout<<kv.first<<kv.second<<endl;
      }
  ```

  







### map

自动排序

![image-20230411100934781](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230411100934781.png)

### unordered_map

![image-20230411101435712](C:\Users\29557\AppData\Roaming\Typora\typora-user-images\image-20230411101435712.png)









# 注意

字符串不能和字符直接+

using namespace 对于std最好不要使用



# 流程

.c文件 --------->  编译 ------>  .obj文件（translation unit）  ------->Link ----------------> .exe



  

### 预编译

include、#if、#endif

include相当于复制内容到使用include的文件里



### 编译  c

任何常数计算在编译时都会被计算出来

根据源文件生成包含机器码和定义常量数据的二进制文件



### 链接 link

cpp文件会互通

声明与定义必须一致，不然匹配不成功

定义两次会产生link报错

- static 意味着只为当前translation unit 声明

- inline函数只是把内容取代过来调用
- 头文件只写声明，cpp文件写定义







# 头文件

```c++
#pragma once
```

表示只能编译一次

也可以通过

```c++
#ifndef _name_
#define _name_
....
....
#endif
```

实现
