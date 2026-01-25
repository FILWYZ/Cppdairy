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

# 📝 文档三：quiz.md（测试题 + 面试答案详解）

## 题目 1（必考）

❓ 为什么 C++ 不推荐 malloc？

### ✅ 面试级答案

1. malloc 不调用构造 / 析构
2. 破坏 RAII 资源管理
3. 类型不安全
4. 不支持多态
5. 不利于异常安全

---

## 题目 2

```cpp
Test* t = (Test*)malloc(sizeof(Test));
```

❓ 这是不是一个合法的 C++ 对象？

### ✅ 答案解析

* ❌ 不是
* 只是原始内存
* 构造函数未执行

---

## 题目 3（进阶）

❓ 那什么时候 C++ 会用 malloc？

### ✅ 答案要点

* 底层库
* 内存池实现
* placement new 场景

---

## Day 2 自检清单

* [ ] 我能说清 new 的三步
* [ ] 我知道 malloc 的边界
* [ ] 我理解 RAII 的意义

---

🚀 Day 2 完成。
你已经正式跨过 **C++ 与 C 的分水岭**。
