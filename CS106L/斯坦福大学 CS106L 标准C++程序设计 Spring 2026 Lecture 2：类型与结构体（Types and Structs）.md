---
theme: custom-1776750089243-uatfgcpy9
themeName: "Main"
title: "CS106L Spring 2026 Lecture 2：类型与结构体（Types and Structs）"
---

本文内容系基于斯坦福大学 **CS106L: Standard C++ Programming** 课程官方网站公开资源整理而成。课程官网：https://web.stanford.edu/class/archive/cs/cs106l/cs106l.1266/

---

斯坦福大学的 CS106L 教学内容始终紧随 C++ 标准的演进步伐，走在教学前沿。今年课程已融入部分 **C++26** 的新特性。然而，由于课程自 2020 年起不再提供公开的 Lecture 视频录像，目前网络上唯一可获取的最新官方学习资料仅限于 **Slide (幻灯片)** 与 **课程作业代码**，并无完整的文字讲义。

为方便广大 C++ 学习者参考，笔者依据该课程最新的 Slide 内容，逐页对照、整理归纳成此份“讲义”，以填补视频资料缺失的空白。文中对专业术语与关键句子均保留了中英双语对照，力求还原课堂原貌。

**免责声明：**

1. **版权归属**：本整理内容引用的所有课程资源版权归斯坦福大学及原授课教师所有。本文仅供学习交流，严禁用于任何商业用途。
    
2. **非官方性质**：本文并非斯坦福官方发布的讲义或教材，而是由第三方学习者基于公开 Slide 的理解与汇编。若官方后续发布正规讲义，请以官方版本为准。
    
3. **理解偏差**：由于缺乏实际的课堂录音与讲解，文中对 Slide 逻辑的串联、术语的解释可能存在笔者个人的理解偏差。如发现错漏之处，欢迎指正交流

---
**讲师**：Rachel Fernandez & Preston Seay  
**课程网站**：[cs106l.stanford.edu](http://cs106l.stanford.edu)

---
# Slide(PPT)
## 1. 编译器 vs 解释器  
### Compiler vs Interpreter

### 1.1 解释型语言（Interpreted Languages）

**核心思想（THE BIG IDEA）**：  
> The interpreter reads each line of code line-by-line, translates each line, and then executes it.

解释器逐行读取源代码，每读一行就翻译一行并立即执行。

```
源代码 → 解释器（逐行翻译执行） → 输出
```

**示例**：Python、JavaScript
![[Pasted image 20260421095836.png]]

---

### 1.2 编译型语言（Compiled Languages）

**核心思想（THE BIG IDEA）**：  
> The compiler translates the ENTIRE program, packages it into an executable file, and then executes it.

编译器将整个程序一次性翻译成机器码，打包成可执行文件，之后再运行该文件。

```
源代码 → 编译器 → 机器码（可执行文件） → 运行可执行文件 → 输出
```

**C++ 正是一种编译型语言（C++ is a compiled language）**。
![[Pasted image 20260421095849.png]]

---

## 2. 编译时与运行时  
### Compile Time vs Run Time

| 阶段      | 解释型语言      | 编译型语言            |
| ------- | ---------- | ---------------- |
| **编译时** | ❌ 无独立的编译阶段 | ✅ 此时进行语法检查、类型检查等 |
| **运行时** | 错误在运行过程中暴露 | 程序已编译完成，直接执行     |
![[Pasted image 20260421095925.png]]
![[Pasted image 20260421095940.png]]
### 2.1 错误发生时机对比

**Python（解释型 + 动态类型）**：
```python
print("Running...")
hello = "Hello "
world = "World!"
print(hello * world)   # 运行时错误（Runtime Error）
```
输出：
```
Running...
TypeError: can't multiply sequence by non-int of type 'str'
```

**C++（编译型 + 静态类型）**：
```cpp
int main() {
    std::cout << "Running..." << std::endl;
    std::string hello = "Hello ";
    std::string world = "World!";
    std::cout << hello * world << std::endl;   // 编译时错误（Compile Time Error）
    return 0;
}
```
编译时输出：
```
error: no match for 'operator*' (operand types are 'std::string' and 'std::string')
```

**核心思想（🧠 THE BIG IDEA 🧠）**：  
> This error occurs during compile time! When we are translating, we see that we try to multiply two strings and the compiler goes “hey that’s not allowed!!”
>该错误发生在编译期间！当编译器在翻译代码时，发现你试图将两个字符串相乘，它会直接告诉你：“嘿，这是不允许的！！”。

---

## 3. 类型系统（Type System）

### 3.1 C++ 内置基本类型

| 类型 | 示例 | 说明 |
|------|------|------|
| `int` | `106` | 整数 |
| `double` | `71.4` | 双精度浮点数 |
| `string` | `"Welcome to CS106L!"` | 字符串（需 `#include <string>`） |
| `bool` | `true` / `false` | 布尔值 |
| `size_t` | `12` | 无符号整数，常用于表示大小 |

> **Types are super important!!** —— 类型极其重要！！

---

### 3.2 动态类型（Dynamic Typing） vs 静态类型（Static Typing）

#### 动态类型（以 Python 为例）

```python
a = 3
b = "test"

def foo(c):
    d = 106
    d = "hello world!"   # 完全合法，变量类型可随时改变
```

> The interpreter assigns variables a type at runtime based on the variable's value at that time.

解释器在运行时根据变量当前的值来赋予其类型。变量类型可以动态改变。

#### 静态类型（以 C++ 为例）

```cpp
int a = 3;
string b = "test";

void foo(string c) {
    int d = 106;
    d = "hello world!";   // ❌ 编译错误！类型一旦声明不可更改
}
```

> Every variable must declare a type. Once declared, the type cannot change.

每个变量必须声明类型。一旦声明，类型就不能改变。

---

### 3.3 为什么选择静态类型？

- **更高效（More efficient）**
- **更易于理解和推理（Easier to understand and reason about）**
- **更好的错误检查（Better error checking）**

#### 错误检查示例

**Python**：  
```python
def add_3(x):
    return x + 3

add_3("CS106L")   # 运行时错误！
```

**C++**：  
```cpp
int add_3(int x) {
    return x + 3;
}

add_3("CS106L");   // 编译时错误！不能将 string 传给期望 int 的函数
```

---

### 3.4 课堂练习：推断类型

```cpp
std::string a = "test";
double b = 3.2 * 5 - 1;
int c = 5 / 2;                          // 整数除法，结果为 2
int d(int foo) { return foo / 2; }
double e(double foo) { return foo / 2; }
int f(double foo) { return (int)(foo + 0.5); }  // 四舍五入取整
void g(double c) { std::cout << c << std::endl; }
```

> **注意**：`(int) x` 是 C 风格强制类型转换，将 `x` 截断小数部分转为 `int`。  
> 例如 `(int) 5.7 = 5`。

---

### 3.5 补充：函数重载（Function Overloading）

> Defining two functions with the same name but different parameters.

定义两个同名但参数列表不同的函数。

```cpp
double axolot(int x) {          // 版本 1
    return (double) x + 3;
}

double axolot(double x) {       // 版本 2
    return x * 3;
}

axolot(2);      // 调用版本 1，返回 5.0
axolot(2.0);    // 调用版本 2，返回 6.0
```

---

## 4. 结构体（Structs）

### 4.1 为什么需要结构体？

**问题**：如何让一个函数返回多个不同类型的值？

```cpp
// 在 Python 中可以这样：
// return "Stanford Tree", "theTREE", 0000002

// 在 C++ 中，函数只能返回一个值。如何打包多个数据？
```

---

### 4.2 结构体基础

> **THE BIG IDEA**  
> A struct bundles named variables into a new type.

结构体将多个具名变量打包成一个新的类型。

```cpp
struct StanfordID {
    std::string name;     // 字段（field）
    std::string sunet;    // 每个字段有名称和类型
    int idNumber;
};

// 创建并初始化结构体实例
StanfordID id;
id.name = "THE Stanford Tree";
id.sunet = "theTREE";
id.idNumber = 0000002;

// 使用结构体作为返回值
StanfordID issueNewID() {
    StanfordID id;
    id.name = "THE Stanford Tree";
    id.sunet = "theTREE";
    id.idNumber = 0000002;
    return id;
}
```

---

### 4.3 统一初始化（Uniform Initialization）

```cpp
// 顺序必须与结构体中字段声明顺序一致，'=' 可选
StanfordID tree = { "THE Stanford Tree", "theTREE", 0000002 };
StanfordID lelandjr { "Leland Stanford Jr", "thejunior", 5430282 };
```

---

### 4.4 更多结构体示例

```cpp
struct Name {
    std::string first;
    std::string last;
};
Name rf = { "Rachel", "Fernandez" };

struct Order {
    std::string item;
    int quantity;
};
Order dozen = { "Eggs", 12 };

struct Point {
    double x;
    double y;
};
Point origin { 0.0, 0.0 };

struct Circle {
    Point center;
    double radius;
};
Circle circle { {0, 0}, 50000000 };
```

---

## 5. `std::pair`

### 5.1 什么是 `std::pair`？

`std::pair` 是标准库提供的一个模板结构体，用于将两个值组合成一个对象。

```cpp
#include <utility>   // std::pair 定义在此头文件中

std::pair<std::string, int> order = { "Eggs", 12 };
```

### 5.2 `std::pair` 的模板定义

```cpp
template <typename T1, typename T2>
struct pair {
    T1 first;
    T2 second;
};
```

### 5.3 应用示例：求解一元二次方程

方程：  $ax^2 + bx + c = 0$ 

求根公式：
$x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$

当判别式 $b^2 - 4ac < 0$ 时，无实数解。

```cpp
std::pair<bool, std::pair<double, double>> solveQuadratic(double a, double b, double c);
//         ^是否有解     ^两个解（如果有解）
```

返回值示例：
- 有解：`{ true, { 1.0, 2.0 } }`
- 无解：`{ false, { 0.0, 0.0 } }` （第二个值无意义）

---

## 6. `using` 关键字

当类型名过长时，可以使用 `using` 创建类型别名（Type Alias）。

```cpp
// 原始写法
std::pair<bool, std::pair<double, double>> solveQuadratic(double a, double b, double c);

// 使用 using 简化
using QuadraticSolution = std::pair<bool, std::pair<double, double>>;
QuadraticSolution solveQuadratic(double a, double b, double c);
```

> `using` 就像是类型的变量！

---

## 7. `auto` 关键字

`auto` 告诉编译器自动推断变量的类型。

```cpp
auto i = 1;               // i 被推断为 int
i = "hello!";             // ❌ 编译错误！i 仍然是 int 类型

auto result = solveQuadratic(1, -3, 2);   // 自动推断返回类型
```

> **注意**：`auto` 仍然是静态类型！一旦类型被推断出来，就不能改变。

**代码清晰度讨论**：
```cpp
std::pair<bool, std::pair<double, double>> result = ...;   // 冗长
auto result = ...;                                          // 简洁
```

---

## 8. 标准库（std）简介

### 8.1 什么是 `std::` ？

- C++ 标准库中提供的类型、函数等都位于 `std` 命名空间中。
- 使用时需要包含对应的头文件，并加上 `std::` 前缀。

| 头文件 | 内容 |
|--------|------|
| `#include <string>` | `std::string` |
| `#include <utility>` | `std::pair` |
| `#include <iostream>` | `std::cout`, `std::endl` |

> ⚠️ 不推荐使用 `using namespace std;`，因为可能引起命名冲突（例如你定义了自己的 `sort` 函数时）。

### 8.2 如何查阅 C++ 标准库文档

> • See the official standard at **cppreference.com**!  
> • Avoid **cplusplus.com**…  
> • It is outdated and filled with ads 😭

- ✅ **推荐**：[cppreference.com](https://en.cppreference.com/) —— C++ 官方标准的详细参考，内容权威且持续更新。
- ❌ **不推荐**：cplusplus.com —— 内容陈旧且广告繁多。

### 8.3 `#include` 做了什么？

```cpp
#include <utility>

std::pair<double, double> p { 1.0, 2.0 };
```

预处理器会将 `<utility>` 头文件中的内容**原样复制**到当前文件中，因此 `std::pair` 的定义变得可见。

---
## 9. 总结（Recap）

- **C++ 是一门编译型、静态类型的语言**  
  *C++ is a compiled, statically typed language.*
- **编译时错误 vs 运行时错误**：C++ 的强类型系统能在编译阶段捕获大量错误。
- **结构体（struct）** 用于将相关数据打包成新的类型。
- **`std::pair`** 是一个便捷的模板结构体，用于存储两个值。
- **`using`** 用于创建类型别名，提高代码可读性。
- **`auto`** 用于自动类型推断，但仍保持静态类型特性。
- **标准库（std）** 提供了丰富的功能，使用时需包含对应头文件并加上 `std::` 前缀。

---

**下周见！Have a great weekend! 😄**

# Code
## README.md

[Official Github Repository](https://github.com/cs106l/cs106l-lecture-code/tree/main/lecture02#lecture-2-types-and-structs)

To run this code, make sure you have followed [the setup instructions in A0](https://github.com/cs106l/cs106l-assignments/tree/main/assign0). Then, you can compile and run using:

```shell
g++ main.cpp -o main
```

and

```shell
./main
```

## `main.cpp`

```cpp
#include <iostream>
#include <utility>

#include <cmath>

using Zeros = std::pair<double, double>;
using Solution = std::pair<bool, Zeros>; 

/**
 * Solves the equation ax^2 + bx + c = 0
 * @param a The coefficient of x^2
 * @param b The coefficient of x
 * @param c The constant term
 * @return A pair. The first element (bool) indicates if the equation has a solution.
 *                 The second element is a pair of the roots if they exist.
 */
Solution solveQuadratic(double a, double b, double c)
{
  // Your code here...
  double discrim = b * b - 4 * a * c;
  if (discrim < 0) return { false, { 106, 106 }};

  double root = sqrt(discrim);
  return { true, { (-b - root) / (2 * a), (-b + root) / (2 * a) }};
}

int main() {
  // Get the values for a, b, and c from the user
  double a, b, c;
  std::cout << "a: "; std::cin >> a;
  std::cout << "b: "; std::cin >> b;
  std::cout << "c: "; std::cin >> c;

  // Solve the quadratic equation, using our quadratic function above
  auto result = solveQuadratic(a, b, c);
  if (result.first) {
    auto solutions = result.second;
    std::cout << "Solutions: " << solutions.first << ", " << solutions.second << std::endl;
  } else {
    std::cout << "No solutions" << std::endl;
  }

  return 0;
}
```

# Lecture Slide
![[2026Spring-02-TypesAndStructs.pdf]]