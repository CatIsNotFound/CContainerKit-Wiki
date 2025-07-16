# CVariant APIs

## 定义

CVariant 是一个数据类型（并非容器），用于存储任意类型的数据。也是大多数容器存储变量的数据类型。

关于此节，这里需要着重介绍并说明：

### 结构定义

CVariant 类的结构定义如下：

```c
typedef struct {
    void*    value;     // 数据值
    uint8_t  data_type; // 数据类型
} CVariant;
```

对应的成员变量如下：

- `value`：数据值，用于存储任意类型的数据。
- `data_type`：定义数据类型（即：枚举类型），用于标识该值所对应的数据类型。

### 数据类型定义

CVariant 类包含多种数据类型定义，具体如下表：

| 数据类型 | 对应值 | 描述 |
| --- | --- | --- |
| `TYPE_NULL` | 0 | 空类型 |
| `TYPE_BOOL` | 1 | 布尔类型 |
| `TYPE_INT8` | 2 | 有符号 8 位整数类型 |
| `TYPE_UINT8` | 3 | 无符号 8 位整数类型 |
| `TYPE_INT16` | 4 | 有符号 16 位整数类型 |
| `TYPE_UINT16` | 5 | 无符号 16 位整数类型 |
| `TYPE_INT32` | 6 | 有符号 32 位整数类型 |
| `TYPE_UINT32` | 7 | 无符号 32 位整数类型 |
| `TYPE_INT64` | 8 | 有符号 64 位整数类型 |
| `TYPE_UINT64` | 9 | 无符号 64 位整数类型 |
| `TYPE_FLOAT` | 10 | 单精度浮点数类型 |
| `TYPE_DOUBLE` | 11 | 双精度浮点数类型 |
| `TYPE_LONG_DOUBLE` | 12 | 长精度浮点数类型 |
| `TYPE_POINTER` | 13 | 指针类型 |
| `TYPE_STRING` | 14 | 字符串类型 `(const char *)` |
| `TYPE_STRUCT` | 15 | 结构体类型 |
| `TYPE_ENUM` | 16 | 枚举类型 |
| `TYPE_FUNCTION` | 17 | 函数指针类型 |
| `TYPE_CUSTOM` | 18 | 自定义类型 |


## 专用类型定义

CVariant 类针对某些特殊或复杂数据类型，定义了专用类型定义，具体如下：

### VStruct

`VStruct` 是针对于类型为 `TYPE_STRUCT` 的数据结构，具体结构如下：

```c
typedef struct VStruct {
    void* data;
    const char* type_name;
} VStruct;
```

对应的成员变量如下：

- `data`：结构体数据指针。
- `type_name`：结构体类型名称。

!!! danger "特别注意"
    `VStruct` 类型的数据结构，其 `data` 成员变量指向的是一个结构体类型的指针，而不是结构体类型本身。

!!! warning
    由于 `VStruct` 类型的数据结构，其 `data` 成员变量指向的是一个结构体类型的指针，因此在使用 `VStruct` 类型的数据结构时，需要注意以下几点：

    - 在使用 `VStruct` 类型的数据结构时，需要确保 `data` 成员变量指向的结构体类型的定义与实际使用的结构体类型定义一致。
    - `VStruct` 类型**不负责结构体类型的内存分配和释放**，因此在使用 `VStruct` 类型的数据结构时，需要确保结构体类型的内存分配和释放与实际使用的结构体类型定义一致。

### VEnum

`VEnum` 是针对于类型为 `TYPE_ENUM` 的枚举类型，具体结构如下：

```c
typedef struct VEnum {
    int8_t* data;
    const char* type_name;
} VEnum;
```

对应的成员变量如下：

- `data`：枚举类型数据指针。
- `type_name`：枚举类型名称。

### VCustom

`VCustom` 是针对于类型为 `TYPE_CUSTOM` 的自定义类型，具体结构如下：

```c
typedef struct VCustom {
    void* data;
    const char* type_name;
} VCustom;
```

对应的成员变量如下：

- `data`：自定义类型数据指针。
- `type_name`：自定义类型名称。

!!! bug
    目前 `VCustom` 暂未使用，后续会进行完善。


## 定义变量函数

CVariant 类对应不同的[数据类型定义](#_3)，提供了多种其对应的定义变量函数，具体如下：

### `_varEmpty()`

#### 函数原型

```c
CVariant _varEmpty(void);
```

#### 返回值

- `CVariant`：返回一个空类型 (类似于 `void`) 的空对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_NULL`，数据值为 `NULL`。

### `varBool()`

#### 宏函数

```c
#define varBool(value)      _varBool(value)
```

#### 函数原型

```c
CVariant _varBool(bool value);
```

#### 参数

- `value`：布尔类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_BOOL`，数据值为 `value`。

### `varChar()`

#### 宏函数

```c
#define varChar(value)      _varInt8(value)
```

#### 函数原型

```c
CVariant _varInt8(int8_t value);
```

#### 参数

- `value`：有符号 8 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_INT8`，数据值为 `value`。

### `varUChar()`

#### 宏函数

```c
#define varUChar(value)     _varUInt8(value)
```

#### 函数原型

```c
CVariant _varUInt8(uint8_t value);
```

#### 参数

- `value`：无符号 8 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_UINT8`，数据值为 `value`。

### `varShort()`

#### 宏函数
```c
#define varShort(value)     _varInt16(value)
```

#### 函数原型

```c
CVariant _varInt16(int16_t value);
```

#### 参数

- `value`：有符号 16 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_INT16`，数据值为 `value`。

### `varUShort()`

#### 宏函数

```c
#define varUShort(value)    _varUInt16(value)
```

#### 函数原型

```c
CVariant _varUInt16(uint16_t value);
```

#### 参数

- `value`：无符号 16 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_UINT16`，数据值为 `value`。

### `varInt()`

#### 宏函数

```c
#define varInt(value)       _varInt32(value)
```

#### 函数原型

```c
CVariant _varInt32(int32_t value);
```

#### 参数

- `value`：有符号 32 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_INT32`，数据值为 `value`。

### `varUInt()`

#### 宏函数

```c
#define varUInt(value)      _varUInt32(value)
```

#### 函数原型

```c
CVariant _varUInt32(uint32_t value);
```

#### 参数

- `value`：无符号 32 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_UINT32`，数据值为 `value`。

### `varLongLong()`

#### 宏函数

```c
#define varLongLong(value)  _varInt64(value)
```

#### 函数原型

```c
CVariant _varInt64(int64_t value);
```

#### 参数

- `value`：有符号 64 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_INT64`，数据值为 `value`。

### `varULongLong()`

#### 宏函数

```c
#define varULongLong(value) _varUInt64(value)
```

#### 函数原型

```c
CVariant _varUInt64(uint64_t value);
```

#### 参数

- `value`：无符号 64 位整数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_UINT64`，数据值为 `value`。

### `varFloat()`

#### 宏函数

```c
#define varFloat(value)     _varFloat(value)
```

#### 函数原型

```c
CVariant _varFloat(float value);
```

#### 参数

- `value`：单精度浮点数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_FLOAT`，数据值为 `value`。

### `varDouble()`

#### 宏函数

```c
#define varDouble(value)    _varDouble(value)
```

#### 函数原型

```c
CVariant _varDouble(double value);
```

#### 参数

- `value`：双精度浮点数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_DOUBLE`，数据值为 `value`。

### `varLongDouble()`

#### 宏函数

```c
#define varLongDouble(value) _varLongDouble(value)
```

#### 函数原型

```c
CVariant _varLongDouble(long double value);
```

#### 参数

- `value`：长精度浮点数类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_LONG_DOUBLE`，数据值为 `value`。

### `varPointer()`

#### 宏函数

```c
#define varPointer(value)   _varPointer(value)
```

#### 函数原型

```c
CVariant _varPointer(void* value);
```

#### 参数

- `value`：指针类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_POINTER`，数据值为 `value`。

### `varString()`

#### 宏函数

```c
#define varString(value)    _varString(value)
```

#### 函数原型

```c
CVariant _varString(const char* value);
```

#### 参数

- `value`：字符串类型数据。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_STRING`，数据值为 `value`。

!!! warning
    由于 `varString()` 函数的参数为 `const char*` 类型（也就是定义**字符串的指针**），因此在使用该函数时，需要确保传入的字符串的定义与实际使用的字符串定义一致。
    例如，定义字符串时使用的是 `char str[]` 而实际使用的是 `char* str`，则会导致字符串的定义不一致，从而导致程序出现错误。

### `varStruct()`

#### 宏函数

```c
#define varStruct(value, type_name)  _varStruct(value, type_name)
```

#### 函数原型

```c
CVariant _varStruct(void* value, const char* type_name);
```

#### 参数

- `value`：结构体类型数据。
- `type_name`：结构体类型名称。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_STRUCT`，
  `value` 参数即为 [`VStruct`](#vstruct) 数据指针，`type_name` 参数即为结构体类型名称。

!!! warning
    由于 `varStruct()` 函数的参数为 `void*` 类型（也就是定义**结构体的指针**），因此在使用该函数时，需要确保传入的结构体类型数据的定义与实际使用的结构体类型定义一致。

### `varEnum()`

#### 宏函数

```c
#define varEnum(value, type_name)  _varEnum(value, type_name)
```

#### 函数原型

```c
CVariant _varEnum(int8_t* value, const char* type_name);
```

#### 参数

- `value`：枚举类型数据。
- `type_name`：枚举类型名称。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_ENUM`，
  `value` 参数即为 [`VEnum`](#venum) 数据指针，`type_name` 参数即为枚举类型名称。

### `varFunction()`

#### 宏函数

```c
#define varFunction(function)  _varFunction(function)
```

#### 函数原型

```c
CVariant _varFunction(void (*value)(void*));
```

#### 参数

- `value`：函数指针。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_FUNCTION`，
  `value` 参数即为函数指针。

!!! bug
    目前 `varFunction()` 函数暂未使用，后续会进行完善。

### `varCustom()`

#### 宏函数

```c
#define varCustom(value, type_name, destructor)   _varCustom(value, type_name, destructor)
```

#### 函数原型

```c
CVariant _varCustom(void *value, const char *type_name, void (*destructor)(void *));
```

#### 参数

- `value`：自定义类型数据。
- `type_name`：自定义类型名称。
- `destructor`：自定义类型析构函数。

#### 返回值

- `CVariant`：返回一个 `CVariant` 类型的对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 `TYPE_CUSTOM`，
  `value` 参数即为 [`VCustom`](#vcustom) 数据指针，`type_name` 参数即为自定义类型名称，`destructor` 参数即为自定义类型析构函数。

!!! bug
    目前 `varCustom()` 函数暂未使用，后续会进行完善。

