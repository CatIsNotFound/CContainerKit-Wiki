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

### `varToString()`

#### 函数原型

```c
CString varToString(CVariant variant);
```

#### 参数

- `variant`：要转换的 [CVariant](#CVariant.md) 对象。

#### 功能

- 将 CVariant 对象转换为 CString 对象。

!!! note "注意"
    `variant` 参数必须为 `TYPE_STRING` 类型，否则将返回一个空字符串。

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

- 创建一个 CString 列表，将可变参数列表中的字符串数据**按顺序**连接为字符串。

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

- 初始化一个 CString 对象，并将截取到的字符串赋值给新的 CString 对象。

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

## 字符串修改函数

### `stringCopy()`

#### 宏函数

```c
#define stringCopy(string, buffer)                      _strCopyStr(&string, buffer)
```

#### 函数原型

```c
void _strCopyStr(CString *string, const char *buffer);
```

#### 参数

- `string`：指向 CString 对象的指针。
- `buffer`：指向要覆盖的字符串的指针。

#### 功能

- 将指定的字符串数据**覆盖**为新的 CString 对象的字符串数据。

#### 示例

将字符串 "Hello, World!" 覆盖为 "Hello, CContainerKit!"：

```c
CString str = string("Hello, World!");
stringCopy(str, "Hello, CContainerKit!");
printf("%s", str.data);
```

通过输出其字符串，得到如下结果：`Hello, CContainerKit!`。

### `stringInsert()`

#### 宏函数

```c
#define stringInsert(string, index, buffer)             _strInsertStr(&string, index, buffer)
```

#### 函数原型

```c
void _strInsertStr(CString *string, uint32_t index, const char *buffer);
```

#### 参数

- `string`：指向 CString 对象的指针。
- `index`：插入的位置。
- `buffer`：指向要插入的字符串的指针。

#### 功能

- 在指定位置插入字符串。
- 若该位置超出字符串的长度，则默认插入到字符串末尾。

#### 示例

将字符串 "World!" 插入到字符串 "Hello, " 的后面。

```c
CString str = string("Hello, ");
stringInsert(str, 7, "World!");
printf("%s", str.data);
```

通过输出其字符串，得到如下结果：`Hello, World!`。

### `stringPushBack()` / `stringAppend()`

#### 宏函数

```c
#define stringPushBack(string, buffer)                  stringInsert(string, string.length, buffer)
#define stringAppend(string, buffer)                    stringPushBack(string, buffer)
```

#### 函数原型

见 [`stringInsert()`](#stringinsert) 函数。

#### 参数

- `string`：指向 CString 对象的指针。
- `buffer`：指向要插入的字符串的指针。

#### 功能

- 将字符串 `buffer` 插入到 CString 对象的末尾。

### `stringPushFront()`

#### 宏函数

```c
#define stringPushFront(string, buffer)                 stringInsert(string, 0, buffer)
```

#### 函数原型

见 [`stringInsert()`](#stringInsert) 函数。

#### 参数

- `string`：指向 CString 对象的指针。
- `buffer`：指向要插入的字符串的指针。

#### 功能

- 将字符串 `buffer` 插入到 CString 对象的开头。

### `stringRemove()`

#### 宏函数

```c
#define stringRemove(string, index, count)              _strRemoveStr(&string, index, count)
```

#### 函数原型

```c
void _strRemoveStr(CString *string, uint32_t index, uint32_t count);
```

#### 参数

- `string`：指向 CString 对象的指针。
- `index`：要删除的字符串的起始索引。
- `count`：要删除的字符串的长度。

#### 功能

- 删除指定位置的字符串。
- 若该位置超出字符串的长度，则默认不删除。
- 若要删除的字符串长度超出字符串的长度，则默认删除字符串的剩余部分。


### `stringPopBack()`

#### 宏函数

```c
#define stringPopBack(string, count)                    stringRemove(string, string.length - count, count)
```

#### 函数原型

见 [`stringRemove`](#stringremove) 函数。

#### 参数

- `string`：指向 CString 对象的指针。
- `count`：要删除的字符串的长度。

#### 功能

- 删除字符串末尾的指定长度的字符串。

### `stringPopFront()`

#### 宏函数

```c
#define stringPopFront(string, count)                   stringRemove(string, 0, count)
```

#### 函数原型

见 [`stringRemove()`](#stringremove) 函数。

#### 参数

- `string`：指向 CString 对象的指针。
- `count`：要删除的字符串的长度。

#### 功能

- 删除字符串开头的指定长度的字符串。

### `stringUpper()`

#### 宏函数

```c
#define stringUpper(string)                             _strUpper(&string)
```

#### 函数原型

```c
void _strUpper(CString* string);
```

#### 参数

- `string`：指向 CString 对象的指针。

#### 功能

- 将 CString 对象的字符串中的所有小写字母转换为大写。

### `stringLower()`

#### 宏函数

```c
#define stringLower(string)                             _strLower(&string)
```

#### 函数原型

```c
void _strLower(CString* string);
```

#### 参数

- `string`：指向 CString 对象的指针。

#### 功能

- 将 CString 对象的字符串中的所有大写字母转换为小写。


## 其它函数

### `stringIsEqual()`

#### 宏函数

```c
#define stringIsEqual(string, buffer, case_sensitive)   _strIsEqual(&string, buffer, case_sensitive)
```

#### 函数原型

```c
bool _strIsEqual(CString* string, const char* buffer, bool case_sensitive);
```

#### 参数

- `string`：指向 CString 对象的指针。
- `buffer`：指向要比较的字符串的指针。
- `case_sensitive`：是否区分大小写。

#### 功能

比较 CString 对象和字符串 `buffer` 是否相等。

#### 示例

比较 CString 对象和字符串 `"Hello, world!"` 是否相等：

```c
CString str = string("Hello, World!");
bool ret1 = stringIsEqual(str, "Hello, world!", true);  // 区分大小写
bool ret2 = stringIsEqual(str, "hello, world!", false); // 不区分大小写
printf("ret1: %s, ret2: %s\n", ret1 ? "true" : "false", ret2 ? "true" : "false");
```

如上代码示例，得到的输出结果为如下：

```
ret1: false, ret2: true
```

### `stringIsContain()`

#### 宏函数

```c
#define stringIsContain(string, buffer, case_sensitive) _strIsContain(&string, buffer, case_sensitive)
```

#### 函数原型

```c
bool _strIsContain(CString* string, const char* buffer, bool case_sensitive);
```

#### 参数

- `string`：指向 CString 对象的指针。
- `buffer`：指向要比较的字符串的指针。  
- `case_sensitive`：是否区分大小写。

#### 功能

- 检查字符串是否包含指定的子字符串。

#### 示例

比较 CString 对象中是否包含字符串 `"World"`：

```c
CString str = string("Hello, world!");
bool ret1 = stringIsContain(str, "World", true);    // 区分大小写
bool ret2 = stringIsContain(str, "World", false);   // 不区分大小写
printf("ret1 = %s, ret2 = %s\n", ret1 ? "true" : "false", ret2 ? "true" : "false");
```

按照如上代码，得到的输出结果如下：

```
ret1 = false, ret2 = true
```

### `stringSplit()`

#### 宏函数

```c
#define stringSplit(string, char)                       _splitToVector(&string, char)
```

#### 函数原型

```c
CVector _splitToVector(CString* str, const char split);
```

#### 参数

- `str`: 指向 CString 的指针。
- `split`: 分隔符 **（单个字符）**。

#### 功能

- 将字符串按指定分隔符进行分割，将分割的字符串**分别都转换为 [`TYPE_STRING`](CVariant.md/#_3) 类型的 [CVariant](CVariant.md) 变量**存储到 [CVector](CVector.md) 中，并返回该 [CVector](CVector.md)。
- 若找不到指定分隔符，则自动创建一个仅有一个元素的 [CVector](CVector.md)。

!!! note
    - 此函数会**将字符串进行分割**，但不会改变原 CString 对象里的字符串。
    - 此函数不会将原有的 CString 对象销毁！而是返回一个新的 [CVector](CVector.md) 对象。


#### 示例

将字符串 `"Hello, world!"` 按空格进行分割：

```c
CString str = string("Hello, world!");
CVector vec = stringSplit(str, ' ');
for (int i = 0; i < vec.size; ++i) {
    printVarData(vec.data[i]);
    printf("\n");
}
```

执行如上代码后，得到的输出结果如下：

```
Hello,
world!
```

### `stringToVector()`

#### 宏函数

```c
#define stringToVector(string)                          _strToVector(&string)
```

#### 函数原型

```c
CVector _strToVector(CString* str);
```

#### 参数

- `str`: 指向 CString 的指针。

#### 功能

- 将 CString 对象转换为 [CVector](CVector.md) 对象。
- 原有的字符串都将被**拆分为一个个字符**（即 [`TYPE_INT8`](CVariant.md/#_3) 类型的 [CVariant](CVariant.md) 变量），并添加到 [CVector](CVector.md) 中。

!!! note
    - 此函数不会将原有的 CString 对象销毁！而是返回一个新的 [CVector](CVector.md) 对象。

#### 示例

将字符串 `"Hello, world!"` 转换为 [CVector](CVector.md) 对象：

```c
CString str = string("Hello, world!");
CVector vec = stringToVector(str);
for (int i = 0; i < vec.size; ++i) {
    printVarData(vec.data[i]);
    printf(" ");
}
```

执行如上代码后，得到的输出结果如下：

```
H e l l o ,   w o r l d !
```



