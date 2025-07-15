# CArray APIs

## 定义

由以下两个成员变量存储，总共占用 16 字节：

- `elements`：指向 [CVariant](CVariant.md) 数组的指针。
- `length`：数组的长度。

-----

## 构造函数

!!! warning
    以下构造函数必须经过[析构函数](#_13)才能彻底销毁。否则会导致内存泄漏！

### `arrayInit()`

#### 函数原型

```c
CArray arrayInit(size_t length);
```

#### 参数

- `length`：指定数组的长度。

#### 功能

- 初始化一个空的 CArray 数组。
- 分配指定长度的 CVariant 数组，并将其元素初始化为默认值。
- 返回初始化后的 CArray 数组。

### `arrayInitType()`

#### 函数原型

```c
CArray arrayInitType(dataType data_type, size_t length);
```

#### 参数

- `data_type`：指定数组元素的数据类型。
- `length`：指定数组的长度。

#### 功能

- 初始化一个指定数据类型的 CArray 数组。
- 分配指定长度的 CVariant 数组，并将其元素初始化为指定的数据类型的默认值。

### `arrayList()`

#### 函数原型

```c
CArray arrayList(size_t length, ...);
```

#### 参数

- `length`：指定数组的长度。
- `...`：可变参数列表，用于初始化数组元素的值。

#### 功能

- 初始化一个 CArray 数组。
- 分配指定长度的 CVariant 数组，并将其元素初始化为可变参数列表中的值。
- 如果可变参数列表中的值数量不足，剩余的元素将被初始化为默认值。

#### 示例

初始化一个长度为 3 的 CArray 数组，并分别存放不同数据类型的变量。

```c
CArray arr = arrayList(3, varInt(1), varChar('A'), varString("Hello World!")); 
```

-----

## 析构函数

### `destroyArray()`

#### 宏函数

```c
#define destroyArray(arr) _destroyArray(&arr)
```

#### 函数原型

```c
void _destroyArray(CArray *arr);
```

#### 参数

- `arr`：指向要销毁的 CArray 数组的指针。

#### 功能

- 释放 CArray 数组的内存空间。

-----

## 元素相关函数

### `arrayErase()`

#### 宏函数

```c
#define arrayErase(array, start_pos, end_pos)    _arrayErase(&array, start_pos, end_pos)
```

#### 函数原型

```c
void _arrayErase(CArray *array, size_t start_pos, size_t end_pos);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。
- `start_pos`：起始位置。
- `end_pos`：结束位置。

!!! note
    规定的区间范围（按照数学语言）：`[start_pos, end_pos]`

#### 功能

- 擦除 CArray 数组中指定范围内的元素并填充为[空值]()。

### `arrayEraseAll()`

#### 宏函数

```c
#define arrayEraseAll(array)    _arrayEraseAll(&array)
```

#### 函数原型

```c
void _arrayEraseAll(CArray *array);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。

#### 功能

- 擦除 CArray 数组中的所有元素并将其填充为[空值]()。

### `arrayIsContain()`

#### 宏函数

```c
#define arrayIsContain(array, key, pos)          _arrayIsElementContain(&array, key, pos)
```

#### 函数原型

```c
bool _arrayIsElementContain(CArray *array, CVariant key, size_t pos);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。
- `key`：要查找的元素。
- `pos`：起始位置。

#### 功能

- 查找 CArray 数组中指定范围内是否包含指定的元素。
- 返回 `true` 表示包含，`false` 表示不包含。

### `arrayIndexOf()`

#### 宏函数

```c
#define arrayIndexOf(array, element, pos)        _arrayIndexOf(&array, element, pos)
```

#### 函数原型

```c
size_t _arrayIndexOf(CArray *array, CVariant element, size_t pos);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。
- `element`：要查找的元素。
- `pos`：起始位置。

#### 功能

- 查找 CArray 数组中指定范围内第一次出现指定元素的索引。
- 如果找到元素，则返回其索引；否则返回 `SIZE_MAX` 或 `N_POS`。

### `arrayAt()`

#### 宏函数

```c
#define arrayAt(array, index)                    _arrayAt(&array, index)
```

#### 函数原型

```c
CVariant _arrayAt(CArray *array, size_t index);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。
- `index`：要访问的元素的索引。

#### 功能

- 获取 CArray 数组中指定索引位置的元素。
- 如果索引超出数组范围，则返回[空类型的元素]()。

### `arrayModify()`

#### 宏函数

```c
#define arrayModify(array, index, value)         _arrayModify(&array, index, value)
```

#### 函数原型

```c
void _arrayModify(CArray *array, size_t index, CVariant value);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。
- `index`：要修改的元素的索引。
- `value`：要设置的新值。

#### 功能
- 修改 CArray 数组中指定索引位置的元素的值。
- 如果索引超出数组范围，则不进行任何操作。

