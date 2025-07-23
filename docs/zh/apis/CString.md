# CString APIs

## 定义

CString 是一个动态字符串，用于存储字符串数据。使用此容器，可以在有限的存储空间中轻松处理字符串数据。

由以下两个成员变量存储，总共占用 16 字节：

- `data`: 指向字符串数据的指针。
- `length`: 字符串的长度。
- `capacity`: 字符串的容量（用于实现动态扩容）。

## 构造函数

!!! note
    所有构造函数都需要经过[析构函数](#_15)才能彻底销毁。否则会导致内存泄漏！

### `string()`

#### 函数原型

```c
CString string(const char* str);
```

#### 参数

- `str`：字符串数据。

#### 功能

- 初始化一个 CString 对象，将指定的字符串数据**完全复制**为新的字符串。

### `strInit()`

#### 函数原型

```c
CString strInit(const char ch, size_t length);
```

#### 参数

- `ch`：字符串的初始字符。
- `length`：字符串的长度。

#### 功能

- 初始化一个 CString 对象，将指定的字符重复指定次数生成一个新的字符串。

#### 示例

初始化一个长度为 5 的字符串，每个字符为 'A'。

```c
CString str = strInit('A', 5);
printf("%s", str.data);
```

通过输出其字符串，得到如下结果：`AAAAA`。

### `strList()`

#### 函数原型

```c
CString strList(const char* split, uint32_t count, ...);
```

#### 参数

- `split`：字符串的分隔符。
- `count`：字符串的个数。
- `...`：可变参数列表，用于初始化字符串。

#### 功能

创建一个 CString 列表，将可变参数列表中的字符串数据**按顺序**连接为字符串。

#### 示例

创建一个包含三个字符串的 CString 列表。

```c
CString str = strList(" ", 3, "Use", "for", "CString!");
printf("%s", str.data);
```

通过输出其字符串，得到如下结果：`Use for CString!`。

### `stringSub()`

#### 宏函数

```c
#define stringSub(string, start_pos, count)             _strSub(&string, start_pos, count)
```

#### 函数原型

```c
CString _strSub(CString* str, uint32_t start_pos, uint32_t count);
```

#### 参数

- `str`: 待截取的字符串。
- `start_pos`: 截取的起始位置。
- `count`: 截取的长度。

#### 功能

初始化一个 CString 对象，并将截取到的字符串赋值给新的 CString 对象。

#### 示例

将字符串 "Hello, World!" 中的 `World` 截取出来，并赋给新的 CString 对象。

```c
CString str = string("Hello, World!");
CString substr = stringSub(str, 7, 5);
printf("%s\n", substr.data);
```

## 析构函数

### `destroyString()`

#### 宏函数

```c
#define destroyString(string)                           _destroyString(&string)
```

#### 函数原型

```c
void _destroyString(CString* string);
```

#### 参数

- `string`：指向要销毁的 CString 对象的指针。

#### 功能

- **完全销毁**一个 CString 对象，释放其占用的内存。


