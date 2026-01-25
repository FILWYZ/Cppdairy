## 阶段 1：最 naive 版本（只能用一次）

```cpp
class MyVector {
public:
    MyVector() {
        data_ = new int[1];
        size_ = 0;
        cap_ = 1;
    }

    void push_back(int x) {
        data_[size_] = x;  // ❌ 没有扩容判断
        size_++;
    }
};
```

问题：

* 第二次 push_back 直接越界

---

## 阶段 2：加入扩容逻辑（核心）

```cpp
void push_back(int x) {
    if (size_ == cap_) {
        size_t newCap = cap_ * 2;
        int* newData = new int[newCap];

        for (size_t i = 0; i < size_; ++i) {
            newData[i] = data_[i];
        }

        delete[] data_;
        data_ = newData;
        cap_ = newCap;
    }

    data_[size_++] = x;
}
```

📌 你已经在：

* 手动管理生命周期
* 控制所有权转移

---

## 阶段 3：补上析构函数（否则必泄漏）

```cpp
~MyVector() {
    delete[] data_;
}
```

没有它：

* Valgrind 会明确报泄漏

---

## 阶段 4：完整最小可用版本

```cpp
class MyVector {
public:
    MyVector() : data_(nullptr), size_(0), cap_(0) {}

    ~MyVector() {
        delete[] data_;
    }

    void push_back(int x) {
        if (size_ == cap_) {
            size_t newCap = cap_ == 0 ? 1 : cap_ * 2;
            int* newData = new int[newCap];

            for (size_t i = 0; i < size_; ++i)
                newData[i] = data_[i];

            delete[] data_;
            data_ = newData;
            cap_ = newCap;
        }
        data_[size_++] = x;
    }

    int operator[](int i) const {
        return data_[i];
    }

    size_t size() const { return size_; }

private:
    int* data_;
    size_t size_;
    size_t cap_;
};
```

---

## Valgrind 自检（必须做）

```bash
g++ -g vector.cpp -o vector
valgrind --leak-check=full ./vector
```

目标：

* **0 leaks**

---
