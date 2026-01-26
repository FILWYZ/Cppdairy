### 示例 1：默认构造 + 析构

```cpp
#include <iostream>
using namespace std;

class Test {
public:
    Test() {
        cout << "Test 构造" << endl;
    }

    ~Test() {
        cout << "Test 析构" << endl;
    }
};

int main() {
    Test t;
}
```

**输出：**

```
Test 构造
Test 析构
```

---

### 示例 2：拷贝构造触发

```cpp
#include <iostream>
using namespace std;

class Test {
public:
    Test() {
        cout << "默认构造" << endl;
    }

    Test(const Test& t) {
        cout << "拷贝构造" << endl;
    }
};

int main() {
    Test a;
    Test b = a;
}
```

**输出：**

```
默认构造
拷贝构造
```

---

### 示例 3：生命周期顺序

```cpp
class A {
public:
    A() { cout << "A 构造" << endl; }
    ~A() { cout << "A 析构" << endl; }
};

class B {
public:
    A a;
    B() { cout << "B 构造" << endl; }
    ~B() { cout << "B 析构" << endl; }
};

int main() {
    B b;
}
```

**输出顺序：**

```
A 构造
B 构造
B 析构
A 析构
```

---
