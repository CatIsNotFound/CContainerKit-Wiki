# CVector APIs

## 定义

CVector 是一个动态数组，用于存储多个 [CVariant](CVariant.md) 类型的元素。使用此容器，可以在有限的存储空间中方便管理并操作多个变量。

由以下两个成员变量存储，总共占用 16 字节：

- `elements`：指向 [CVariant](CVariant.md) 数组的指针。
- `length`：动态数组的长度。
- `capacity`：动态数组的容量（用于实现动态扩容）。

## 构造函数

!!! warning "注意"
    以下构造函数必须经过[析构函数](#_15)才能彻底销毁。否则会导致内存泄漏！

### `vectorInit()`

#### 函数原型

```c
CVector vectorInit(size_t length);
```

#### 参数

- `length`：指定动态数组的长度。

#### 功能

- 初始化一个空的 CVector 类。
- 分配指定长度的 [CVariant](CVariant.md) 数组，并将其元素初始化为默认值。
- 返回初始化后的 CVector 类。

#### 示例

创建一个长度为 3 的动态数组：

```c
CVector vec = vectorInit(3);
for (size_t i = 0; i < vec.length; ++i) {
    printf("[%zu] ", i);
    printVarData(vec.elements[i]);
    printf(", ");
} 
```

执行后，CVector 里的元素为：`[0] (null), [1] (null), [2] (null)`。

!!! tip "另请参阅"
    - [printVarData()](CVariant.md#printvardata)


### `vectorInitType()`

#### 函数原型

```c
CVector vectorInitType(dataType data_type, size_t length);
```

#### 参数

- `data_type`：指定动态数组元素的数据类型。
- `length`：指定动态数组的长度。

#### 功能

- 初始化一个指定数据类型的 CVector 类。
- 分配指定长度的 [CVariant](CVariant.md) 数组，并将其元素初始化为指定的数据类型的默认值。
- 返回初始化后的 CVector 类。

#### 示例

创建一个长度为 5 的动态数组，元素类型为 `int` 并将其初始化为 `100`。

```c
CVector vec = vectorInitType(TYPE_INT32, 5);
for (size_t i = 0; i < vec.length; ++i) {
    printf("[%zu] ", i);
    printVarData(vec.elements[i]);
    printf(", ");
} 
```

执行后，CVector 里的元素为：`[0] 100, [1] 100, [2] 100, [3] 100, [4] 100`。

!!! tip "另请参阅"

    - [TYPE_INT32](CVariant.md#_3)
    - [printVarData()](CVariant.md#printvardata) 
    
### `vectorList()`

#### 函数原型

```c
CVector vectorList(size_t length, ...);
```

#### 参数

- `length`：指定动态数组的长度。
- `...`：指定动态数组元素的值。要求使用 [CVariant](CVariant.md) 类型的参数。

#### 功能

- 初始化一个 CVector 类。
- 分配指定长度的 [CVariant](CVariant.md) 数组，并将其元素初始化为指定的值。
- 如果指定的值数量不足，剩余的元素将被初始化为默认值。
- 返回初始化后的 CVector 类。

#### 示例

创建一个长度为 3 的动态数组，并分别定义不同数据类型的变量。

```c
float f = 12.34f;
CVector vec = vectorList(3, varInt(100), varFloat(f), varString("hello"));
```

定义后的 CVector 里的元素为：`100, 12.34, hello`（浮点值结果不一定正确，需要根据实际情况调试）。

!!! tip "另请参阅"
    如果对示例中的部分函数有疑惑，建议访问任一函数的详细文档。

    - [varInt()](CVariant.md#varint)
    - [varFloat()](CVariant.md#varfloat)
    - [varString()](CVariant.md#varstring)


## 析构函数

### `destroyVector()`

#### 宏函数

```c
#define destroyVector(vector)                    _destroyVector(&vector)
```

#### 函数原型

```c
void _destroyVector(CVector *vector);
```

#### 参数

- `vector`：指向要销毁的 CVector 类的指针。

#### 功能

- 仅销毁 CVector，**但不会销毁 CVector 里的每个元素**。

### `deleteVector()`

#### 宏函数

```c
#define deleteVector(vector)                     _deleteVector(&vector)
```

#### 函数原型

```c
void _deleteVector(CVector *vector);
```

#### 参数

- `vector`：指向要销毁的 CVector 类的指针。

#### 功能

- **销毁 CVector 和 CVector 里的每个元素**，并释放其占用的内存。

## 类型转换函数

### `arrToVector()`

#### 宏函数

```c
#define arrToVector(array, delete_array, ok)     _arrayToVector(&array, delete_array, ok)
```

#### 函数原型

```c
CVector _arrayToVector(CArray *array, bool delete_array, bool *ok);
```

#### 参数

- `array`：指向要转换的 [CArray](CArray.md) 数组的指针。
- `delete_array`：指定是否删除 [CArray](CArray.md) 数组。
- `ok`：指向布尔值的指针，用于指示转换是否成功。

#### 功能

- 将 [CArray](CArray.md) 数组转换为 CVector 类。
- 转换后的 CVector 类将拥有 [CArray](CArray.md) 数组的元素。
- 如果 `delete_array` 为 `true`，则转换后的 CVector 类将负责删除 [CArray](CArray.md) 数组。
- 如果转换失败，`ok` 指向的布尔值将被设置为 `false`。

### `vecToArray()`

#### 宏函数

```c
#define vecToArray(vector, delete_vector, ok)    _vectorToArray(&vector, delete_vector, ok)
```

#### 函数原型

```c
CArray _vectorToArray(CVector *vector, bool delete_vector, bool *ok);
```

#### 参数

- `vector`：指向要转换的 CVector 类的指针。
- `delete_vector`：指定是否删除 CVector 类。
- `ok`：指向布尔值的指针，用于指示转换是否成功。

#### 功能

- 将 CVector 类转换为 [CArray](CArray.md) 数组。
- 转换后的 [CArray](CArray.md) 数组将拥有 CVector 类的元素。
- 如果 `delete_vector` 为 `true`，则转换后的 [CArray](CArray.md) 数组将负责删除 CVector 类。
- 如果转换失败，`ok` 指向的布尔值将被设置为 `false`。

## 添加、删除元素函数

### `vecInsertOne()`

#### 宏函数

```c
#define vecInsertOne(vector, index, var)         _vectorInsert(&vector, index, var)
```

#### 函数原型

```c
void _vectorInsert(CVector* vector, size_t index, CVariant new_value);
```

#### 参数

- `vector`：指向要插入元素的 CVector 类的指针。
- `index`：指定插入元素的索引位置。
- `new_value`：指定要插入的元素值。

#### 功能

- 在指定索引位置插入一个元素。
- 如果索引位置超出范围，将导致未定义行为。
- 插入元素后，CVector 类的长度将增加 1。
- 插入元素后，CVector 类的容量将根据需要自动调整。

### `vecPushBack()`

#### 宏函数

```c
#define vecPushBack(vector, var)                 _vectorInsert(&vector, vector.length, var);
```

#### 函数原型

见 [`_vectorInsert()`](#_35) 原型函数介绍。

#### 参数

- `vector`：指向要插入元素的 CVector 类的指针。
- `var`：指定要插入的元素值。

#### 功能

插入元素到 CVector 类的末尾。

### `vecPushFront()`

#### 宏函数

```c
#define vecPushFront(vector, var)                _vectorInsert(&vector, 0, var)
```

#### 函数原型

见 [`_vectorInsert()`](#_35) 原型函数介绍。

#### 参数

- `vector`：指向要插入元素的 CVector 类的指针。
- `var`：指定要插入的元素值。

#### 功能

插入元素到 CVector 类的开头。

### `vecInsertFromArray()`

#### 宏函数

```c
#define vecInsertFromArray(vector, index, array) _vectorInsertFromArray(&vector, index, &array)
```

#### 函数原型

```c
void _vectorInsertFromArray(CVector *vector, size_t index, CArray *array);
```

#### 参数

- `vector`：指向要插入元素的 CVector 类的指针。
- `index`：指定插入元素的索引位置。
- `array`：指向要插入的 [CArray](CArray.md) 数组的指针。

#### 功能

- 在指定索引位置插入一个 [CArray](CArray.md) 数组。
- **如果索引位置超出范围，将自动在 CVector 的尾部添加数组。**

#### 示例

将一个 [CArray](CArray.md) 数组插入到 CVector 类的第 2 个索引位置：

```c
CVector *vec = vectorInitType(TYPE_INT, 5);
CArray *arr = arrayList(3, varInt(1), varInt(2), varInt(3));
vectorInsertFromArray(vec, 2, arr);
```

原有的 CVector 类元素为：`0, 0, 0, 0, 0`，

经过执行上述代码后，CVector 类的元素将变为：`0, 0, 1, 2, 3, 0, 0, 0`。

!!! tip "另请参阅"
    - [arrayList()](CArray.md#arraylist): 用于创建一个 [CArray](CArray.md) 数组。

### `vecAppendArray()` / `vecPushBackArray()`

#### 宏函数

```c
#define vecAppendArray(vector, array)            _vectorInsertFromArray(&vector, vector.length, &array)
#define vecPushBackArray(vector, array)          vecAppendArray(vector, array)
```

#### 函数原型

见 [`_vectorInsertFromArray()`](CVector.md#_47) 函数原型。

#### 参数

- `vector`: 指定 CVector 类的对象
- `array`: 插入的 [CArray](CArray.md) 数组

#### 功能

在指定 CVector 类的对象中，将 [CArray](CArray.md) 数组追加到 CVector 类的末尾。

### `vecPushFrontArray()`

#### 宏函数

```c
#define vecPushFrontArray(vector, array)         _vectorInsertFromArray(&vector, 0, &array)
```

#### 函数原型

见 [`_vectorInsertFromArray()`](CVector.md#_47) 函数原型。

#### 参数

- `vector`: 指定 CVector 类的对象
- `array`: 插入的 [CArray](CArray.md) 数组

#### 功能

在指定 CVector 类的对象中，将 [CArray](CArray.md) 数组追加到 CVector 类的开头。

### `vecRemove()`

#### 宏函数

```c
#define vecRemove(vector, start_pos, count)      _vectorRemove(&vector, start_pos, count)
```

#### 函数原型

```c
void _vectorRemove(CVector* vector, size_t start_pos, size_t count);
```

#### 参数

- `vector`: 指定 CVector 类的对象
- `start_pos`: 起始位置
- `count`: 要删除的元素数量

#### 功能

删除 CVector 类的对象中指定范围内的元素。

### `vecRemoveOne()`

#### 宏函数

```c
#define vecRemoveOne(vector, index)              _vectorRemove(&vector, index, 1)
```

#### 函数原型

见 [`_vectorRemove()`](CVector.md#_60) 函数原型。

#### 参数

- `vector`: 删除的 CVector 类的对象
- `index`: 删除的索引位置

#### 功能

删除 CVector 类的对象中指定索引位置的元素。

### `vecPopBack()`

#### 宏函数

```c
#define vecPopBack(vector)                       _vectorRemove(&vector, vector.length - 1, 1)
```

#### 函数原型

见 [`_vectorRemove()`](CVector.md#_60) 函数原型。

#### 参数

- `vector`: 删除的 CVector 类的对象

#### 功能

删除 CVector 类的对象中的最后一个元素。

### `vecPopFront()`

#### 宏函数

```c
#define vecPopFront(vector)                      _vectorRemove(&vector, 0, 1)
```

#### 函数原型

见 [`_vectorRemove()`](CVector.md#_60) 函数原型。

#### 参数

- `vector`: 删除的 CVector 类的对象

#### 功能

删除 CVector 类的对象中的第一个元素。

### `vecClear()`

#### 宏函数

```c
#define vecClear(vector)                         _vectorClear(&vector)
```

#### 函数原型

```c
void _vectorClear(CVector *vector);
```

#### 参数

- `vector`: 删除的 CVector 类的对象

#### 功能

删除 CVector 类的对象中的所有元素。

## 访问元素函数

### `vecIsContain()`

#### 宏函数

```c
#define vecIsContain(vector, key, start_pos)     _vectorIsElementContain(&vector, key, start_pos)
```

#### 函数原型

```c
bool _vectorIsElementContain(CVector* vector, CVariant key, size_t start_pos);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针
- `key`: 指定要查找的元素
- `start_pos`: 指定开始查找的索引位置

#### 功能

判断 CVector 类的对象中是否包含指定元素。

### `vecIndexOf()`

#### 宏函数

```c
#define vecIndexOf(vector, key, start_pos)       _vectorIndexOf(&vector, key, start_pos)
```

#### 函数原型

```c
size_t _vectorIndexOf(CVector *vector, CVariant element, size_t start_pos);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针
- `element`: 指定要查找的元素
- `start_pos`: 指定开始查找的索引位置

#### 功能

返回 CVector 类的对象中指定元素的索引位置。如果未找到指定元素，返回 `N_POS` 或 `SIZE_MAX`。

### `vecAt()`

#### 宏函数

```c
#define vecAt(vector, index)                     _vectorAt(&vector, index)
```

#### 函数原型

```c
CVariant _vectorAt(CVector *vector, size_t index);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针
- `index`: 指定要访问的元素的索引位置

#### 功能

返回 CVector 类的对象中指定索引位置的元素。如果索引超出范围，返回空类型的元素（[`varEmpty()`](CVariant.md#varempty)）。

## 修改元素函数

### `vecModify()`

#### 宏函数

```c
#define vecModify(vector, index, new_var)        _vectorModify(&vector, index, new_var)
```

#### 函数原型

```c
bool _vectorModify(CVector *vector, size_t index, CVariant new_var);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针
- `index`: 索引位置
- `new_var`: 新的元素

#### 功能

修改 CVector 类的对象中指定索引位置的元素，并返回 `true`；若索引超出范围，则返回 `false`。

### `vecFill()`

#### 宏函数

```c
#define vecFill(vector, start_pos, count, var)   _vectorFill(&vector, start_pos, count, var)
```

#### 函数原型

```c
void _vectorFill(CVector* vector, size_t start_pos, uint32_t count, CVariant new_value);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针
- `start_pos`: 起始位置
- `count`: 要填充的元素数量
- `new_value`: 要填充的新元素

#### 功能

填充 CVector 类的对象中指定范围内的元素为新元素。

### `vecFillAll()`

#### 宏函数

```c
#define vecFillAll(vector, var)                  _vectorFill(&vector, 0, vector.length, var)
```

#### 函数原型

见 [`_vectorFill()`](CVector.md#_98) 函数原型。

#### 参数

- `vector`: 填充的 CVector 类的对象
- `var`: 要填充的新元素

#### 功能

填充 CVector 类的对象中的**所有元素**为新元素。

## 其它函数

### `vecSubVector()`

#### 宏函数

```c
#define vecSubVector(vector, start_pos, count)   _vectorSubVec(&vector, start_pos, count)
```

#### 函数原型

```c
CVector _vectorSubVec(CVector* vector, size_t start_pos, uint32_t count);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针
- `start_pos`: 起始位置
- `count`: 要提取的元素数量

#### 功能

- 返回 CVector 类的对象中指定范围内的元素组成的新的 CVector 类的对象。

#### 示例

从 `vector1` 中提取索引位置为 1 开始取 3 个元素，并保存在 `vector2` 中：

```c
CVector vector1 = vectorList(5, varInt(1), varInt(2), varInt(3), varInt(4), varInt(5));
CVector vector2 = vecSubVector(vector1, 1, 3);
for (size_t i = 0; i < vector2.length; ++i) {
    printVarData(vector2.data[i]);
    printf(", ");
}
```

`vector2` 的元素为：`2, 3, 4`。

### `resizeVector()` / `assignVector()`

#### 宏函数

```c
#define resizeVector(vector, new_size)           _resizeVector(&vector, new_size)
#define assignVector(vector, new_size)           _resizeVector(&vector, new_size)
```

#### 函数原型

```c
void _resizeVector(CVector* vector, size_t new_size);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针
- `new_size`: 新的大小

#### 功能

- 调整 CVector 类的对象的容量（即 [`capacity`](CVector.md#_1) 成员变量）设置为新的大小，并重新分配新的内存。

!!! note "注意"
    该函数**不支持直接缩小当前对象的大小**。如有需求，请调用 [`shrinkToFitVector()`](CVector.md#shrinktofitvector) 函数。

### `shrinkToFitVector()`

#### 宏函数

```c
#define shrinkToFitVector(vector, new_size)      _shrinkToFitVector(&vector, new_size)
```

#### 函数原型

```c
void _shrinkToFitVector(CVector* vector, size_t new_size);
```

#### 参数

- `vector`: 指向 CVector 类的对象的指针。
- `new_size`: 新的数组大小。

#### 功能

- 调整 CVector 类的对象的容量（即 [`capacity`](CVector.md#_1) 成员变量）设置为新的大小，并重新分配新的内存。
- 缩小原有容量：若 `new_size` 小于当前对象的长度（即 [`size`](CVector.md#_1) 成员变量），则删除并丢弃多余的元素。

