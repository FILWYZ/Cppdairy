### 示例 1：没有 RAII 的内存泄漏

```cpp
void func() {
    int* p = new int(10);
    if (error)
        return;
    delete p;
}
```

---

### 示例 2：RAII 包装内存

```cpp
class Buffer {
public:
    char* data;

    Buffer(size_t n) {
        data = new char[n];
    }

    ~Buffer() {
        delete[] data;
    }
};
```

---

### 示例 3：RAII 管理锁

```cpp
void func() {
    std::lock_guard<std::mutex> lock(m);
    // 临界区
}
```

---

### 示例 4：RAII 管理文件

```cpp
void write() {
    std::ofstream ofs("a.txt");
    ofs << "hello";
}
```

---
