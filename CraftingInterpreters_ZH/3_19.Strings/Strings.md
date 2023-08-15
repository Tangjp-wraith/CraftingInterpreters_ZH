# �ַ���

> �������������Ͷ���һ����񣿡�ҽ��������üë����������⣬���ô��˵ط�������Ӧ����ϧ��Щ������æµ����˼������鲻�������ĵ�����ζ�Ĺ�������
> 
> -- ̩�¡�����ķ˹��*����֮��*

���ǵ�С VM ���ڿ��Ա�ʾ�������͵�ֵ�����֡�����ֵ��`nil`. ��Щ������������Ҫ�Ĺ�ͬ�㣺�����ǲ��ɱ�Ĳ������Ǻ�С�����������ģ�������Ȼ�ʺ����� 64 λ�֡�����һ���㹻С�Ĵ��ۣ����ǿ���Ϊ����ֵ֧��������������Ҫ��ô��ռ�Ĳ���ֵ����ֵ��

���ҵ��ǣ��ַ���������ô��С���ַ���û����󳤶ȡ���ʹ������Ϊ�ؽ���������ĳЩ��Ϊ�����ƣ���255 ���ַ���������Ȼ��Ҫ̫����ڴ棬�޷���ÿ��ֵ�϶�������

> UCSD Pascal �� Pascal �ĵ�һ��ʵ��֮һ�������ȷ�е����ơ�Pascal �ַ����Գ���ֵ��ʼ���������� C ����ʹ����ֹ���ֽ���ָʾ�ַ����Ľ�β������ UCSD ��ʹ��һ���ֽ����洢���ȣ�����ַ����ĳ��Ȳ��ܳ��� 255 ���ַ���

![](./assets/19ea268166e6b5c45dfe286c1659b1d59209dc55.png)

������Ҫһ�ַ�����֧�ִ�С�仯�ܴ��ֵ���͡������Ƕ��϶�̬�����Ŀ�ġ����Ը�����Ҫ���価���ܶ���ֽڣ�����һ��ָ�룬������ֵ���� VM ʱ���ٸ�ֵ��

## 19.1  ֵ���ͺͶ�������

�������ڽϴ�Ŀɱ��С��ֵ����ջ���ڽ�С��ԭ��ֵ�ᵼ��������ʾ���� Lox ��˵��  ÿ���洢�ڱ����л�ӱ��ʽ���صĶ�����һ��Value������������������С�͹̶���С���ͣ���Ч����ֱ�Ӵ洢�� Value �ṹ�����С�

�������ϴ���������λ�ڶ��С�Ȼ�� Value ����Ч������*ָ��*���ڴ���ָ�롣�������ս��� clox ��ӵ��һЩ�ѷ�������ͣ��ַ�����ʵ����������ÿ�����Ͷ����Լ����ص����ݣ���Ҳ�����ǹ����״̬��[����δ���������ռ���](http://craftinginterpreters.com/garbage-collection.html)��ʹ�ø�״̬���������ǵ��ڴ档

![](./assets/69e5a1ac43ab472b345d7f7dda0cf5b1fd755daf.png)

���ǽ�����ͨ�ñ�ʾ��Ϊ��Obj����״̬�����ڶ��ϵ�ÿ�� Lox ֵ����һ�� Obj����ˣ����ǿ���ʹ��һ���µ� ValueType  case���������жѷ�������͡�

> ��Obj���ǡ�object������д��natch��

```c
  VAL_NUMBER,
  VAL_OBJ
} ValueType;
// value.h, in enum ValueType
```

�� Value ������Ϊ`VAL_OBJ`ʱ����Ч������ָ����ڴ��ָ�룬�������Ϊ�������������һ�� case

```c
    double number;
    Obj* obj;
  } as; 
// value.h, in struct Value
```

�������Ƕ�����ֵ�������������������������˼������õĺ������� Obj ֵ��

```c
#define IS_NUMBER(value)  ((value).type == VAL_NUMBER)
#define IS_OBJ(value)     ((value).type == VAL_OBJ)

#define AS_BOOL(value)    ((value).as.boolean)
// value.h, add after struct Value
```

���������ֵ�� Obj��ô���������`true`����������������ǿ���ʹ�������

```c
#define IS_OBJ(value)     ((value).type == VAL_OBJ)

#define AS_OBJ(value)     ((value).as.obj)
#define AS_BOOL(value)    ((value).as.boolean)
// value.h
```

����ֵ����ȡ Obj ָ�롣����Ҳ��������һ��·��

```c
#define NUMBER_VAL(value) ((Value){VAL_NUMBER, {.number = value}})
#define OBJ_VAL(object)   ((Value){VAL_OBJ, {.obj = (Obj*)object}})

typedef struct {
// value.h
```

����Ҫһ���� Obj ָ�벢�����װ��һ��������ֵ�С�

## 19.2 ���ڽṹ�ļ̳�

ÿ���ѷ����ֵ����һ�� Obj���� Obj������ȫ��ͬ�������ַ�����������Ҫ�ַ����顣�����ʵ��������Ҫ�����ֶΡ�һ������������Ҫ�����ֽ���顣��δ���ͬ����Ч�غɺʹ�С�����ǲ���������Ϊ Value ����������ʹ����һ�����ϣ���Ϊ��С�������ǡ�

> ������Ҳ��֪����objs����ô�������о�ĳ��Ӧ����һ��Ԫ����

�෴�����ǽ�ʹ����һ�ּ��������Ѿ������˺ܳ�ʱ�䣬������ C �淶Ϊ���ƶ����ض���֧�֣����Ҳ�֪������һ���淶�����ơ�[*��������˫����*](https://en.wikipedia.org/wiki/Type_punning)��һ�����ӣ����������̫���ˡ���û���κθ��õ��뷨������£��ҽ����Ϊ**struct inheritance**����Ϊ�������� structs ���Ҵ�����ѭ״̬�ĵ�һ�̳���������������еĹ�����ʽ��

��tagged unionһ����ÿ�� Obj ����һ��tag�ֶο�ʼ����tag�ֶα�ʶ����ʲô���͵Ķ��󡪡��ַ�����ʵ���ȡ�����������Ч�����ֶΡ���ͳһ��union��ͬ��ÿ�����Ͷ������Լ��Ķ����ṹ�����ֵĲ��������ͳһ�Դ���Щ�ṹ����Ϊ C û�м̳л��̬�Եĸ���Һܿ�ͻ���ͣ����������������˽�һ�³��������ݡ�

���ơ�Obj������ָ����һ���������ж������͹����״̬�Ľṹ�����е������ġ����ࡱ������ֵ�Ͷ���֮�����һЩѭ�������������ڡ�value��ģ����ǰ����������

```c
#include "common.h"

typedef struct Obj Obj;

typedef enum {
// value.h
```

ʵ�ʶ�����һ����ģ���С�

```c
#ifndef clox_object_h
#define clox_object_h

#include "common.h"
#include "value.h"

struct Obj {
  ObjType type;
};

#endif
// object.h, create new file
```

���ڣ���ֻ�������ͱ�ǩtype���ܿ죬���ǽ�Ϊ�ڴ�������һЩ������Ϣ������ö���������ģ�

```c
#include "value.h"

typedef enum {
  OBJ_STRING,
} ObjType;

struct Obj {
// object.h
```

��Ȼ����������Ӹ���ѷ������ͺ����ں�����½��л�����á��������ǽ�����������Щ��ǩ���ͣ�����б�Ҫ����һ��С�����Ӹ�����ֵ����ȡ�������ͱ�ǩ��

```c
#include "value.h"

#define OBJ_TYPE(value)        (AS_OBJ(value)->type)

typedef enum {
// object.h
```

�������ǵĻ�����

���ڣ�����������֮�Ϲ����ַ������ַ�������Ч�����ڵ����Ľṹ�ж��塣ͬ����������Ҫǰ������(forward-declare)����

```c
typedef struct Obj Obj;
typedef struct ObjString ObjString;

typedef enum {
// value.h
```

�ö����� Obj ���档

```c
};

struct ObjString {
  Obj obj;
  int length;
  char* chars;
};

#endif
// object.h, add after struct Obj
```

�ַ����������һ���ַ����顣���Ǵ洢��һ�������Ķѷ��������У��������ֻΪÿ���ַ�����������Ŀռ䡣���ǻ����ֽ����洢�������С��ⲻ�Ǿ��Ա�Ҫ�ģ���������֪��Ϊ�ַ��������˶����ڴ棬����������ַ����������ҿ���ֹ����

��ΪObjString��һ��Obj��������Ҳ��Ҫ����Objs�����״̬����ͨ������һ���ֶ���Ϊ Obj ��ʵ����һ�㡣**C ָ���ṹ�ֶΰ���������˳���������ڴ���**�����⣬��Ƕ�׽ṹʱ���ڲ��ṹ���ֶλ����ʵ���λ��չ�������� Obj �� ObjString ���ڴ濴������������

![](./assets/43601b65bd0104665166ef74d74db8406cae9ea6.png)

��ע�� ObjString �ĵ�һ���ֽ������ Obj ��ȫ���롣�ⲻ���ɺϡ���CҪ����������Ϊ��ʵ��һ�������ģʽ�������Ի�ȡһ��ָ��ṹ��ָ�룬�����䰲ȫ��ת��Ϊָ�����һ���ֶε�ָ�룬Ȼ�󷵻ء�

> �淶�Ĺؼ������ǣ�
> 
> �� 6.7.2.1 13
> 
> �ڽṹ�����У���λ���Ա��λ�����ڵĵ�Ԫ�ĵ�ַ��������������˳�������ָ��ṹ�����ָ�뾭���ʵ�ת����ָ�����ʼ��Ա����������ó�Ա��λ����ָ�������ڵĵ�Ԫ������֮��Ȼ���ṹ�����п�����δ��������䣬�����������Ŀ�ͷ��

����һ��`ObjString*`�������԰�ȫ�ؽ���ת��Ϊ`Obj*`Ȼ����з���`type`�ֶΡ��� OOP �ġ�is���������� ,ÿ�� ObjString ��is��һ�� Obj ���������Ժ����������������ʱ��ÿ���ṹ����һ�� Obj ��Ϊ���һ���ֶΡ��κ���Ҫ�������ж���Ĵ��붼���Խ�������Ϊ����`Obj*`�������Կ������ɸ�����κ������ֶΡ�

��Ҳ��������һ�������ߡ���һ��`Obj*`�������Խ���������ת����Ϊһ��`ObjString*`.��Ȼ����Ҫȷ��ӵ�е�`Obj*`ָ��ȷʵָ��ʵ�� ObjString ��`obj`�ֶΡ�������������ȫ�����½���������ڴ�λ��Ϊ�˼��������ת���Ƿ�ȫ�������������һ���ꡣ

```c
#define OBJ_TYPE(value)        (AS_OBJ(value)->type)

#define IS_STRING(value)       isObjType(value, OBJ_STRING)

typedef enum {
// object.h
```

����Ҫһ��ֵ��������ԭʼ`Obj*`ֵ����Ϊ VM �еĴ�������붼ʹ��ֵ�����������������������

```c
};

static inline bool isObjType(Value value, ObjType type) {
  return IS_OBJ(value) && AS_OBJ(value)->type == type;
}

#endif
// object.h, add after struct ObjString
```

ͻ�����飺Ϊʲô��ֱ�ӽ��˺�����������ں����أ���������ȣ������ʲô��ͬ���ԣ�����Ϊ������ʹ��`value`�����Ρ�ͨ���ڲ������Ƴ����������е�ÿ���ط��������*���ʽ*����չ�ꡣ�������ʹ��һ����������ñ��ʽ�ᱻ�����Ρ�

������ʽ�и����ã��Ǿ�̫����ˡ�������ǽ� `isObjType()`����궨����,�����������²���������˵��

```c
IS_STRING(POP())
```

Ȼ������Ӷ�ջ��pop����ֵ��ʹ��һ���������Խ��������⡣

ֻҪ����ȷ���ڴ���ĳ�����͵� Obj ʱ��ȷ�������ͱ�ǩ���ú�ͻ�������Ǻ�ʱ���԰�ȫ�ؽ�ֵת��Ϊ�ض��������͡����ǿ���ʹ����Щ��������һ�㣺

```c
#define IS_STRING(value)       isObjType(value, OBJ_STRING)

#define AS_STRING(value)       ((ObjString*)AS_OBJ(value))
#define AS_CSTRING(value)      (((ObjString*)AS_OBJ(value))->chars)

typedef enum {
// object.h
```

�����������һ��ֵ����ֵӦ����ָ�������Ч ObjString ��ָ�롣��һ������`ObjString*`ָ�롣�ڶ���ͨ���������ַ����鱾����Ϊ��ͨ��������������Ҫ�ġ�

## 19.3 �ַ���

�õģ����ǵ� VM ���ڿ��Ա�ʾ�ַ���ֵ�ˡ���ʱ������Ա�������ַ����ˡ�������һ�������Ǵ�ǰ�˿�ʼ���ʷ��������Ѿ�������ַ������֣������ֵ��������ˡ�

```c
  [TOKEN_IDENTIFIER]    = {NULL,     NULL,   PREC_NONE},
  [TOKEN_STRING]        = {string,   NULL,   PREC_NONE},
  [TOKEN_NUMBER]        = {number,   NULL,   PREC_NONE},
// compiler.c, replace 1 line
```

�������������ַ���tokenʱ��������ô˽���������

```c
static void string() {
  emitConstant(OBJ_VAL(copyString(parser.previous.start + 1,
                                  parser.previous.length - 2)));
}
// compiler.c, add after number()
```

��ֱ�ӴӴ����л�ȡ�ַ������ַ���`+ 1`��`- 2`�����޼�ǰ���ź�β�����š�Ȼ��������һ���ַ������󣬽����װ��һ��ֵ�У���������䵽�������С�

> ��� Lox ֧����`\n`�������ַ���ת�����У����ǻ��ڴ˴����з��롣��Ȼû�У����ǿ�����ԭ��ʹ���ַ���

Ҫ�����ַ���������ʹ��`copyString()`������ ������`object.h`��

```c
};

ObjString* copyString(const char* chars, int length);

static inline bool isObjType(Value value, ObjType type) {
// object.h, add after struct ObjString
```

������ģ����Ҫ��������

```c
#define clox_compiler_h

#include "object.h"
#include "vm.h"
// compiler.h
```

���ǵġ�object��ģ������һ��ʵ���ļ������������ж������º�����

```c
#include <stdio.h>
#include <string.h>

#include "memory.h"
#include "object.h"
#include "value.h"
#include "vm.h"

ObjString* copyString(const char* chars, int length) {
  char* heapChars = ALLOCATE(char, length + 1);
  memcpy(heapChars, chars, length);
  heapChars[length] = '\0';
  return allocateString(heapChars, length);
}
// object.c, create new file
```

���ȣ������ڶ��Ϸ���һ�������飬�պ��㹻�����ַ������ַ��ͽ�β����ֹ����ʹ������ͼ������һ�����и���Ԫ�����ͺʹ�С�����飺

```c
#include "common.h"

#define ALLOCATE(type, count) \
    (type*)reallocate(NULL, 0, sizeof(type) * (count))

#define GROW_CAPACITY(capacity) \
// memory.h
```

һ�������������飬���ǾʹӴ����и����ַ�����ֹ����

> ������Ҫ�Լ���ֹ�ַ�������Ϊ����ָ������Դ�ַ����е�һϵ���ַ�����û����ֹ��
> 
> ���� ObjString ��ʽ�洢���ȣ�����*����*���ַ����鱣��δ��ֹ״̬������ĩβ���һ����ֹ��ֻ�Ứ������һ���ֽڣ��������ǽ��ַ����鴫�ݸ���Ҫ��ֹ�ַ����� C ��׼�⺯����

��������֪��Ϊʲô ObjString ����ֱ��ָ��Դ�ַ����е�ԭʼ�ַ�����Ϊ���ӵ��ַ��������Ľ����һЩ ObjString ��������ʱ��̬��������Щ�ַ�����Ȼ��ҪΪ�ַ���̬�����ڴ棬����ζ���ַ���Ҫ�ڲ�����Ҫʱ*�ͷŸ��ڴ档*

���������һ���ַ������ֵ� ObjString������ͼ�ͷ���ָ��ԭʼԴ�����ַ������ַ����飬�ͻᷢ�����õ����顣��ˣ��������֣������ȷ����˵ؽ��ַ����Ƶ����С�������ÿ�� ObjString ���ɿ���ӵ�������ַ����鲢�����ͷ�����

�����ַ������������������������������У�

```c
#include "vm.h"

static ObjString* allocateString(char* chars, int length) {
  ObjString* string = ALLOCATE_OBJ(ObjString, OBJ_STRING);
  string->length = length;
  string->chars = chars;
  return string;
}
// object.c
```

���ڶ��ϴ���һ���µ� ObjString��Ȼ���ʼ�������ֶΡ����е��� OOP �����еĹ��캯������ˣ�������ʹ���º���á����ࡱ���캯������ʼ�� Obj ״̬��

```c
#include "vm.h"

#define ALLOCATE_OBJ(type, objectType) \
    (type*)allocateObject(sizeof(type), objectType)

static ObjString* allocateString(char* chars, int length) {
// object.c
```

��ǰ��ĺ�һ�������Ĵ�����Ҫ��Ϊ�˱��������ת��`void*`����������͡�ʵ�ʹ��������

> �ҳ�����һ���д����ĸ��������ͺ���Ҫ�Ķ����Ҿ������ִ�������÷ֽ⣬����ᵼ��΢С�����ķ�ɢ���������Ժ���������ʱ�����ǻ�õ��ر���

```c
#define ALLOCATE_OBJ(type, objectType) \
    (type*)allocateObject(sizeof(type), objectType)

static Obj* allocateObject(size_t size, ObjType type) {
  Obj* object = (Obj*)reallocate(NULL, 0, size);
  object->type = type;
  return object;
}

static ObjString* allocateString(char* chars, int length) {
// object.c
```

���ڶ��Ϸ��������С�Ķ�����ע�⣬��С*��������*Obj ����Ĵ�С�������ߴ����ֽ������Ա�Ϊ���ڴ������ض�������������Ķ�����Ч�����ֶ������ռ䡣

Ȼ������ʼ�� Obj ״̬�������ڣ���ֻ��type ��ǩ���˺������ص�`allocateString()`����� ObjString �ֶεĳ�ʼ����*��*�����ǿ��Ա����ִ���ַ������֡�

![](./assets/0e96eac649b946972e05945e3000e4bd493ba424.png)

> ��Ҫ����voil�����롰viola��������һ����˼�ǡ������������һ����������������С���ٺʹ�����֮�䡣�ǵģ���ȷʵ��������Сʱ����һ�������٣�ֻ��Ϊ����һ�¡�

## 19.4 �ַ�������

����Ư�����ַ���������������ǻ�û�����ܶ����顣һ���õĵ�һ���������еĴ�ӡ���벻��ܾ��µ�ֵ���͡�

```c
    case VAL_NUMBER: printf("%g", AS_NUMBER(value)); break;
    case VAL_OBJ: printObject(value); break;
  }
// value.c, in printValue()
```

�����ֵ��һ���ѷ���Ķ��������ӳٵ��á�����ģ���еĸ���������

```c
ObjString* copyString(const char* chars, int length);
void printObject(Value value);

static inline bool isObjType(Value value, ObjType type) {
// object.h, add after copyString()
```

ʵ�ֿ�������������

```c
void printObject(Value value) {
  switch (OBJ_TYPE(value)) {
    case OBJ_STRING:
      printf("%s", AS_CSTRING(value));
      break;
  }
}
// object.c, add after copyString()
```

��������ֻ��һ���������ͣ���������������ں�����½��в�������� switch case�������ַ���������ֻ�ǽ��ַ������ӡΪ C �ַ�����

> �Ҹ��߹�����ֹ�ַ����������ó���

������������Ҫ���ŵش����ַ��������ǣ�

```c
"string" == "string"
```

���������������ַ������֡�����������`copyString()`�������ζ����ĵ��ã�����������ͬ�� ObjString ���󲢽�������Ϊ���������洢�ڿ��С������Ƕ��еĲ�ͬ���󡣵����û���������ǣ�ϣ���ַ���������ȵ�ֵ���������ʽ�ļ�����ӦΪ`true`.����ҪһЩ�����֧�֡�

```c
    case VAL_NUMBER: return AS_NUMBER(a) == AS_NUMBER(b);
    case VAL_OBJ: {
      ObjString* aString = AS_STRING(a);
      ObjString* bString = AS_STRING(b);
      return aString->length == bString->length &&
          memcmp(aString->chars, bString->chars,
                 aString->length) == 0;
    }
    default:         return false; // Unreachable.
// value.c, in valuesEqual()
```

���������ֵ�����ַ�������ô������ǵ��ַ����������ͬ���ַ�����������ȣ��������������������Ķ�������ȫ��ͬ�Ķ�����ȷʵ��ζ���ַ�������Ա��������͵������������Ϊ��������������ַ����������Ժ��������[�޸ģ�](http://craftinginterpreters.com/hash-tables.html)������ʱΪ�����ṩ����ȷ�����塣

���Ϊ��ʹ��`memcmp()`�͡�object��ģ���е������ݣ�������Ҫ����һЩͷ�ļ������

```c
#include <stdio.h>
#include <string.h>

#include "memory.h"
// value.c
```

�����

```c
#include <string.h>

#include "object.h"
#include "memory.h"
// value.c
```

### 19.4.1 �ַ�������

����������ṩ����ദ���ַ����Ĳ����������ʵ����ַ����ַ����ĳ��ȡ��ı��Сд����֡����ӡ������ȡ�����ʵ���������ʱ���������Ҫ������Щ�����Ƕ��ڱ��飬���ǽ����ݱ���*�÷ǳ�*����

����֧�ֵ�Ψһ��Ȥ���ַ���������`+`.������������ַ���������ʹ�ø����������������һ�����ַ��������ַ��������������������ӡ����� Lox �Ƕ�̬���͵ģ������޷��ڱ���ʱ�ж���Ҫ������Ϊ����Ϊֱ������ʱ���ǲ�֪�������������͡���ˣ���`OP_ADD`ָ�̬����������ѡ����ȷ�Ĳ�����

```c
      case OP_LESS:     BINARY_OP(BOOL_VAL, <); break;
      case OP_ADD: {
        if (IS_STRING(peek(0)) && IS_STRING(peek(1))) {
          concatenate();
        } else if (IS_NUMBER(peek(0)) && IS_NUMBER(peek(1))) {
          double b = AS_NUMBER(pop());
          double a = AS_NUMBER(pop());
          push(NUMBER_VAL(a + b));
        } else {
          runtimeError(
              "Operands must be two numbers or two strings.");
          return INTERPRET_RUNTIME_ERROR;
        }
        break;
      }
      case OP_SUBTRACT: BINARY_OP(NUMBER_VAL, -); break;
// vm.c, in run(), replace 1 line
```

������������������ַ�������������������������Ƕ������֣���������ӡ����������͵��κ�������϶�������ʱ����

> ��ȴ�������Ը����ء������������У����һ�����������ַ���������һ���������κ����ͣ���������������֮ǰ����ʽת��Ϊ�ַ�����
> 
> ����Ϊ����һ���ܺõ����ԣ�����ҪΪÿ�����ͱ�д��ζ�ġ�ת��Ϊ�ַ��������룬�����ҽ����� Lox ��ɾ���ˡ�

Ϊ�������ַ��������Ƕ�����һ���º�����

```c
static void concatenate() {
  ObjString* b = AS_STRING(pop());
  ObjString* a = AS_STRING(pop());

  int length = a->length + b->length;
  char* chars = ALLOCATE(char, length + 1);
  memcpy(chars, a->chars, a->length);
  memcpy(chars + a->length, b->chars, b->length);
  chars[length] = '\0';

  ObjString* result = takeString(chars, length);
  push(OBJ_VAL(result));
}
// vm.c, add after isFalsey()
```

���ǳ��߳�����Ϊ�����ַ����� C ����������ˡ����ȣ����Ǹ��ݲ������ĳ��ȼ������ַ����ĳ��ȡ�����Ϊ�������һ���ַ����飬Ȼ�����븴�ƽ�ȥ��һ�������������ϸȷ���ַ�����ֹ��

Ϊ�˵���`memcpy()`��VM ��Ҫ����һ��ͷ�ļ���

```c
#include <stdio.h>
#include <string.h>

#include "common.h"
// vm.c
```

�����������һ�� ObjString ��������Щ�ַ����������ʹ��һ���º�����`takeString()`.

```c
};

ObjString* takeString(char* chars, int length);
ObjString* copyString(const char* chars, int length);
// object.h, add after struct ObjString
```

ʵ�ֿ�������������

```c
ObjString* takeString(char* chars, int length) {
  return allocateString(chars, length);
}
// object.c, add after allocateString()
```

ǰ���`copyString()`�����ٶ���*����*ȡ����������ַ�������Ȩ���෴�������ص��� ObjString ����ӵ�еĶ��ϴ����ַ��ĸ��������ڴ����ַ�λ��Դ�ַ����м���ַ���������˵��������ȷ��������

���ǣ������ַ������ӣ��Ѿ��ڶ��϶�̬������һ���ַ����顣������һ���������Ƕ���ģ�������ζ��`concatenate()`�����ס�ͷ��丱�������෴���˺������������ṩ���ַ���������Ȩ��

������һ�������˹���ƴ����һ����Ҫ��������ͷ�ļ���

```c
#include "debug.h"
#include "object.h"
#include "memory.h"
#include "vm.h"
// vm.c
```

## 19.5 �ͷŶ���

��������������˴��ŵı��ʽ��

```c
"st" + "ri" + "ng"
```

����������ϸ������ʱ����Ϊ�������ַ��������е�ÿһ������һ�� ObjString �������Ǵ洢�ڿ�ĳ������в���������ֽ��룺

> ����ÿ��ָ����ջ�����ӣ�

![](./assets/c52eb31430d84a3ccaab042fc27bbb8c1b178dec.png)

```
0000    OP_CONSTANT         0 "st"
0002    OP_CONSTANT         1 "ri"
0004    OP_ADD
0005    OP_CONSTANT         2 "ng"
0007    OP_ADD
0008    OP_RETURN
```

ǰ����ָ�`"st"`��`"ri"`ѹ���ջ��Ȼ��`OP_ADD`������Щ����������������������ڶ��϶�̬����һ����`"stri"`�ַ�����VM push����Ȼ��push`"ng"`���������`OP_ADD`����`"stri"`��`"ng"`��������������������push�����`"string"`��̫���ˣ�����������������ġ�

���ǵȵȡ��Ǹ�`"stri"`�ַ�������ô���£����Ƕ�̬��������Ȼ�� VM �ڽ����� `"ng"` ���Ӻ����������ǽ����Ӷ�ջ�е����������ж��������ã�����δ�ͷ������ڴ档���������˵��͵��ڴ�й©��

��Ȼ��*Lox ����*��ȫ���������м��ַ��������õ����ͷ����ǡ�Lox �����û��Զ������ڴ档�����ڴ�����β���*��ʧ*���෴��������������Ϊ VM ʵ���ߵļ��ϡ�

�����Ľ���������ڳ�������ʱ����δʹ�õ��ڴ��[�����ռ�����](http://craftinginterpreters.com/garbage-collection.html)������׼���ô����Ǹ���Ŀ֮ǰ������һЩ�����Ķ���Ҫ��λ������֮ǰ�����ǵ������ǽ����ġ����ǵȴ�����ռ�����ʱ��Խ������Խ��������

> ���Ѿ������ܶ����ڳ��Կ�ʼ GC ֮ǰʵ���˴������ǵ����ԡ�������ͨ���ڿ�������ʱ���е�������߳�����ʵ���ϲ����ڳ������֮ǰ�����ڴ棬������������ߵø�Զ��
> 
> ����͹����Ժ���������ռ������Ѷȡ�*�ռ���*����ȷ���������ҵ�����ʹ�õ�ÿһλ�ڴ棬*����*���Ͳ����ռ�ʵʱ���ݡ�����ʵ�ֿ��������ٸ��ط��洢��ĳ����������á������û���ҵ�������Щ����ͻ�����ج�ΰ�Ĵ���
> 
> ���Ѿ���������ʵ�������ˣ���Ϊ�Ժ������ GC ���롣������������Ҫ GC���뾡����������������һ���漰���������ĺ��й�ע�㡣

���죬��������Ӧ��������޶ȵ����飺ͨ��ȷ�� VM ��Ȼ�����ҵ�ÿ������Ķ����������ڴ�*й©*����ʹ Lox ���������������ǡ��߼��ڴ������ʹ����ิ�ӵļ�����Ϊ�������͸����ڴ档���ǽ��������ʵ�õķ�����

���ǽ�����һ���������洢ÿ�� Obj��VM ���Ա������б����ҵ����ڶ��Ϸ����ÿ�����������û��ĳ���� VM �Ķ�ջ�Ƿ��Ծ��ж��������á�

���ǿ��Զ���һ������������ڵ�ṹ��������Ҳ����������ǡ��෴�����ǽ�ʹ��**����ʽ�б�**����Obj�ṹ����������ڵ㡣ÿ�� Obj ����һ��ָ��������һ�� Obj ��ָ�롣

```c
struct Obj {
  ObjType type;
  struct Obj* next;
};
// object.h, in struct Obj
```

VM �洢ָ���б�ͷ����ָ�롣

```c
  Value* stackTop;
  Obj* objects;
} VM;
// vm.h, in struct VM
```

�����ǵ�һ�γ�ʼ�� VM ʱ��û�з���Ķ���

```c
  resetStack();
  vm.objects = NULL;
}
// vm.c, in initVM()  
```

ÿ�����Ƿ���һ�� Obj ʱ�����ǽ������뵽�б��С�

```c
  object->type = type;

  object->next = vm.objects;
  vm.objects = object;
  return object;
// object.c, in allocateObject()
```

��Ϊ����һ�������������������ײ���ĵط����Ǳ�ͷ�����������ǾͲ���Ҫ�洢ָ��β����ָ�벢���ָ��¡�

��object��ģ��ֱ��ʹ��`vm`��vm��ģ���е�ȫ�ֱ��������������Ҫ���䱩¶���ⲿ��

```c
} InterpretResult;

extern VM vm;

void initVM();
// vm.h, add after enum InterpretResult
```

���գ������ռ������� VM ��������ʱ�ͷ��ڴ档���ǣ�������ˣ����û��ĳ������ʱ��ͨ������һЩδʹ�õĶ��������ڴ����ǻ��������ҲӦ���ͷ����ǡ�

û�и��ӵ��߼���������ɺ����ǿ����ͷ�*ÿ��*�����������ڿ��Զ���Ӧ��ʵ������

```c
void freeVM() {
  freeObjects();
}
// vm.c, in freeVM()
```

������finally ��ĳ��[ʱ](http://craftinginterpreters.com/a-virtual-machine.html#an-instruction-execution-machine)������Ǹ��պ���������֮Ϊ��

```c
void* reallocate(void* pointer, size_t oldSize, size_t newSize);
void freeObjects();

#endif
// memory.h, add after reallocate()
```

��������������ͷŶ���

```c
void freeObjects() {
  Obj* object = vm.objects;
  while (object != NULL) {
    Obj* next = object->next;
    freeObject(object);
    object = next;
  }
}
// memory.c, add after reallocate()
```

���Ǳ��������ͷ���ڵ�� CS 101 �̿���ʵ�֡�����ÿ���ڵ㣬���ǵ��ã�

```c
static void freeObject(Obj* object) {
  switch (object->type) {
    case OBJ_STRING: {
      ObjString* string = (ObjString*)object;
      FREE_ARRAY(char, string->chars, string->length + 1);
      FREE(ObjString, object);
      break;
    }
  }
}
// memory.c, add after reallocate()
```

���ǲ���Ҫ�ͷ� Obj ��������ĳЩ�������ͻ�����������ӵ�е������ڴ棬������ǻ���ҪһЩ�ض������͵Ĵ���������ÿ���������͵������������������ζ�������ͷ��ַ����飬Ȼ���ͷ� ObjString����Щ��ʹ�����һ���ڴ����ꡣ

```c
     (type*)reallocate(NULL, 0, sizeof(type) * (count))

#define FREE(type, pointer) reallocate(pointer, sizeof(type), 0)

#define GROW_CAPACITY(capacity) \
// memory.h
```

���Ƕ�`reallocate()`�򵥰�װ��ֻ�ǽ���������ݡ�������С�������ֽڡ�

> ʹ��`reallocate()`�ͷ��ڴ��ƺ��������塣Ϊʲô��ֱ�ӵ���`free()`���Ժ��⽫���� VM ��������ʹ�õ��ڴ�����������еķ�����ͷŶ�ͨ����`reallocate()`���ͺ����׶Է����ڴ���ֽ������ж�̬������

������һ����������Ҫһ����������������������һ��

```c
#include "common.h"
#include "object.h"

#define ALLOCATE(type, count) \
// memory.h
```

Ȼ����ʵ���ļ��У�

```c
#include "memory.h"
#include "vm.h"

void* reallocate(void* pointer, size_t oldSize, size_t newSize) {
// memory.c
```

���������ǵ� VM �Ͳ�����й©�ڴ��ˡ�����һ���õ� C ����һ���������˳�֮ǰ�������Ļ��ҡ����������� VM ����ʱ�ͷ��κζ����Ժ󣬵����Ա�д����ʱ������� Lox ����ʱ��VM �������й���������Խ��Խ����ڴ棬ֱ������������ɲŷ���һ���ֽڡ�

[���������һ�������������ռ���](http://craftinginterpreters.com/garbage-collection.html)֮ǰ���ǲ�����������⣬������һ�󲽡���������ӵ��֧�ָ��ֲ�ͬ���͵Ķ�̬�������Ļ�����ʩ���������������ַ�����ӵ� clox�����Ǵ���������������õ�����֮һ���ַ���������ʹ�����ܹ�������һ�ֻ����������ͣ��������ڶ�̬�����У����ϵ�[��ϣ��](http://craftinginterpreters.com/hash-tables.html)����������һ�µ�����...

---

## [��ս](http://craftinginterpreters.com/strings.html#challenges)

1. ÿ���ַ�������Ҫ���������Ķ�̬���䡪��һ������ ObjString����һ�������ַ����顣��һ��ֵ�����ַ���Ҫ����ָ����Ѱַ������ܶ����ܲ���������Ч�Ľ������������һ�ֳ�Ϊ[��������Ա](https://en.wikipedia.org/wiki/Flexible_array_member)�ļ�����ʹ������ ObjString �����ַ�����洢�ڵ������������С�

2. ������Ϊÿ���ַ������ִ��� ObjString ʱ�����ǽ��ַ����Ƶ����ϡ����������Ժ��ͷ��ַ���ʱ������֪���ͷ��ַ�Ҳ�ǰ�ȫ�ġ�
   
   ����һ�ָ��򵥵ķ����������˷�һЩ�ڴ棬���ڷǳ����޵��豸�Ͽ����Ǹ����⡣�෴�����ǿ��Ը�����Щ ObjStrings ӵ�����ǵ��ַ����飬��Щ�ǡ������ַ�����������ֻ��ָ��ԭʼԴ�ַ���������һЩ�����ͷŵ�λ�á���ӶԴ˵�֧�֡�

3. ��� Lox ���������ԣ����û�����ʹ��`+`һ���ַ�������������һ��ʹ����������ʱ������������ʲô��֤�����ѡ������������ʲô���ã�

## [���˵�����ַ�������](http://craftinginterpreters.com/strings.html#design-note)

�ڱ����У��Ҿ������ر�����ʵ������ʵ���л������ļ������⡣���ǿ��ܲ�������ʹ����*���ӵ�*������������Ͼ�����һ�������Ե��顪��������Ϊ��װ��������������ǲ���ʵ�ġ�Ȼ������ȷʵ�ܿ���һ���ǳ����ֵ����⣺������α�ʾ�ַ�����

�ַ����������������棺

- **ʲô���ַ����еĵ������ַ���**  �ж����ֲ�ͬ��ֵ�����Ǵ���ʲô����һ�����㷺���õı�׼����[ASCII](https://en.wikipedia.org/wiki/ASCII)���������� 127 ����ͬ���ַ�ֵ��ָ����������ʲô���ܰ�...�����ֻ����Ӣ���Ȼ����һЩ��ֵġ��������������ַ����硰��¼�ָ������͡�ͬ�����С�������û��һ�������������������������������ܴ���jalape?o������na?ve������Gruy��re����M?tley Cr��e����
  
  ����˵��һ�ֲ������������� Gruy��re �� M?tley Cr��e ��������һ�ֲ�ֵ��ʹ�õ����ԡ�
  
  ��������[Unicode](https://en.wikipedia.org/wiki/Unicode)���������֧�� 16,384 ����ͬ���ַ���**code points**������Щ�ַ��ǳ��ʺ� 16 λ������һЩ����λ��������Խ��Խ�������г��� 100,000 ����ͬ�Ĵ���㣬�������ཻ������Ҫ���ߣ��磨Unicode �ַ���PILE OF POO����`U+1F4A9`����
  
  ��ʹ����ô���Ĵ�����б�Ҳ�����Ա�ʾһ�����Կ���֧�ֵ�ÿ�����ܵĿɼ����Ρ�Ϊ�˽��������⣬Unicode ������?�޸�ǰ�������**����ַ���**���磬��a���������ַ����������ǡ�?������Ϊ������������ң�Unicode*����*һ����������?���ĵ�һ����㡣��
  
  ����û����ʡ�na?ve���еĵ��ĸ����ַ����������������ء�v�����ǡ�������ǰ����ζ�����ǽ�ÿ������㼰������ַ���Ϊһ����Ԫ����Unicode��֮Ϊ?**��չ���ش�**����������ζ���������ڿ��ǵ����Ĵ���㡣�����û������ĸ���

- **������Ԫ���ڴ�����α�ʾ��**�����ʹ�� ASCII ��ϵͳΪÿ���ַ�����һ���ֽڣ���������λδʹ�á�Unicode ��һЩ�����ı��롣UTF-16 ���������������� 16 λ����ÿ������㶼�ʺϸô�Сʱ����ܰ����������ʱ�����������ʹ�ö�� 16 λ���뵥Ԫ����ʾ����������*����ԡ�*UTF-32 �� UTF-16 ����һ����չ������Ϊÿ��������ṩ�������� 32 λ��
  
  UTF-8 �������κ�һ�������ӡ���ʹ�ÿɱ��������ֽ����������㡣�ϵ�ֵ�Ĵ�����ʺϽ��ٵ��ֽڡ�����ÿ���ַ�����ռ�ò�ͬ���ֽ��������������ֱ�������ַ����������ض��Ĵ���㡣�������Ҫ���ȷ�˵���� 10 ������㣬�㲻֪����û�б����ͽ�������ǰ����ַ���������£��ַ������ж����ֽڡ�

ѡ���ַ���ʾ�ͱ����漰������Ȩ�⡣���񹤳��е��������һ����û�������Ľ��������

��������ж��ѵ�һ���������� Python���� Python 2 �� 3 ��������������ʹ�࣬��Ҫ����Ϊ��Χ���ַ�����������˸��ġ�

- ASCII �ڴ�Ч�ʸ����ٶȿ죬�����Ὣ��������������һ�ߡ�

- UTF-32 �ٶȺܿ첢��֧������ Unicode ��Χ�������˷Ѵ����ڴ棬��Ϊ��������������λ�ڽϵ͵�ֵ��Χ�ڣ�����Ҫ������ 32 λ��

- UTF-8 �ڴ�Ч�ʸߣ�֧������ Unicode ��Χ������ɱ䳤�ȱ���ʹ���������������ٶȱ�����

- UTF-16 ��������Щ����⡪������ Unicode ���������� 16 λ��Χ�ĳ�ª����������ڴ�Ч�ʵ��� UTF-8�������ڴ���ԣ�����Ȼ��һ�ֿɱ䳤�ȱ��롣������ԣ������ʹ������las���������������Ҫ���������JVM �� CLR �����л����������JVM �� CLR �������������ܻ���ʹ��������Ϊ���ǵ��ַ�����ʹ�� UTF-16���������������ת��ÿ�����ַ������ݸ��ײ�ϵͳ��ʱ�䡣

һ��ѡ���ǲ�ȡ����޶ȵķ�������������ȷ�������顣֧������ Unicode ����㡣���ڲ�������ÿ���ַ���������Ϊÿ���ַ���ѡ��һ�ֱ��롪�����ÿ������㶼�ʺ�һ���ֽڣ���ʹ�� ASCII�����û�д���ԣ���ʹ�� UTF-16���ȵȡ��ṩ API ���û�������������չ���δ�.

�⺭�����������л�������ȷʵ�ܸ��ӡ�ʵʩ�����Ժ��Ż��Ĺ������ܴ������л��ַ�����������ϵͳ������ʱ�������봦�����б��롣�û���Ҫ�˽����������� API ��֪����ʱʹ���ĸ������Ǹ��µĴ������������ڲ��õķ����������� Raku �� Swift��

һ�����򵥵����Է�����ʼ��ʹ�� UTF-8 ���б��룬����ֻ���������ڴ����� API��������Ҫʹ�����شص��û���������Ϊ��ʹ�õ������⡣�ⲻ�� ASCII ������������Ϊ���ģ���Ҳû�и��ӵöࡣ��ʧȥ�˰���������ֱ������������ͨ������û�������߸�����������*O(n)*������*O(1)*��

�����ҪΪ��д����Ӧ�ó���������һ�ִ����������ԣ��ҿ��ܻ������󷽷��������ҵ�С��Ƕ��ʽ�ű�����[Wren](http://wren.io/)����ʹ���� UTF-8 �ʹ���㡣