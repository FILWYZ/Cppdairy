### 示例 1：浅拷贝导致的问题

```cpp
class A {
public:
    int* p;

    A(int x) {
        p = new int(x);
    }

    ~A() {
        delete p;
    }
};

int main() {
    A a(10);
    A b = a; // 浅拷贝
}
```

❌ 问题：

* `a.p` 和 `b.p` 指向同一地址
* 析构时 double delete

---

### 示例 2：正确的深拷贝（拷贝构造）

```cpp
class A {
public:
    int* p;

    A(int x) {
        p = new int(x);
    }

    A(const A& other) {
        p = new int(*other.p);
    }

    ~A() {
        delete p;
    }
};
```

---

### 示例 3：自定义拷贝赋值运算符

```cpp
class A {
public:
    int* p;

    A(int x = 0) {
        p = new int(x);
    }

    A(const A& other) {
        p = new int(*other.p);
    }

    A& operator=(const A& other) {
        if (this == &other)
            return *this;

        delete p;                // 清理旧资源
        p = new int(*other.p);   // 深拷贝
        return *this;
    }

    ~A() {
        delete p;
    }
};
```

---
