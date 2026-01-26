### 题目 1（判断）

为什么 String 必须自己实现拷贝构造？

**答案要点：**

* 内部有 `char*`
* 默认拷贝是浅拷贝
* 会 double delete

---

### 题目 2（综合）

String 类为什么非常适合作为面试题？

**答案：**

* 覆盖 Rule of Five
* 覆盖 RAII
* 覆盖内存管理

---

### 题目 3：用 valgrind 检查

```bash
valgrind --leak-check=full ./a.out
```

**期望结果：**

* 0 leaks
* 0 errors

---
