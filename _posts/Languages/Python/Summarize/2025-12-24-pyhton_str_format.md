---
title: "python str format"
date: 2025-12-24 22:33:00 +0800
categories: [Languages, Python]
tag: [Python, str format]
permalink: /posts/Languages/Python/Summarize/python_str_format
---

* TOC
{:toc}

---

### 一、字符串格式化方法

#### 1.1 `%`

```python
# 所有python 版本支持
result = "app_uid=%s, app_name=%s" % (self.app_uid, self.app_name)
```

#### 1.2 `format()`
```python
# Python 2.6+ 和 Python 3.0+
result = "app_uid={}, app_name={}".format(self.app_uid, self.app_name)
# 或者
result = "app_uid={app_uid}, app_name={app_name}".format(
    app_uid=self.app_uid, 
    app_name=self.app_name
)
```

#### 1.3 `f-strings`
```python
# Python 3.6+
result = f"app_uid={self.app_uid}, app_name={self.app_name}"
```

#### 1.4 `str + str`
```python
# 所有python 版本支持
result = "app_uid=" + self.app_uid + ", app_name=" + self.app_name
```

### 二、总结与对比

#### 2.1 适用说明
前三种是标准的格式化字符串方式

1. **百分号格式化字符串 (`%`):**
    - 支持范围：所有的Python版本。
    - 注意：虽然这是旧式的格式化方式，但仍然可以在现代Python代码中使用。
2. **`.format()` 方法:**
    - 支持范围：Python 2.6+ 和 Python 3.0+。
    - 注意：`.format()` 方法是在Python 2.6中引入的，并且在之后的版本中持续存在。
3. **f-strings (Python 3.6+):**
    - 支持范围：Python 3.6+。
    - 注意：f-strings 是在Python 3.6中引入的，它提供了一种更简洁和直观的字符串格式化方式。 
4. **字符串拼接 (str + str)**
    - 支持范围：所有的Python版本。
    - 注意：性能比较低。


#### 2.2 对比表格

| **方法**   | **Python版本** | **性能** | **可读性** | **推荐度**          |
| ---------- | -------------- | -------- | ---------- | ------------------- |
| `%` 格式   | 所有版本       | 中       | 一般       | ⭐⭐ (仅限旧代码维护) |
| `format()` | 2.6+           | 中       | 好         | ⭐⭐⭐ (兼容性好)      |
| f-strings  | 3.6+           | **优**   | **优**     | ⭐⭐⭐⭐⭐ (首选)        |
| 字符串拼接 | 所有版本       | 差       | 差         | ⭐ (特殊情况使用)    |
