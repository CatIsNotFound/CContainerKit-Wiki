# CVector APIs

## 定义

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

### `vectorList()`

#### 函数原型

```c
CVector vectorList(size_t length,...);
```

#### 参数

- `length`：指定动态数组的长度。
- `...`：指定动态数组元素的值。要求使用 [CVariant](CVariant.md) 类型的参数。

#### 功能

- 初始化一个 CVector 类。
- 分配指定长度的 [CVariant](CVariant.md) 数组，并将其元素初始化为指定的值。
- 如果指定的值数量不足，剩余的元素将被初始化为默认值。
- 返回初始化后的 CVector 类。

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

- 销毁 CVector 类，并释放其占用的内存。

