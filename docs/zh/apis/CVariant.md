# CVariant APIs

## 定义

CVariant 是一个数据类型（并非容器），用于存储任意类型的数据。也是大多数容器存储变量的数据类型。

关于此节，这里需要着重介绍并说明：

### 结构定义

CVariant 类的结构定义如下：

```c
typedef struct {
    void*    value;     
    uint8_t  data_type; 
} CVariant;
```

对应的成员变量如下：

- `value`：数据值，用于存储任意类型的数据。
- `data_type`：定义数据类型（即：枚举类型），用于标识该值所对应的数据类型。

### 数据类型定义

CVariant 类包含多种数据类型定义，具体如下表：

| 数据类型 | 对应值 | 描述 | 输出时显示内容 |
| --- | --- | --- | --- |
| `TYPE_NULL` | 0 | 空类型 | undefined |
| `TYPE_BOOL` | 1 | 布尔类型 | bool |
| `TYPE_INT8` | 2 | 有符号 8 位整数类型 | signed char (int8) |
| `TYPE_UINT8` | 3 | 无符号 8 位整数类型 | unsigned char (uint8) |
| `TYPE_INT16` | 4 | 有符号 16 位整数类型 | short (int16) |
| `TYPE_UINT16` | 5 | 无符号 16 位整数类型 | unsigned short (uint16) |
| `TYPE_INT32` | 6 | 有符号 32 位整数类型 | int (int32) |
| `TYPE_UINT32` | 7 | 无符号 32 位整数类型 | unsigned int (uint32) |
| `TYPE_INT64` | 8 | 有符号 64 位整数类型 | long long int (int64) |
| `TYPE_UINT64` | 9 | 无符号 64 位整数类型 | unsigned long long int (uint64) |
| `TYPE_FLOAT` | 10 | 单精度浮点数类型 | float |
| `TYPE_DOUBLE` | 11 | 双精度浮点数类型 | double |
| `TYPE_LONG_DOUBLE` | 12 | 长精度浮点数类型 | long double |
| `TYPE_POINTER` | 13 | 指针类型 | void* (pointer) |
| `TYPE_STRING` | 14 | 字符串类型 `(const char *)` | string (const char *) |
| `TYPE_STRUCT` | 15 | 结构体类型 | struct |
| `TYPE_ENUM` | 16 | 枚举类型 | enum |
| `TYPE_FUNCTION` | 17 | 函数指针类型 | function |
| `TYPE_CUSTOM` | 18 | 自定义类型 | custom |


## 专用类型定义

CVariant 类针对某些特殊或复杂数据类型，定义了专用类型定义，具体如下：

### VStruct

`VStruct` 是针对于类型为 [`TYPE_STRUCT`](#_3) 的数据结构，具体结构如下：

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

`VEnum` 是针对于类型为 [`TYPE_ENUM`](#_3) 的枚举类型，具体结构如下：

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

`VCustom` 是针对于类型为 [`TYPE_CUSTOM`](#_3) 的自定义类型，具体结构如下：

```c
typedef struct VCustom {
    void* data;
    const char* type_name;
} VCustom;
```

对应的成员变量如下：

- `data`：自定义类型数据指针。
- `type_name`：自定义类型名称。


## 获取变量数据类型

`CVariant` 可以用于获取变量数据类型。具体如下：

### `varTypeName()`

#### 宏函数

```c
#define varTypeName(variant)    _varTypeName(&variant)
```

#### 函数原型

```c
const char *_varTypeName(CVariant *variant);
```

#### 参数

- `variant`：CVariant 类型对象。

#### 返回值

返回 CVariant 类型对象所对应的数据类型名称。

!!! tip
    关于 CVariant 类型对象的数据类型名称，请参考 [数据类型定义](#_3) 一节。



## 定义变量函数

CVariant 类对应不同的[数据类型定义](#_3)，提供了多种其对应的定义变量函数，具体如下：

### `varEmpty()`

#### 函数原型

```c
CVariant varEmpty(void);
```

#### 返回值

- `CVariant`：返回一个空类型 (类似于 `void`) 的空对象。

#### 描述

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_NULL`](#_3)，数据值为 `NULL`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_BOOL`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_INT8`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_UINT8`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_INT16`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_UINT16`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_INT32`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_UINT32`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_INT64`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_UINT64`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_FLOAT`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_DOUBLE`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_LONG_DOUBLE`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_POINTER`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_STRING`](#_3)，数据值为 `value`。

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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_STRUCT`](#_3)，
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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_ENUM`](#_3)，
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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_FUNCTION`](#_3)，
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

- 该函数用于创建一个 `CVariant` 类型的对象，其数据类型为 [`TYPE_CUSTOM`](#_3)，
  `value` 参数即为 [`VCustom`](#vcustom) 数据指针，`type_name` 参数即为自定义类型名称，`destructor` 参数即为自定义类型析构函数。

!!! bug
    目前 `varCustom()` 函数暂未使用，后续会进行完善。


## 修改变量函数

!!! note
    目前 CVariant 类仅针对部分**不包含指针**的数据类型提供了修改函数，后续将考虑是否更新。

### `varIntModifyData()`

#### 宏函数

```c
#define varIntModifyData(variant, new_value) _varIntModifyValue(&variant, new_value)
```

#### 函数原型

```c
void _varIntModifyValue(CVariant* variant, int64_t new_value);
```

#### 参数

- `variant`：指向 `CVariant` 类型对象的指针。
- `new_value`：新的整数值。

#### 功能

- 该函数用于修改 `CVariant` 类型对象的数据值为新的整数值。

#### 示例

```c
CVariant variant = varInt(10);
_varIntModifyValue(&variant, 20);
```

!!! note

    - 该函数只能用于修改 `CVariant` 类型对象的数据值为整数类型。
    - 该函数只能修改 `CVariant` 类型对象的数据值，不能修改数据类型。

### `varUIntModifyData()`

#### 宏函数

```c
#define varUIntModifyData(variant, new_value) _varUIntModifyValue(&variant, new_value)
```

#### 函数原型

```c
void _varUIntModifyValue(CVariant* variant, uint64_t new_value);
```

#### 参数

- `variant`: `CVariant` 类型对象。
- `new_value`: 新的无符号整数值。

#### 功能

- 该函数用于修改 `CVariant` 类型对象的数据值为新的无符号整数值。

#### 示例

```c
CVariant variant = varUInt(10);
_varUIntModifyValue(&variant, 20);
```

!!! note

    - 该函数只能用于修改 `CVariant` 类型对象的数据值为无符号整数类型。
    - 该函数只能修改 `CVariant` 类型对象的数据值，不能修改数据类型。

### `varBoolModifyData()`

#### 宏函数

```c
#define varBoolModifyData(variant, new_value) _varIntModifyValue(&variant, new_value)
```

#### 函数原型

见 [`varIntModifyData()`](#varintmodifydata) 函数。

## 销毁函数

针对包含指针的变量，CVariant 提供了销毁指针的函数：`_varDestroyPointer()`, `_varDestroyString()`,`_varDestroyStruct()`, `_varDestroyEnum()`, `_varDestroyCustom()` 等。

这里非常推荐使用 `varDestroy()` 函数以销毁 CVariant 类型对象。

### `varDestroy()`

#### 宏函数

```c
#define varDestroy(variant)     _varDestroy(&variant)
```

#### 函数原型

```c
void _varDestroy(CVariant* variant);
```

#### 参数

- `variant`: 待销毁的 CVariant 类型对象。

#### 功能

销毁指定的 CVariant 类型对象。若指定的 CVariant 类型对象所对应的数据类型为如下类型，则销毁该指针所对应的数据。

- [`TYPE_STRING`](#_3)
- [`TYPE_POINTER`](#_3)
- [`TYPE_ENUM`](#_3)
- [`TYPE_CUSTOM`](#_3)

### 示例

定义一个整数指针并释放。

```c
CVariant var = varPointer(malloc(sizeof(int)));
varDestroy(var);
```

!!! note

    - 该函数只能销毁 `CVariant` 类型对象包含的**指针类型数据**，不能销毁数据类型本身。


## 变体值访问函数

CVariant 类型对象提供了以下函数用于访问（获取）对应数据类型的数据值。具体需要根据不同数据类型的变量调用对应的访问函数。

### `varBoolData()`

#### 宏函数

```c
#define varBoolData(variant)       _varBoolValue(&variant)
```

#### 函数原型

```c
bool _varBoolValue(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 bool 值。

### `varCharData()`

#### 宏函数

```c
#define varCharData(variant)       _varInt8Value(&variant)
```

#### 函数原型

```c
int8_t _varInt8Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `int8_t` / `char` 值。

### `varUCharData()`

#### 宏函数

```c
#define varUCharData(variant)      _varUInt8Value(&variant)
```

#### 函数原型

```c
uint8_t _varUInt8Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `uint8_t` / `unsigned char` 值。


### `varShortData()`

#### 宏函数

```c
#define varShortData(variant)      _varInt16Value(&variant)
```

#### 函数原型

```c
int16_t _varInt16Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `int16_t` / `short` 值。

### `varUShortData()`

#### 宏函数

```c
#define varUShortData(variant)     _varUInt16Value(&variant)
```

#### 函数原型

```c
uint16_t _varUInt16Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `uint16_t` / `unsigned short` 值。

### `varIntData()`

#### 宏函数

```c
#define varIntData(variant)        _varInt32Value(&variant)
```

#### 函数原型

```c
int32_t _varInt32Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `int32_t` / `int` 值。

### `varUIntData()`

#### 宏函数

```c
#define varUIntData(variant)       _varUInt32Value(&variant)
```

#### 函数原型

```c
uint32_t _varUInt32Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `uint32_t` / `unsigned int` 值。

### `varLongLongData()`

#### 宏函数

```c
#define varLongLongData(variant)       _varInt64Value(&variant)
```

#### 函数原型

```c
int64_t _varInt64Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `int64_t` / `long long` 值。

### `varULongLongData()`

#### 宏函数

```c
#define varULongLongData(variant)      _varUInt64Value(&variant)
```

#### 函数原型

```c
uint64_t _varUInt64Value(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `uint64_t` / `unsigned long long` 值。

### `varFloatData()`

#### 宏函数

```c
#define varFloatData(variant)          _varFloatValue(&variant)
```

#### 函数原型

```c
float _varFloatValue(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `float` 值。

### `varDoubleData()`

#### 宏函数

```c
#define varDoubleData(variant)         _varDoubleValue(&variant)
```

#### 函数原型

```c
double _varDoubleValue(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `double` 值。

### `varLongDoubleData()`

#### 宏函数

```c
#define varLongDoubleData(variant)      _varLongDoubleValue(&variant)
```

#### 函数原型

```c
long double _varLongDoubleValue(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的 `long double` 值。

### `varStringData()`

#### 宏函数

```c
#define varStringData(variant)         _varStringValue(&variant)
```

#### 函数原型

```c
CString _varStringValue(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的字符串。

!!! warning "特别注意"

    此函数返回的值并非 [`CString`](CString.md) 类型对象，而是 `const char *` 类型指针。

### `varPointerData()`

!!! tip
    此函数可以用于获取具有指针的 CVariant 类型对象所对应的数据。
    简而言之，**只要该 CVariant 类型对象对应的数据类型为指针，则此函数返回该指针所对应的数据。**



#### 宏函数

```c
#define varPointerData(variant, ptr_type)    (ptr_type *)_varPtr(&variant)
```

- `variant`: CVariant 类型对象
- `ptr_type`: 指针类型

#### 函数原型

```c
void* _varPtr(CVariant* variant);
```

#### 参数

- `variant`: CVariant 类型对象

#### 返回值

返回 CVariant 类型对象对应的指针。若 CVariant 类型对象为空，则返回 `NULL`。

!!! warning "特别注意"

    此函数返回的值为 `void *` 类型，**若需要获取自己定义的指针类型，建议使用宏函数**。


### `varStructPtr()`

#### 宏函数

```c
#define varStructPtr(variant, struct_type)   (struct_type *)_varStructPtr(&variant)
```

- `variant`: CVariant 类型对象。
- `struct_type`: 结构体类型。

#### 函数原型

```c
void* _varStructPtr(CVariant* variant);
```

- `variant`: CVariant 类型对象。

#### 返回值

返回 CVariant 类型对象对应的**结构体指针**。若 CVariant 类型对象为空，则返回 `NULL`。

### `varStructData()`

#### 宏函数

```c
#define varStructData(variant, struct_type)  *(varStructPtr(variant, struct_type))
```

- `variant`: CVariant 类型对象。
- `struct_type`: 结构体类型。

#### 函数原型

见 [`varStructPtr()`](#varstructptr)。

#### 返回值

返回 CVariant 类型对象对应的**结构体数据**。若 CVariant 类型对象为空，则返回 `NULL`。


## 其它函数

### `printVarData()`

#### 宏函数

```c
#define printVarData(variant)      _printVarData(&variant)
```

#### 函数原型

```c
void printVarData(CVariant *variant);
```

#### 参数

- `variant`：CVariant 类型对象。

#### 功能

在控制台（终端）中输出 CVariant 类型对象对应的数据。

#### 示例

```c
CVariant var = varInt(10);
printVarData(var);
```

在控制台（终端）下你可能会看到如下输出：

```
10
```
