## 示例 1：返回局部变量地址

```cpp
int* foo() {
    int x = 10;
    return &x;
}
```

### 面试表达示例

> 这是典型的生命周期问题。局部变量位于栈上，函数返回后栈帧销毁，指针继续访问会产生悬空指针，属于未定义行为。

---

## 示例 2：多路径 return 泄漏

```cpp
int* create(bool ok) {
    int* p = new int(10);
    if (!ok) return nullptr;
    return p;
}
```

### 面试表达示例

> 这是典型的多路径 return 导致的内存泄漏问题，错误路径提前返回，资源释放逻辑未执行，说明需要 RAII 来统一管理生命周期。

---

## 示例 3：默认拷贝导致 double free

```cpp
MyVector v1;
MyVector v2 = v1;
```

### 面试表达示例

> 默认拷贝是浅拷贝，两个对象共享同一块堆内存，析构时会发生 double free，这违反了资源所有权唯一性的原则。

---
