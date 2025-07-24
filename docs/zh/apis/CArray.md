# CArray APIs

## 定义

CArray 是一个静态数组，用于存储多个 [CVariant](CVariant.md) 类型的元素。使用此容器，可以在有限的存储空间中方便管理并操作多个变量。

!!! note
    区别于 [CVector](CVector.md)，CArray 的**数组长度（大小）是固定的**，无法动态调整。

由以下两个成员变量存储，总共占用 16 字节：

- `elements`：指向 [CVariant](CVariant.md) 数组的指针。
- `length`：数组的长度。

-----

## 构造函数

!!! warning
    以下构造函数必须经过[析构函数](#_15)才能彻底销毁。否则会导致内存泄漏！

### `arrayInit()`

#### 函数原型

```c
CArray arrayInit(size_t length);
```

#### 参数

- `length`：指定数组的长度。

#### 功能

- 初始化一个空的 CArray 数组。
- 分配指定长度的 [CVariant](CVariant.md) 数组，并将其元素初始化为默认值。
- 返回初始化后的 CArray 数组。

#### 示例

初始化一个长度为 3 的 CArray 数组。

```c
CArray arr = arrayInit(3);
```

通过遍历该数组获得每个元素，将会得到 3 个[空类型]()的 [CVariant](CVariant.md) 变量。

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
- 分配指定长度的 [CVariant](CVariant.md) 数组，并将其元素初始化为指定的数据类型的默认值。
- 返回初始化后的 CArray 数组。

#### 示例

初始化一个长度为 3 的 CArray 数组，每个元素的数据类型为 `int`。

```c
CArray arr = arrayInitType(TYPE_INT, 3);
```

通过遍历该数组获得每个元素，将会得到 3 个[整数类型]()的 [CVariant](CVariant.md) 变量且值为 0。

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
- 分配指定长度的 [CVariant](CVariant.md) 数组，并将其元素初始化为可变参数列表中的值。
- 如果可变参数列表中的值数量不足，剩余的元素将被初始化为默认值。

#### 示例

初始化一个长度为 3 的 CArray 数组，并分别存放不同数据类型的变量。

```c
CArray arr = arrayList(3, varInt(1), varChar('A'), varString("Hello World!")); 
```

通过遍历该数组获得每个元素，将会得到 3 个不同类型的 [CVariant](CVariant.md) 变量。

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

- 仅释放 CArray 数组的内存空间，**而不会释放数组中的元素。**


### `deleteArray()`

#### 宏函数

```c
#define deleteArray(arr) _deleteArray(&arr)
```

#### 函数原型

```c
void _deleteArray(CArray *arr);
```

#### 参数

- `arr`：指向要销毁的 CArray 数组的指针。

#### 功能

- 释放 CArray 数组的内存空间，**并会释放数组中的元素。**


-----

## 元素相关函数

### `arrayExpand()`

#### 宏函数

```c
#define arrayExpand(array, new_length)           _arrayExpand(&array, new_length)
```

#### 函数原型

```c
void _arrayExpand(CArray *array, size_t length);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。
- `length`：新的长度。

#### 功能

- 扩展 CArray 数组的长度。
- 如果新的长度小于当前长度，将不会进行任何操作。
- 如果新的长度大于当前长度，将扩展数组长度并将新增的元素初始化为空值。

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
    首先，规定的区间范围（按照数学语言）：`[start_pos, end_pos]`，即包含 `start_pos` 和 `end_pos` 位置的元素。

    其次，还需要满足以下条件：`start_pos <= end_pos`。

#### 功能

- 释放 CArray 数组中指定范围内的元素并填充为空值。



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

- 释放 CArray 数组中的所有元素并将其填充为空值。

### `arrayIsEqual()`

#### 宏函数

```c
#define arrayIsEqual(array, key, pos, cmp)       _arrayIsElementEqual(&array, key, pos, cmp)
```

#### 函数原型

```c
bool _arrayIsElementEqual(CArray *array, CVariant key, size_t pos, CmpFunc cmp);
```

#### 参数

- `array`：指向要操作的 CArray 数组的指针。
- `key`：要查找的元素。
- `pos`：起始位置。
- `cmp`：比较函数指针，用于自定义元素的比较方式。如果为 `NULL`，则使用默认的比较方式。

#### 功能

- 比较 CArray 数组中指定位置的元素是否与指定的元素相等。
- 如果相等，返回 `true`；否则返回 `false`。

### `arrayIsContain()`

#### 宏函数

```c
#define arrayIsContain(array, key, pos)          _arrayIsElementEqual(&array, key, pos, NULL)
```

#### 函数原型

见 [`arrayIsEqual()`](#arrayisequal) 函数原型。

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

