---
theme: custom-1776750089243-uatfgcpy9
themeName: "Main"
title: "斯坦福大学 CS106L 标准C++程序设计 Spring 2026 Lecture 4：流 (Streams)"
---

本文内容系基于斯坦福大学 **CS106L: Standard C++ Programming** 课程官方网站公开资源整理而成。课程官网：https://web.stanford.edu/class/archive/cs/cs106l/cs106l.1266/

---

斯坦福大学的 CS106L 教学内容始终紧随 C++ 标准的演进步伐，走在教学前沿。今年课程已融入部分 **C++26** 的新特性。然而，由于课程自 2020 年起不再提供公开的 Lecture 视频录像，目前网络上唯一可获取的最新官方学习资料仅限于 **Slide (幻灯片)** 与 **课程作业代码**，并无完整的文字讲义。

为方便广大 C++ 学习者参考，笔者依据该课程最新的 Slide 内容，逐页对照、整理归纳成此份“讲义”，以填补视频资料缺失的空白。文中对专业术语与关键句子均保留了中英双语对照，力求还原课堂原貌。

**免责声明：**

1. **版权归属**：本整理内容引用的所有课程资源版权归斯坦福大学及原授课教师所有。本文仅供学习交流，严禁用于任何商业用途。
2. **非官方性质**：本文并非斯坦福官方发布的讲义或教材，而是由第三方学习者基于公开 Slide 的理解与汇编。若官方后续发布正规讲义，请以官方版本为准。
3. **理解偏差**：由于缺乏实际的课堂录音与讲解，文中对 Slide 逻辑的串联、术语的解释可能存在笔者个人的理解偏差。如发现错漏之处，欢迎指正交流。

**本节相关资源链接：**

- Slide：https://web.stanford.edu/class/cs106l/lectures/2026Spring-04-Streams.pdf
- Code：https://github.com/cs106l/cs106l-lecture-code/tree/main/lecture04
- Assignments：https://github.com/cs106l/cs106l-assignments/tree/main/assignment1

---

**斯坦福 CS106L, 2026春季**  
授课教师：Rachel Fernandez & Preston Seay

---
## 目录

1. 快速回顾 (Quick recap)
2. 什么是流？(What are streams???!!!)
3. stringstreams
4. cout 和 cin
5. 输出流 (Output streams)
6. 输入流 (Input streams)

---
# 课程PPT (Slide)

## 1. 快速回顾 (Quick recap)

### 上一讲内容：初始化与引用 (Initialization & References)

- **统一初始化 (Uniform Initialization)**：一种普遍且安全的方式，使用 `{}` 来初始化变量。
  > A ubiquitous and safe way of initializing things using `{}`

- **引用 (References)**：为变量起别名，让多个变量都指向同一块内存。
  > A way of giving variables aliases and having multiple variables all refer the same memory.

---

## 2. 什么是流？(What are streams???!!!)

> "Designing and implementing a general input/output facility for a programming language is notoriously difficult" – Bjarne Stroustrup  
> “为编程语言设计和实现一个通用的输入/输出设施是出了名的困难。” —— 比雅尼·斯特劳斯特鲁普

**流 (Streams)**：C++ 的通用输入/输出设施。
> Streams: a general input/output facility for C++

**抽象 (Abstraction)**：隐藏不必要的细节，只暴露相关的内容。
> Abstraction = hide unnecessary details and expose what is only relevant

**核心思想 (THE BIG IDEA)**：流帮助我们在程序中读取和写入数据。
> Streams help us read and write data

### 熟悉的流 (A familiar stream!)

```cpp
std::cout << "Hello, World" << std::endl;
```

**流就像一条传送带 (streams are like a conveyer belt!)** —— 数据从源头流向目的地。

### 各种流的类型

| 输出流 (Output streams) | 输入流 (Input streams) |
|------------------------|------------------------|
| `cout`, `cerr`, `clog` | `cin` |
| `ofstream` (输出文件流) | `ifstream` (输入文件流) |
| `ostringstream` (输出字符串流) | `istringstream` (输入字符串流) |

### CS106B 中的例子

```cpp
ifstream in;
openFile(in, my_file);
Vector<std::string> lines = readLines(in);
```
> 注意：这里使用了斯坦福库，但仍然是流的例子 —— 你一直在使用流！

### `cout` 与 `cin` 例子

```cpp
std::cout << "hello CS106L!";

std::string student_input;
std::cin >> student_input;   // 允许用户写入内容
```

### 文件流 (`fout` & `fin`) 例子

```cpp
// 创建一个名为 "data.txt" 的文件
std::ofstream fout("data.txt");
fout << "I'm writing to this file";

std::ifstream fin("data.txt");
std::string first_word;
fin >> first_word;   // 从文件中读取第一个单词
```

> 注意到 `<<` 和 `>>` 了吗？这就是抽象的作用！我们可以用一致的接口处理输入和输出。
> notice the `<<` and `>>`? That's abstraction at work! We can use a consistent interface to work with input and output

---

## 流的继承体系 (Stream Hierarchy)

### `ios_base` —— 流相关的基础

- 维护**状态信息 (State Information)** 和**控制信息 (Control Information)**，确保流正常工作。
- **状态信息示例**：
  - `failbit`：逻辑错误（例如类型错误）
  - `eofbit`：到达流末尾
- **控制信息示例**：255 应该打印为 `"255"`、`"FF"` 还是 `"377"`？

### `basic_ios` —— 构建在 `ios_base` 之上

确保流正常工作，并标明流的来源（控制台、键盘或文件）。

### 继承树 (Inheritance Tree)

```
ios_base
    └── basic_ios
            ├── istream
            │       ├── ifstream
            │       ├── istringstream
            │       └── cin
            └── ostream
                    ├── ofstream
                    ├── ostringstream
                    └── cout
```

### 头文件包含

- `#include <iostream>` —— `cin` & `cout`
- `#include <istream>` —— `cin`
- `#include <ostream>` —— `cout`

---

## 输入流与输出流 (Input and Output Streams)

### 输入流 (Input Streams) —— `I`

- 从源读取数据的方式
  - 继承自 `std::istream`
  - 例如：从控制台读取 (`std::cin`)
  - 主要运算符：`>>`（提取运算符，extraction operator）

### 输出流 (Output Streams) —— `O`

- 向目的地写入数据的方式
  - 继承自 `std::ostream`
  - 例如：向控制台写入 (`std::cout`)
  - 主要运算符：`<<`（插入运算符，insertion operator）

> 流提供了一种处理外部数据的通用方式。
> Streams allow for a universal way of dealing with external data.

---

## 3. stringstreams

### 什么是 `stringstream`？

`std::stringstream` 是一个基于字符串的流，允许你将字符串当作流来操作。

### 示例代码

```cpp
void foo() {
    // 部分 Bjarne 名言 (partial Bjarne Quote)
    std::string initial_quote = "Bjarne Stroustrup C makes it easy to shoot yourself in the foot\n";

    // 创建一个 stringstream 并用字符串构造
    std::stringstream ss(initial_quote);
    // 或者这样插入：
    // std::stringstream ss;
    // ss << initial_quote;

    // 数据目的地
    std::string first;
    std::string last;
    std::string language, extracted_quote;

    // 从流中提取数据
    ss >> first >> last >> language;
    std::cout << first << " " << last << " said this: " << language << std::endl;
}
```

### `<<` 和 `>>` 的行为

- `<<` 和 `>>` 会读取直到遇到任何空白字符 (whitespace)，包括：
  1. 空格 `' '`
  2. 换行 `'\n'`
  3. 制表符 `'\t'`
  4. 回车 `'\r'`
  5. 换页 `'\f'`
  6. 垂直制表符 `'\v'`

### 问题：`>>` 只读取到下一个空白字符

```cpp
ss >> first >> last >> language >> extracted_quote;
```

- `first` 得到 `"Bjarne"`
- `last` 得到 `"Stroustrup"`
- `language` 得到 `"C"`
- `extracted_quote` 会尝试读取 `"makes"` —— 但这不是我们想要的！

**解决方案：使用 `getline()`**

### `getline()` 函数

```cpp
istream& getline(istream& is, string& str, char delim);
```

- 从输入流 `is` 中读取，直到遇到分隔符 `delim`，将内容存入 `str`。
- 默认分隔符是 `'\n'`。
- **`getline()` 会消费掉分隔符字符** —— 注意这一点！
  > getline() consumes the delim character! PAY ATTENTION TO THIS :)

### 修正后的代码

```cpp
void foo() {
    std::string initial_quote = "Bjarne Stroustrup C makes it easy to shoot yourself in the foot\n";
    std::stringstream ss(initial_quote);

    std::string first;
    std::string last;
    std::string language, extracted_quote;

    ss >> first >> last >> language;
    std::getline(ss, extracted_quote);  // 读取剩余的所有内容
    std::cout << first << " " << last << " said this: '" << language << " " << extracted_quote << "'" << std::endl;
}
```

---

## 4. 输出流深入 (Output Streams in Depth)

### 输出流的作用

- 向目的地/外部源写入数据
  - 例如：向控制台写入 (`std::cout`)
  - 使用 `<<` 插入运算符发送到输出流

### 缓冲区与刷新 (Buffer & Flush)

输出流中的字符在发送到目的地之前，会先存储在一个**中间缓冲区 (intermediary buffer)** 中，然后才被**刷新 (flushed)**。

**刷新发生的时机 (When do we flush?)：**

- `std::cout << std::flush;` —— 强制刷新
- `std::cout << std::endl;` —— 插入换行并刷新
- 程序结束时
- 缓冲区满时
- **当绑定的流交互时**（例如，`cout` 必须在 `cin` 读取输入之前刷新）

### `std::endl` 示例

```cpp
int main() {
    for (int i = 1; i <= 5; ++i) {
        std::cout << i << std::endl;
    }
    return 0;
}
// 输出：
// 1
// 2
// 3
// 4
// 5
```

### 不带 `std::endl` 的示例

```cpp
int main() {
    for (int i = 1; i <= 5; ++i) {
        std::cout << i;
    }
    return 0;
}
// 输出：12345
```

### `cerr` 与 `clog`

- **`cerr`**：用于输出错误，**无缓冲 (unbuffered)** —— 立即发送错误信息
- **`clog`**：用于非关键事件日志，**缓冲 (buffered)**

### 一个重要说明 (A shoutout and clarification)

> 在许多实现中，标准输出是行缓冲的，写入 `'\n'` 也会导致刷新，除非执行了 `std::ios::sync_with_stdio(false)`。
> In many implementations, standard output is line-buffered, and writing '\n' causes a flush anyway, unless `std::ios::sync_with_stdio(false)` was executed.

**性能优化技巧：**

```cpp
int main() {
    std::ios::sync_with_stdio(false);
    for (int i = 1; i <= 5; ++i) {
        std::cout << i << '\n';
    }
    return 0;
}
```

> 这可能会带来巨大的性能提升。但请注意：此优化仅当输出流是**非交互式 (non-interactive)** 时才有效（例如文件、Unix 管道）。对于终端（交互式），`'\n'` 仍然会立即刷新。
> This only works if your output stream is non-interactive!

---

## 5. 输出文件流 (Output File Streams)

- 类型：`std::ofstream`
- 用于向文件写入数据
- 使用 `<<` 插入运算符
- 常用方法：
  - `is_open()` —— 检查文件是否成功打开
  - `open()` —— 打开文件
  - `close()` —— 关闭文件
  - `fail()` —— 检查是否有错误

### 文件打开标志 (File flags)

| 标志 | 含义 |
|------|------|
| `std::ios::trunc` (默认) | 打开文件并截断（清空）内容 |
| `std::ios::app` | 追加模式 (append) |
| `std::ios::ate` | 打开后立即跳转到文件末尾 |

### 示例代码

```cpp
int main() {
    // 构造时关联文件
    std::ofstream ofs("hello.txt");

    if (ofs.is_open()) {
        ofs << "Hello CS106L!" << '\n';
    }

    ofs.close();  // 关闭文件

    // 尝试写入已关闭的流 —— 静默失败 (will silently fail)
    ofs << "this will not get written";

    // 重新打开文件
    ofs.open("hello.txt", std::ios::app);  // 追加模式
    ofs << "this will though! It's open again";
    return 0;
}
```

---

## 6. 输入文件流 (Input File Streams)

- 类型：`std::ifstream`
- 用于从文件读取数据
- 使用 `>>` 提取运算符
- 常用方法：`get()`, `getline()`, `peek()`, `seekg()`

### 示例代码

```cpp
int inputFileStreamExample() {
    std::ifstream ifs("input.txt");

    if (ifs.is_open()) {
        std::string line;
        std::getline(ifs, line);
        std::cout << "Read from the file: " << line << '\n';
    }

    if (ifs.is_open()) {
        std::string lineTwo;
        std::getline(ifs, lineTwo);
        std::cout << "Read from the file: " << lineTwo << '\n';
    }
    return 0;
}
```

---

## 7. `std::cin` 详解

### `std::cin` 的特点

- `std::cin` 是**缓冲的 (buffered)**
- 可以看作一个用户存储数据并从中读取的地方
- `std::cin` 的缓冲区在遇到空白字符时停止
- C++ 中的空白字符包括：空格 `" "`、换行 `\n`、制表符 `\t`

### 基本使用

```cpp
int main() {
    double pi;
    std::cin >> pi;  // 如果 cin 缓冲区为空，会提示用户输入
    std::cout << "pi is: " << pi << '\n';
    return 0;
}
```

用户输入 `3.14`，程序输出 `pi is: 3.14`。

### 当 `std::cin` 失败时 (When std::cin fails!)

考虑以下代码：

```cpp
int main() {
    double pi;
    double tao;
    std::string name;
    std::cin >> pi;
    std::cin >> name;
    std::cin >> tao;
    std::cout << "my name is: " << name << " tao is: " << tao << " pi is: " << pi << '\n';
    return 0;
}
```

**输入：** `3.14\nRachel Fernandez\n`

**实际发生：**
- `pi` 读取 `3.14`
- `name` 读取 `"Rachel"`
- `tao` 试图读取 `"Fernandez"` —— 但 `"Fernandez"` 不是 `double`，所以 `tao` 读取失败（得到 0）

---

## 8. 常见陷阱：`getline()` 与 `cin >>` 混用

### 问题演示

```cpp
void cinGetlineBug() {
    double pi;
    double tao;
    std::string name;
    std::cin >> pi;
    std::getline(std::cin, name);
    std::cin >> tao;
    std::cout << "my name is: " << name << " tao is: " << tao << " pi is: " << pi << '\n';
}
```

**输入：** `3.14\nRachel Fernandez\n`

**问题：** `std::cin >> pi` 读取 `3.14` 后，输入缓冲区中还剩下 `\n`。紧接着的 `std::getline(std::cin, name)` 会立即读取这个换行符，得到空字符串，而不是 `"Rachel Fernandez"`。

### 解决方案

**方法1：** 使用额外的 `getline()` 消费掉残留的换行符

```cpp
void cinGetline() {
    double pi;
    double tao;
    std::string name;
    std::cin >> pi;
    std::getline(std::cin, name);  // 读取并丢弃残留的换行符
    std::getline(std::cin, name);  // 真正读取名字
    std::cin >> tao;
    std::cout << "my name is: " << name << " tao is: " << tao << " pi is: " << pi << '\n';
}
```

**方法2：** 全部使用 `getline()` 然后解析，或者全部使用 `>>` 然后处理字符串。

### 主要结论 (Main takeaways)

1. **流是程序中读写数据的通用接口。**
   > Streams are a general interface to read and write data in programs.

2. **同一源/目的地的输入流和输出流相互补充。**
   > Input and output streams on the same source/destination type compliment each other!

3. **不要轻易混用 `getline()` 和 `std::cin >>`，除非你非常清楚自己在做什么。**
   > Don't use getline() and std::cin together, unless you really really have to!

---

## 致谢 (Acknowledgements)

感谢 Avery Wang 的流讲座，本讲义在格式和流程上受到了很大启发。
> Credit to Avery Wang's streams lecture which I took a lot of inspiration from, particularly for formatting and flow.

---

# 课程练习代码 (Code)
https://github.com/cs106l/cs106l-lecture-code/tree/main/lecture04
## 课堂练习 (Practice)
### pre-practice.cpp

```cpp
#include <iostream>
#include <fstream>
#include <string>
 
// ============================================================
//  EXERCISE 1 — std::cout
//
//  Task: Print the following to the console:
//    Line 1: "Hello, Streams!"
//    Line 2: The number 42
//    Line 3: The double 3.14
//
//  Expected output:
//    Hello, Streams!
//    42
//    3.14
// ============================================================
void exercise1_cout() {
    std::cout << "=== Exercise 1: cout ===\n";

    // YOUR CODE HERE

    std::cout << "\n";
}
 
 
// ============================================================
//  EXERCISE 2 — std::cin
//
//  Task: Ask the user for their name and favourite number,
//        then print:
//          "Hello [name], your favourite number is [number]!"
//
//  Example:
//    Enter your name: Alice
//    Enter your favourite number: 7
//    Hello Alice, your favorite number is 7!
//
//  Note: For this exercise, assume the name is a single word
// ============================================================
void exercise2_cin() {
    std::cout << "=== Exercise 2: cin ===\n";
 
    std::string name;
    int number;
 
    // YOUR CODE HERE
  
    std::cout << "\n";
}
 
 
// ============================================================
//  EXERCISE 3 — std::ofstream
//
//  Task: Create a file called "numbers.txt" and write
//        the numbers 1 through 5 to it, one per line.
//        Then print "File written!" to the console.
//
//  Expected file contents:
//    1
//    2
//    3
//    4
//    5
//
//  Requirements:
//    - Check the file opened successfully before writing
//    - If it fails, print "Error: could not open file" to std::cerr
// ============================================================
void exercise3_ofstream() {
    std::cout << "=== Exercise 3: ofstream ===\n";
 
    // YOUR CODE HERE
    std::ofstream ofile("numbers.txt");
 
    std::cout << "\n";
}
 
 
// ============================================================
//  EXERCISE 4 — std::ifstream
//
//  Task: Read back the "numbers.txt" file from Exercise 3
//        and print each number to the console in the format:
//          "Number: 1"
//          "Number: 2"
//          ... and so on
//
//  Requirements:
//    - Check the file opened successfully
//    - Use a while loop to read until end of file
// ============================================================
void exercise4_ifstream() {
    std::cout << "=== Exercise 4: ifstream ===\n";

    // YOUR CODE HERE

    std::cout << "\n";
}
 
 
// ============================================================
//  EXERCISE 5 — Stretch goal
//
//  Task:
//    1. Ask the user to enter 3 numbers via cin
//    2. Write them to a file called "user_numbers.txt"
//    3. Read the file back and print each number
//    4. Append the number 99 to the file
//       without overwriting the existing numbers
//    5. Read and print the whole file one more time to confirm
//
//  Hint: For step 4, look up std::ios::app
// ============================================================
void exercise5_combined() {
    std::cout << "=== Exercise 5: Combined ===\n";
 
    // YOUR CODE HERE
    
 
    std::cout << "\n";
}
 

int main() {
    exercise1_cout();
    exercise2_cin();
    exercise3_ofstream();
    exercise4_ifstream();
    exercise5_combined();
 
    return 0;
}
```

### post-practice.cpp

```cpp
#include <iostream>
#include <fstream>
#include <string>

// ============================================================
//  EXERCISE 1 — cout 
//
//  Task: Print a welcome message to the console:
//          "=== Student Grade Tracker ==="
//          "Enter your details below."
// ============================================================
void exercise1() {
    // YOUR CODE HERE
}


// ============================================================
//  EXERCISE 2 — cin 
//
//  Task: Ask the student for their name and their grade (int).
//        Then print:
//          "Student: [name]"
//          "Grade: [grade]"
//
//  Note: assume the name is one word for now
// ============================================================
void exercise2(std::string& name, int& grade) {
    // YOUR CODE HERE
    // note: we pass name and grade by reference so later
    // exercises can use them!
}


// ============================================================
//  EXERCISE 3 — cerr 
//
//  Task: Validate the grade from exercise 2.
//        If the grade is less than 0 or greater than 100:
//          - print "Error: invalid grade!" to cerr
//          - return false
//        Otherwise return true
// ============================================================
bool exercise3(int grade) {
    // YOUR CODE HERE
    return true;
}


// ============================================================
//  EXERCISE 4 — ofstream 
//
//  Task: Save the student's name and grade to "grades.txt"
//        in the format:
//          [name] [grade]
//
//  Requirements:
//    - check the file opened successfully
//    - if it fails, print "Error: could not open file" to cerr
//    - print "Saved to grades.txt!" to cout on success
// ============================================================
void exercise4(const std::string& name, int grade) {
    // YOUR CODE HERE
}


// ============================================================
//  EXERCISE 5 — ifstream  
//
//  Task: Read back "grades.txt" and print each entry as:
//          "Name: [name]  |  Grade: [grade]"
//
//  Requirements:
//    - check the file opened successfully
//    - use a while loop to read until end of file
// ============================================================
void exercise5() {
    // YOUR CODE HERE
}


// ============================================================
//  EXERCISE 6 — putting it all together  (~2 mins)
//
//  Task: Extend the tracker to handle 3 students in a loop.
//        Follow the steps below carefully!
// ============================================================
void exercise6() {
    std::cout << "\n=== Tracking 3 Students ===\n";

    for (int i = 0; i < 3; i++) {

        // ---------------------------------------------------------
        // STEP 1: declare a string called name and an int called grade
        // ---------------------------------------------------------
        // YOUR CODE HERE


        // ---------------------------------------------------------
        // STEP 2: prompt the user and read in name and grade with cin
        //   print "Enter name for student [i+1]: " before reading name
        //   print "Enter grade: " before reading grade
        // ---------------------------------------------------------
        // YOUR CODE HERE


        // ---------------------------------------------------------
        // STEP 3: call exercise3(grade) to validate the grade
        //   if it returns false:
        //     - print "Skipping [name] due to invalid grade."
        //     - use continue to skip to the next student
        // ---------------------------------------------------------
        // YOUR CODE HERE


        // ---------------------------------------------------------
        // STEP 4: open "grades.txt" with ofstream using std::ios::app
        //   check it opened successfully — if not, print error and return
        //   write name and grade to the file in the format: [name] [grade]
        //   print "Saved [name]!" to cout
        // ---------------------------------------------------------
        // YOUR CODE HERE

    }

    // ---------------------------------------------------------
    // STEP 5: print "\n=== All Students ===" then call exercise5()
    //         to read back and print everyone in the file
    // ---------------------------------------------------------
    // YOUR CODE HERE

}


// ============================================================
//  MAIN — comment out exercises you haven't reached yet
// ============================================================
int main() {
    // Exercise 1 — welcome message
    exercise1();
    std::cout << "\n";

    // Exercise 2 — get student info
    std::string name;
    int grade;
    exercise2(name, grade);
    std::cout << "\n";

    // Exercise 3 — validate grade
    if (!exercise3(grade)) {
        std::cerr << "Please restart and enter a valid grade.\n";
        return 1;
    }
    std::cout << "\n";

    // Exercise 4 — save to file
    exercise4(name, grade);
    std::cout << "\n";

    // Exercise 5 — read back from file
    exercise5();
    std::cout << "\n";

    // Exercise 6 — full tracker
    exercise6();

    return 0;
}
```

---

## 参考答案 (Solutions)
### pre-practice-soln.cpp

```cpp
#include <iostream>
#include <fstream>
#include <string>
 
// ============================================================
//  EXERCISE 1 — std::cout
//
//  Task: Print the following to the console:
//    Line 1: "Hello, Streams!"
//    Line 2: The number 42
//    Line 3: The double 3.14
//
//  Expected output:
//    Hello, Streams!
//    42
//    3.14
// ============================================================
void exercise1_cout() {
    std::cout << "=== Exercise 1: cout ===\n";

    // YOUR CODE HERE
    std::cout << "Hello, Streams!" << std::endl;
    std::cout << 42 << std::endl;
    std::cout << 3.14 << std::endl;

    std::cout << "\n";
}
 
 
// ============================================================
//  EXERCISE 2 — std::cin
//
//  Task: Ask the user for their name and favourite number,
//        then print:
//          "Hello [name], your favourite number is [number]!"
//
//  Example:
//    Enter your name: Alice
//    Enter your favourite number: 7
//    Hello Alice, your favorite number is 7!
//
//  Note: For this exercise, assume the name is a single word
// ============================================================
void exercise2_cin() {
    std::cout << "=== Exercise 2: cin ===\n";
 
    std::string name;
    int number;
 
    // YOUR CODE HERE
    std::cin >> name;
    std::cin >> number;

    std::cout << "Hello " << name << ", your favorite number is " << number << "!\n";
 
    std::cout << "\n";
}
 
 
// ============================================================
//  EXERCISE 3 — std::ofstream
//
//  Task: Create a file called "numbers.txt" and write
//        the numbers 1 through 5 to it, one per line.
//        Then print "File written!" to the console.
//
//  Expected file contents:
//    1
//    2
//    3
//    4
//    5
//
//  Requirements:
//    - Check the file opened successfully before writing
//    - If it fails, print "Error: could not open file" to std::cerr
// ============================================================
void exercise3_ofstream() {
    std::cout << "=== Exercise 3: ofstream ===\n";
 
    // YOUR CODE HERE
    std::ofstream ofile("numbers.txt");

    if (!ofile.is_open()) {
        std::cerr << "Error: could not open file\n";
        return;  
    }

    for (int i = 1; i <= 5; i++){
        ofile << i << "\n";
    }
 
    std::cout << "\n";
}
 
 
// ============================================================
//  EXERCISE 4 — std::ifstream
//
//  Task: Read back the "numbers.txt" file from Exercise 3
//        and print each number to the console in the format:
//          "Number: 1"
//          "Number: 2"
//          ... and so on
//
//  Requirements:
//    - Check the file opened successfully
//    - Use a while loop to read until end of file
// ============================================================
void exercise4_ifstream() {
    std::cout << "=== Exercise 4: ifstream ===\n";

    std::ifstream ifile("numbers.txt");
    if (!ifile.is_open()) {
        std::cerr << "Error: could not open file\n";
        return;
    }

    int number;
    // 1. Tries to read the next value into number 2. Checks if that read succeeded
    while (ifile >> number) {
        std::cout << "Number: " << number << "\n";
    }

    std::cout << "\n";
}
 

int main() {
    exercise1_cout();
    exercise2_cin();
    exercise3_ofstream();
    exercise4_ifstream();
 
    return 0;
}
```

### post-practice-soln.cpp

```cpp
// ============================================================
//  streams_10min_SOLUTION.cpp
//  10 minute practice — SOLUTION FILE
// ============================================================
//
//  HOW TO COMPILE & RUN:
//    g++ -o streams_10min_solution streams_10min_SOLUTION.cpp
//    ./streams_10min_solution
//
// ============================================================

#include <iostream>
#include <fstream>
#include <sstream>
#include <string>

// ============================================================
//  EXERCISE 1 — cout
// ============================================================
void exercise1() {
    std::cout << "=== Student Grade Tracker ===" << "\n";
    std::cout << "Enter your details below." << "\n";
}


// ============================================================
//  EXERCISE 2 — cin
// ============================================================
void exercise2(std::string& name, int& grade) {
    std::cout << "Enter your name: ";
    std::cin >> name;

    std::cout << "Enter your grade: ";
    std::cin >> grade;

    std::cout << "Student: " << name << "\n";
    std::cout << "Grade: " << grade << "\n";
}


// ============================================================
//  EXERCISE 3 — cerr
// ============================================================
bool exercise3(int grade) {
    if (grade < 0 || grade > 100) {
        std::cerr << "Error: invalid grade!\n";
        return false;
    }
    return true;
}


// ============================================================
//  EXERCISE 4 — ofstream
// ============================================================
void exercise4(const std::string& name, int grade) {
    std::ofstream out("grades.txt");

    if (!out.is_open()) {
        std::cerr << "Error: could not open file\n";
        return;
    }

    out << name << " " << grade << "\n";
    std::cout << "Saved to grades.txt!\n";
}


// ============================================================
//  EXERCISE 5 — ifstream
// ============================================================
void exercise5() {
    std::ifstream in("grades.txt");

    if (!in.is_open()) {
        std::cerr << "Error: could not open file\n";
        return;
    }

    std::string name;
    int grade;
    while (in >> name >> grade) {
        std::cout << "Name: " << name << "  |  Grade: " << grade << "\n";
    }
}


// ============================================================
//  EXERCISE 6 — putting it all together
// ============================================================
void exercise6() {
    std::cout << "\n=== Tracking 3 Students ===\n";

    for (int i = 0; i < 3; i++) {
        std::string name;
        int grade;

        // get student info
        std::cout << "\nEnter name for student " << i + 1 << ": ";
        std::cin >> name;
        std::cout << "Enter grade: ";
        std::cin >> grade;

        // validate grade
        if (!exercise3(grade)) {
            std::cout << "Skipping " << name << " due to invalid grade.\n";
            continue;   // skip saving this student
        }

        // append to file
        std::ofstream out("grades.txt", std::ios::app);
        if (!out.is_open()) {
            std::cerr << "Error: could not open file\n";
            return;
        }
        out << name << " " << grade << "\n";
        std::cout << "Saved " << name << "!\n";
    }

    // read everyone back
    std::cout << "\n=== All Students ===\n";
    exercise5();
}


// ============================================================
//  MAIN
// ============================================================
int main() {
    // Exercise 1 — welcome message
    exercise1();
    std::cout << "\n";

    // Exercise 2 — get student info
    std::string name;
    int grade;
    exercise2(name, grade);
    std::cout << "\n";

    // Exercise 3 — validate grade
    if (!exercise3(grade)) {
        std::cerr << "Please restart and enter a valid grade.\n";
        return 1;
    }
    std::cout << "\n";

    // Exercise 4 — save to file
    exercise4(name, grade);
    std::cout << "\n";

    // Exercise 5 — read back from file
    exercise5();
    std::cout << "\n";

    // Exercise 6 — full tracker
    exercise6();

    return 0;
}
```

---
# 课程作业 (Assignments)

你应当先去Github下载Assignment的Skeleton代码
https://github.com/cs106l/cs106l-assignments/tree/main/assignment1
---

# Assignment 1: SimpleEnroll

# 作业 1：SimpleEnroll

Due Friday, April 17th, at 11:59PM

截止日期：4月17日（周五）晚上11:59

## Setting up C++

## 设置 C++ 环境

Go find and follow the setup instructions in [Assignment Setup](../assignment-setup/README.md) to get your C++ compiler and autograder set up for this assignment.

请前往并按照 [作业设置](../assignment-setup/README.md) 中的设置说明进行操作，为本次作业配置好 C++ 编译器和自动评分器。

## Overview

## 概述

It’s that time of the quarter again; time to use SimpleEnroll 🤗 Wootwoot.
One thing everyone realizes in their Stanford career at one point is that they
have to eventually graduate — so enrolling in classes becomes a strategic
endeavor to maximize the XP towards graduation, while also being able to sleep
more than 4 hours a night!

又到了学期中的这个时候：该使用 SimpleEnroll 了 🤗 呜呼。
每个斯坦福学生在求学过程中终会意识到，自己终究要毕业 —— 因此选课就变成了一项战略任务，既要最大化毕业所需的经验值，又要保证每晚能睡超过 4 个小时！

In this hopefully short assignment, we’re going to use data from the
ExploreCourses API to figure out which CS classes on ExploreCourses are
offered this year, and which are not! We’ll be taking advantage of streams, while also exercising initialization and references in C++. Lets jump in ʕ•́ᴥ•̀ʔっ

在这个希望不会太长的作业中，我们将使用 ExploreCourses API 的数据，来判断 ExploreCourses 上的哪些 CS 课程今年开设，哪些不开设！我们将利用流（streams），同时练习 C++ 中的初始化和引用。让我们开始吧 ʕ•́ᴥ•̀ʔっ

There are only two files you should need to care about:

你只需要关注两个文件：

* `main.cpp`: All your code goes here 😀!
* `main.cpp`：你所有的代码都写在这里 😀！
* `utils.cpp`: Contains some utility functions. You'll use functions defined in this file, but you don't otherwise need to modify it.
* `utils.cpp`：包含一些工具函数。你会用到这个文件中定义的函数，但除此之外不需要修改它。

## Running your code

## 运行你的代码

To run your code, first you'll need to compile it. Open up a terminal (if you are using VSCode, hit <kbd>Ctrl+\`</kbd> or go to **Terminal > New Terminal** at the top). Then make sure that you are in the `assignment1/` directory and run:

要运行你的代码，首先需要编译它。打开终端（如果你使用 VSCode，请按 <kbd>Ctrl+`</kbd> 或者点击顶部菜单 **Terminal > New Terminal**）。然后确保你在 `assignment1/` 目录下，并运行：

```sh
g++ -std=c++20 main.cpp -o main
```

Assuming that your code compiles without any compiler errors, you can now do:

假设你的代码编译没有产生任何编译器错误，现在你可以运行：

```sh
./main
```

which will actually run the `main` function in `main.cpp`. This will execute your code and then run an autograder that will check that your code is correct.

这将实际执行 `main.cpp` 中的 `main` 函数。它会运行你的代码，然后运行自动评分器来检查你的代码是否正确。

As you are following the instructions below, we recommend intermittently compiling/testing with the autograder as a way to make sure you're on the right track!

在按照下面的说明进行操作时，我们建议你间歇性地使用自动评分器进行编译和测试，以确保你的方向正确！

> [!NOTE]  
> ### Note for Windows
> ### Windows 注意事项
> On Windows, you may need to compile your code using
> 在 Windows 上，你可能需要使用以下命令编译代码：
> ```sh
> g++ -static-libstdc++ -std=c++20 main.cpp -o main
> ```
> in order to see output. Also, the output executable may be called `main.exe`, in which case you'll run your code with:
> 以便看到输出。另外，输出的可执行文件可能名为 `main.exe`，此时你需要使用以下命令运行代码：
> ```sh
> ./main.exe
> ```

## Part 0: Read the code and fill in the `Course` struct

## 第 0 部分：阅读代码并填写 `Course` 结构体

1. In this assignment, we'll be using the `Course` struct to represent records pulled from ExploreCourses in C++. Take a look at the (incomplete) definition of the `Course` struct in `main.cpp` and fill in the field definitions. Ultimately, we'll be using streams to generate `Course`s ---  remember what types streams deal with?
2. 在本作业中，我们将使用 `Course` 结构体来表示从 ExploreCourses 获取的记录。请查看 `main.cpp` 中 `Course` 结构体的（不完整的）定义，并填写字段定义。最终，我们将使用流来生成 `Course` —— 还记得流处理的是什么类型吗？

3. Take a look at the `main` function in `main.cpp`, and take special notice of how `courses` is passed into `parse_csv`, `write_courses_offered`,
and `write_courses_not_offered`. Think about what these functions are doing. Do you need to change anything in the function definition? Spoiler, you do.
3. 查看 `main.cpp` 中的 `main` 函数，特别注意 `courses` 是如何传递给 `parse_csv`、`write_courses_offered` 和 `write_courses_not_offered` 的。想一想这些函数在做什么。你需要在函数定义中修改什么吗？剧透：你需要修改。

## Part 1: `parse_csv`

## 第 1 部分：`parse_csv`

Check out `courses.csv`, it is a CSV file, with three columns: Title, Number of
Units, and Quarter. Implement `parse_csv` so that, for each line in the csv file, it creates a struct `Course` containing the Title, Number of Units, and Quarter for that line.

查看 `courses.csv`，它是一个 CSV 文件，包含三列：课程名称 (Title)、学分数 (Number of Units) 和学期 (Quarter)。实现 `parse_csv`，使其对于 CSV 文件中的每一行，创建一个包含该行课程名称、学分数和学期的 `Course` 结构体。

A couple of things you need to think about:

你需要考虑几件事：

1. How are you going to read in `courses.csv`? Muahahaha, perhaps a
stream 😏?
2. 你打算如何读取 `courses.csv`？姆哈哈哈，也许是用流 😏？
3. How will you get each line in the file?
4. 你将如何获取文件中的每一行？

### Hints

### 提示

1. Take a look at the `split` function we provide in `utils.cpp`. It may come in handy!
    * Feel free to check out the implementation of `split` and ask us any questions about it – you
should be able to reason about it since it’s using a `stringstream`.
2. 查看我们在 `utils.cpp` 中提供的 `split` 函数。它可能会派上用场！
    * 欢迎查看 `split` 的实现，如果有任何问题随时问我们 —— 你应该能够理解它，因为它使用了 `stringstream`。
3. Each **line** is a record! *This is important, so we're saying it again :>)*
4. 每一**行**就是一条记录！*这很重要，所以我们再说一遍 :>)*
5. In CSV files (and specifically in `courses.csv`), the first line is usually a row that defines the column names (a column header row). This line doesn't actually correspond to a `Course`, so you'll need to skip it somehow!
6. 在 CSV 文件中（特别是 `courses.csv`），第一行通常是定义列名的行（列头行）。这一行并不对应一个实际的 `Course`，所以你需要以某种方式跳过它！

## Part 2: `write_courses_offered`

## 第 2 部分：`write_courses_offered`

Ok. Now you have a populated `courses` vector which has all the records
of the `courses.csv` file neatly stored in a `Course` struct! You find yourself
interested in only the courses that are offered, right? **A course is considered offered if its Quarter field is not the string `“null”`.** In this function, write out to `“student_output/courses_offered.csv”` all the courses that don’t have
`“null”` in the quarter field.

好了。现在你已经有了一个填充好的 `courses` 向量，其中整齐地存储了 `courses.csv` 文件中的所有记录，每个记录都是一个 `Course` 结构体！你发现自己只对开设的课程感兴趣，对吧？**如果一个课程的 Quarter 字段不是字符串 `"null"`，则认为该课程开设。** 在本函数中，将所有 Quarter 字段不是 `"null"` 的课程写入 `"student_output/courses_offered.csv"`。

> [!IMPORTANT]  
> When writing out to the CSV file, please follow this format:
> ```
> <Title>,<Number of Units>,<Quarter>
> ```
> Note that there are **no spaces** between the commas! The autograder will not be happy if this format is not followed!
> 
> Also, **make sure to write out the column header row** as the first line in the output. This is the same line you had to skip in `courses.csv` for the previous step!

> [!IMPORTANT]  
> 写入 CSV 文件时，请遵循以下格式：
> ```
> <Title>,<Number of Units>,<Quarter>
> ```
> 注意逗号之间**没有空格**！如果不遵循此格式，自动评分器会不高兴！
>
> 另外，**确保将列头行作为输出的第一行**。这就是你在上一步中需要在 `courses.csv` 中跳过的同一行！

Once `write_courses_offered` has been called, we expect that all of the offered courses (and consequently all the courses you wrote to the output file) will be removed from the `all_courses` vector. **This means that after this
function runs, `all_courses` should ONLY contain courses that are
not offered!** 

一旦 `write_courses_offered` 被调用，我们期望所有开设的课程（以及因此你写入输出文件的所有课程）都将从 `all_courses` 向量中移除。**这意味着在此函数运行之后，`all_courses` 应该只包含不开设的课程！**

One way to do this is to keep track of the courses that are offered perhaps with another vector and delete them from `all_courses`. Just like in Python and many other languages, it is a bad idea to remove elements from a data structure while you are iterating over it, so you'll probably want to do this *after* you have written all offered courses to file.

一种方法是使用另一个向量来记录开设的课程，然后将它们从 `all_courses` 中删除。就像在 Python 和许多其他语言中一样，在遍历数据结构的同时删除其中的元素是一个坏主意，因此你可能希望在将所有开设的课程写入文件**之后**再进行删除操作。

## Part 3: `write_courses_not_offered`

## 第 3 部分：`write_courses_not_offered`

So you’re curious about courses that aren’t offered... In the
`write_courses_not_offered` function, write out to
`“student_output/courses_not_offered.csv”` the courses in
`unlisted_courses`. Remember since you deleted the courses that are
offered in the previous step, `unlisted_courses` trivially contains ONLY courses that are not offered – lucky you. So this step should look really similar to Part 2 except shorter and a *tiny* bit simpler.

那么你对不开设的课程感到好奇…… 在 `write_courses_not_offered` 函数中，将 `unlisted_courses` 中的课程写入 `"student_output/courses_not_offered.csv"`。请记住，由于你在上一步中删除了开设的课程，`unlisted_courses` 自然只包含不开设的课程 —— 算你走运。所以这一步看起来与第 2 部分非常相似，只是更短，而且*稍微*简单一点。

## 🚀 Submission Instructions

## 🚀 提交说明

After compiling and running, if your autograder looks like this:

编译并运行后，如果你的自动评分器显示如下：

![An image showing a terminal window where the autograder has run with all tests passing](https://raw.githubusercontent.com/cs106l/cs106l-assignments/main/assignment1/docs/autograder.png)


then you have finished the assignment! Woot woot. 

那么你就完成了本次作业！呜呼。

To submit the assignment:

提交作业的步骤：

1. Please complete the feedback form [at this link](https://forms.gle/UeD6zjmUpFbhGgw98). 
2. 请填写反馈表单 [点击此链接](https://forms.gle/UeD6zjmUpFbhGgw98)。
3. Submit your assignment on [Paperless](https://paperless.stanford.edu)!
4. 在 [Paperless](https://paperless.stanford.edu) 上提交你的作业！

Your deliverable should be:

你需要提交的文件是：

- `main.cpp`
