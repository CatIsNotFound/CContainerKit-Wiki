# CVector APIs

## 定义

CVector 是一个动态数组，用于存储多个 [CVariant](CVariant.md) 类型的元素。使用此容器，可以在有限的存储空间中方便管理并操作多个变量。

由以下两个成员变量存储，总共占用 16 字节：

- `elements`：指向 [CVariant](CVariant.md) 数组的指针。
- `length`：动态数组的长度。
- `capacity`：动态数组的容量（用于实现动态扩容）。

## 构造函数

!!! warning "注意"
    以下构造函数必须经过[析构函数](#_12)才能彻底销毁。否则会导致内存泄漏！

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

见 [`_vectorInsert()`](#_32) 原型函数介绍。

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

见 [`_vectorInsert()`](#_32) 原型函数介绍。

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



