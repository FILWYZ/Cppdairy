# 示例 1：最经典的泄漏（必须做）

```cpp
#include <iostream>
using namespace std;

void leak() {
    int* p = new int(42);
}

int main() {
    leak();
    return 0;
}
```

---

### 使用 Valgrind

```bash
g++ -g leak.cpp -o leak
valgrind --leak-check=full ./leak
```

你将看到：

* definitely lost
* 栈回溯指向 leak()

---

## 示例 2：多路径 return 泄漏

```cpp
int* create(bool ok) {
    int* p = new int(10);
    if (!ok) return nullptr; // ❌ 泄漏
    return p;
}
```

---

## 示例 3：修复方式（对比）

### ❌ 手动 delete（脆弱）

```cpp
if (!ok) {
    delete p;
    return nullptr;
}
```

### ✅ RAII 思想（预告）

```cpp
unique_ptr<int> p(new int(10));
```

👉 Day 6 将系统讲解。

---

