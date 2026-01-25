本节通过**可运行的代码示例**，直观验证 C++ 中不同类型对象在内存中的分布位置，以及它们的生命周期和地址特性。重点目标是：**用地址变化来理解栈、堆、全局区、静态区和常量区的区别**。

---

## 示例 1：同时创建四种内存变量

### 示例代码

```cpp
#include <iostream>
using namespace std;

int g_var = 100;                 // 全局区
const int g_const = 200;         // 常量区（实现相关）

void demo() {
    int stack_var = 10;              // 栈
    static int static_var = 20;      // 静态区
    int* heap_var = new int(30);     // 堆

    cout << "stack  : " << &stack_var << endl;
    cout << "heap   : " << heap_var << endl;
    cout << "static : " << &static_var << endl;

    delete heap_var;
}

int main() {
    demo();
    demo();

    cout << "global : " << &g_var << endl;
    cout << "const  : " << &g_const << endl;

    const char* s = "hello";
    cout << "string : " << (void*)s << endl;
}
```

---

### 你应该观察到的现象

* **stack_var（栈变量）**

  * 每次调用 `demo()`，地址可能变化
  * 生命周期随函数进入和退出而创建、销毁

* **static_var（静态变量）**

  * 地址固定
  * 只初始化一次，生命周期贯穿整个程序

* **heap_var（堆内存）**

  * 每次 `new` 返回的地址**可能不同**
  * 生命周期由程序员显式控制（`new / delete`）

* **g_var（全局变量）**

  * 地址固定
  * 程序启动时创建，结束时销毁

* **g_const / 字符串字面量**

  * 通常位于只读存储区（常量区）
  * 地址固定，具备只读属性

---

### 一次可能的输出示例（地址因平台而异）

```text
stack  : 0x5ffe34
heap   : 0xe5380
static : 0x7ff66c493004
stack  : 0x5ffe34
heap   : 0xe5380
static : 0x7ff66c493004
global : 0x7ff66c493000
const  : 0x7ff66c494000
string : 0x7ff66c494032
```

> 说明：地址数值本身不重要，**相对位置与是否变化**才是关键。

---

## 示例 2：验证字符串字面量的共享

### 示例代码

```cpp
const char* a = "hello";
const char* b = "hello";

cout << (void*)a << endl;
cout << (void*)b << endl;
```

### 观察结论

* 在大多数编译器与优化设置下：

  * **相同的字符串字面量只会存储一份**
  * `a` 与 `b` 指向同一块只读内存

```text
0x7ff7ecd8401f
0x7ff7ecd8401f
```

---

## 本节核心结论

* **栈**：自动管理，速度快，生命周期短
* **堆**：手动管理，灵活但容易出错
* **静态 / 全局区**：地址固定，生命周期长
* **常量区**：只读、可共享，提高安全性与效率

这些规则是理解 **`new` / `malloc` / RAII / 栈溢出 / 内存泄漏** 的基础。

---
