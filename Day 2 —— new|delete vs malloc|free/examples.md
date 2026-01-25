## 示例 1：对比构造 / 析构调用

```cpp
#include <iostream>
using namespace std;

class Test {
public:
    Test() { cout << "Test Constructor" << endl; }
    ~Test() { cout << "Test Destructor" << endl; }
};

int main() {
    cout << "--- new/delete ---" << endl;
    Test* a = new Test();
    delete a;

    cout << "--- malloc/free ---" << endl;
    Test* b = (Test*)malloc(sizeof(Test));
    free(b);
}
```

### 观察输出

* new：构造 + 析构都会执行
* malloc：**什么都不会发生**

---

## 示例 2：手动补救 malloc（反例）

```cpp
Test* p = (Test*)malloc(sizeof(Test));
new(p) Test();        // placement new
p->~Test();
free(p);
```

⚠️ 工程中极少使用，容易出错

---
