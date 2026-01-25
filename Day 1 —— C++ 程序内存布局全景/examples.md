## 示例 1：同时创建四种内存变量

```cpp
#include <iostream>
using namespace std;

int g_var = 100;             // 全局区
const int g_const = 200;     // 常量区（实现相关）

void demo() {
    int stack_var = 10;      // 栈
    static int static_var = 20; // 静态区
    int* heap_var = new int(30); // 堆

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

### 你应该观察到：

* `stack_var`：每次调用地址变化
* `static_var`：地址固定
* `heap_var`：每次 new 可能不同

---

## 示例 2：验证字符串字面量共享

```cpp
const char* a = "hello";
const char* b = "hello";

cout << (void*)a << endl;
cout << (void*)b << endl;
```

👉 多数情况下地址相同。

/*
stack : 0x5ffe34
heap : 0xe5380
static : 0x7ff66c493004
stack : 0x5ffe34
heap : 0xe5380
static : 0x7ff66c493004
global : 0x7ff66c493000
const : 0x7ff66c494000
string : 0x7ff66c494032
*/

---
