---
theme: custom-1776750089243-uatfgcpy9
themeName: "Main"
title: "CS106L Spring 2026 Lecture 1：欢迎（Welcome）"
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
# CS106L Spring 2026 Lecture 1：欢迎（Welcome）

**CS106L, 2026 春季学期**  
讲师：Rachel Fernandez, Preston Seay

## 📋 今日议程

- 讲师介绍
- 课程主旨宣讲
- 课程教务安排

---

## 👥 讲师介绍

### Preston Seay
- 大二学生
- 计算机科学 & 数学双专业
- 教育
- 跑步爱好者


### Rachel Fernandez

- 大二学生
- 计算机科学 & 数学双专业
- 鸟类爱好者 <3
- 对网络安全领域感兴趣


---

## 🎯 课程主旨宣讲

### 为什么选择 C++？

> *"The invisible foundation of everything."*  
> *“万物无形的基石。”*

### C++ 应用实例

**游戏领域：**
- Valorant（瓦）
- CS:GO / CS2

**高频交易：**
- Optiver
- Citadel Securities
- HRT

**自动驾驶：**
- 多家公司采用 C++ 开发自动驾驶系统

**嵌入式硬件：**
- Arduino（单片机开发）

**GPU 编程：**
- NVIDIA CUDA（并行计算平台）

**更多基础领域：**
- 数据库 (MySQL, MongoDB)
- 网页浏览器 (Chrome, Safari, Edge)
- 虚拟现实Virtual Reality (Quest)
- 底层机器学习框架 (PyTorch, TensorFlow)
- 编译器与虚拟机 (JVM, LLVM, GCC)
- 操作系统 (Windows, MacOS, Linux)

### C++ 的擅长领域

- **Handling lots of data** — 处理海量数据
- **And handling it very efficiently** — 且高效地处理
- **And doing it in an elegant, readable way** — 并以优雅、可读的方式实现

### 行业巨头采用 C++

- Amazon
- Meta
- Bloomberg
- Adobe

### 语言排行榜现状

*C++ 于 1985 年首次发布，至今仍位列第 3！*

|  排名   | 2026年3月 | 2025年3月 | 变化  | 编程语言       | 占有率       | 变化     |
| :---: | :------ | :------ | :-- | :--------- | :-------- | :----- |
|   1   | 1       | 1       | -   | Python     | 21.25%    | -2.59% |
|   2   | 2       | 4       | ▲   | C          | 11.55%    | +2.02% |
| **3** | **3**   | **2**   | ▼   | **C++**    | **8.18%** | -2.90% |
|   4   | 4       | 3       | ▼   | Java       | 7.99%     | -2.37% |
|   5   | 5       | 5       | -   | C#         | 6.36%     | +1.49% |
|   6   | 6       | 6       | -   | JavaScript | 3.45%     | -0.01% |

*数据来源：TIOBE Index, Mar 2026*

### C++ 社区生态

- C++ 拥有**庞大**的用户基础
- C++ 标准每三年修订一次
- 由标准委员会决定语言的新特性变更

### 什么是 C++？

#### 一段合法的 C++ 程序（现代风格）

```cpp
#include <iostream>
#include <string>

int main() {
    auto str = std::make_unique<std::string>("Hello World!");
    std::cout << *str << std::endl;
    return 0;
}
// 输出 "Hello World!"
```

#### 也是一段合法的 C++ 程序（C 风格）

```cpp
#include "stdio.h"
#include "stdlib.h"

int main(int argc, char *argv) {
    printf("%s", "Hello, world!\n"); // 这是一个 C 函数！
    return EXIT_SUCCESS;
}
```

#### 甚至这也是一段合法的 C++ 程序（内联汇编风格）

```cpp
#include "stdio.h"
#include "stdlib.h"

int main(int argc, char *argv) {
    asm(".LCO:\n\t"
        ".string \"Hello, world!\"\n\t"
        "main:\n\t"
        "push rbp\n\t"
        "mov rbp, rsp\n\t"
        "sub rsp, 16\n\t"
        "mov DWORD PTR [rbp-4], edi\n\t"
        "mov QWORD PTR [rbp-16], rsi\n\t"
        "mov edi, OFFSET FLAT:.LCO\n\t"
        "call puts\n\t");
    return EXIT_SUCCESS;
}
```

---

## 📜 C++ 简史

### 汇编语言时代

```assembly
section .text
global _start          ; 必须为链接器 (ld) 声明
_start:                ; 告诉链接器入口点
    mov edx, len       ; 消息长度
    mov ecx, msg       ; 要写入的消息
    mov ebx, 1         ; 文件描述符 (stdout)
    mov eax, 4         ; 系统调用号 (sys_write)
    int 0x80           ; 调用内核
    mov eax, 1         ; 系统调用号 (sys_exit)
    int 0x80           ; 调用内核

section .data
msg db 'Hello, world!', 0xa   ; 亲爱的字符串
len equ $ - msg               ; 字符串长度
```

**汇编语言的优点：**
- 极其简单的指令
- 极快（编写良好的情况下）
- 对程序的完全控制

**汇编语言的缺点：**
- 代码量大（即使简单任务）
- 极难阅读理解
- 极度不可移植

### C 语言的诞生

- 1972 年，Dennis Ritchie 创建了 C 语言，广受赞誉。
- C 语言使得编写代码变得容易：
    - 快速
    - 简单
    - 跨平台
- **编译器！** 源代码 → 汇编代码

> *"When I read C I know what the output Assembly is going to look like."*  
> *“当我阅读 C 代码时，我知道生成的汇编代码会是什么样子。”*  
> —— Linus Torvalds (Linux 创始人)

**C 语言的弱点：**
- 没有对象或类
- 编写泛型或模板代码困难
- 编写大型程序繁琐

### C++ 的登场

- 1983 年，丹麦计算机科学家 **Bjarne Stroustrup** 开始创造 C++ 的雏形。
- 他的目标语言是：
    - 快速
    - 使用简单
    - 跨平台
    - 具备高级语言特性

### C++ 设计哲学

1.  **Express ideas and intent directly in code.** — 在代码中直接表达想法与意图。
2.  **Enforce safety at compile time whenever possible.** — 尽可能在编译期强制执行安全性。
3.  **Do not waste time or space.** — 不浪费时间和空间。
4.  **Compartmentalize messy constructs.** — 将混乱的构造隔离封装。
5.  **Allow the programmer full control, responsibility, and choice.** — 赋予程序员完全的控制权、责任与选择自由。

> *"Code should be elegant and efficient; I hate to have to choose between those."*  
> *“代码应当既优雅又高效；我讨厌不得不在两者之间做取舍。”*  
> —— Bjarne Stroustrup

---

## 🤔 为什么要选修 CS106L？

### 课程覆盖主题

- 欢迎与入门
- 类型与结构体
- 初始化与引用
- 流操作
- 容器
- 迭代器与指针
- 类
- 高级类特性
- 模板
- 高级模板
- 函数与 Lambda 表达式
- 运算符重载
- 特殊成员函数
- 移动语义
- `std::optional` 与类型安全
- RAII 与智能指针
- C++ 项目实践

### CS106L 对比 CS106B/X

| 对比维度 | CS106B/X | CS106L |
|:---|:---|:---|
| **关注点** | 抽象、递归、指针等**概念** | **代码本身**：什么是好代码，强大优雅的代码长什么样 |
| **C++ 用法** | 仅用满足概念教学的最低限度 C++ | **真家伙**：无 Stanford 库，只用 STL；理解 C++ 的设计初衷与原理 |
| **工作量** | 较重 | 轻松的 1 学分课程 |

### 你在斯坦福哪些课程会用到 C++

- CS 111：操作系统原理
- CME 213：并行计算导论（MPI, OpenMP, CUDA）
- CS 143：编译器
- CS 144：计算机网络导论
- CS 248A：计算机图形学
- MUSIC 256A：音乐、计算与设计艺术
- ...以及更多

### C++ 助你养成良好编码习惯

- 我是否以对象应有的方式使用它们？（对象使用规范）
- **Type checking, type safety** — 类型检查与安全
- 我是否高效利用了内存？（内存效率）
- **Reference/copy semantics, move semantics** — 引用/拷贝语义与移动语义
- 我是否修改了不该修改的内容？
- **const and const correctness** — const 与常量正确性

> *"Nobody should call themselves a professional if they only know one language."*  
> *“只懂一门语言的人，不应自称为专业人士。”*  
> —— Bjarne Stroustrup

---

## 📚 课程教务安排

### 提问环节

- 我们欢迎提问！
- 随时可以举手
- 我们也会定期停下来收集反馈

### 无障碍支持

- 残障学生是斯坦福社区重要且受重视的一员，欢迎来到课堂。
- 请通过 OAE（可访问教育办公室）协调，如有需要请告知我们以便提供便利。

### 课堂规范

- 零羞耻学习区
- 友善尊重对待同学与讲师
- 保持好奇心
- 沟通是关键
- 承认我们都在成长过程中（保持谦逊、勇于提问、拒绝完美主义）

### 指导原则

- 我们会尽一切努力支持你，提供最大程度的灵活性。
- 我们希望听到反馈以改善课程体验。
- 遇到个人情况请及时沟通，我们在这里支持你。

### 讲座安排

- **时间地点**：周二 & 周四 下午 3:00 – 4:20，Thornton 110 教室
- **录像**：讲座**不**提供录像。
- **考勤**：**必须出席**。从第 2 周起，课前会有简短测验（1-2 题）。每人有 **2 次免费缺勤**机会。
- **签到方式**：需扫描幻灯片上的二维码，二维码仅在**每节课前 10 分钟**展示。

### 生病与紧急情况

- 如果生病，请为了自己和他人的健康**居家休息**，联系我们说明情况——我们不希望你抱病上课。
- 紧急或特殊情况，请务必联系我们以便提供帮助。

### 答疑时间

- 时间待定，为线下形式。
- 将在第 2 周前确定（第一次作业前）。
- 信息将发布在 Ed 论坛上。

### 评分与作业

- **学分与等级**：1 学分，仅分 **S/NC**（通过/不通过）。预期全员通过。
- **如何获得 S**：
    1.  在第 2 周至第 9 周的 **14 次**课中出勤 **12 次**。
    2.  成功完成全部 **8 次**周作业。
- **作业详情**：
    - 8 次短作业（每次约 1-2 小时）。
    - 通过 Paperless 提交。
    - 每周五发布，次周五截止。
    - 每人拥有 **3 次免费迟交天数**。

### 联系方式

- **邮箱**：`cs106l-spr2526-staff@lists.stanford.edu`
    - *请使用此邮箱而非个人邮箱，以便两位讲师都能收到。*
- **Ed 讨论论坛**：通过 Canvas 加入，可公开发帖或私密提问。
- **线下**：课后或答疑时间交流。

---

> **周四见！:)**
> *See you all on Thursday :)*

