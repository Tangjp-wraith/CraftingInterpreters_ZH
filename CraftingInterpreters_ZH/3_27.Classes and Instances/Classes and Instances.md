# ���ʵ��

> ���ֹ��Ķ��������㡣ֻ�ǡ���������㹻����һ���£����ͻ�ӵ���Լ��������������������ȫ�����塪��������������ѵ����������ǽ�����ĳ�ָ��������ϵ������
> 
> -- Donna Tartt,*The Goldfinch*

clox �����һ����Ҫʵ�ֵ���������������̡�OOP��һ���໥��֯�����ԣ��ࡢʵ�����ֶΡ���������ʼ�����ͼ̳С�ʹ����Ը߼��� Java�����ǽ��������ݴ�������¡��������������� C ��д���룬��о���������ǩ���찣�ƶ�������ģ�ͣ����ǽ��������½���������ͬ��������ʹ�ô��ݵ�������ʵ�ֹ����С�[������հ�](http://craftinginterpreters.com/closures.html)��[�����ռ���](http://craftinginterpreters.com/garbage-collection.html)֮��ļ���½�֮�����Ѿ��������Ϣ����ʵ�ϣ��Ȿ������ڿ�ʼӦ�ú����ס�

> �������������ǿ�ҿ������ˡ���������ÿ���ˡ�������������Ϊ OOP ��ζ��һЩ�ǳ���������������б���ʵ������һ��������̽���ռ䣬ÿ�����Զ����Լ��ĳɷֺ��䷽��
> 
> Self �ж���û���ࡣCLOS �з������������Ǹ��ӵ��ض��ࡣC++ ���û������ʱ��̬�ԡ���û�����ⷽ����Python �ж��ؼ̳У��� Java û�С�Ruby ���������ӵ��࣬����Ҳ�����ڵ��������϶��巽����

�ڱ����У����ǽ�����ǰ�������ԣ��ࡢʵ�����ֶΡ�������������״̬���档Ȼ���ڽ������������У����ǽ���ͣ��Щ�������Ϊ�ʹ������á�

## 27.1 �����

�ڻ������������������У�һ�ж����࿪ʼ�����Ƕ����˳����д��ڵĶ��������Լ�����������ʵ���Ĺ��������¶��ϣ����ǽ������ǵ�����ʱ��ʾ��ʼ��Ȼ�������ӵ������С�

���ˣ������Ѿ��ǳ���Ϥ�� VM ����¶������͵Ĺ��̡����Ǵ�һ���ṹ��ʼ��

```c
} ObjClosure;

typedef struct {
  Obj obj;
  ObjString* name;
} ObjClass;

ObjClosure* newClosure(ObjFunction* function);
// object.h, add after struct ObjClosure
```

�� Obj ͷ֮�����Ǵ洢������ơ��Ⲣ�����û�����������ģ���������������ʱ��ʾ��ջ���ٵ����ݵ����ơ�

��������Ҫ ObjType ö���е���Ӧ��Сд��

```c
typedef enum {
  OBJ_CLASS,
  OBJ_CLOSURE,
// object.h, in enum ObjType
```

�Ǹ����͵õ�һ����Ӧ�ĺꡣ���ȣ�����һ����������ͣ�

```c
#define OBJ_TYPE(value)        (AS_OBJ(value)->type)

#define IS_CLASS(value)        isObjType(value, OBJ_CLASS)
#define IS_CLOSURE(value)      isObjType(value, OBJ_CLOSURE)
// object.h
```

Ȼ�� Value ת��Ϊ ObjClass ָ�룺

```c
#define IS_STRING(value)       isObjType(value, OBJ_STRING)

#define AS_CLASS(value)        ((ObjClass*)AS_OBJ(value))
#define AS_CLOSURE(value)      ((ObjClosure*)AS_OBJ(value))
// object.h
```

VM ʹ�ô˺��������µ������

```c
} ObjClass;

ObjClass* newClass(ObjString* name);
ObjClosure* newClosure(ObjFunction* function);
// object.h, add after struct ObjClass
```

ʵ�������

```c
ObjClass* newClass(ObjString* name) {
  ObjClass* klass = ALLOCATE_OBJ(ObjClass, OBJ_CLASS);
  klass->name = name; 
  return klass;
}
// object.c, add after allocateObject()
```

�����������塣�������������Ϊ�ַ������ղ��洢��ÿ���û�����һ������ʱ��VM ���ᴴ��һ���µ� ObjClass �ṹ����ʾ����

![](./assets/1637bc0fb529635700d5560480f178b2a5ec773f.png)

> �ҽ���������Ϊ��klass������������Ϊ�˸� VM һ�ֻ�����ѧǰ�ࡰKidz Korner���о�����ʹ�ý� clox ����Ϊ C++ ��ø����ף����С��ࡱ�Ǳ����֡�

�� VM ������Ҫĳ����ʱ�������������ͷ�����

```c
  switch (object->type) {
    case OBJ_CLASS: {
      FREE(ObjClass, object);
      break;
    } 
    case OBJ_CLOSURE: {
// memory.c, in freeObject()
```

> ����Ĵ��������ں������壬������һ���������� switch case ��Ӹ������ʱ������á�

����������һ���ڴ���������������ǻ���Ҫ֧��ͨ���������и��١�

```c
  switch (object->type) {
    case OBJ_CLASS: {
      ObjClass* klass = (ObjClass*)object;
      markObject((Obj*)klass->name);
      break;
    }
    case OBJ_CLOSURE: {
// memory.c, in blackenObject()
```

�� GC ����һ�������ʱ�������Ǹ����������ʹ���ַ���Ҳ���ֻ״̬��

VM ���Զ���ִ�е����һ�������Ǵ�ӡ����

```c
  switch (OBJ_TYPE(value)) {
    case OBJ_CLASS:
      printf("%s", AS_CLASS(value)->name->chars);
      break;
    case OBJ_CLOSURE:
// object.c, in printObject() 
```

һ����ֻ��˵�����Լ������֡�

## 27.2 ������

����ʱ��ʾ���֣�����׼��Ϊ������Ӷ����֧�֡������������ǽ����������

```c
static void declaration() {
  if (match(TOKEN_CLASS)) {
    classDeclaration();
  } else if (match(TOKEN_FUN)) {
    funDeclaration();
// compiler.c, in declaration(), replace 1 line
```

����������䣬������ͨ��ǰ��`class`�ؼ���ʶ��һ��������ı��뷢�������

```c
static void classDeclaration() {
  consume(TOKEN_IDENTIFIER, "Expect class name.");
  uint8_t nameConstant = identifierConstant(&parser.previous);
  declareVariable();

  emitBytes(OP_CLASS, nameConstant);
  defineVariable(nameConstant);

  consume(TOKEN_LEFT_BRACE, "Expect '{' before class body.");
  consume(TOKEN_RIGHT_BRACE, "Expect '}' after class body.");
}
// compiler.c, add after function()
```

������`class`�ؼ���֮�����������ơ����ǻ�ȡ�ñ�ʶ����������Ϊ�ַ�����ӵ���Χ�����ĳ������С�������������ӡһ�������ʾ�����ƣ���˱�������Ҫ�������ַ�����䵽����ʱ�����ҵ���ĳ��λ�á���������������һ��ķ�����

����Ҳ���ڽ������󶨵�ͬ����������ˣ�������ʹ����token����������һ�����иñ�ʶ���ı�����

> ���ǿ�������������Ϊ*���ʽ*��������䡪�����Ǳ�������һ������ֵ�����֡�Ȼ���û�������ȷ�ؽ���󶨵��Լ��ı��������磺
> 
> ```js
> var Pie = class {}
> ```

> �е��� lambda �������������ࡣ������������ͨ��ϣ����������ζ������������Խ�������Ϊ������������ġ�

�����������Ƿ���һ����ָ����������ʱʵ�ʴ�������󡣸�ָ������Ƶĳ�����������Ϊ��������

֮�󣬵��ڱ���������֮ǰ������Ϊ�������������*����*����������ӵ��������У�������һ��[ǰһ��](http://craftinginterpreters.com/local-variables.html#another-scope-edge-case)���ǲ���*ʹ��*����ֱ������*����*�������࣬����������֮ǰ����������������û����������Լ��ķ����������ð����ࡣ��������������ʵ���Ĺ�������֮��Ķ��������á�

������Ǳ������塣���ǻ�û�з���������������ֻ��һ�Կ����š�Lox ����Ҫ�����������ֶΣ����������Ѿ����������ͽ�������

���������ڷ���һ����ָ����������Ƕ�������

```c
  OP_RETURN,
  OP_CLASS,
} OpCode;
// chunk.h, in enum OpCode
```

��������ӵ����������У�

```c
    case OP_RETURN:
      return simpleInstruction("OP_RETURN", offset);
    case OP_CLASS:
      return constantInstruction("OP_CLASS", chunk, offset);
    default:
// debug.c, in disassembleInstruction()
```

��������һ���������ܴ�Ĺ��ܣ���������֧�������ٵġ�

```c
        break;
      }
      case OP_CLASS:
        push(OBJ_VAL(newClass(READ_STRING())));
        break;
    }
// vm.c, in run()
```

���Ǵӳ������м��������Ƶ��ַ��������䴫�ݸ�`newClass()`.�⽫����һ�����и������Ƶ�������󡣽�������ջ�����Ǻܺá��������󶨵�ȫ�ֱ����������������`defineVariable()`�����������Խ��ö���Ӷ�ջ�洢��ȫ�ֱ������С������������¾ֲ�������Ҫ�ڶ�ջ�ϵ�λ�á�

> �����ء��ࡪ���ں�������ڲ��������࣬��һ����Ѱ���ĸ��������Ը������������ǡ������� Lox ��һ�ֶ�̬���͵Ľű����ԣ���ͳһ�������Ķ����Լ������Ϳ�����塣��ֻ����һ����������Ϊ�������ڿ������������ͺ�����������Ҳ���������������ࡣ

���ˣ����ǵ� VM ����֧�����ˡ���������������

```c
class Brioche {}
print Brioche;
```

���ҵ��ǣ���ӡ�����������Զ�����е�*���в�����������һ����ʹ���Ǹ����á�*

## 27.3 ���ʵ��

������������������ҪĿ�ģ�

- **�����Ǵ�����ʵ���ķ�ʽ**����ʱ���漰`new`�ؼ��֣���ʱ���Ƕ������ķ������ã�ͨ������ĳ�ַ�ʽͨ�������ἰ���Ի�ȡ��ʵ����

- **���ǰ�������**����Щ�����˸��������ʵ������Ϊ��ʽ��

����Ҫ����һ�²Ż�̸��������������������ֻ���ĵ�һ���֡�������Դ���ʵ��֮ǰ��������Ҫ���ǵı�ʾ��

```c
} ObjClass;

typedef struct {
  Obj obj;
  ObjClass* klass;
  Table fields; 
} ObjInstance;

ObjClass* newClass(ObjString* name);
// object.h, add after struct ObjClass
```

ʵ��֪�����ǵ��ࡪ��ÿ��ʵ������һ��ָ�����������ָ�롣�����ڱ����в����õ���ô�࣬���ǵ�������ӷ���ʱ�����úܹؼ���

���¸���Ҫ����ʵ����δ洢���ǵ�״̬��Lox �����û�������ʱ���ɵ���ʵ������ֶΡ�����ζ��������Ҫһ�ֿ��������Ĵ洢���ơ�����ʹ�ö�̬���飬��Ҳϣ�����찴���Ʋ����ֶΡ���һ�����ݽṹ�ǳ��ʺ�ͨ�����ƿ��ٷ���һ��ֵ�����ҡ������������㡪�������Ѿ�ʵ��������ÿ��ʵ����ʹ�ù�ϣ��洢���ֶΡ�

> �ܹ�������ʱ���ɵ����������ֶ��Ǵ������̬���Ժ;�̬����֮���һ���ܴ��ʵ�����𡣾�̬��������ͨ����Ҫ��ʽ�����ֶΡ��������������Ϳ���ȷ�е�֪��ÿ��ʵ������Щ�ֶΡ�������ʹ������ȷ��ÿ��ʵ������ľ�ȷ�ڴ����Լ������ڸ��ڴ����ҵ�ÿ���ֶε�ƫ������
> 
> �� Lox ��������̬�����У������ֶ�ͨ���ǹ�ϣ����ҡ�����ʱ�䣬����Ȼ�൱������������ C++ �����������У�����һ���ֶξ�����һ����������ƫ��һ��ָ��һ���졣

����ֻ��Ҫ���һ���������͸㶨�ˡ�

```c
#include "chunk.h"
#include "table.h"
#include "value.h"
// object.h
```

����½ṹ�����һ���µĶ������͡�

```c
  OBJ_FUNCTION,
  OBJ_INSTANCE,
  OBJ_NATIVE,
// object.h, in enum ObjType 
```

�������������һ�㣬��Ϊ Lox���Եġ����͡������ VM*ʵ�ֵ�*�����͡������Կ�����������ķ�ʽ�໥��ͻ�������� clox �� C �����У�����಻ͬ���͵� Obj����?ObjString��ObjClosure �ȡ�ÿ�������Լ����ڲ���ʾ�����塣

�� Lox*������*���û����Զ��������Լ����ࡪ������ Cake �� Pie����Ȼ�󴴽���Щ���ʵ�������û��ĽǶ�������Cake ʵ���� Pie ʵ���ǲ�ͬ���͵Ķ��󡣵��ǣ��� VM �ĽǶ��������û������ÿ���඼ֻ�� ObjClass ���͵���һ��ֵ��ͬ�����û������е�ÿ��ʵ�������������ĸ����ʵ��������һ�� ObjInstance��һ�� VM �������ͺ����������ʵ���������������������໥ӳ�䣺

![](./assets/81bdb54ead152d90cb017f66b8348019794a362d.png)

֪���ˣ�OK���ص�ʵ�֡�����Ҳ�õ������ǳ��õĺꡣ

```c
#define IS_FUNCTION(value)     isObjType(value, OBJ_FUNCTION)
#define IS_INSTANCE(value)     isObjType(value, OBJ_INSTANCE)
#define IS_NATIVE(value)       isObjType(value, OBJ_NATIVE)
// object.h
```

�ͣ�

```c
#define AS_FUNCTION(value)     ((ObjFunction*)AS_OBJ(value))
#define AS_INSTANCE(value)     ((ObjInstance*)AS_OBJ(value))
#define AS_NATIVE(value) \
// object.h
```

�����ֶ����ڴ���ʵ������ӵģ���ˡ����캯��������ֻ��Ҫ֪���ࡣ

```c
ObjFunction* newFunction();
ObjInstance* newInstance(ObjClass* klass);
ObjNative* newNative(NativeFn function);
// object.h, add after newFunction()
```

����������ʵ�ָù��ܣ�

```c
ObjInstance* newInstance(ObjClass* klass) {
  ObjInstance* instance = ALLOCATE_OBJ(ObjInstance, OBJ_INSTANCE);
  instance->klass = klass;
  initTable(&instance->fields);
  return instance;
}
// object.c, add after newFunction()
```

���Ǵ洢��ʵ��������á�Ȼ���ֶα��ʼ��Ϊһ���յĹ�ϣ��һ���µ�Ӥ���������ˣ�

��ʵ���������ڵ�ĩβ�����ᱻ�ͷš�

```c
      FREE(ObjFunction, object);
      break;
    }
    case OBJ_INSTANCE: {
      ObjInstance* instance = (ObjInstance*)object;
      freeTable(&instance->fields);
      FREE(ObjInstance, object);
      break;
    }
    case OBJ_NATIVE:
// memory.c, in freeObject()
```

��ʵ��ӵ�����ֶα�������ͷŸ�ʵ��ʱ������Ҳ�ͷ��˸ñ�����û����ʽ�ͷű��е���Ŀ����Ϊ���ܻ��ж���Щ������������á������ռ�����Ϊ���Ǵ�����Щ����������ֻ�ͷű������Ŀ���顣

˵�������ռ���������Ҫ֧��ͨ��ʵ�����и��١�

```c
      markArray(&function->chunk.constants);
      break;
    }
    case OBJ_INSTANCE: {
      ObjInstance* instance = (ObjInstance*)object;
      markObject((Obj*)instance->klass);
      markTable(&instance->fields);
      break;
    }
    case OBJ_UPVALUE:
// memory.c, in blackenObject()
```

���ʵ�������ţ�������Ҫ���������ࡣ���⣬��Ҫ����ʵ���ֶ����õ�ÿ�����󡣴�������Ǹ��Ļ�����ǿɷ��ʵģ���ΪĳЩʵ�������ֶ��еĶ������˵��ǣ������Ѿ�����һ���ܺõ�`markTable()`������ʹ�������Ǳ�����ס�

��̫��Ҫ����Ȼ��Ҫ���Ǵ�ӡ��

```c
      break;
    case OBJ_INSTANCE:
      printf("%s instance",
             AS_INSTANCE(value)->klass->name->chars);
      break;
    case OBJ_NATIVE:
// object.c, in printObject()
```

ʵ����ӡ�����ƺ����instance��������ʵ����������Ҫ��Ϊ�������ʵ�������ӡ��ͬ�����ݡ���

> ����������������Զ����ඨ��ĳ��`toString()`?����������ָ����ν���ʵ��ת��Ϊ�ַ�������ӡ��������� Lox ����һ��������ԣ���Ҳ��֧������

��������Ȥ�����ڽ��������ϡ�Lox û�������`new`�ؼ��֡�������ʵ���ķ���������ú���һ�������౾������ʱ�Ѿ�֧�ֺ������ã������鱻���ö����������ȷ���û����᳢�Ե������ֻ�������Ч���͡�

������һ����case ��չ������ʱ��顣

```c
    switch (OBJ_TYPE(callee)) {
      case OBJ_CLASS: {
        ObjClass* klass = AS_CLASS(callee);
        vm.stackTop[-argCount - 1] = OBJ_VAL(newInstance(klass));
        return true;
      }
      case OBJ_CLOSURE:
// vm.c, in callValue()
```

��������õ�ֵ�����������������ı��ʽʱ�����Ķ��󡪡���һ���࣬��ô���ǽ�����Ϊ���캯�����á����Ǵ���һ�������������ʵ����������洢��ջ�С�

> ������ʱ���Դ��ݸ����õ��κβ�������������ӶԳ�ʼ������֧��ʱ�����ǽ���[��һ��](http://craftinginterpreters.com/methods-and-initializers.html)���·�����δ��롣

���Ǹ���һ���ˡ��������ǿ��Զ����ಢ�������ǵ�ʵ����

```c
class Brioche {}
print Brioche();
```

������ע��ڶ���`Brioche`��������š��⽫��ӡ��Brioche instance����

## 27.4 get��set ���ʽ

ʵ�������ʾ�Ѿ����Դ洢״̬������ʣ�µľ������û������ù��ܡ�ʹ�� get �� set ���ʽ���ʺ��޸��ֶΡ�Lox �����ƴ�ͳ����ʹ�þ���ġ��㡱�﷨��

```js
eclair.filling = "pastry creme";
print eclair.filling;
```

��㡪�����ҵ�Ӣ��������˵�Ǿ�š����е�����׺������������һ�����ʽ���ȱ���ֵ������һ��ʵ����֮����`.`�ֶ����ơ�����ǰ����һ�������������ǽ�����Ϊ��׺���ʽ�ҽӵ��������С�

> ��˵���е�������Ϊ��`.`֮����Ҳ಻�Ǳ��ʽ������һ����ʶ������������ get �� set ���ʽ����������ĸ��ӽ��ں�׺���ʽ��

```c
  [TOKEN_COMMA]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_DOT]           = {NULL,     dot,    PREC_CALL},
  [TOKEN_MINUS]         = {unary,    binary, PREC_TERM},
// compiler.c, replace 1 line
```

����������һ����`.`��������ܰ󶨣����ȼ��뺯�������е�����һ���ߡ�������ʹ�õ�token��������ɸ��µĽ���������

```c
static void dot(bool canAssign) {
  consume(TOKEN_IDENTIFIER, "Expect property name after '.'.");
  uint8_t name = identifierConstant(&parser.previous);

  if (canAssign && match(TOKEN_EQUAL)) {
    expression();
    emitBytes(OP_SET_PROPERTY, name);
  } else {
    emitBytes(OP_GET_PROPERTY, name);
  }
}
// compiler.c, add after call()
```

������ϣ���ڵ�֮�������ҵ�һ���������ơ����ǽ���token�Ĵ�����Ϊ�ַ������ص��������У��Ա������������ʱ���á�

> ������������ʹ�á����ԡ������ǡ��ֶΡ�����Ϊ���ס��Lox ��������ʹ�õ��﷨�����ʷ��������������������ԡ�����������ָ����������ʵ���Ϸ��ʵ��κ�����ʵ���ͨ������ֶ�����ʵ��**״̬**֧�ֵ����Ե��Ӽ���

�����������µı��ʽ��ʽ����getter�� setter���������������������������ֶ����ƺ󿴵�һ���Ⱥţ���һ����һ����ֵ���ֶεļ��ϱ��ʽ�������ǲ���*����*������Ҫ������ֶκ�ʹ�õȺš����ǣ�

```js
a + b.c = 3
```

���� Lox ���﷨�����ھ䷨������Ч�ģ�����ζ�����ǵ� Lox ʵ���������Ⲣ����������`dot()`��Ĭ������`= 3`���֣����ǽ�����ؽ��ʹ��룬�ͺ����û���д�ˣ�

```js
a + (b.c = 3)
```

������set���ʽ��`=`�������ȼ���`.`���ֵ͵öࡣ���������ܻ������ȼ����߶������� setter ���ֵ������� �е���`dot()`��Ϊ���������������ǽ���`canAssign`Ϊ��ʱ�Ž����ͱ���`=`���֡������`canAssign`Ϊ falseʱ���ֵȺű�ǣ�`dot()`������������ء�����������£����������ս�չ����`parsePrecedence()`�������һ��token ��Ȼ��һ�������`=`����������ֹͣ���������

�����������������������ҵ�`=`����ô���������ı��ʽ��֮�󣬷���һ����`OP_SET_PROPERTY`ָ�����Ҫһ����������Ϊ���������������Ƶ��������������û�б���һ��set���ʽ�����Ǽ�������һ�� getter ������һ��`OP_GET_PROPERTY`ָ���ָ��Ҳ����һ����������Ϊ�������ơ�

> ������*����*���ֶ����ԣ���������Ϊ��ָ�������`OP_SET_FIELD`��������Ϊ�� get ָ���һ�¿��������á�

�����Ƕ�����������ָ��ĺ�ʱ����

```c
  OP_SET_UPVALUE,
  OP_GET_PROPERTY,
  OP_SET_PROPERTY,
  OP_EQUAL,
// chunk.h, in enum OpCode
```

����ӶԷ�������ǵ�֧�֣�

```c
      return byteInstruction("OP_SET_UPVALUE", chunk, offset);
    case OP_GET_PROPERTY:
      return constantInstruction("OP_GET_PROPERTY", chunk, offset);
    case OP_SET_PROPERTY:
      return constantInstruction("OP_SET_PROPERTY", chunk, offset);
    case OP_EQUAL:
// debug.c, in disassembleInstruction()
```

### 27.4.1 ���� getter �� setter ���ʽ

��������ʱ�����ǽ��� get ���ʽ��ʼ����Ϊ���Ǹ���һЩ��

```c
      }
      case OP_GET_PROPERTY: {
        ObjInstance* instance = AS_INSTANCE(peek(0));
        ObjString* name = READ_STRING();

        Value value;
        if (tableGet(&instance->fields, name, &value)) {
          pop(); // Instance.
          push(value);
          break;
        }
      }
      case OP_EQUAL: {
// vm.c, in run()
```

����������������ָ��ʱ������ߵı��ʽ�Ѿ���ִ�У����ʵ����ջ�������Ǵӳ������ж�ȡ�ֶ���������ʵ�����ֶα��в��ҡ������ϣ��������и����Ƶ���Ŀ�����ǽ�������ʵ��������Ŀ��ֵ��Ϊ���push��

��Ȼ�����ֶο��ܲ����ڡ��� Lox �У����䶨��Ϊ����ʱ������ˣ�����Ϊ�������һ����飬�������������ֹ��

```c
          push(value);
          break;
        }

        runtimeError("Undefined property '%s'.", name->chars);
        return INTERPRET_RUNTIME_ERROR;
      }
      case OP_EQUAL: {
// vm.c, in run()
```

�������Ѿ�ע�⵽����һ����Ҫ����Ĺ���ģʽ������Ĵ������ ����ߵı��ʽ����Ϊһ�� ObjInstance������û��ʲô������ֹ�û�����д��

```c
var obj = "not an instance";
print obj.field;
```

�û��ĳ�������ˣ��� VM ��Ȼ��Ҫ���ŵش����������ڣ����Ὣ ObjString ��λ�������Ϊ ObjInstance ���ң��Ҳ�֪�����Ż���������Բ����ŵ����顣

�� Lox �У�ֻ����ʵ�����ֶΡ����ܽ��ֶ���䵽�ַ����������ϡ�������Ҫ�ڷ��������κ��ֶ�֮ǰ����ֵ�Ƿ���һ��ʵ����

> Lox*����*֧�����������͵�ֵ����ֶΡ��������ǵ����ԣ����ǿ����������������¡���������Ǹ������⡣���������ܵķ�ʽʹʵ�ֱ�÷ǳ����ӡ������磬�ַ���פ����ø������ѡ�
> 
> ���⣬���������Χ�Ƽ�ֵ����Ⱥ�ͬһ�ԵĴֲ��������⡣����ҽ�һ���ֶθ��ӵ� number`3`������Ƿ�`1 + 2`Ҳ�и��ֶΣ������������ʵʩ��θ��٣�������ǣ���ô����������������Ƿ���Ȼ����Ϊ����ȵģ�

```c
case OP_GET_PROPERTY: {
        if (!IS_INSTANCE(peek(0))) {
          runtimeError("Only instances have properties.");
          return INTERPRET_RUNTIME_ERROR;
        }

        ObjInstance* instance = AS_INSTANCE(peek(0));
// vm.c, in run()
```

���ջ�ϵ�ֵ����ʵ�������ǽ���������ʱ���󲢰�ȫ�˳���

��Ȼ����û��ʵ�����κ��ֶ�ʱ��get ���ʽ���Ǻ����á�Ϊ�ˣ�������Ҫ������

```c
        return INTERPRET_RUNTIME_ERROR;
      }
      case OP_SET_PROPERTY: {
        ObjInstance* instance = AS_INSTANCE(peek(1));
        tableSet(&instance->fields, READ_STRING(), peek(0));
        Value value = pop();
        pop();
        push(value);
        break;
      }
      case OP_EQUAL: {
// vm.c, in run()
```

��� `OP_GET_PROPERTY`��΢����һ�㡣ִ�д˲���ʱ��ջ��������Ҫ�������ֶε�ʵ�������Ϸ���Ҫ�洢��ֵ������ǰһ�������Ƕ�ȡָ��Ĳ��������ҵ��ֶ������ַ�����ʹ��������ջ������ֵ�洢��ʵ�����ֶα��С�

֮����һЩջ��ˣ�����ǵ����洢��ֵ��Ȼ�󵯳�ʵ�������ֵ�ƻء����仰˵�����ǴӶ�ջ���Ƴ�*�ڶ���*Ԫ�أ�ͬʱ��������Ԫ�ء�setter ������һ�����ʽ�������Ƿ����ֵ�����������Ҫ����ֵ���ڶ�ջ�ϡ�������ҵ���˼��

> ��ջ�����������ģ�

![](./assets/c52eb31430d84a3ccaab042fc27bbb8c1b178dec.png)

```js
class Toast {}
var toast = Toast();
print toast.jam = "grape"; // Prints "grape".
```

���ȡ�ֶ�ʱ��ͬ���������赣�Ĺ�ϣ���������ֶΡ������Ҫ��setter ����ʽ�������ֶΡ�����ȷʵ��Ҫ�����û�����س��Խ��ֶδ洢�ڲ���ʵ����ֵ�ϡ�

```c
      case OP_SET_PROPERTY: {
        if (!IS_INSTANCE(peek(1))) {
          runtimeError("Only instances have fields.");
          return INTERPRET_RUNTIME_ERROR;
        }

        ObjInstance* instance = AS_INSTANCE(peek(1));
vm.c, in run()
```

�� get ���ʽ��ȫһ�������Ǽ��ֵ�����Ͳ�������Чʱ��������ʱ���󡣲��ң�Lox ����������̵�֧�ֵ���״̬�����Ѿ���λ����һ�ԣ�

```js
class Pair {}

var pair = Pair();
pair.first = 1;
pair.second = 2;
print pair.first + pair.second; // 3.
```

����ĸо����Ǻ�����*����*����������һ����ֵġ���̬���ͻ��� C ���壬���ж�������ɢ�����ƽṹ�����ݰ���һ�ֶ�̬�������ԡ������Ǳ�������һ�󲽡����ǵ� Lox ʵ�����������û����ɵؽ����ݾۺϳɸ���ĵ�Ԫ������һ���У����ǽ�Ϊ��Щ���ԵĿ�ע��������

---

## [��ս](http://craftinginterpreters.com/classes-and-instances.html#challenges)

1. ���Է��ʶ����ϲ����ڵ��ֶλ�������ֹ���� VM���û��޷��Ӵ�����ʱ�����лָ���Ҳ�޷��ڳ��Է���ĳ���ֶ�֮ǰ�鿴���Ƿ���ڡ����û�����ȷ��ֻ��ȡ��Ч�ֶΡ�
   
   ������̬����������δ���ȱʧ���ֶΣ������LoxӦ����ô����ʵ�����Ľ��������

2. ������ʱͨ��*�ַ���*���Ʒ����ֶΡ��������Ʊ���ʼ����Ϊ��ʶ��tokenֱ�ӳ�����Դ�����С��û�������ǿ���Եع���һ���ַ���ֵ��Ȼ���������ֶε����ơ�����Ϊ����Ӧ���ܹ������һ�����Թ�����ʵ������ʵ������

3. �෴��Lox û���ṩ��ʵ����ɾ���ֶεķ����������Խ��ֶε�ֵ����Ϊ`nil`������ϣ���е���Ŀ��Ȼ���ڡ�����������δ���������⣿Ϊ Lox ѡ��ʵʩ���ԡ�

4. ��Ϊ������ʱ�����Ʒ����ֶΣ�����ʹ��ʵ��״̬�������Ӽ����Ͻ�������һ���㶨ʱ��Ĳ���������л��ϣtable����������������Խϴ����Ƕ�̬���ԱȾ�̬��������������Ҫԭ��
   
   ��̬�������Եĸ���ʵ����δ�����Ż�����