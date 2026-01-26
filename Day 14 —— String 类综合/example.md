### 完整 String 实现（可直接编译）

```cpp
#include <cstring>
#include <iostream>

class String {
private:
    char* data_;
    size_t size_;

public:
    String() : data_(nullptr), size_(0) {}

    String(const char* s) {
        size_ = std::strlen(s);
        data_ = new char[size_ + 1];
        std::strcpy(data_, s);
    }

    ~String() {
        delete[] data_;
    }

    // 拷贝构造
    String(const String& other) : size_(other.size_) {
        if (other.data_) {
            data_ = new char[size_ + 1];
            std::strcpy(data_, other.data_);
        } else {
            data_ = nullptr;
        }
    }

    // 拷贝赋值
    String& operator=(const String& other) {
        if (this == &other)
            return *this;

        delete[] data_;
        size_ = other.size_;
        if (other.data_) {
            data_ = new char[size_ + 1];
            std::strcpy(data_, other.data_);
        } else {
            data_ = nullptr;
        }
        return *this;
    }

    // 移动构造
    String(String&& other) noexcept
        : data_(other.data_), size_(other.size_) {
        other.data_ = nullptr;
        other.size_ = 0;
    }

    // 移动赋值
    String& operator=(String&& other) noexcept {
        if (this == &other)
            return *this;

        delete[] data_;
        data_ = other.data_;
        size_ = other.size_;

        other.data_ = nullptr;
        other.size_ = 0;
        return *this;
    }
};

int main() {
    String a("hello");
    String b = a;
    String c = std::move(a);
}
```

---

### 示例说明

* `b = a` → 拷贝构造（深拷贝）
* `c = std::move(a)` → 移动构造（资源转移）

---
