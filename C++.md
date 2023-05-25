# C++（4.27开始）

## C++基本

### 七种基本数据类型

![image-20230428215804793](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20230428215804793.png)

**int强制转换**

对于浮点数类型，强制转换将截断小数部分并将其转换为整数；

对于指针类型，强制转换将其转换为整数类型，即指向该指针所指向的内存地址的整数值；

对于字符类型，强制转换将其转换为对应的ASCII码值。



## C++面向对象



## C++高级教程



## C++资源库

## C++基础语法（牛客）

[牛客网在线编程_编程学习|练习题_数据结构|系统设计题库 (nowcoder.com)](https://www.nowcoder.com/exam/oj?page=1&tab=语法篇&topicId=225&fromPut=pc_kol_aaaxiu)



### **输出一位小数**

```c++
#include <iomanip>
cout<<fixed<<setprecision(1)<<sum<<" "<<setprecision(1)<<(cur/2)<<endl;
```

在C++中，`fixed`是一个操纵符，用于设置浮点数的输出精度，使其输出定点形式而不是科学计数法形式。当使用`fixed`操纵符时，`setprecision`指定的精度将表示小数点后的位数，而不是有效数字的位数。

### 判断质数

逐个判断 注意缩小判断范围

判断一个数是否为质数可以采用试除法，即从**2开始到这个数的平方根为止**，逐一判断能否整除。如果在2到sqrt(n)之间找到了一个数能够整除n，那么n就不是质数；反之，n就是质数。

判断到平方根就可以的原因是，如果一个数 N 不是质数，那么它必定可以分解成两个数 a 和 b 的乘积，其中 a 和 b 至少有一个小于等于 sqrt(N)。

举个例子，如果要判断 21 是否为质数，可以尝试将 21 分解成两个数的乘积，如 21 = 3 * 7，其中 3 和 7 都小于等于 sqrt(21)。

```cpp
#include <iostream>
#include <cmath>
using namespace std;

bool isPrime(int n) {
    if (n <= 1) {
        return false;
    }
    int sqr = sqrt(n);
    for (int i = 2; i <= sqr; i++) {
        if (n % i == 0) {
            return false;
        }
    }
    return true;
}
```

### 冒泡排序

它重复地遍历过要排序的数列，一次比较两个元素，如果它们的顺序错误就把它们交换过来。遍历数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。

```cpp
#include <iostream>
using namespace std;

// 就是i遍历一次就把最大的送到最后面，然后最大的就不用管了
void bubble_sort(int arr[], int len) {
    for (int i = 0; i < len - 1; ++i) {		// 注意两个遍历的范围 提高效率
        for (int j = 0; j < len - i - 1; ++j) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                // 使用swap函数可以方便地交换两个变量的值，无需使用额外的中间变量。
            }
        }
    }
}

int main() {
    int arr[] = {5, 2, 8, 4, 1, 9};
    int len = sizeof(arr) / sizeof(int);
    bubble_sort(arr, len);

    for (int i = 0; i < len; ++i) {
        cout << arr[i] << " ";
    }

    return 0;
}
```

这段代码定义了一个 `bubble_sort` 函数，它接收两个参数：一个整型数组和该数组的长度。函数内部使用了两个嵌套的循环，每次循环比较相邻的两个元素，如果前一个元素大于后一个元素，就交换它们的位置。经过一轮循环后，最大的元素被交换到了最后一个位置，然后进行下一轮循环。在外层循环中，每次都会把未排序的数组长度减一，因为每次循环后最后一个元素已经排好序了。最终，当所有元素都被排好序后，函数返回。在 `main` 函数中，我们定义了一个整型数组，然后调用 `bubble_sort` 函数进行排序，最后输出排序后的数组。

### 选择排序

选择排序（Selection Sort）是一种简单直观的排序算法，其基本思想是将待排序的序列分为两部分：已排序部分和未排序部分，每次从未排序部分中选择最小的元素放到已排序部分的末尾，直到所有元素都排序完成。其时间复杂度为 O(n^2)，是一种比较低效的排序算法，但是代码简单易懂。

```c++
#include <iostream>
using namespace std;

void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        swap(arr[i], arr[minIndex]);
    }
}
```

### 结构体简单使用

以下是一个示例程序，演示如何定义学生结构体，输入数据并输出学生信息：

```c++
#include <iostream>
#include <string>
using namespace std;

struct Student {
    string name;
    int age;
    double height;
};

int main() {
    Student stu;

    cout << "请输入姓名：";cin >> stu.name;
    cout << "请输入年龄：";cin >> stu.age;
    cout << "请输入身高：";cin >> stu.height;

    cout << "学生信息：" << endl;
    cout << "姓名：" << stu.name << endl;
    cout << "年龄：" << stu.age << endl;
    cout << "身高：" << stu.height << endl;

    return 0;
}
```

### 指针遍历数组

可以使用指针遍历数组，示例代码如下：

```c++
#include <iostream>
using namespace std;

int main() {
    int arr[5] = {1, 2, 3, 4, 5};
    int* ptr = arr;  // 将指针指向数组首元素
    int len = sizeof(arr) / sizeof(int);

    for (int i = 0; i < len; i++) {
        cout << *ptr << " ";  // 输出当前指向的元素
        ptr++;  // 指针加1，指向下一个元素
    }
    cout << endl;

    return 0;
}
```

### 字符指针

可以使用strlen函数获取字符串的长度，需要包含头文件cstring。使用字符指针指向输入的字符串，然后调用strlen函数获取字符串长度即可。代码如下：

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main() {
    const int MAX_SIZE = 100;
    char str[MAX_SIZE];
    cin.getline(str, MAX_SIZE);  // 获取输入字符串

    char * p = str;  // 指针指向字符串
    int len = strlen(p);  // 获取字符串长度

    cout << "字符串长度为：" << len << endl;
    return 0;
}
```

`strlen(p)` 函数是用来计算字符串长度的，它从字符串首地址开始遍历直到遇到字符串结尾的空字符（`'\0'`）为止，然后返回遍历过程中经过的字符个数。因此，在 C/C++ 中，字符串常常以字符数组的形式存储，字符数组的最后一个元素为 `'\0'`，这样可以方便计算字符串长度。因此，如果你用一个字符指针指向了一个以 `'\0'` 结尾的字符串的首地址，那么 `strlen(p)` 就可以直接通过指针获得该字符串的长度。

### 指针截取字符串

指针可以用于截取字符串，通过指针对原字符串进行操作，实现字符串截取的目的。以下是一个示例代码：

```c++
#include <iostream>
using namespace std;

int main() {
    char str[] = "hello world";
    char *p = str + 6; // 指向第7个字符，即w
    *p = '\0'; // 将w替换为字符串结束符
    cout << str << endl; // 输出hello
    return 0;
}
```

这段代码中，指针 `p` 指向 `str` 数组的第7个字符，即字符 `w`，然后将该位置的字符替换为字符串结束符 `\0`，实现了字符串截取的目的。最终输出的结果是 `hello`。

```C++
#include <iostream>
using namespace std;
// 将此字符串中从第 m 个字符开始的剩余全部字符复制成为另一个字符串，并将这个新字符串输出
int main() {
    char str[30] = { 0 };
    cin.getline(str, sizeof(str));

    int m;
    cin >> m;

    char *p=str;
    string s=p+m-1;
    cout<<s<<endl;
    return 0;
}
```

### 动态数组

主要在于先创建 进行操作之后可以使用delete来释放内存

在C++中，可以使用new运算符来动态分配内存并创建动态数组。动态数组的大小可以在运行时决定，而不是在编译时固定。

以下是创建一个包含n个元素的整型动态数组的示例代码：

```C++
int n;
cin >> n;

// 创建动态数组
int* arr = new int[n];

// 对动态数组进行赋值操作
for (int i = 0; i < n; i++) {
    arr[i] = i;
}

// 使用动态数组
for (int i = 0; i < n; i++) {
    cout << arr[i] << " ";
}

// 释放动态数组内存
delete[] arr;
```

在创建动态数组时，我们使用了new运算符来分配内存，并将分配的内存地址赋值给了指针变量arr。然后，我们使用for循环对数组进行赋值操作和输出操作。最后，我们使用delete[]运算符来释放动态数组所占用的内存。

### 二维动态数组

注意二维动态数组分配内存和释放内存的过程

```C++
int rows = 3;
int cols = 4;

// 分配内存
// int** 可以用来指向一个动态分配的二维整数数组。它的声明表示该指针指向一个 int* 类型的指针，也就是指向一维整数数组的指针。
int** array = new int*[rows];
for (int i = 0; i < rows; ++i) {
    array[i] = new int[cols];
}

// 使用数组
array[0][0] = 1;
array[0][1] = 2;
// ...

// 释放内存
for (int i = 0; i < rows; ++i) {
    delete[] array[i];
}
delete[] array;
```

### 类的构造函数

类的构造函数是一种特殊的成员函数，用于创建对象时进行初始化操作。**它具有与类同名的函数名，并且没有返回类型（包括 `void`）**。构造函数在对象创建时自动调用，用于设置对象的初始状态。

构造函数可以有多个重载版本，每个版本可以接受不同的参数，以满足不同的对象初始化需求。

一些常见的构造函数特点包括：

1. 构造函数的名称与类名相同。
2. **构造函数可以有参数，也可以没有参数。**
3. 构造函数可以进行成员变量的初始化。
4. 构造函数可以进行一些额外的初始化工作。
5. 构造函数可以被重载，即可以有多个不同参数列表的构造函数。
6. 如果没有定义任何构造函数，编译器会提供一个默认的无参构造函数。

以下是一个示例，展示了一个名为 `Person` 的类和其构造函数的定义和使用：

```cpp
class Person {
public:
    // 构造函数
    Person(const std::string& name, int age) {
        this->name = name;
        this->age = age;
    }

    // 成员函数
    void introduce() {
        std::cout << "My name is " << name << " and I am " << age << " years old." << std::endl;
    }

private:
    std::string name;
    int age;
};

int main() {
    // 创建对象并调用构造函数进行初始化
    Person person("John", 25);
    // 调用对象的成员函数
    person.introduce();
    return 0;
}
```



### 深拷贝浅拷贝

现有一个 Person 类,成员变量：姓名（string name）和年龄（int age）,请给 Person 添加一个拷贝构造函数，让程序能够正确运行。

```c++
#include <cstring>
#pragma warning(disable : 4996)
using namespace std;

class Person {

    public:
        char* name; // 姓名
        int age;    // 年龄

    // 构造函数用于初始化这两个成员变量，使用了动态内存分配来存储姓名，以避免在对象销毁时出现内存错误。
        Person(const char* name, int age) {
            this->name = new char[strlen(name) + 1];
            strcpy(this->name, name);
            this->age = age;
        }

    /* `strcpy` 是 C/C++ 标准库中的一个字符串操作函数，用于将一个字符串复制到另一个字符串中。

在这段代码中，`strcpy(this->name, name)` 的作用是将传入的 `name` 字符串复制到 `this->name` 所指向的内存位置中。这样做是为了在构造函数中为 `name` 成员变量分配动态内存，并将传入的字符串内容复制到该内存位置，以便在对象的生命周期内使用。

在这个例子中，`this->name` 已经通过 `new` 运算符进行了内存分配，以适应复制的字符串内容。然后，`strcpy(this->name, name)` 将传入的 `name` 字符串复制到 `this->name` 所指向的内存位置中，完成字符串的复制操作。
*/
        // write your code here......
    // 拷贝构造函数使用引用参数 Person &p，表示传入的参数是一个已存在的 Person 对象的引用。通过引用传递参数可以避免在函数调用时进行对象的复制，提高效率。
    // & 用于引用类型的定义，表示参数 p 是一个引用，而不是创建新的对象副本。引用类型允许我们在函数内部直接访问和操作传入的对象，而无需进行对象的复制。
        Person(Person &p)
        {
            this->name=new char[strlen(p.name)+1];
            strcpy(this->name,p.name);
            this->age=p.age;
        }

        void showPerson() {
            cout << name << " " << age << endl;
        }

    // 释放了动态分配的姓名内存，确保在对象销毁时释放资源。
        ~Person() {
            if (name != nullptr) {
                delete[] name;
                name = nullptr;
            }
        }

};
```

### 友元函数

友元函数是一种特殊的函数，它**可以访问类的私有成员和保护成员**。

友元函数**不是类的成员函数，它可以在类的内部或外部声明和定义**。在类中声明一个函数为友元函数时，需要使用 `friend` 关键字。友元函数可以访问类的私有成员、保护成员和公有成员，就像访问自己的成员一样。

友元函数可以用于实现类之间的非成员函数共享数据和操作，提供了灵活性和便利性。但同时，过度使用友元函数可能会破坏类的封装性和数据隐藏原则，

示例，展示了如何在类中声明和定义友元函数：

```cpp
#include <iostream>

class MyClass {
private:
    int data;

public:
    MyClass(int value) : data(value) {}

    friend void friendFunction(const MyClass& obj);
};

void friendFunction(const MyClass& obj) {
    std::cout << "Accessing private data of MyClass: " << obj.data << std::endl;
}

int main() {
    MyClass obj(42);

    friendFunction(obj);  // 调用友元函数

    return 0;
}
```

在上述示例中，我们定义了一个名为 `MyClass` 的类，并在类中声明了一个友元函数 `friendFunction()`。该友元函数可以访问 `MyClass` 类的**私有成员** `data`。

### 友元类

友元类（Friend Class）是指在一个类中将另一个类声明为友元，从而使得被声明的类能够访问友元类的私有成员和保护成员。

友元类的声明可以在类的内部或外部进行。

以下是一个示例，展示了如何在一个类中将另一个类声明为友元类：

```cpp
class FriendClass {
private:
    int privateData;

public:
    FriendClass() : privateData(42) {}

    friend class MyClass;
};

class MyClass {
public:
    void accessFriendClass(const FriendClass& obj) {
        int data = obj.privateData;
        // 可以访问 FriendClass 的私有成员 privateData
    }
};
```

在上述示例中，我们定义了两个类 `FriendClass` 和 `MyClass`。在 `FriendClass` 中将 `MyClass` 声明为友元类。这意味着在 `MyClass` 中可以直接访问 `FriendClass` 的私有成员 `privateData`。在 `MyClass` 中的成员函数 `accessFriendClass()` 中，我们可以使用友元类的对象 `obj` 来访问其私有成员 `privateData`。

需要注意的是，友元关系是单向的，即如果类 A 声明类 B 为友元类，那么类 B 不一定也能访问类 A 的私有成员。友元关系也不具有传递性，即如果类 A 是类 B 的友元类，类 B 是类 C 的友元类，那么类 C 并不是类 A 的友元类。

友元类的使用应该谨慎，因为它破坏了类的封装性和数据隐藏原则。仅在确实需要在类之间共享私有成员时才使用友元类，并且需要慎重考虑其影响和设计合理的访问权限。

### 重载

以下是一个重载加号操作符（+）的例子：

```cpp
#include <iostream>

class MyNumber {
private:
    int value;
public:
    MyNumber(int val) : value(val) {}
    // 重载加号操作符
    MyNumber operator+(const MyNumber& other) {
        int sum = value + other.value;
        return MyNumber(sum);
    }

    int getValue() const {
        return value;
    }
};

int main() {
    MyNumber num1(5);
    MyNumber num2(10);

    MyNumber sum = num1 + num2; // 使用重载的加号操作符进行相加

    std::cout << "Sum: " << sum.getValue() << std::endl;

    return 0;
}
```

在上述例子中，我们定义了一个名为 `MyNumber` 的类，表示一个整数。我们重载了加号操作符（+），使得两个 `MyNumber` 对象可以通过加号进行相加。在 `operator+` 函数中，我们将两个 `MyNumber` 对象的值相加，并返回一个新的 `MyNumber` 对象表示它们的和。

在 `main` 函数中，我们创建了两个 `MyNumber` 对象 `num1` 和 `num2`，然后使用重载的加号操作符将它们相加，将结果保存到 `sum` 中。最后，我们输出了 `sum` 对象的值。

通过重载加号操作符，我们可以实现自定义类型的对象之间的相加操作，使其具有与内置类型相似的行为。

**加号运算法重载**

**输入描述：**

键盘输入两个正整数，分别为小时 h 和分钟 m。要求分钟 m 范围为 0 - 59

**输出描述：**

输出两个 Time 对象（t1 和 t2）相加后的时间结果，通过调用 show() 输出。

```cpp
#include <iostream>
using namespace std;

class Time {
    public:
        int hours;      // 小时
        int minutes;    // 分钟
        Time() {
            hours = 0;
            minutes = 0;
        }

        Time(int h, int m) {
            this->hours = h;
            this->minutes = m;
        }

        void show() {
            cout << hours << " " << minutes << endl;
        }
        // write your code here......
        Time operator+(Time t2){
            int hour=this->hours+t2.hours;
            int min=this->minutes+t2.minutes;
            hour+=min/60;
            min=min%60;
            return Time(hour,min);
        }
};

int main() {
    int h, m;
    cin >> h;
    cin >> m;

    Time t1(h, m);
    Time t2(2, 20);
    Time t3 = t1 + t2;
    t3.show();   
    return 0;
}
```

### **子类中调用父类构造**

```cpp
class Base {

    private:
        int x;
        int y;

    public:
        Base(int x, int y) {
            this->x = x;
            this->y = y;
        }

        int getX() {
            return x;
        }

        int getY() {
            return y;
        }

};

class Sub : public Base {

    private:
        int z;

    public:
        Sub(int x, int y, int z) :Base( x,y ) {
            // write your code here
            this->z=z;
        }

        int getZ() {
            return z;
        }

        int calculate() {
            return Base::getX() * Base::getY() * this->getZ();
        }

};
```

以上是一个名为 `Sub` 的子类继承自 `Base` 的基类。子类 `Sub` 增加了一个私有成员变量 `z`，并在构造函数中进行初始化。构造函数使用了基类 `Base` 的构造函数来初始化基类的成员变量。

子类 `Sub` 还实现了两个成员函数，`getZ()` 和 `calculate()`。`getZ()` 函数返回私有成员变量 `z` 的值，`calculate()` 函数使用基类的 `getX()` 和 `getY()` 函数获取基类的成员变量值，并与子类的 `z` 相乘，返回乘积结果。

通过使用 `public` 关键字继承基类 `Base`，子类 `Sub` 继承了基类的成员变量和成员函数，并可以在子类中访问和使用它们。

这种继承关系使得子类可以重用基类的代码和功能，并且可以在子类中添加新的成员变量和成员函数，以满足特定的需求。

### 多态

```cpp
#include <iostream>
using namespace std;

class BaseCalculator {
    public:
        int m_A;
        int m_B;
        // write your code here......
        virtual int getResult()=0;
    // virtual 多态关键字 
};

// 加法计算器类
class AddCalculator : public BaseCalculator {
    // write your code here......
    int getResult(){
        return m_A+m_B;
    }
};
// 减法计算器类
class SubCalculator : public BaseCalculator {
    // write your code here......
        int getResult(){
        return m_A-m_B;
    }
};

int main() {

    BaseCalculator* cal = new AddCalculator;
    cal->m_A = 10;
    cal->m_B = 20;
    cout << cal->getResult() << endl;
    delete cal;

    cal = new SubCalculator;
    cal->m_A = 20;
    cal->m_B = 10;
    cout << cal->getResult() << endl;
    delete cal;

    return 0;
}
```

在C++中，`virtual` 是一个关键字，用于实现多态性。当基类中的成员函数被声明为 `virtual` 时，它可以在派生类中被重写，并且根据对象的实际类型来调用相应的函数。

以下是使用 `virtual` 的一个示例：

```cpp
class Base {
public:
    virtual void show() {
        cout << "Base class" << endl;
    }
};

class Derived : public Base {
public:
    void show() override {
        cout << "Derived class" << endl;
    }
};

int main() {
    Base* basePtr;
    Derived derivedObj;

    basePtr = &derivedObj;
    basePtr->show(); // 调用派生类的重写函数

    return 0;
}
```

在上述示例中，`Base` 类中的 `show()` 函数被声明为 `virtual`，并在派生类 `Derived` 中被重写。在 `main()` 函数中，我们使用基类指针 `basePtr` 指向派生类对象 `derivedObj`。当调用 `basePtr->show()` 时，由于 `show()` 函数被声明为 `virtual`，所以会根据实际对象的类型来决定调用哪个版本的函数。因此，输出结果为 "Derived class"。

通过使用 `virtual` 关键字，我们可以实现运行时多态性，即根据对象的实际类型来决定调用哪个函数。这对于实现基于继承的多层次的对象行为非常有用，提高了代码的灵活性和可维护性。

## STL

STL是C++标准模板库（Standard Template Library）的简称，它是C++标准库的一部分，提供了一套丰富的通用模板类和函数，用于实现常见的数据结构和算法。STL的设计目标是提供一种通用、高效、可复用的编程工具，以提高C++程序的开发效率和代码质量。

STL包含了多个模块，其中包括容器（Containers）、算法（Algorithms）、迭代器（Iterators）和函数对象（Function Objects）。容器提供了各种数据结构，如向量（vector）、链表（list）、集合（set）、映射（map）等，用于存储和组织数据。算法提供了常见的操作和算法，如排序、查找、遍历等，可以对容器中的数据进行处理和操作。迭代器用于遍历容器中的元素，提供了一种统一的访问容器元素的方式。函数对象是一种可调用的对象，可以在算法中使用，用于指定具体的操作或比较规则。

STL的设计基于泛型编程思想，通过模板技术实现了通用性，使得用户可以方便地使用和扩展库中的功能。STL的优点包括高效性、可复用性、类型安全性和代码简洁性，它极大地提高了C++程序的开发效率，并成为C++程序员不可或缺的工具之一。

### 迭代器

**迭代器遍历容器**

- 正向： vector::iterator it;

  begin()指向第一个元素

  end()指向最后一个元素的后一个位置

  for(it=vc.begin();it!=vc.end();it++)

- 反向： vector::reverse_iterator itr;

  for(it=vc.rbegin();it!=vc.rend();it++)

  rbegin()指向最后一个元素

  rend()指向第一个元素的前一个位置

```cpp
// write your code here......
#include<vector>
using namespace std;

int main() {
    // write your code here......
    vector<int> vc(5);
    for(int i=0;i<5;i++)
        cin>>vc[i];
    vector<int>::iterator it; 
    for(it = vc.begin();it!=vc.end();it++)
    {
        cout<< *it<<" ";
    }
    cout<<endl;
    vector<int>::reverse_iterator it1;
    for(it1 = vc.rbegin(); it1 != vc.rend();it1++)
    {
        cout<<*it1<<" ";
    }

    return 0;
}
```

**迭代器遍历set**

在C++中，`set`是一种关联容器，它用于存储一组唯一的元素，并按照一定的排序规则进行排序。由于`set`中的元素是唯一的，因此不允许进行赋值操作来修改元素的值。

当你尝试修改`set`中的元素时，编译器会发出错误，因为`set`的设计目的是为了维护元素的有序性和唯一性。如果你想修改`set`中的元素，你需要先从`set`中删除该元素，然后再插入一个新的元素来替代它。这样做可以保持`set`的有序性和唯一性。

例如，假设我们有一个存储整数的`set`对象`mySet`，如果我们想将某个元素修改为新的值，可以按照以下步骤进行：

```cpp
// 假设要将元素3修改为新值6
mySet.erase(3);  // 从set中删除旧值3
mySet.insert(6); // 插入新值6
```

通过先删除旧值，再插入新值的方式，我们可以在`set`中更新元素的值。

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
	set<int>s;
	// write your code here......
	for(int i=0;i<5;i++)
    {
        int temp=0;
        cin>>temp;
        s.insert(temp);//插入元素
    }
    for(set<int>:: iterator it=s.begin();it!=s.end();it++)
    {
        cout<<*it<<" ";
    }
    
	return 0;
}
```



### STL序列式容器

**deque**

```cpp
#include <iostream>
#include <deque>
using namespace std;

class Guest {
public:
    string name;
    bool vip;

    Guest(string name, bool vip) {
        this->name = name;
        this->vip = vip;
    }
};

int main() {
    Guest guest1("张三", false);
    Guest guest2("李四", false);
    Guest vipGuest("王五", true);
    deque<Guest> deque;

    // write your code here......
    deque.push_back(guest1);
    deque.push_back(guest2);
    deque.push_front(vipGuest);

    for (Guest g : deque) {
        cout << g.name << " ";
    }
    return 0;
}
```

**最后k个元素**

```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
    int n, k;
    vector<int>a;
    // write your code here......
    //重点在vector的索引，如何给vector分配内存？new,不能用下标建立索引！最好用push_back
    cin >> n >> k;
//     vector<int> a new vector<int>;
    int x;
    for (int i = 0; i < n; ++i) {
        cin >> x;
        a.push_back(x);
//         a[i] = x;//错误！
    }
//     a.end()是最后一个元素还是最后一个元素的后一个元素？最后一个元素的后一个元素
//     for (auto iter = a.end() - 1; iter != a.end() - k - 1; --iter) {
//         cout << *iter << " ";
//     }
    for(int i = n-1; i>=n-k; --i){
        cout<<a[i]<<" ";
    }
    return 0;
}
```



### 关联式容器

**#include < set>**

```cpp
#include <iostream>
#include <set>

using namespace std;

int main() {

    char str[100] = { 0 };
    cin.getline(str, sizeof(str));

    // 利用set去除字符串中重复的字符
    set<char> s;
    int i;
    //将字符串添加进s中
    while(str[i]){
        s.insert(str[i]);
        i++;
    }
    //迭代器遍历输出
    for(auto iter = s.begin(); iter != s.end(); iter++){	// auto自动推断
        cout << *iter;
    }
    return 0;
}
```

**#include <map>**

在C++中，`std::map`是关联容器的一种，它是基于红黑树实现的有序映射。`std::map`提供了一种键值对的存储方式，其中每个键都唯一且按照特定的排序规则进行排序。

与哈希表不同，`std::map`使用红黑树作为底层数据结构来实现，而不是哈希表。红黑树是一种自平衡的二叉搜索树，它具有良好的平衡性能，能够在插入、删除和查找等操作上保持较稳定的性能。

在`std::map`中，键值对按照键的大小顺序进行排序，并且每个键都是唯一的。这使得`std::map`非常适合于需要按照键进行排序和查找的场景。如果需要快速的插入和查找，可以考虑使用`std::unordered_map`，它是基于哈希表实现的无序映射。

总结起来，`std::map`是一种有序映射容器，使用红黑树实现，而不是哈希表。

```cpp
#include <iostream>
#include<map>
using namespace std;
int main() {
     char str[100] = { 0 };
     cin.getline(str, sizeof(str));  
         map<char,int>maps;          //改变了原来key和value的类型和顺序
         for(int i=0;str[i]!='\0';i++){  
     if(isalpha(str[i]))//判断是否是字符
     {
         maps[str[i]]++;
         }
 }   
    for(auto it=maps.begin();it!=maps.end();it++)
    {
        cout<< it->first <<":"<< it->second <<endl;
        // it->first 表示获取迭代器指向的键，it->second 表示获取迭代器指向的值。
    }    
    return 0;
}
```

**返回第一个大于x的元素对应的迭代器指针**

```cpp
set<int>s;
auto it=s.upper_bound(x);
```



### 算法

**排序算法**

**遍历算法**

```cpp
#include <iostream>
#include <vector>
#include <algorithm> //使用STL排序算法
using namespace std;

int main() {
    int num;
    vector<int> v;
    for (int i = 0; i < 5; i++) {
        cin >> num;
        v.push_back(num);
    }

    // 使用 STL 排序算法对元素进行排序（从大到小）
    //使用sort和reverse
    sort (v.begin(),v.end());
    reverse (v.begin(),v.end());
    // auto n:v
    for (auto n:v){
        cout << n << ' ';
    }
    return 0;
}
```

## 函数

### 1

编写一个函数，实现两个整数的交换，要求采用指针的方式实现。

```cpp
void swapIntegers(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}
int main() {
    int num1 = 5;
    int num2 = 10;
    std::cout << "Before swapping: num1 = " << num1 << ", num2 = " << num2 << std::endl;
    // 注意参数传入的方式：
    swapIntegers(&num1, &num2);
    std::cout << "After swapping: num1 = " << num1 << ", num2 = " << num2 << std::endl;
    return 0;
}
```

在函数调用 `swapIntegers(&num1, &num2);` 中，使用取地址运算符 `&` 可以获取变量 `num1` 和 `num2` 的内存地址。这些地址作为参数传递给 `swapIntegers` 函数，使函数能够操作这些地址所对应的变量。

函数 `swapIntegers` 的参数类型是指针类型 `int*`，它接收两个指向整数的指针作为参数。通过传递 `&num1` 和 `&num2`，实际上将 `num1` 和 `num2` 的地址传递给了 `swapIntegers` 函数。

因此，通过将变量地址作为参数传递给函数，并在函数内部通过指针操作变量，可以实现变量的交换操作。

### find()

在 C++ 中，`find()` 函数用于在容器（如字符串、向量、列表、集合等）中查找特定元素的位置。它返回一个迭代器，指向找到的元素，如果未找到，则返回容器的结束迭代器。

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    // 在向量中查找元素 3
    auto it = std::find(numbers.begin(), numbers.end(), 3);
    // 需要注意的是，numbers.end() 返回的迭代器指向的是容器的尾后位置，而不是最后一个元素。因此，在使用算法函数时，通常将 end() 迭代器作为查找操作的终止条件，表示已经遍历完容器的所有元素。

    // 检查是否找到
    if (it != numbers.end()) {
        std::cout << "元素 3 在向量中的位置：" << std::distance(numbers.begin(), it) << std::endl;
    } else {
        std::cout << "元素 3 未找到" << std::endl;
    }

    return 0;
}
```

在上面的示例中，我们使用 `std::find()` 函数在整数向量 `numbers` 中查找元素 3。如果找到了元素，则输出其位置；如果未找到，则输出未找到的消息。

请注意，`std::find()` 函数需要包含 `<algorithm>` 头文件。它是 C++ 标准库中的算法之一，可以应用于各种容器类型。根据需要，你可以将其应用于不同的容器和元素类型。

### 2

寻找子字符串出现的次数

```cpp
#include <iostream>
#include <cstring>
using namespace std;

int main() {

    char str[100] = { 0 };
    char substr[100] = { 0 };

    cin.getline(str, sizeof(str));
    cin.getline(substr, sizeof(substr));

    int count = 0;

    //转化为字符串
    string str1(str);
    string str2(substr);
    
    int i=0;
    //从str1下标i开始查找str2，注意
    while(str1.find(str2,i)!=-1){
        //如果找得到，计数加1
        count++;
        //i从找到的位置，后移一位
        i=str1.find(str2,i)+1;
    }

    cout << count << endl;

    return 0;
}
```

`str1.find(str2, i)` 用于在字符串 `str1` 中从位置 `i` 开始查找子字符串 `str2`。如果找到了子字符串，则返回子字符串的起始位置；如果找不到，则返回 `string::npos`，即一个特殊的无效位置标识。

在C++中，`string::find()` 函数如果找不到指定的子字符串，会返回 `string::npos`，而不是返回 `-1`。

但是-1也过了，还是写string::npos吧。

### strlen()

在C++中，函数 `strlen()` 是用于计算字符串的长度的，但是它是针对以空字符（`'\0'`）结尾的字符数组（C风格字符串）的。

如果你的字符串 `str` 是 `std::string` 类型的对象，应该使用 `str.length()` 或 `str.size()` 来获取字符串的长度，而不是使用 `strlen()` 函数。

如果你的字符串 `str` 是以空字符结尾的字符数组（C风格字符串），则可以使用 `strlen()` 函数计算其长度。但需要确保你已经包含了正确的头文件 `<cstring>`，其中包含了 `strlen()` 函数的声明。

请检查你的代码，确保你使用了正确的字符串类型和相关的头文件。如果问题仍然存在，请提供更多的代码和错误信息，以便我能够帮助你更准确地解决问题。

### 3 引用

就是加上了引用符号然后正常处理

引用的方式交换两个数

```cpp
#include <iostream>
using namespace std;
void swap(int &m, int &n){ //交换两个整数的函数，用引用
    int temp = m; //交换两个数
    m = n;
    n = temp;
}
int main() {
	int m, n;
	cin >> m;
	cin >> n;
    // 调用的时候不需要&引用符号
    swap(m, n); //函数交换
	cout << m << " " << n << endl;
	return 0;
}
```

引用参数是一种函数参数传递的机制，它允许函数直接访问并修改传递给它的变量，而无需通过指针或副本进行操作。通过引用参数，可以在函数内部修改传入的变量，并使得修改在函数外部也可见。

在C++中，引用参数使用引用符号 `&` 来声明。通过将变量作为引用参数传递给函数，函数可以直接访问并修改原始变量的值，而不是对变量进行复制或创建副本。

引用参数的优点包括：

1. 避免了在函数调用时创建副本，提高了程序的效率。
2. 允许函数修改原始变量的值，使得函数的影响在函数外部可见。
3. 代码更加简洁和易读，不需要使用指针操作或返回值来实现修改。

以下是一个使用引用参数的示例：

```cpp
void increment(int& num) {
    num++; // 修改传入的变量的值
}

int main() {
    int num = 10;

    increment(num);

    cout << num; // 输出 11

    return 0;
}
```

在上述示例中，`increment` 函数接受一个整数的引用参数 `num`，并将其值加一。通过在 `main` 函数中调用 `increment(num)`，函数直接修改了 `num` 变量的值，使得修改在 `main` 函数中可见，输出结果为 11。

### 打印乘法表

```cpp
#include <iostream>
using namespace std;

int main() {
    
    int n;
    cin >> n;

    //i表示乘法表行数.
    for(int i=1;i<=n;i++){
        //j表示乘法表列数
        for(int j=1;j<=i;j++){
            //按对应格式输出乘法表
            cout<<j<<" * "<<i<<" = "<<j*i<<"    ";
        }
        //每打印完一行，进行换行
        cout<<endl;
    }

    return 0;
}
```

