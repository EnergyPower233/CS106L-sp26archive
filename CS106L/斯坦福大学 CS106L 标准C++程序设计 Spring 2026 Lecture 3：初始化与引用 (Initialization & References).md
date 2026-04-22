---
theme: custom-1776750089243-uatfgcpy9
themeName: "Main"
title: "CS106L Spring 2026 Lecture 3：初始化与引用 (Initialization & References)"
---

本文内容系基于斯坦福大学 **CS106L: Standard C++ Programming** 课程官方网站公开资源整理而成。课程官网：https://web.stanford.edu/class/archive/cs/cs106l/cs106l.1266/

---

斯坦福大学的 CS106L 教学内容始终紧随 C++ 标准的演进步伐，走在教学前沿。今年课程已融入部分 **C++26** 的新特性。然而，由于课程自 2020 年起不再提供公开的 Lecture 视频录像，目前网络上唯一可获取的最新官方学习资料仅限于 **Slide (幻灯片)** 与 **课程作业代码**，并无完整的文字讲义。

为方便广大 C++ 学习者参考，笔者依据该课程最新的 Slide 内容，逐页对照、整理归纳成此份“讲义”，以填补视频资料缺失的空白。文中对专业术语与关键句子均保留了中英双语对照，力求还原课堂原貌。

**免责声明：**

1. **版权归属**：本整理内容引用的所有课程资源版权归斯坦福大学及原授课教师所有。本文仅供学习交流，严禁用于任何商业用途。
    
2. **非官方性质**：本文并非斯坦福官方发布的讲义或教材，而是由第三方学习者基于公开 Slide 的理解与汇编。若官方后续发布正规讲义，请以官方版本为准。
    
3. **理解偏差**：由于缺乏实际的课堂录音与讲解，文中对 Slide 逻辑的串联、术语的解释可能存在笔者个人的理解偏差。如发现错漏之处，欢迎指正交流
---# Slide

**讲师：** Preston Seay, Rachel Fernandez

---

### 前情回顾 (Recap)

- **`auto` 关键字**
    - **含义：** "嘿编译器，你自己把这个类型推断出来吧。" ("Hey compiler, figure out this type.")
    - **用法：** 由你自行斟酌使用 (Use at your discretion)。
    - **适用场景：** 当类型名称非常繁琐时特别有用 (Helpful when type is annoying)。
    
    **示例：简化复杂迭代器类型**
    ```cpp
    #include <iostream>
    #include <string>
    #include <map>
    #include <unordered_map>
    #include <vector>
    
    int main() {
        std::map<std::string, std::vector<std::pair<int, std::unordered_map<char, double>>>> complexType;
        // 令人困惑的迭代器类型 (我们会在迭代器讲座中详细讲解！)
        // confusing iterator type (we'll find out what this is in the iterators lecture!)
        std::map<std::string, std::vector<std::pair<int, std::unordered_map<char, double>>>>::iterator it = complexType.begin();
        
        // 清晰多了的迭代器类型！
        // clear(er) iterator type!
        auto it_auto = complexType.begin();
        return 0;
    }
    ```

- **`struct` 关键字**
    - **含义：** "请把这些变量组合在一起，形成一个自定义类型，谢谢 😂。" ("Group these variables together, in one type, please 😂.")

---

### 本讲大纲 (Agenda)

1.  **初始化 (Initialization)**
2.  **引用 (References)**
3.  **左值 与 右值 (L-values vs R-values)**
4.  **`const` 关键字**
5.  **编译 C++ 程序 (Compiling C++ programs)**

---

### 1. 初始化 (Initialization)

#### 什么是初始化？(What it is...)
**定义：** "在对象构造时为其提供初始值。" ("Provides initial values at the time of construction.")
*参考：C++ Reference Definition*

#### 如何进行初始化？(How to...)
主要有三种方式：
1. 直接初始化 (Direct initialization)
2. 统一初始化 (Uniform initialization)
3. 结构化绑定 (Structured Binding)

---

#### (1) 直接初始化 (Direct Initialization)

直接初始化允许隐式类型转换，但这可能会带来风险。

**代码示例：窄化转换 (Narrowing Conversion)**
```cpp
#include <iostream>

int main() {
    int numOne = 12.0;      // OK，但发生隐式转换
    int numTwo(12.0);       // 直接初始化，同样 OK
    std::cout << "numOne is: " << numOne << std::endl; // 输出 12
    std::cout << "numTwo is: " << numTwo << std::endl; // 输出 12
    return 0;
}
// 12.0 不是 int... 这能行吗？ YES
```

**窄化转换的潜在问题：**
编译器并不关心这种精度丢失，它会默认执行窄化转换 (Narrowing Conversion)。
*C++ does not care: "You want 100.8 to be an integer? Okay - compiler."*

```cpp
#include <iostream>

void checkCool(float temperature) {
    if (temperature > 100.0) {
        std::cout << "Emergency cooling activated!" << std::endl;
    } else {
        std::cout << "Temperature normal. No emergency cooling required.";
    }
}

int main() {
    float temperatureReading(100.8);
    // 直接初始化导致 100.8 被截断为 100，可能引发逻辑错误
    int temperature = temperatureReading; 
    checkCool(temperature); // 本应触发紧急冷却，但实际不会
    return 0;
}
```

---

#### (2) 统一初始化 (Uniform Initialization)

**特点：** 注意这里使用的是**花括号 `{}`**！
**安全性：** 禁止窄化转换。

```cpp
#include <iostream>

int main() {
    int numOne = {12.0}; // 编译错误！窄化转换不被允许
    int numTwo{12.0};    // 编译错误！
    std::cout << "numOne is: " << numOne << std::endl;
    std::cout << "numTwo is: " << numTwo << std::endl;
    return 0;
}
// 12.0 不是 int... 这能行吗？ NO
```

**统一初始化的好处 (Benefits):**
1.  **安全 (Safe):** 禁止窄化转换。
2.  **通用 (Ubiquitous):** 可用于向量、映射、自定义类等所有场景。

**应用于 `std::map` 示例：**
```cpp
#include <iostream>
#include <map>

int main() {
    // 使用统一初始化 map
    std::map<std::string, int> ages{
        {"Alice", 25},
        {"Bob", 30},
        {"Charlie", 35}
    };
    // 访问 map 元素
    std::cout << "Alice's age: " << ages["Alice"] << std::endl;
    std::cout << "Bob's age: " << ages.at("Bob") << std::endl;
    return 0;
}
```

**应用于 `std::vector` 示例：**
```cpp
#include <iostream>
#include <vector>

int main() {
    // 使用统一初始化 vector
    std::vector<int> numbers{1, 2, 3, 4, 5};
    // 访问 vector 元素
    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    return 0;
}
```

**对比：传统初始化 vs 统一初始化 (自定义结构体)**
```cpp
// 传统写法
StanfordID issueID() {
    StanfordID id;
    id.name = "THE Stanford Tree";
    id.sunet = "theTREE";
    id.idNumber = 0000002;
    return id;
}

// 统一初始化写法 —— 更简洁
StanfordID issueID() {
    StanfordID id = {"THE Stanford Tree", "theTREE", 0000002};
    return id;
}
```

---

#### (3) 结构化绑定 (Structured Binding)

**定义：** 从固定大小的数据结构中一次性初始化多个变量。用于访问函数返回的多个值。**大小必须在编译时已知。**

**代码示例：解包元组 (Tuple)**
```cpp
#include <iostream>
#include <tuple>
#include <string>

std::tuple<std::string, std::string, std::string> getClassInfo() {
    std::string className = "CS106L";
    std::string buildingName = "Thornton 110";
    std::string language = "C++";
    // 这里返回时使用了什么语法？ 统一初始化 (Uniform initialization)
    return {className, buildingName, language};
}

int main() {
    // 结构化绑定：自动将元组成员赋值给三个变量
    auto [className, buildingName, language] = getClassInfo();
    std::cout << "Come to " << buildingName << " and join us for " << className
              << " to learn " << language << "!" << std::endl;
    return 0;
}
```

**对比：有无结构化绑定的区别**
```cpp
// 繁琐的旧方法
auto classInfo = getClassName();
std::string className = std::get<0>(classInfo);
std::string buildingName = std::get<1>(classInfo);
std::string language = std::get<2>(classInfo);

// 简洁的结构化绑定
auto [className, buildingName, language] = getClassName();
```

---

### 2. 引用 (References)

#### 什么是引用？(What they are...)
**定义：** "已存在的对象或函数的别名 (Alias)。"
*参考：C++ Reference Definition*

#### 引用示例 (References Example)

```cpp
#include <iostream>

int main() {
    int miToMoon = 238855; // 地球到月球的距离 (英里)
    std::cout << "Moon is " << miToMoon << "mi away." << std::endl;
    
    // ISS 是 miToMoon 的引用 (别名)
    int& ISS = miToMoon;
    ISS -= 254; // 国际空间站轨道高度 (ISS orbital height)
    
    std::cout << "ISS is " << ISS << "mi to moon." << std::endl;
    std::cout << "Moon is " << miToMoon << "mi away." << std::endl;
    return 0;
}
```

**内存视角分析：**
1. `int miToMoon = 238855;` -> 内存中有一个名为 `miToMoon` 的变量，值为 `238855`。
2. `int& ISS = miToMoon;` -> `ISS` 成为 `miToMoon` 的别名，它们指向**同一块内存地址**。
3. `ISS -= 254;` -> 修改 `ISS` 即是修改 `miToMoon`，最终值变为 `238601`。

---

#### 引用传递 (Pass by Reference)

通过引用传递参数，函数内部可以直接修改外部变量，且不会发生拷贝开销。

```cpp
#include <iostream>
#include <math.h>

void squareN(int& n) {
    n = pow(n, 2);
}

int main() {
    int num = 5;
    squareN(num);
    std::cout << num << std::endl; // num 被更新为 25
    return 0;
}
```

#### 对比：值传递 vs 引用传递 (Value vs Reference)

| 值传递 (Pass by Value) | 引用传递 (Pass by Reference) |
| :--- | :--- |
| `void squareN(int n)` | `void squareN(int& n)` |
| **拷贝**了变量！(开销可能很大) | 使用**同一个**变量及内存！(可以直接修改) |
| `n = pow(n, 2);` 仅修改副本 | `n = pow(n, 2);` 修改原始数据 |

---

#### 经典的引用拷贝 Bug (A Classic Reference Copy Bug)

**Bug 代码：**
```cpp
#include <iostream>
#include <math.h>
#include <vector>

void shift(std::vector<std::pair<int, int>> &nums) {
    // 虽然 nums 是引用传递，但循环中的 auto 没有使用引用！
    // 这里发生了 Structured Binding 的拷贝！
    for (auto [num1, num2] : nums) {
        num1++; // 仅仅修改了拷贝出来的副本
        num2++; 
    }
    // 结果：LOTS OF NOTHING DONE. 原始 nums 毫无变化。
}
```

**修复后的代码：**
```cpp
#include <iostream>
#include <math.h>
#include <vector>

void shift(std::vector<std::pair<int, int>> &nums) {
    // 注意这里的关键符号 '&' 
    // auto& 使得结构绑定绑定的是引用，而非副本
    for (auto& [num1, num2] : nums) {
        num1++; // 现在修改的是 nums 内部的值
        num2++; 
    }
    // Fixed!
}
```

---

### 3. 左值 与 右值 (L-values vs R-values)

**直观理解：Yay or nay?**
- `int x = 5;` ✅ "变量" 可以出现在等号左边或右边。
- `int y = x;` ✅ "值" 只能出现在等号右边。
- `int 5 = x;` ❌ "值" 不能出现在等号左边。

**概念对比表：**

| 术语 | L-value | R-value |
| :--- | :--- | :--- |
| **全称** | Locator Value | Read Value |
| **相对于等号位置** | 左边或右边 (left or right) | 右边 (right) |
| **内存** | 拥有内存地址 (Has a memory address) | 临时值，无内存地址 (Temporary value) |
| **示例** | `int x = 10;` 中的 `x` | `int x = 10;` 中的 `10` |
| | `int y = x;` 中的 `x` 或 `y` | |

**引用与 L-value 的关联：**
```cpp
void squareN(int& n) { // 注意 & 符号
    n = pow(n, 2);
}
```
- **`&` / 引用** 意味着参数必须是一个非临时值，即 **L-value**。
- 因此，调用 `squareN(num)` 时，`num` 必须是 L-value。

---

### 4. `const` 关键字

#### 什么是 const？(What it is...)
**定义：** "此类对象不可被修改。" ("Such object cannot be modified.")
*参考：C++ Reference Definition*

**代码示例：哪些操作是合法的？**
```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> vec{ 1, 2, 3 };            // 普通 vector
    const std::vector<int> const_vec{ 1, 2, 3 }; // 常量 vector
    std::vector<int>& ref_vec( vec );           // 普通引用
    const std::vector<int>& const_ref{ vec };   // 常量引用

    vec.push_back(3);        // ✅ OK
    const_vec.push_back(3);  // ❌ 编译错误！const 对象不能被修改
    ref_vec.push_back(3);    // ✅ OK
    const_ref.push_back(3);  // ❌ 编译错误！const 引用指向的对象不可通过该引用修改
    return 0;
}
```

#### 常量与引用 (Const & Reference)

**错误示例：**
```cpp
int main() {
    const int a = 5;   // a 是只读的
    int& b = a;        // ❌ 错误！不能用普通引用绑定 const 对象 (这会失去 const 保护)
    b++;               // 如果允许，就能修改 const 变量了，这不合逻辑
    return 0;
}
```

**正确示例：**
```cpp
int main() {
    const int a = 5;
    const int& b = a;  // ✅ 正确：用 const 引用绑定 const 对象
    // b++;            // 尝试修改 b 会导致编译错误，保护了 a 的值
    std::cout << a << std::endl;
    return 0;
}
```

---

### 5. 编译 C++ 程序 (Compiling C++ programs)

**编译流程概览：**
- 源代码 (`source.cpp`) -> **编译器 (Compiler)** -> 可执行机器码 (`machine.exe`)

**重要概念：**
- C++ 是一门**编译型语言 (Compiled)**。
- 它不能像 Python 那样直接被“解释 (Interpreted)”或运行。
- C++ 需要编译器 (Compiler)，将源代码转换为二进制程序。
- 流行的编译器有 `clang` 和 `g++`。

**编译命令示例：**
```bash
g++ --std=c++23 main.cpp -o main
```
- `g++` : 编译器程序。
- `--std=c++23` : 指定使用 C++ 23 标准。
- `main.cpp` : 输入的源文件。
- `-o main` : 指定输出文件名为 `main`。

**运行程序：**
```bash
# Linux / Mac
$ ./main

# Windows
$ ./main.exe
```

**补充背景知识：C++ 在实际工业中的应用**
> *幻灯片第 54 页提及：TensorFlow 是一个端到端的开源机器学习平台。其核心部分主要使用 C++ 编写，包含 **2000+** 个源文件。掌握 C++ 的编译与性能优化对于大型系统开发至关重要。*

---

### 本讲总结 (Recap)

1.  **尽量使用统一初始化 (Uniform initialization)!** —— 安全且通用。
2.  **引用 (References)** 可以为变量创建别名，实现零拷贝访问。
3.  普通引用只能绑定 **L-values**。
4.  **`const`** 关键字确保变量在初始化后不可被修改，增强代码安全性。

---

# Initialization & References Lecture Code

## Overview

In this folder you'll find that there is code for the code that was in the lecture slides for your perusal. We encourage you to take a look at this and play around with the code, try to break it, etc. This is where the real learning really happens.

## To Compile

Not to make you fetch-quest but take a look at the lecture for information on how to do this, it should be relatively simple. We really want you to get comfortable with compiling C++ programs from the terminal/command line.

[![](https://github.com/cs106l/cs106l-lecture-code/raw/main/lecture03/meme.jpeg)](https://github.com/cs106l/cs106l-lecture-code/blob/main/lecture03/meme.jpeg)


## Reactor.cpp

```cpp
#include <iostream>


/*
 * I don't know whether or not Reactor code is written in C++,
 * but I wouldn't be surprised. Oftentimes these systems are controlled
 * using industrial PLCs, which may run C++ code. The reason I'm telling
 * you this is because oftentimes these narrowing conversions
 * seem trivial, but they're actually quite consequential :)
 */
class Reactor {
public:
	// this is called a Constructor, more on this later in the course
	Reactor(double temperature): temperature(temperature) {}

	void checkCool() {
		if (temperature > 100.0) {
			std::cout << "Emergency cooling!" << std::endl;
		}
		else {
			std::cout << "Temperature is normal. No emergency cooling required" << std::endl;
		}
	}


private:
	double temperature;
};


int main () {
	// narrowing conversion, saving 100.8 into an integer, see type on line 32
	int criticalTemperature(100.8);
	Reactor reactor(criticalTemperature);
	reactor.checkCool();
	return 0;
}
```

## const.cpp

```cpp
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> vec{ 1, 2, 3 };  /// a normal vector
    const std::vector<int> const_vec{ 1, 2, 3 };  /// a const vector
    std::vector<int>& ref_vec{ vec };  /// a reference to 'vec'
    const std::vector<int>& const_ref{ vec };  /// a const reference

    vec.push_back(3);
    const_vec.push_back(3);
    ref_vec.push_back(3);
    const_ref.push_back(3);

    return 0;
}

```

## initialization.cpp

```cpp
#include <iostream>

int main() {
	// Uniform Initialization, will fail
	int numOne{12.0};
	float numTwo{12.0};

	std::cout << "numOne is: " << numOne << std::endl;
	std::cout << "numTwo is: " << numTwo << std::endl;
	return 0;
}
```

## reference.cpp

```cpp
#include <iostream>
#include <math.h>

#define WITH_REF 1

/*
 * This is called a preprocessor macro, in the compilation step
 * the preprocessor, will decide which one of the function
 * signatures to keep depending on whether or not the variable
 * 'WITH_REF' is truthy. 
 */
#if WITH_REF
void squareN(int& n) 
#else
void squareN(int n)
#endif
{
	n = std::pow(n, 2);
}


int main() {
	int num = 5;
	std::cout << "(1) num is: " << num << std::endl;
	squareN(5);
	std::cout << "(2) num is " << num << std::endl;
	return 0;
}

```


