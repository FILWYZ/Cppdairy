### 示例 1：循环引用导致泄漏

```cpp
struct Node {
    std::shared_ptr<Node> next;
    std::shared_ptr<Node> prev; // ❌
};
```

---

### 示例 2：weak_ptr 修复

```cpp
struct Node {
    std::shared_ptr<Node> next;
    std::weak_ptr<Node> prev;   // ✅
};
```

---

### 示例 3：安全访问 weak_ptr

```cpp
if (auto p = prev.lock()) {
    // 安全使用 p
}
```

---
