### 示例 1：unique_ptr 基本用法

```cpp
std::unique_ptr<int> p = std::make_unique<int>(10);
```

---

### 示例 2：unique_ptr 转移所有权

```cpp
auto p1 = std::make_unique<int>(10);
auto p2 = std::move(p1);
```

---

### 示例 3：shared_ptr 引用计数

```cpp
auto p1 = std::make_shared<int>(10);
auto p2 = p1;
```

---

### 示例 4：weak_ptr 解决循环引用

```cpp
struct Node {
    std::shared_ptr<Node> next;
    std::weak_ptr<Node> prev;
};
```
