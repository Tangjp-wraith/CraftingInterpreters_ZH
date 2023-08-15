# �հ�

> ����������˵������ÿ�����ӵ����ⶼ��һ���򵥵Ľ�����������Ǵ���ġ�
> 
> -- Umberto Eco,*Foucault��s Pendulum*

��������[��һ��](http://craftinginterpreters.com/calls-and-functions.html)�������Ͷ����Ѿ�ӵ����һ�����Թ��������������ȱ�ٵ��Ǳհ�������ȫ�ֱ��������������Լ��Ķ���Ʒ�֣�һ������û�а취���������Լ�������֮�������ı�����

```js
var x = "global";
fun outer() {
  var x = "outer";
  fun inner() {
    print x;
  }
  inner();
}
outer();
```

�������д�ʾ���������ӡ��global������Ӧ�ô�ӡ��outer����Ϊ�˽��������⣬��Ҫ�ڽ�������ʱ����������Χ�����������ʷ�������

��������� clox �б��� jlox �и��ѣ���Ϊ�ֽ��� VM ���ֲ������洢��ջ�С�ʹ��ջ����Ϊ�����ƾֲ���������ջ���塪���������봴�������෴��˳�򱻶��������Ƕ��ڱհ�����ֻ��*�����*���������ȷ�ġ�

```js
fun makeClosure() {
  var local = "local";
  fun closure() {
    print local;
  }
  return closure;
}

var closure = makeClosure();
closure();
```

�ⲿ����`makeClosure()`������һ������`local`.����������һ���ڲ�������`closure()`���ڲ���ñ�����Ȼ��`makeClosure()`���ضԸú��������á����ڱհ��ڸ�ֵ���ֲ�����ʱ����(var closure =)��`local`Ӧ�ñ���ȴ������ĺ����������ڸ�����

![](./assets/e5f22aae415ed60d9592ef78abe784b71a2c221d.png)

> Ŷ�������������ݣ�

����ͨ��Ϊ���оֲ�������̬�����ڴ������������⡣�� jlox �����������ģ��������ж�������������Environment ����,  ��Environment ������java���д��������������ǲ��롣ʹ��ջ�ǳ��졣������ֲ����������հ������Ҿ���ջ���塣Ϊ�˸���(���հ�����)�ľֲ����������棬��������Щ���������Ǻ����Ĵ�ʩ��

> �Ͼ���C �� Java ��ջ���ھֲ���������ԭ��ġ�

����ζ�ű�֮ǰ�� Java ��������ʹ�õķ��������ӡ���ΪһЩ�ֲ��������������ڲ���ܴ����ǽ�������ʵʩ���ԡ����ڲ��ڱհ���ʹ�õľֲ�������������������ջ�С���һ���ֲ��������հ�����ʱ����������һ�ֽ�����������������������ϣ����������ǿ��Ը�����Ҫ���档

������ Lisp �������հ���һֱ���ڣ���ʱ�ڴ��ֽں� CPU ���ڱ�������������ļ�ʮ����ڿ�����˸��ַ�ʽ�����հ�����Ϊ�����Ż�������ʱ��ʾ����ЩЧ�ʸ��ߣ�����Ҫ�����ӵı�����̣����Բ�̫�������ɵظ��쵽 clox �С�

> ������closure conversion����lambda lifting����ʼ̽����

����������͵ļ������� Lua VM ����ơ����ٶȿ죬��ʡ�ڴ棬��������Խ��ٵĴ���ʵ�֡�������ӡ����̵��ǣ�����Ȼ�ʺ� clox �� Lua ��ʹ�õĵ��̱�����������������Щ���ӡ�������Ҫһ��ʱ������������Ժ��н�����Ƭ��ƴ����һ�����ǽ�һ��һ���ع������ǣ��ҽ����Էֽ׶ν�����Щ���

## 25.1 �հ�����

VM ʹ�� ObjFunction ��ʾ����ʱ��������Щ������ǰ���ڱ���ʱ�����ġ�������ʱ��VM ������ֻ�Ǵӳ������м��غ������󲢽���󶨵����ơ�û��������ʱ�������������Ĳ��������ַ��������ַǳ����ƣ������Ǵ����ڱ���ʱʵ�����ĳ�����

> ���仰˵��Lox �еĺ���������һ������������һ�ֶ����������ͳ���ֵ���﷨��

�����е���ģ���Ϊ��ɺ��������������ڱ���ʱ������֪�ģ��Ӻ������������ֽ���飬�Լ�������ʹ�õĳ��������ǣ�һ������հ������ֱ�ʾ��ʽ�Ͳ����㹻�ˡ�������

```js
fun makeClosure(value) {
  fun closure() {
    print value;
  }
  return closure;
}

var doughnut = makeClosure("doughnut");
var bagel = makeClosure("bagel");
doughnut();
bagel();
```

��`makeClosure()`�������岢����һ�����������ǵ��������β����������հ�����������ͬ��Ƕ�׺�������`closure`����������ղ�ͬ��ֵ���������������հ�ʱ��ÿ������ӡ��ͬ���ַ���������ζ�� ��Ҫ�հ�������ʱ��ʾ������������Χ�ľֲ���������Ϊ������ִ�к�������ʱ���ڣ������������ڱ���ʱ��

���ǽ��𲽲����������������ڵ�һ������ö����ʾ�����е� ObjFunction ���ͱ�ʾ ���������ġ�ԭʼ������ʱ״̬����Ϊ�ӵ����������������бհ�������ͬ�Ĵ���ͳ�����������ʱ����ִ�к�������ʱ�����ǽ� ObjFunction ��װ��һ���µ� ObjClosure �ṹ�С����� ������ԭʼ�ĺ��� �������� ԭʼ�����հ�������  ����ʱ״̬��

> Lua ʵ�ֽ������ֽ����ԭʼ���������Ϊ��ԭ�͡�������ʺ��ʺ�������һ�㣬��������ʻ���������ָ��[ԭ�ͼ̳�](https://en.wikipedia.org/wiki/Prototype-based_programming)��

![](./assets/db88c08e19b9cd8ddb1a5e04902a53f2ad9d7e97.png)

���ǽ���ÿ����������װ��һ�� ObjClosure �У���ʹ�ú���ʵ���ϲ�û�йرղ������κ���Χ�ľֲ����������е��˷ѣ��������� VM����Ϊ�������ǿ��Լ�����õĺ�����һ�� ObjClosure������½ṹ��������ʼ�ģ�

```c
typedef struct {
  Obj obj;
  ObjFunction* function;
} ObjClosure;
// object.h, add after struct ObjString
```

���ڣ���ֻ��ָ��һ�� ObjFunction ����ӱ�Ҫ�Ķ���ͷ���ݡ�ͨ���� clox ����¶������͵�ͨ����ʽ����������һ�� C ����������һ���µıհ���

```c
} ObjClosure;

ObjClosure* newClosure(ObjFunction* function);
ObjFunction* newFunction();
// object.h, add after struct ObjClosure
```

Ȼ��������ʵ������

```c
ObjClosure* newClosure(ObjFunction* function) {
  ObjClosure* closure = ALLOCATE_OBJ(ObjClosure, OBJ_CLOSURE);
  closure->function = function;
  return closure;
}
// object.c, add after allocateObject()
```

����Ҫһ��ָ������װ�� ObjFunction ��ָ�롣�����������ֶγ�ʼ��Ϊ�����͡�

```c
typedef enum {
  OBJ_CLOSURE,
  OBJ_FUNCTION,
// object.h, in enum ObjType
```

���������һ���հ�ʱ�������ͷ������ڴ档

```c
  switch (object->type) {
    case OBJ_CLOSURE: {
      FREE(ObjClosure, object);
      break;
    }
    case OBJ_FUNCTION: {
// memory.c, in freeObject()
```

����ֻ�ͷ� ObjClosure ���������ͷ� ObjFunction��������Ϊ�հ���ӵ�и�ObjFunction�������ж���հ�������ͬһ��ObjFunction������û��һ���հ����ƶ������κ�������Ȩ���������������ж�����ʧ֮ǰ�������޷��ͷ� ObjFunction������������������Χ����,��Ϊ���������а������������������ܼ��֣�����ȷʵ��ˣ������Ϊʲô���Ǻܿ�ͻ��дһ�������ռ�����Ϊ���ǹ�������

���ǻ������ڼ��ֵ���͵ĳ��úꡣ

> Ҳ����Ӧ�ö���һ���꣬�Ա�����׵�������Щ�ꡣҲ���ǻ��е�̫Ԫ����ˡ�

```c
#define OBJ_TYPE(value)        (AS_OBJ(value)->type)

#define IS_CLOSURE(value)      isObjType(value, OBJ_CLOSURE)
#define IS_FUNCTION(value)     isObjType(value, OBJ_FUNCTION)
// object.h
```

��ת��ֵ��

```c
#define IS_STRING(value)       isObjType(value, OBJ_STRING)

#define AS_CLOSURE(value)      ((ObjClosure*)AS_OBJ(value))
#define AS_FUNCTION(value)     ((ObjFunction*)AS_OBJ(value))
// object.h
```

�հ���һ��������������Դ�ӡ���ǡ�

```c
  switch (OBJ_TYPE(value)) {
    case OBJ_CLOSURE:
      printFunction(AS_CLOSURE(value)->function);
      break;
    case OBJ_FUNCTION:
// object.c, in printObject()
```

���ǵ���ʾ�� ObjFunction ��ȫһ�������û��ĽǶ�������ObjFunction �� ObjClosure ֮������𴿴���һ�����ص�ʵ��ϸ�ڡ�������������Ǿ�����һ����Ч���յıհ���ʾ��

### 25.1.1 ����հ�����

�������˱հ����󣬵�������Ӳ��������ǡ���һ�����ñ���������ָ���������ʱ��ʱ�����µ� ObjClosure ����װ������ ObjFunction���ⷢ���ں���������ĩβ��

```c
  ObjFunction* function = endCompiler();
  emitBytes(OP_CLOSURE, makeConstant(OBJ_VAL(function)));
}
// compiler.c, in function(), replace 1 line
```

��ǰ�����������������ֽ����ǵ���OP_CONSTANT`ָ����ڴ���Χ�����ĳ������м����ѱ��뺯��������ѹ���ջ��������һ����ָ�

```c
  OP_CALL,
  OP_CLOSURE,
  OP_RETURN,
// chunk.h, in enum OpCode  
```

�� `OP_CONSTANT`һ��������һ������������ֵ�Ǻ������������������ǵ�����ת������ʱʵ��ʱ������һЩ����Ȥ�����顣

���ȣ������ǳ�Ϊ�ڷܵ� VM �ڿͣ����ڷ��������в���ָ��֧�֡�

```c
    case OP_CALL:
      return byteInstruction("OP_CALL", chunk, offset);
    case OP_CLOSURE: {
      offset++;
      uint8_t constant = chunk->code[offset++];
      printf("%-16s %4d ", "OP_CLOSURE", constant);
      printValue(chunk->constants.values[constant]);
      printf("\n");
      return offset;
    }
    case OP_RETURN:
// debug.c, in disassembleInstruction()
```

���﷢�������������ͨ���ڷ��������з�����Ҫ�ࡣ�����½���ʱ�����ᷢ����`OP_CLOSURE`��һ���ǳ���Ѱ����ָ��������ܼ򵥡���ֻ��һ�����ֽڲ��������������ǻ������������Ĵ���Ԥʾ���Ǹ�δ����

### 25.1.2 ���ͺ�������

��Ҫ���Ĵ󲿷ֹ�����������ʱ����Ȼ�أ����Ǳ��봦����ָ�������Ҫ���� VM ���� ObjFunction һ��ʹ�õ�ÿһ�δ��룬�������Ϊʹ�� ObjClosure���������á�����֡�ȡ����������ǽ���ָ�ʼ��

```c
      }
      case OP_CLOSURE: {
        ObjFunction* function = AS_FUNCTION(READ_CONSTANT());
        ObjClosure* closure = newClosure(function);
        push(OBJ_VAL(closure));
        break;
      }
      case OP_RETURN: {
// vm.c, in run()
```

����֮ǰʹ�õ�`OP_CONSTANT`ָ��һ�������ȴӳ������м��ر����ĺ��������ڵĲ�֮ͬ�����ڽ��ú�����װ��һ���µ� ObjClosure �в������ѹ���ջ��

һ������һ���հ��������ջ���Ҫ������

```c
    switch (OBJ_TYPE(callee)) {
      case OBJ_CLOSURE:
        return call(AS_CLOSURE(callee), argCount);
      case OBJ_NATIVE: {
// vm.c, in callValue(), replace 2 lines
```

����ɾ���˵�������Ϊ `OBJ_FUNCTION`�Ķ���Ĵ��롣���ڽ����к�����װ�� ObjClosures �У�����ʱ����Զ�����ٳ��Ե����� ObjFunction����Щ����ֻ�����ڳ������У����������κζ�����������֮ǰ�����������ڱհ��С�

> ���ǲ�ϣ���κ��㺯���� VM ��Χ�ǻ����ھӻ���ô˵��

�÷ǳ����ƵĴ����滻�ɴ��������ñհ���Ψһ�������Ǵ��ݸ�`call()`�Ķ������͡��ù��ܵ������仯����������������С����ȣ����Ǹ�������ǩ����

```c
static bool call(ObjClosure* closure, int argCount) {
  if (argCount != function->arity) {
// vm.c, function call(), replace 1 line
```

Ȼ���������У���Ҫ�޸��������øú��������ݣ��Դ���������һ����Ӳ����ʵ���� arity ��鿪ʼ��

```c
static bool call(ObjClosure* closure, int argCount) {
  if (argCount != closure->function->arity) {
    runtimeError("Expected %d arguments but got %d.",
        closure->function->arity, argCount);
    return false;
// vm.c, in call(), replace 3 lines
```

Ψһ�ı仯�����Ǵ򿪱հ��Ի�ȡ�ײ㺯����`call()`������Ҫ�����Ǵ���һ���µ� CallFrame�����Ǹ��ĸô����Խ��հ��洢�� CallFrame �в��ӱհ��ĺ����л�ȡ�ֽ���ָ�롣

```c
  CallFrame* frame = &vm.frames[vm.frameCount++];
  frame->closure = closure;
  frame->ip = closure->function->chunk.code;
  frame->slots = vm.stackTop - argCount - 1;
// vm.c, in call(), replace 2 lines
```

��Ҳ��Ҫ���� CallFrame ��������

```c
typedef struct {
  ObjClosure* closure;
  uint8_t* ip;
// vm.h, in struct CallFrame, replace 1 line
```

�ø��Ļᴥ������һЩ�������ġ�VM �з��� CallFrame ������ÿ���ط�����Ҫʹ�ñհ������ȣ��ӵ�ǰ�����ĳ������ж�ȡ�����ĺ꣺

```c
    (uint16_t)((frame->ip[-2] << 8) | frame->ip[-1]))

#define READ_CONSTANT() \
    (frame->closure->function->chunk.constants.values[READ_BYTE()])

#define READ_STRING() AS_STRING(READ_CONSTANT())
// vm.c, in run(), replace 2 lines
```

`DEBUG_TRACE_EXECUTION`����ʱ������Ҫ�ӱհ��л�ȡ�顣

```c
    printf("\n");
    disassembleInstruction(&frame->closure->function->chunk,
        (int)(frame->ip - frame->closure->function->chunk.code));
#endif
// vm.c, in run(), replace 2 lines
```

ͬ���ڱ�������ʱ����ʱ��

```c
    CallFrame* frame = &vm.frames[i];
    ObjFunction* function = frame->closure->function;
    size_t instruction = frame->ip - function->chunk.code - 1;
// vm.c, in runtimeError(), replace 1 line
```

����ˡ����һ�������õ�һ�� CallFrame �Կ�ʼִ�� Lox �ű��Ķ�������顣

```c
  push(OBJ_VAL(function));
  ObjClosure* closure = newClosure(function);
  pop();
  push(OBJ_VAL(closure));
  call(closure, 0);

  return run();
// vm.c, in interpret(), replace 1 line
```

����ű�ʱ����������Ȼ����һ��ԭʼ�� ObjFunction����ܺã�������ζ����Ҫ�� VM ִ����֮ǰ������װ�� ObjClosure �С�

> ���뿴�����е�ɵ����Ϊ������Ȼ��ԭʼ ObjFunction ѹ���ջ��Ȼ�������ڴ����հ��󵯳�����Ȼ���ѹ��հ���ΪʲôҪ�� ObjFunction �������������һ�������㿴����ֵĶ�ջ���鷢��ʱ������Ϊ����[���������������ռ���](http://craftinginterpreters.com/garbage-collection.html)֪��һЩ�ѷ���Ķ���

�����ֻص��˽��������û���������ʲô���𣬵��Ǳ������������ɴ������ VM Ϊÿ��������������һ���հ���ÿ�� VM ִ�к�������ʱ�������Ὣ ObjFunction ��װ��һ���µ� ObjClosure �С�VM �����ಿ�����ڴ�����Щ������ ObjClosures���������ĵ����顣��������׼��������Щ�հ���������ʲô�ˡ�

## 252 ��ֵ Upvalues

���еĶ�д�ֲ�������ָ������ڵ���������ջ���ڡ�������Χ�����ľֲ�����λ���ڲ������Ĵ���֮�⡣������ҪһЩ�µ�ָ�

��򵥵ķ���������һ��ָ���ָ��ʹ�õ�ǰ��������ջ�� ���ƫ��,  �Ա��ȡ��ǰ�ߺ�������ջ�����ݡ������ձ���������ջ�ϣ��ǽ������á�������֮ǰ�����ģ���Щ������ʱ���������ǵĺ����������ڻ���������ζ�����ǲ���������ջ��(����ִ����������ջ֡)��

��ô����һ����򵥵ķ��������κα��رյľֲ�����������ʼ�մ����ڶ��С�����Χ�����еľֲ�����������ִ��ʱ��VM �ᶯ̬��Ϊ������ڴ档�������Ϳ�������Ҫ��ʱ���ӳ��������ڡ�

��� clox ���ǵ��̱��������⽫��һ���ܺõķ������������ǵ�ѡ����ʵ��ʱ�ܶ�����ʹ�����ø������ѡ�����������ӣ�

```js
fun outer() {
  var x = 1;    // (1)
  x = 2;        // (2)
  fun inner() { // (3)
    print x;
  }
  inner();
}
```

�ڵ���`inner()`����`(3)`ʱ������������ `x` ����`(1)` ��������ֵ����`(2)`����Щ�Ѿ�����ˣ� Ȼ����`x`ʵ�����Ѿ���ա�����û��һ�ּ򵥵ķ������Է��ز��޸��ѷ����Ĵ����Խ���`x`���⴦�����ԣ�������Ҫһ���������������հ�������ȫ����ͨ�ֲ�����һ��������ջ�У�ֱ��������ա�

���˵��ǣ������ Lua �����Ŷӣ���������һ�����������ʹ�����ǳ�Ϊ **��ֵ(upvalue)** �ļ�Ӳ㡣��ֵ(upvalue)ָ���Ƿ�պ����еľֲ�������ÿ���հ���ά��һ����ֵ���飬ÿ����ֵ��Ӧ�հ�ʹ�õ�ÿ����Χ�ֲ�������

upvalue ָ��ջ��������ı������ڵ�λ�á����հ���Ҫ����һ����ձ���ʱ������ͨ����Ӧ����ֵ����ȡ�����״�ִ�к���������Ϊ�䴴���հ�ʱ��VM �ᴴ����ֵ���鲢���������������ԡ����񡱱հ��������Χ�ֲ�������

���磬�����������򶪸� clox��

```js
{
  var a = 3;
  fun f() {
    print a;
  }
}
```

������������ʱ����ͬ���ڴ��й���һ�����������ʾ��

![](./assets/9dfacd45cf5f0b2fa04e70c1273e9dee4f01b3f8.png)

����ܿ������Ʋ��ɵ�������Ҫ���¡����ǻ�Ŭ������ġ���Ҫ�Ĳ����� upvalues �����������Ҳ���ľֲ���������ļ�Ӳ㣬��ʹ���뿪ջҲ����ˡ����ڿ�ʼ֮ǰ��������רע�ڱ��벶��ı�����

### 25.2.1 ������ֵ compiling upvalues

������һ��������ϣ���ڱ����ڼ��������ܶ�Ĺ������Ա�������ʱ�򵥺Ϳ��١����ھֲ������� Lox ���Ǵʷ�������ģ��ڱ���ʱ���㹻��֪ʶ���������������Щ��Χ�ֲ������Լ�������Щ�ֲ�������λ�á�������������ζ������֪���հ���Ҫ������ֵ�����ǲ�����Щ�������Լ�����������ջ��������Щջ�۰�����Щ������

Ŀǰ��������������(resolves)һ����ʶ��ʱ����������ڲ㵽����������ǰ�����Ŀ�����������ڸú�����û���ҵ��ñ���������ñ���������ȫ�ֱ��������ǲ����Ǳհ������ľֲ������򡪡����ǻᱻֱ����������ô����һ���仯��Ϊ��Щ��Χ�������������һ����������(The first change, then, is inserting a resolution step for those outer local scopes.)��

```c
  if (arg != -1) {
    getOp = OP_GET_LOCAL;
    setOp = OP_SET_LOCAL;
  } else if ((arg = resolveUpvalue(current, &name)) != -1) {
    getOp = OP_GET_UPVALUE;
    setOp = OP_SET_UPVALUE;
  } else {
// compiler.c, in namedVariable() 
```

�����`resolveUpvalue()`�����������κ���Χ�����������ľֲ�����������ҵ�һ�������᷵�ظñ����ġ���ֵ���������������Ժ���˽�����ζ��ʲô�������������� -1 ��ָʾδ�ҵ��ñ���������ҵ������ǽ�ʹ����������ָ��ͨ������ֵ��ȡ��д�������

```c
  OP_SET_GLOBAL,
  OP_GET_UPVALUE,
  OP_SET_UPVALUE,
  OP_EQUAL,
// chunk.h, in enum OpCode
```

�������϶��µ�ʵ�ַ�ʽ�������ҽ��ܿ�����չʾ��Щ������ʱ����ι����ġ�����Ҫ��ע�Ĳ����Ǳ��������ʵ�ʽ�����ʶ����

```c
static int resolveUpvalue(Compiler* compiler, Token* name) {
  if (compiler->enclosing == NULL) return -1;

  int local = resolveLocal(compiler->enclosing, name);
  if (local != -1) {
    return addUpvalue(compiler, (uint8_t)local, true);
  }

  return -1;
}
// compiler.c, add after resolveLocal()
```

�ڵ�ǰ�������������޷������ֲ�����(resolveLocal)�������(addUpvalue)�����֪���ñ������ڵ�ǰ�������С�����һ�£�Compiler �洢��һ��ָ��հ������� Compiler ��ָ�룬��Щָ���γ���һ������һֱ����������ĸ� Compiler����ˣ������յ� Compiler ��`NULL`������֪���Ѿ����������ĺ�����û���ҵ��ֲ��������ñ����ض���ȫ�ֵģ����Է��� -1��

> ���������ճ�Ϊһ����ȫδ����ı�������������ȫ�ֱ��������� Lox �У�����ֱ������ʱ�ż�⵽�ô�����˴ӱ������ĽǶ����������ǡ�ϣ����ȫ�ֵġ���

�������ǻ᳢�Խ���ʶ������Ϊ*���*�������еľֲ����� ��Ҳ����˵�������ڵ�ǰ����֮��Ѱ���������磺

```js
fun outer() {
  var x = 1;
  fun inner() {
    print x; // (1)
  }
  inner();
}
```

�ڱ����ʶ�����ʽʱ`(1)`��`resolveUpvalue()`������`outer()`�������ľֲ�����`x`������ҵ��������������������һ��������ô�ͳɹ��ؽ����˱��������Ǵ���һ����ֵ���Ա��ڲ���������ͨ�������ʱ�����upvalue �����ﴴ����

```c
static int addUpvalue(Compiler* compiler, uint8_t index,
                      bool isLocal) {
  int upvalueCount = compiler->function->upvalueCount;
  compiler->upvalues[upvalueCount].isLocal = isLocal;
  compiler->upvalues[upvalueCount].index = index;
  return compiler->function->upvalueCount++;
}
// compiler.c, add after resolveLocal()
```

����������һ�� upvalue ��������������ÿ�������������н����ķ�ձ�ʶ�������ǵñ������� Local ������η�ӳ����ʱ�ֲ��������ڵ�ջ������������µ���ֵ������ͬ���ķ�ʽ�����������������е�����������ʱ ObjClosure ����ֵ���ڵ�������ƥ�䡣

�˺�������������һ���µ���ֵ���������ٺ���ʹ�õ���ֵ������������ֱ�Ӵ洢�� ObjFunction �����У���Ϊ���ǻ���Ҫ������ʱʹ�ø����֡�

> �볣���ͺ���Ԫ��(arity)һ����upvalue ��������һ�����ɱ�����������ʱ֮����������ЩС�����С�

`index`�ֶθ��ٷ�վֲ������Ĳ�������������������֪����Ҫ�����պ����е��ĸ����������Ǻܿ�ͻ�ص��Ǹ�`isLocal`�ֶε���;�����`addUpvalue()`���ش�������ֵ�ں�������ֵ�б��е���������������Ϊ`OP_GET_UPVALUE`��`OP_SET_UPVALUE`ָ��Ĳ�������

���ǽ�����ֵ�Ļ���˼�룬���ú�����δ��ȫ���졣�հ����Զ��������Χ�����е�ͬһ������������������£����ǲ����˷�ʱ����ڴ�Ϊÿ����ʶ�����ʽ����һ����������ֵ��Ϊ�˽��������⣬���������һ���µ���ֵ֮ǰ���������ȼ�麯���Ƿ��Ѿ���һ����ոñ�������ֵ��

```c
  int upvalueCount = compiler->function->upvalueCount;

  for (int i = 0; i < upvalueCount; i++) {
    Upvalue* upvalue = &compiler->upvalues[i];
    if (upvalue->index == index && upvalue->isLocal == isLocal) {
      return i;
    }
  }

  compiler->upvalues[upvalueCount].isLocal = isLocal;
// compiler.c, in addUpvalue()
```

����������������ҵ�һ����ֵ�����������������ӵ��Ǹ���ƥ�䣬ֻ�践�ظ���ֵ����������ʹ���������򣬲���ʧ�ܺ�����µ���ֵ��

�������������ʺ��޸�һ����״̬�����������Ƕ����������ȣ�����ֵ������ӵ� ObjFunction��

```c
  int arity;
  int upvalueCount;
  Chunk chunk;
// object.h, in struct ObjFunction
```

����������� C ����Ա�������ڵ�һ�η��� ObjFunction ʱ����������ʼ����

```c
  function->arity = 0;
  function->upvalueCount = 0;
  function->name = NULL;
// object.c, in newFunction()
```

�ڱ������У�Ϊ��ֵ���������һ���ֶΡ�

```c
  int localCount;
  Upvalue upvalues[UINT8_COUNT];
  int scopeDepth;
// compiler.c, in struct Compiler
```

Ϊ�˼������������һ���̶��Ĵ�С��`OP_GET_UPVALUE`�� `OP_SET_UPVALUE`ָ��ʹ�õ��ֽڲ���������ֵ�������б��룬��˺�������ӵ�ж��ٸ���ֵ�������Ƶġ��������Թرն��ٸ�Ψһ���������ڴˣ����ǿ��Ը���������ô��ľ�̬���顣���ǻ���Ҫȷ��������������������ơ�

```c
    if (upvalue->index == index && upvalue->isLocal == isLocal) {
      return i;
    }
  }

  if (upvalueCount == UINT8_COUNT) {
    error("Too many closure variables in function.");
    return 0;
  }

  compiler->upvalues[upvalueCount].isLocal = isLocal;
// compiler.c, in addUpvalue()
```

���Upvalue �ṹ���ͱ���

```c
typedef struct {
  uint8_t index;
  bool isLocal;
} Upvalue;
// compiler.c, add after struct Local
```

`index`�ֶδ洢���ڲ������ֵ���ز�ۡ���`isLocal`�ֶ�ӵ���Լ��Ĳ��֣����ǽ��ڽ��������н��ܡ�

### 25.2.2 չ����ֵ flattening-upvalues

����֮ǰչʾ��ʾ���У��հ����ڷ����� ���ڵķ�պ����������ı�����Lox ��֧�ַ�����*�κ�*��շ�Χ�������ľֲ��������磺

```js
fun outer() {
  var x = 1;
  fun middle() {
    fun inner() {
      print x;
    }
  }
}
```

�����������`inner()`����`x`���ñ���������`middle()`�ж���ģ�������`outer()`�ж���ġ�����Ҳ��Ҫ�����������������������Ϊ�Ⲣ���ѣ���Ϊ����ֻ��λ��ջ�и����µ�λ�á����ǿ�������ƻ������ӣ�

> ������ڱ�������Ϲ�����ʱ���㹻�����㽫������һ�־���ĥ���ļ��ܣ����Դ��������������������Щ�����ڼ���������Ч�ģ������ܻ����Щ��������������˱�д��ʵ�֡�

```js
fun outer() {
  var x = "value";
  fun middle() {
    fun inner() {
      print x;
    }

    print "create inner closure";
    return inner;
  }

  print "return from outer";
  return middle;
}

var mid = outer(); //(1)
var in = mid();    //(2)
in();
```

����������ʱ����Ӧ�ô�ӡ��

```
return from outer
create inner closure
value
```

��֪������ܸ��ӡ���Ҫ�Ĳ�����`outer()`(`x`�ڴ˴�����) ,   ��ִ��`inner()`����֮ǰ (1)�� outer()�Ѿ����ز�������ջ�е����б�������ˣ������Ǵ���`inner()`�հ���ʱ��`x`�Ѿ�������ջ��

�������Ϊ��׷����ִ�����̣�

![](./assets/72d2d936849206616463a3284a45881beab0792e.png)

˳��ִ�м�ͷ�鿴���`x`�ڲ��� �� ֮ǰ������ , Ȼ���Ժ���� �ۣ�����ȷʵ���������⣺

1. ��Ҫ������ ���ڷ�պ���֮�����Χ�����������ľֲ�������

2. ��Ҫ�ܹ������Ѿ��뿪��ջ�ı�����

���˵��ǣ����������� VM �����ֵ��������ֵ��ȷ������ڸ��������Ѷ�ջ�ı�������ˣ���һ��������������У�����ʹ�� upvalues ��  ���� upvalues �����ڽ��ں����ⲿ�����ı�����

�������������հ�����ֲ���������ڷ�պ����е�**����**��ֵ�����һ�����Ƕ�׵ĺ���������һ���ڼ���֮�������ľֲ���������ͨ����ÿ����������һ����ֵ�Թ���һ��������ȡ����������ͨ�����е��м亯����

![](./assets/de28c91dcc24c7bd172ed271b0e925ee4c576bfa.png)

�������ʾ���У�`middle()`����`x`���ڷ�պ���`outer()`�еľֲ�����������洢�����Լ�����ֵ�С���ʹ`middle()`��������`x`.Ȼ�󣬵�`inner()`������ִ��ʱ�����ıհ���`middle()` ObjClosure�л�ȡ����� upvalue`x`��һ������ֻ�ӽ��ڵĺ����в��񡪡������Ǿֲ�ֵ������ֵ��������֤���ڲ���������ִ�е���һ����Ȼ���ڡ�

Ϊ��ʵ����һ�㣬`resolveUpvalue()`��õݹ顣

```c
  if (local != -1) {
    return addUpvalue(compiler, (uint8_t)local, true);
  }

  int upvalue = resolveUpvalue(compiler->enclosing, name);  //<<
  if (upvalue != -1) {
    return addUpvalue(compiler, (uint8_t)upvalue, false); 
  }                                                         //<<

  return -1;
// compiler.c, in resolveUpvalue()
```

��������д��룬���ҷ������������һ��������ĺ�����ս�ԡ�������ʵ���Ҳ�û�з����κ��¶�����ֻ�Ǵ� Lua ��ֲ��������������ݹ麯��Ҫô�ڵݹ����֮ǰ������й�����**ǰ�����**���򡰽����С�����Ҫô�ڵݹ����֮��������й�����**�������**���򡰽����б��ݡ���������������߼����֮���ݹ���þ����м䡣

���ǽ����������������ȣ������ڷ�պ�����Ѱ��ƥ��ľֲ�����������ҵ�һ���������Ǹ����ز����ء����ǻ��������

> ��Ȼ����һ�ֻ��������û�з�պ���������������£��ñ����޷����ʷ����������Ǳ���Ϊȫ�ֱ�����

�������ǻ��ڽ��ڷ�յĺ���֮��Ѱ�Ҿֲ�������ͨ���ݹ����`resolveUpvalue()` ����*enclosing*�����������ǵ�ǰ�ı�������������һ�㡣��һϵ��`resolveUpvalue()`��������Ƕ�ױ����������У�ֱ��������һ�������������Ҫô���ҵ�һ��ʵ�ʵľֲ�����������Ҫô�������������

���ҵ��ֲ�����ʱ��Ƕ�������`resolveUpvalue()`���ûᲶ������������ֵ�������⽫���ص����ڲ�������������һ�ε��á��õ��ò���*��Χ*��������ֵ���������ơ����Ŷ�ÿ��`resolveUpvalue()`Ƕ�׵��õķ��أ�����������ȡ��������ĺ������������г������������ڽ����ı�ʶ�����ڴ˹����е�ÿһ�������Ƕ������м亯�����һ����ֵ���������ɵ���ֵ�������ݸ���һ�����á�

> ÿ��`resolveUpvalue()`�ݹ���ö����߳�һ�㺯��Ƕ�ס���ˣ��ڲ��ݹ���������ⲿǶ�����������Ҿֲ����������ڲ�`resolveUpvalue()`�ݹ���ý����*�����*����������ʵ�������ñ����ķ�պ����ڡ�

����`x`ʱ������ԭʼʾ�����ܻ�����������

![](./assets/3803da5525a13c9692cb5cd4586ecac1c5418448.png)

ע�⣬��`addUpvalue()` �µĵ��ò���`isLocal`ֵ��`false`�������������ñ�־���Ʊհ��ǲ���ֲ��������ǲ�����Χ��������ֵ��

�����������ﺯ��������ĩβʱ��ÿ���������ö��ѽ���Ϊ�ֲ�����ֵ��ȫ�֡�ÿ�� upvalue �������δ���Χ�ĺ����в���һ���ֲ������������ڴ��ݱհ�������²���һ�� upvalue���������㹻�������������ֽ��룬��������ʱ����һ���հ�������������ȷ�ı�����

```c
  emitBytes(OP_CLOSURE, makeConstant(OBJ_VAL(function)));

  for (int i = 0; i < function->upvalueCount; i++) {
    emitByte(compiler.upvalues[i].isLocal ? 1 : 0);
    emitByte(compiler.upvalues[i].index);
  }
}
// compiler.c, in function()
```

`OP_CLOSURE`ָ��Ķ���֮�����������пɱ��С�ı��롣���ڱհ������ÿ����ֵ�������������ֽڲ�������ÿ�Բ�����ָ������ֵ��������ݡ������һ���ֽ���1�����Ჶ���պ����е�һ���ֲ����������Ϊ0�����Ჶ������һ����ֵ����һ���ֽ���Ҫ����ı��زۻ���ֵ������

������ֵı�����ζ��������Ҫ�ڷ���������Ϊ`OP_CLOSURE`.

```c
      printf("\n");

      ObjFunction* function = AS_FUNCTION(
          chunk->constants.values[constant]);
      for (int j = 0; j < function->upvalueCount; j++) {
        int isLocal = chunk->code[offset++];
        int index = chunk->code[offset++];
        printf("%04d      |                     %s %d\n",
               offset - 2, isLocal ? "local" : "upvalue", index);
      }

      return offset;
// debug.c, in disassembleInstruction()
```

���磬�������½ű���

```js
fun outer() {
  var a = 1;
  var b = 2;
  fun middle() {
    var c = 3;
    var d = 4;
    fun inner() {
      print a + c + b + d;
    }
  }
}
```

������Ƿ���ഴ���հ���ָ��`inner()`�������ӡ��

```
0004    9 OP_CLOSURE          2 <fn inner>
0006      |                     upvalue 0
0008      |                     local 1
0010      |                     upvalue 1
0012      |                     local 2
```

���ǻ��������������򵥵�ָ������ӷ�������֧�֡�

```c
    case OP_SET_GLOBAL:
      return constantInstruction("OP_SET_GLOBAL", chunk, offset);
    case OP_GET_UPVALUE:
      return byteInstruction("OP_GET_UPVALUE", chunk, offset);
    case OP_SET_UPVALUE:
      return byteInstruction("OP_SET_UPVALUE", chunk, offset);
    case OP_EQUAL:
// debug.c, in disassembleInstruction()
```

���Ƕ���һ�����ֽڲ�����������û��ʲô�����˷ܵ����鷢��������ȷʵ��Ҫ���һ��incluce���Ա����ģ����Է���`AS_FUNCTION()`.

```c
#include "debug.h"
#include "object.h"
#include "value.h"
// debug.c
```

�����������ǵı���������������Ҫ�ĵط�������ÿ�����������������һ��`OP_CLOSURE`ָ����һϵ�в������ֽڶԣ�����������ʱ��Ҫ�����ÿ����ֵ����ʱ������ VM ����һ�߲���ʼ�����ˡ�

## 25.3 ��ֵ���� upvalue objects

���ڣ�ÿ��`OP_CLOSURE`ָ����һϵ���ֽڣ���Щ�ֽ�ָ�� ObjClosure Ӧӵ�е���ֵ���ڴ�����Щ������֮ǰ����Ҫһ����ֵ������ʱ��ʾ��

```c
typedef struct ObjUpvalue {
  Obj obj;
  Value* location;
} ObjUpvalue;
// object.h, add after struct ObjString
```

����֪�� upvalues ��������ٴ����ڶ�ջ�еķ�ձ���������ζ��һ�����Ķ�̬���䡣�� VM ��ִ�д˲�������򵥷����������еĶ���ϵͳ�Ϲ�������������[��һ��](http://craftinginterpreters.com/garbage-collection.html)ʵ�������ռ���ʱ��GC Ҳ���Թ��� upvalues ���ڴ档

��ˣ�����ʱ��ֵ�ṹ��һ�����е��� Obj ��ͷ�ֶε� ObjUpvalue����������һ��`location`ָ���ձ������ֶΡ���ע�⣬����ָ��ֵ��ָ�룬������ֵ�������ǶԱ��������ã�������ֵ�������Ҫ����Ϊ����ζ�ŵ�����ֵ���������ı���ʱ���������ڷ����ʵ�ʱ����������Ǹ��������磺

```js
fun outer() {
  var x = "before";
  fun inner() {
    x = "assigned";
  }
  inner();
  print x;
}
outer();
```

�������Ӧ�ô�ӡ��assigned������ʹ�հ���ֵ��`x`��������Χ�ĺ�����������

��Ϊ��ֵ�Ƕ����������������г����Ķ�����ƣ���һ�����ƹ��캯���ĺ�����ʼ��

```c
ObjString* copyString(const char* chars, int length);
ObjUpvalue* newUpvalue(Value* slot);
void printObject(Value value);
// object.h, add after copyString()
```

�����÷�ձ������ڵĲ۵ĵ�ַ������ʵ�֣�

```c
ObjUpvalue* newUpvalue(Value* slot) {
  ObjUpvalue* upvalue = ALLOCATE_OBJ(ObjUpvalue, OBJ_UPVALUE);
  upvalue->location = slot;
  return upvalue;
}
// object.c, add after copyString()
```

���Ǽ򵥵س�ʼ�����󲢴洢ָ�롣����Ҫһ���µĶ������͡�

```c
  OBJ_STRING,
  OBJ_UPVALUE
} ObjType;
// object.h, in enum ObjType
```

��Ȼ����һ���������������ĺ�����

```c
      FREE(ObjString, object);
      break;
    }
    case OBJ_UPVALUE:
      FREE(ObjUpvalue, object);
      break;
  }
// memory.c, in freeObject()
```

����հ����Թر�ͬһ����������� ObjUpvalue ��ӵ�������õı�������ˣ�ΨһҪ�ͷŵ��� ObjUpvalue ����

��󣬴�ӡ��

```c
    case OBJ_STRING:
      printf("%s", AS_CSTRING(value));
      break;
    case OBJ_UPVALUE:
      printf("upvalue");
      break;
  }
// object.c, in printObject()
```

��ӡ�������û�û���á�Upvalues ֻ�Ƕ���������ǿ������� VM ���ڴ�������ǲ��� Lox �û������ڳ�����ֱ�ӷ��ʵ�һ��ֵ��������δ�����Զ��������ִ��... �������Է�ֹ�����������Ǵ󺰴��δ����� switch case���������ǵ��ˡ�

### 25.3.1 �հ��е���ֵ upvalues in closures

����һ�ν�����ֵʱ��˵��ÿ���հ�����һ����ֵ���顣��������Ŭ���ص�ʵʩ��һ�㡣

```c
  ObjFunction* function;
  ObjUpvalue** upvalues;
  int upvalueCount;
} ObjClosure;
// object.h, in struct ObjClosure
```

��ͬ�ıհ������в�ͬ��������ֵ��������Ҫһ����̬���顣upvalues ����Ҳ�Ƕ�̬����ģ��������յõ�һ������ָ�롪��һ��ָ��̬����� upvalues ָ�������ָ�롣���ǻ��洢������Ԫ�ص�������

> ����ֵ�����洢�ڱհ����Ƕ���ģ���Ϊ ObjClosure ���õ� ObjFunction Ҳ�����ü�����������һ���������ֵĴ�����Ϊ�˰��� GC���ڱհ���Ӧ�� ObjFunction �Ѿ����ͷź��ռ���������Ҫ֪�� ObjClosure ����ֵ�����С��

������һ�� ObjClosure ʱ��������һ���ʵ���С����ֵ���飬�ڱ���ʱȷ�����洢�� ObjFunction �С�

```c
ObjClosure* newClosure(ObjFunction* function) {
  ObjUpvalue** upvalues = ALLOCATE(ObjUpvalue*,
                                   function->upvalueCount);
  for (int i = 0; i < function->upvalueCount; i++) {
    upvalues[i] = NULL;
  }

  ObjClosure* closure = ALLOCATE_OBJ(ObjClosure, OBJ_CLOSURE);
// object.c, in newClosure()
```

�ڴ����հ�������֮ǰ��������ֵ���鲢������ȫ����ʼ��Ϊ`NULL`.���Χ�Ƽ���������ʽ��һ�־��ĵ��赸����ȡ�ã����������ģ������ռ�����ȷ���ڴ��������Զ���ῴ��δ��ʼ�����ڴ档

Ȼ�����ǽ�����洢���µıհ��У����� ObjFunction ���Ƽ�����

```c
  closure->function = function;
  closure->upvalues = upvalues;
  closure->upvalueCount = function->upvalueCount;
  return closure;
// object.c, in newClosure()
```

�������ͷ�һ�� ObjClosure ʱ������Ҳ�ͷ�����ֵ���顣

```c
    case OBJ_CLOSURE: {
      ObjClosure* closure = (ObjClosure*)object;
      FREE_ARRAY(ObjUpvalue*, closure->upvalues,
                 closure->upvalueCount);
      FREE(ObjClosure, object);
// memory.c, in freeObject()
```

ObjClosure ������ӵ�� ObjUpvalue ���󣬵���ӵ�а���ָ����Щ��ֵ��ָ������顣

��������һ���հ�ʱ���ڽ������������ֵ���顣��`OP_CLOSURE`֮�󣬱������в��������Բ鿴ÿ����۲������ֵ���͡�

```c
        push(OBJ_VAL(closure));
        for (int i = 0; i < closure->upvalueCount; i++) {
          uint8_t isLocal = READ_BYTE();
          uint8_t index = READ_BYTE();
          if (isLocal) {
            closure->upvalues[i] =
                captureUpvalue(frame->slots + index);
          } else {
            closure->upvalues[i] = frame->closure->upvalues[index];
          }
        }
        break;
// vm.c, in run()
```

��δ����Ǳհ���Ч������ʱ�̡����ǵ����հ�������ÿ����ֵ������ÿһ������ȡһ�Բ������ֽڡ���� upvalue �ڷ�պ����йر���һ���ֲ����������Ǿ���`captureUpvalue()`��ɹ�����

���򣬴���Χ�ĺ����в���һ����ֵ���ں���������ĩβ����һ��`OP_CLOSURE`ָ�������ִ�и���������һ�̣�*��ǰ*����������Χ�ĺ���������ζ�ŵ�ǰ�����ıհ��洢�ڵ��ö�ջ������ CallFrame �С���ˣ�Ҫ�ӷ�պ����л�ȡ��ֵ�����ǿ���ֱ�Ӵӱ��ر���`frame`�ж�ȡ�����ñ��������˶Ը� CallFrame �����á�

�رվֲ���������Ȥ���󲿷ֹ���������һ�������ĺ����У����������Ǽ���Ҫ���ݸ����Ĳ�����������Ҫ����Χ������ջ�����л�ȡָ�򲶻�ı��ز۵�ָ�롣�ô��ڴ� `frame->slots`��ʼ��ָ�����㡣��`index`ƫ������ӵ�����Ҫ����ı��ز���С����������ﴫ�ݸ�ָ�룺

```c
static ObjUpvalue* captureUpvalue(Value* local) {
  ObjUpvalue* createdUpvalue = newUpvalue(local);
  return createdUpvalue;
}
// vm.c, add after callValue()
```

���ƺ��е�ɵ����������ֻ�Ǵ���һ���µ� ObjUpvalue ����������Ķ�ջ�۲����䷵�ء�������Ҫһ�������ĺ������ţ�������*û��*��������֪���������ջ���������Ӹ�����롣

���ȣ��������ܽ�һ�������������顣�ص����ڴ���`OP_CLOSURE`�Ľ����������У�������������˶���ֵ����ĵ�������ʼ����ÿ�����顣��ɺ�������һ���µıհ������а���һ������ָ���������ֵ�����顣

�����������ǾͿ���ʵ������Щ��ֵһ��ʹ�õ�ָ�

```c
      }
      case OP_GET_UPVALUE: {
        uint8_t slot = READ_BYTE();
        push(*frame->closure->upvalues[slot]->location);
        break;
      }
      case OP_EQUAL: {
// vm.c, in run()
```

�������ǵ�ǰ��������ֵ������������������Ǽ򵥵ز�����Ӧ����ֵ����������λ��ָ���Զ�ȡ�ò��е�ֵ�����ñ��������Ƶġ�

```c
      }
      case OP_SET_UPVALUE: {
        uint8_t slot = READ_BYTE();
        *frame->closure->upvalues[slot]->location = peek(0);
        break;
      }
      case OP_EQUAL: {
// vm.c, in run()
```

���ǽ�ջ������ֵ�洢����ѡ��ֵָ��Ĳ��С�����ֲ�������ָ��һ������Щָ����ٶȺ���Ҫ���û����򲻶ϵض�ȡ��д������������������������ôһ�ж����������ң�������һ����ʹ���ǿ��ٵķ����Ǳ������ǵļ򵥡���������ָ��ǳ��ã�û�п�������û�и��ӵ��㷨��ֻ�м������ָ���һ��`push()`.

> set ָ���Ӷ�ջ��*����*ֵ����Ϊ���ס����ֵ�� Lox �еı��ʽ����ˣ���ֵ�Ľ��������ֵ������Ҫ��������Χ���ʽ�Ķ�ջ�С�

����һ����̱���ֻҪ���б����������ڶ�ջ�У����Ǿ����˿��Թ����ıհ������������

```js
fun outer() {
  var x = "outside";
  fun inner() {
    print x;
  }
  inner();
}
outer();
```

��������������ȷ��ӡ��outside����

## 25.4  ��յ���ֵ closed-upvalues

��Ȼ���հ���һ���ؼ����������ǻ������Ҫ������������ʹ�����������ĺ�������֮��Ҳ����ˡ�������һ��*Ӧ��*�����õ����ӣ�

```c
fun outer() {
  var x = "outside";
  fun inner() {
    print x;
  }

  return inner;
}

var closure = outer();
closure();
```

�������������������... ˭֪���������ã�������ʱ�������Ӳ��ٰ�����ձ�����ջ���ж�ȡ�������Ҷ���ᵽ�������������֢�����ڱհ��еı���û��ջ���塣����ζ�ŵ��������ǵĺ�������ʱ�����뽫���Ǵ�ջ��ȡ�������µ����һ�ھ����������ġ�

### 25.4.1  ֵ�ͱ���

�ڿ�ʼ��д����֮ǰ����������̽��һ����Ҫ������㡣�հ��ǹر�һ��ֵ����һ��(����)�������ⲻ������һ��ѧ�����⡣�Ҳ�ֻ�Ǵ�ë��á����ǣ�

> ��� Lox ��������䣬��*��*��һ��ѧ�����⡣

```c
var globalSet;
var globalGet;

fun main() {
  var a = "initial";

  fun set() { a = "updated"; }
  fun get() { print a; }

  globalSet = set;
  globalGet = get;
}

main();
globalSet();
globalGet();
```

�ⲿ`main()`�������������հ��������Ǵ洢��ȫ�ֱ����У��Ա����Ǳ�`main()`�����ִ��ʱ��������������հ���������ͬ�ı�������һ���հ�Ϊ�����һ����ֵ���ڶ����հ���ȡ�ñ�����

> ��ʹ�ü���ȫ�ֱ�����һ��ʵ������Ҫ������ҪһЩ������һ��������������ֵ�������� Lox ��û���κ����͵ļ������ͣ��ҵ�ѡ�������޵ġ�

`globalGet()`��ӡ�ĵ�����ʲô������հ�����*ֵ*����ôÿ���հ��������Լ�`a`�ĸ��������а����հ���������ִ��ʱ��ֵ����`globalSet()`�ĵ��ý��޸�`set()`�ĸ���`a`����`get()`�ĸ���������Ӱ�졣��ˣ�����`globalGet()`����ӡ��initial����

����հ��رձ�������`get()`��`set()`�����������ã�ͬ*һ���ɱ����*��`set()`����`a`ʱ���������`get()`��ȡ`a`�����ݡ�ֻ��һ��`a`��������������ζ�ŵ���`globalGet()`����ӡ��updated����

Ӧ�����ĸ���Lox ������֪���Ĵ���������հ����ԵĴ��Ǻ��ߡ��հ���������������Խ�������Ϊ����*ֵ���ڵ�λ��*�������Ǵ������ڶ�ջ�ϵķ�ձ���ʱ�����ס��һ�����Ҫ����һ�������ƶ�������ʱ��������Ҫȷ������ñ��������бհ�����������*һ��*��λ�õ����á������������������仯ʱ�����бհ����ῴ���仯��

### 25.4.2  ���ڵ���ֵ closing upvalues

����֪���ֲ��������ǴӶ�ջ��ʼ�������ٶȸ��죬���������ǵĵ��̱������ڷ��ֱ����ѱ�����֮ǰ�������롣��֪��������հ���������������ĺ����������ڸ�������հ�������Ҫ�ƶ������С�

�� Lua ֮�����ǽ�ʹ��**open upvalue**������ָ�����ڶ�ջ�ϵľֲ������� upvalue����һ�������ƶ�������ʱ����������*�ر�*��ֵ�������Ȼ��һ��**�رյ���ֵ ��closed upvalue��**��������Ҫ�ش�����������ǣ�

1. ��ձ����ڶ��ϵ�ʲôλ�ã�

2. ʲôʱ��ر�upvalue��

��һ������Ĵ𰸺ܼ򵥡������Ѿ��ڶ�����һ������Ķ�������ʾ�Ա��������á���ObjUpvalue������ձ������ƶ��� ObjUpvalue �ṹ�ڲ������ֶ��С������Ͳ���Ҫ���κζ���Ķѷ������ر���ֵ��

�ڶ�������Ҳ�ܼ򵥡�ֻҪ�����ڶ�ջ�ϣ��Ϳ����д��������������Ҹô�������������������Խ������������ѵ��߼�ʱ��Խ��Խ�á���������ھֲ���������������ʱ����ת�ƣ����Կ϶��ڸõ�֮��û�д���᳢�Դ�ջ�з��������������������������κδ�����ͼʹ���������������������

> ����ġ�֮�󡱣���ָ���Ǵʻ���ı����������ձ��������Ŀ�`}`֮��Ĵ��롣

���ֲ�����������Χʱ���������Ѿ�����`OP_POP`ָ����һ���������հ��������ǽ���Ϊ����һ����ͬ��ָ��ñ�����ջ�����������Ӧ����ֵ�С�Ϊ�ˣ���������Ҫ֪����Щ�ֲ��������رա�

> ���������ᵯ�������������������Ĳ����;ֲ�����������Ҳ��������ʱ������Щ��

�������Ѿ�Ϊ�����е�ÿ���ֲ�����ά����һ�� Upvalue �ṹ���飬��׼ȷ���ٸ�״̬��������ǳ��ʺϻش�����հ�ʹ����Щ��������  ������̫�ʺϻش�  ����*û��*������������ֲ���������  �ر��ǣ�һ��ĳ���հ��ı�������ɣ�������ѱ�����ķ�պ����ı��������޷��ٷ����κ���ֵ״̬��

���仰˵��������ά������ֵ�����ǲ���ľֲ�������ָ�룬����������һ��������������������Ҫ�����еı��ؽṹ�����һЩ����ĸ��٣��Ա����ǿ����жϸ����ı����Ƿ񱻱հ�����

```c
  int depth;
  bool isCaptured;
} Local;
// compiler.c, in struct Local
```

 ���local���κ��Ժ��Ƕ�׺�������������ֶ���`true`���������δ�������оֲ�������

```c
  local->depth = -1;
  local->isCaptured = false;
}
// compiler.c, in addLocal()
```

ͬ������������ʽ���������⡰slot zero local��Ҳû�б�����

> �ڱ���ĺ��棬�û�*��*�п��ܲ���˱�����ֻ�������ｨ��һЩԤ�ڡ�

```c
  local->depth = 0;
  local->isCaptured = false;
  local->name.start = "";
// compiler.c, in initCompiler()
```

�ڽ�����ʶ��ʱ�������������Ϊ�ֲ�����������һ����ֵ�����ǽ�����Ϊ�Ѳ���

```c
  if (local != -1) {
    compiler->enclosing->locals[local].isCaptured = true;
    return addUpvalue(compiler, (uint8_t)local, true);
// compiler.c, in resolveUpvalue()
```

���ڣ��ڿ��������ĩβ������������������Ϊ�ֲ������ͷŶ�ջ��ʱ�������ж���Щ��Ҫ���������ϡ����ǽ�Ϊ��ʹ��һ����ָ�

```c
  while (current->localCount > 0 &&
         current->locals[current->localCount - 1].depth >
            current->scopeDepth) {
    if (current->locals[current->localCount - 1].isCaptured) {
      emitByte(OP_CLOSE_UPVALUE);
    } else {
      emitByte(OP_POP);
    }
    current->localCount--;
  }
// compiler.c, in endScope(), replace 1 line
```

��ָ���Ҫ������������֪����������ָ��ִ�е�ʱ�򣬱���������ջ������������ָ�

```c
  OP_CLOSURE,
  OP_CLOSE_UPVALUE,
  OP_RETURN,
// chunk.h, in enum OpCode
```

��Ϊ����Ӽ򵥵ķ�������֧�֣�

```c
    }
    case OP_CLOSE_UPVALUE:
      return simpleInstruction("OP_CLOSE_UPVALUE", offset);
    case OP_RETURN:
// debug.c, in disassembleInstruction()
```

�������������ɵ��ֽ���׼ȷ�ظ�������ʱÿ������ľֲ����������ʱ�ƶ������С����õ��ǣ���ֻ�Ա��հ�ʹ�ò���Ҫ������������ľֲ����������������������ϣ���û�ֻΪ����ʹ�õĹ��ܸ��ѵ���������Ŀ�ꡣû�б��հ�ʹ�õı�����ȫ��ջ�����������������������ǰһ����

### 25.4.3 ���ٿ��ŵ���ֵ  tracking-open-upvalues

������ת������ʱ���档�����ǿ��Խ���`OP_CLOSE_UPVALUE`ָ��֮ǰ����һ��������Ҫ�������Щʱ�򣬵���̸���հ��ǲ����������ֵʱ����˵����Ҫ���ǣ��������հ�����ͬһ���������������ջ������ڴ�����ȫ��ͬ�Ĵ洢λ�á�����һ�������һ���հ�д���������һ���հ��ͻῴ���仯��

���ڣ���������հ�������ͬ�ľֲ��������� VM ��Ϊÿ���հ�����һ�������� Upvalue��ȱ�ٱ�Ҫ�Ĺ��������ǽ������Ƴ�ջʱ��������ǽ�������������һ����ֵ������һ����ֵ�����й���ֵ��

> ���һ���հ�����Χ�ĺ����в�����һ��*��ֵ*����VMȷʵ������ֵ��Ƕ�װ����������������ǣ���������ֵܱհ�������ͬ�ľֲ������������Ǹ��Դ���һ�������� ObjUpvalue��

Ϊ�˽��������⣬ÿ�� VM ��Ҫһ�������ض��ֲ������۵���ֵʱ������������ָ��ò۵�������ֵ������ҵ������ظ�ʹ��������ս����������ǰ��������ֵ�������ڸ��ֱհ�����ֵ�����С���Щ�հ�����λ�� VM �ڴ��е��κ�λ�á�

��һ����Ϊ VM �ṩ���Լ������п�����ֵ�б���Щ��ֵָ������ջ�еı�����ÿ�� VM ��Ҫһ�� upvalue ʱ�����б����������ܺ�������ʵ���ϣ��⻹����ʵ�ʹرյĶ�ջ�ϵı�������������С�������հ��ĺ����������ٳ������û���������ܹؼ�ִ��·���ϡ�

> �հ���������ѭ���б����á���һ�봫�ݸ������ϵ��͵ĸ߽׺����ıհ�������[`map()`](https://en.wikipedia.org/wiki/Map_(higher-order_function))��[`filter()`](https://en.wikipedia.org/wiki/Filter_(higher-order_function))����Ӧ�úܿ졣*���Ǵ���*�հ��ĺ�������ֻ����һ�β���ͨ����ѭ��֮�⡣

���õ��ǣ����ǿ���ͨ������ָ���ջ���������Կ���ֵ�б�������򡣳���������ǲ��*��δ*�����񡪡��ڱհ�֮�乲��������������������ұհ������ڲ��񿿽���ջ�����ľֲ�������������ǰ���ջ��˳��洢���ŵ���ֵ���飬һ������Խ���������ڲ���ı������ڵĲۣ����Ǿ�֪�������ᱻ���֡����ֲ������ӽ�ջ��ʱ�����ǿ��Ժ�����˳�ѭ����

ά��һ�������б���Ҫ���м���Ч�ز���Ԫ�ء��⽨��ʹ�������б�����Ƕ�̬���顣���������Լ������� ObjUpvalue �ṹ����򵥵�ʵ����һ������ʽ�б����� next ָ����� ObjUpvalue �ṹ������ڲ���

```c
  Value* location;
  struct ObjUpvalue* next;
} ObjUpvalue;
// object.h, in struct ObjUpvalue
```

�����Ƿ���һ�� upvalue ʱ����??��û�и��ӵ��κ��б�����������?`NULL`��

```c
  upvalue->location = slot;
  upvalue->next = NULL;
  return upvalue;
// object.c, in newUpvalue()
```

VM ӵ�и��б����ͷָ������λ���� VM �ṹ�ڡ�

```c
  Table strings;
  ObjUpvalue* openUpvalues;
  Obj* objects;
// vm.h, in struct VM
```

���б�һ��ʼ�ǿյġ�

```c
  vm.frameCount = 0;
  vm.openUpvalues = NULL;
}
// vm.c, in resetStack()
```

�� VM ָ��ĵ�һ�� upvalue ��ʼ��ÿ���򿪵� upvalue ��ָ����һ���򿪵� upvalue���� upvalue ���ö�ջ���·��ľֲ�����������ű������磬

```js
{
  var a = 1;
  fun f() {
    print a;
  }
  var b = 2;
  fun g() {
    print b;
  }
  var c = 3;
  fun h() {
    print c;
  }
}
```

Ӧ�ò���һϵ�����ӵ���ֵ��������ʾ��

![](./assets/1674588218099b340fce203e4dac28040652ccfb.png)

ÿ�����ǹر�һ���ֲ�����ʱ���ڴ���һ���µ���ֵ֮ǰ�����Ƕ������б���Ѱ��һ�����еġ�

```c
static ObjUpvalue* captureUpvalue(Value* local) {
  ObjUpvalue* prevUpvalue = NULL;
  ObjUpvalue* upvalue = vm.openUpvalues;
  while (upvalue != NULL && upvalue->location > local) {
    prevUpvalue = upvalue;
    upvalue = upvalue->next;
  }

  if (upvalue != NULL && upvalue->location == local) {
    return upvalue;
  }

  ObjUpvalue* createdUpvalue = newUpvalue(local);
// vm.c, in captureUpvalue()
```

���Ǵ��б��ͷ����ʼ��������ӽ���ջ��������ֵ�����Ǳ����б�ʹ��һ��С��ָ��Ƚ�������ÿ��ָ����������Ѱ�ҵĲ��Ϸ��Ĳ۵���ֵ������������ͬʱ�����ǻ�����б���ǰ�����ֵ�����������������֮�����һ���ڵ㣬���ǽ���Ҫ���¸ýڵ��`next`ָ�롣

> ����һ�����������˴�ͷ��ʼ��������ǰ��������û������ѡ��

���ǿ����˳�ѭ��������ԭ��

1. **ͣ�ڵı��ز�۾�����������Ѱ�ҵĲ��**�����Ƿ���һ�����е���ֵ����������������������Ǹ���ֵ��

2. **������ upvalues ������**����`upvalue`��ʱ`NULL`������ζ���б��е�ÿ��������ֵ��ָ����������Ѱ�ҵĲ���Ϸ��ľֲ����������ߣ����п��ܣ���ֵ�б�Ϊ�ա��������ַ�ʽ�����Ƕ�û��Ϊ���ǵĲ���ҵ���ֵ��

3. **�����ҵ���һ�� upvalue�����ı��ز�λ������������Ѱ�ҵĲ�λ**�������б�����������ζ�������Ѿ��������������ڹرյĲ�ۣ������һ��û�����е���ֵ��

�ڵ�һ������£������Ѿ���ɲ������ˡ���������Ϊ���ز۴���һ���µ���ֵ����������б�����ȷ��λ�á�

```c
  ObjUpvalue* createdUpvalue = newUpvalue(local);
  createdUpvalue->next = upvalue;

  if (prevUpvalue == NULL) {
    vm.openUpvalues = createdUpvalue;
  } else {
    prevUpvalue->next = createdUpvalue;
  }

  return createdUpvalue;
// vm.c, in captureUpvalue()
```

��������ĵ�ǰ�汾�Ѿ���������ֵ����������ֻ��Ҫ��Ӵ���������ֵ���뵽�б��С�����ͨ��Խ���б�ĩβ��ֹͣ��ջ�۵�����������Ѱ�ҵ�ջ�۵ĵ�һ����ֵ���˳��б���������κ�һ������£�����ζ��������Ҫ��ָ��Ķ���*֮ǰ*�����µ���ֵ��������ǵ����б��ĩβ`upvalue`������`NULL`����

���������������ݽṹ 101 ���˽⵽��������Ҫ��һ���ڵ���뵽�����У�����Ҫ��ǰһ���ڵ��`next`ָ������Ϊָ�������½ڵ㡣�����Ǳ����б�ʱ������һֱ�ڷ���ظ���ǰ��Ľڵ㡣����Ҫ�������б�ͷ����������ֵ���������������������£���next��ָ���� VM ��ͷָ�롣

> ��һ���϶̵�ʵ�֣���ͨ��ʹ��ָ��ָ���ָ����ͳһ����ͷָ���ǰһ����ֵָ��ĸ���`next`�������ִ��뼸��������û�дﵽ��ʦˮƽ��ָ��רҵ֪ʶ���˸е������Ҳ����˻���`if`��䷽����

����������µĹ��ܣ�VM ���ڿ���ȷ���κθ����ı��ز��ֻ��һ�� ObjUpvalue����������հ�������ͬ�ı��������ǽ������ͬ����ֵ������׼���ý���Щ��ֵ�Ƴ���ջ��

### 25.4.4 ����ʱ�ر���ֵ closing upvalues at runtime

�������ᷢ��һ��`OP_CLOSE_UPVALUE`ָ�׼ȷ�ظ��� VM ��ʱӦ���ֲ��������������ϡ�ִ�и�ָ���ǽ����������Ρ�

```c
      }
      case OP_CLOSE_UPVALUE:
        closeUpvalues(vm.stackTop - 1);
        pop();
        break;
      case OP_RETURN: {
// vm.c, in run()
```

�����е���ָ��ʱ�����������ı������ڶ�ջ�Ķ��������ǵ���һ����������������ջ�۵ĵ�ַ���ú�������ر���ֵ�������ش�ջ�ƶ����ѡ�֮��VM ����ͨ������`pop()`���ɶ�����ջ��.

��Ȥ�����鷢�������

```c
static void closeUpvalues(Value* last) {
  while (vm.openUpvalues != NULL &&
         vm.openUpvalues->location >= last) {
    ObjUpvalue* upvalue = vm.openUpvalues;
    upvalue->closed = *upvalue->location;
    upvalue->location = &upvalue->closed;
    vm.openUpvalues = upvalue->next;
  }
}
// vm.c, add after captureUpvalue()
```

�����������һ��ָ��ջ�۵�ָ�롣���رտ����ҵ��Ŀ�����ֵ(open upvalue)������ջ�ϵ�ǰ�ۻ�ǰ���Ϸ������(vm.openUpvalues->location >= last)�����ڣ�����ֻ��ָ�봫�ݸ�ջ�Ķ���λ�ã���ˡ���ǰ���Ϸ������ֲ��������ã����ܿ�ͻ������á�

Ϊ�ˣ������ٴδ��ϵ��±��� VM �Ŀ�����ֵ�б���� upvalue ��λ��ָ��Ҫ�رյĲ�۷�Χ�����ǽ��ر� upvalue������һ�����Ǵﵽ��Χ֮�����ֵ������֪��ʣ�ಿ��Ҳ����ˣ�����ֹͣ������

upvalue �رյķ�ʽ�ǳ��ᡣ���ȣ����ǽ�������ֵ���Ƶ�ObjUpvalue ��`closed`�ֶ��С�����Ƿ�ձ��������ڶ��еĵط���`OP_GET_UPVALUE`��`OP_SET_UPVALUE`ָ����Ҫ���ƶ���ĵط����ұ��������ǿ����ڽ�����������Ϊ��Щָ�����һЩ�����߼����Լ����ֵ�Ǵ򿪻��ǹرյ�һЩ��־��

�����Ѿ���һ���̶ȵļ�����������á�����Щָ�������`location`ָ���Ի�ȡ������ֵ����������ջ�ƶ���`closed`�ֶ�ʱ������ֻ�轫`location`����Ϊ ObjUpvalue��`closed`�ֶεĵ�ַ��

�Ҳ��������������Լ����ⶼ��Lua�����ŶӵĴ��¡�

![](./assets/8e039832a5a46fec5b18ccb3f9d4ae8ca8d20ed9.png)

��������Ҫ�ı�`OP_GET_UPVALUE`��`OP_SET_UPVALUE`���ͷ�ʽ����ʹ���Ǳ��ּ򵥣��Ӷ�ʹ���Ǳ��ֿ��١�������ȷʵ��Ҫ�����ֶ���ӵ� ObjUpvalue��

```c
  Value* location;
  Value closed;
  struct ObjUpvalue* next;
// object.h, in struct ObjUpvalue
```

�����Ǵ���һ�� ObjUpvalue ʱ������Ӧ�ý������㣬������û��δ��ʼ�����ڴ�����Χ������

```c
  ObjUpvalue* upvalue = ALLOCATE_OBJ(ObjUpvalue, OBJ_UPVALUE);
  upvalue->closed = NIL_VAL;
  upvalue->location = slot;
// object.c, in newUpvalue()
```

ÿ��������������ĩβʱ�����ᶪ���ÿ��е����оֲ���������Ϊÿ�����رյľֲ���������һ��`OP_CLOSE_UPVALUE`������������*��*���庯������������������ĩβ�����κ�ָ�����������������Ĳ������ں����ڲ������������κξֲ���������ЩҲ��Ҫ�رա�

> û��ʲô��*��ֹ*���ǹرձ������������ĺ��������򲢷���`OP_POP`ָ��`OP_CLOSE_UPVALUE`���������ǲ���Ҫ�ģ���Ϊ����ʱ�ڵ�������֡ʱ����ʽ��������ʹ�õ����ж�ջ�ۡ�

�����`closeUpvalues()`����ָ��ջ�۵�ָ���ԭ�򡣵�һ����������ʱ�����ǵ���ͬһ�� �������� �����뺯��ӵ�еĵ�һ��ջ�ۡ�

```c
        Value result = pop();
        closeUpvalues(frame->slots);
        vm.frameCount--;
vm.c, in run()
```

ͨ�����ݺ���ջ�����еĵ�һ���ۣ����ǹر��˷��غ���ӵ�е�����ʣ��Ŀ�����ֵ���������������ھ�����һ�������걸�ıհ�ʵ�֡�ֻҪ�������ǵĺ�����Ҫ���ǣ���ձ����ͻ���ڡ�

���Ǻܶ๤������ jlox �У��հ���Ȼ�ش����ǵĻ�����ʾ����ʧ�ˡ��� clox �У����ǲ��ò���Ӵ������롪���µ��ֽ���ָ��������еĸ������ݽṹ�Լ��µ�����ʱ����VM �ںܴ�̶��Ͻ��հ��еı���������������������

������ԭ��ġ���ʵ�ָ����Է��棬jlox Ϊ���ǡ���ѡ��ṩ�˱հ�������*����*���ԣ�jlox �ıհ�ȴ�޼����¡�ͨ���ڶ��Ϸ���*����*������jlox Ϊ*����*�ֲ��������������ŵ����ܴ��ۣ���ʹ�Ǵ������δ���հ�����ı�����

ʹ�� clox��������һ�������ӵ�ϵͳ�������������ǵ���ʵ������Ӧ���ǹ۲쵽�ľֲ�����������ʹ��ģʽ�����ڴ����ȷʵ����ջ����ı�������������ȫ������ջ�ϣ������ȼ��ֿ��١�Ȼ�󣬶��������������õľֲ��������еڶ���������·�������Ը�����Ҫѡ����롣

���˵��ǣ��û������������еĸ����ԡ������ǿ�����Lox �еľֲ������Ǽ򵥺�ͳһ�ġ���*���Ա���*�� jlox ��ʵ��һ���򵥡�����Ļ��clox ���ڹ۲��û�����Ϊ��������ǵ��ض���;�����Ż���������������ʵ�ֱ��Խ��Խ���ӣ����ᷢ���Լ���Խ��Խ������������ܴ�һ���֡��Ż����ǹ�����Ӽ���ض���;������������룬��Ϊ�ʺϸ�ģʽ�Ĵ����ṩ���Ƶġ������·����

���������Ѿ��� clox ����ȫʹ���˴ʷ���Χ������һ����Ҫ����̱������ң������������˾��и����������ڵĺ����ͱ���������Ҳ��*�ܶ�*����Ư���� clox �Ķ��У�ָ�����罫���Ǵ���һ��[��һ��](http://craftinginterpreters.com/garbage-collection.html)��Ū�����ι�����ڴ棬�Ա����ǿ����ڲ�����Ҫʱ�ͷ�����һЩ����

---

## [��ս](http://craftinginterpreters.com/closures.html#challenges)

1. ��ÿ�� ObjFunction ��װ�� ObjClosure �л�����һ���������ܳɱ��ļ�Ӳ㡣���� ���ر��κα����ĺ�����˵���óɱ����Ǳ���ģ�����ȷʵ������ʱͳһ�������е��á�
   
   �� clox ����Ϊ����װ��Ҫ��ֵ�� ObjClosures �еĺ�������ʼ�հ�װ������ȣ����븴���Ժ�������Σ�ע��ʹ�úͲ�ʹ�ñհ��Ļ�׼������Ӧ�����Ȩ��ÿ����׼����Ҫ�ԣ����һ����ø�����һ����ø��죬����ξ�����ѡ��ʵ�ֲ���ʱ����ʲô����Ȩ�⣿

2. �Ķ���������˵�����һ�ȡ����ڣ�����Ϊ LoxӦ����α��֣�����ʵ��Ϊÿ��ѭ����������һ���±�����

3. һ��[�����Ĺ���](http://wiki.c2.com/?ClosuresAndObjectsAreEquivalent)�������ǡ����������˵ıհ�  (objects are a poor man��s closure)������֮��Ȼ�������ǵ� VM ����֧�ֶ��󣬵��������˱հ������ǿ��Խ������ǡ�ʹ�ñհ�����дһ��ģ���ά���������󡱵� Lox ������Ӧ�ã�
   
   - ����һ�������캯�����Դ������и���*x*��*y*�������������
   
   - �ṩ���������Է��ʴӸù��캯�����ص�ֵ��*x*��*y*���ꡣ
   
   - ����һ���ӷ�����������������������Ӳ�����������������

## [���˵����ѭ�������հ�](http://craftinginterpreters.com/closures.html#design-note)

�հ�����������������հ�������ͬ�ı���ʱ�����ǹ������ͬ�ײ�洢λ�õ����á���������������ֵʱ��������б����ǿɼ��ġ���Ȼ����������հ�����ͬ�ı�������û�й���

```js
var globalOne;
var globalTwo;

fun main() {
  {
    var a = "one";
    fun one() {
      print a;
    }
    globalOne = one;
  }

  {
    var a = "two";
    fun two() {
      print a;
    }
    globalTwo = two;
  }
}

main();
globalOne();
globalTwo();
```

�⽫��ӡ��one��Ȼ��two��������������У�������������`a`�����ǲ�ͬ�ġ���������������ô���ԡ����ǣ�

```c
var globalOne;
var globalTwo;

fun main() {
  for (var a = 1; a <= 2; a = a + 1) {
    fun closure() {
      print a;
    }
    if (globalOne == nil) {
      globalOne = closure;
    } else {
      globalTwo = closure;
    }
  }
}

main();
globalOne();
globalTwo();
```

����ܸ��ӣ���Ϊ Lox û�м������͡���Ҫ�Ĳ����Ǹ�`main()`����ִ��ѭ�������ε���`for`��ÿ��ͨ��ѭ���������ᴴ��һ���հ�������ѭ��������������һ���հ��洢�� ��`globalOne`���ڶ����հ��洢�� ��`globalTwo`��

�϶������ֲ�ͬ�Ĺرշ�ʽ�����ǹر�������ͬ�ı�������ѭ������������ʱ�����Ƿ�ֻ��һ��`a`������ÿ�ε��������Լ���ͬ��`a`������

�˴��Ľű����������Ϊ��ƣ�����ȷʵ�Բ��� clox ������С�����Գ�������ʵ�����С�����һ�� JavaScript ʾ����

```js
var closures = [];
for (var i = 1; i <= 2; i++) {
  closures.push(function () { console.log(i); });
}

closures[0]();
closures[1]();
```

�����ȴ�ӡ��1���ٴ�ӡ��2�������Ǵ�ӡ���Ρ�3�� ����������ӡ�����Ρ�3���������ܻ�е����ȡ������ JavaScript �����У�ֻ��һ��`i`���������������ڰ���ѭ�������е��������������˳���

> ����֪��*3*����ν��뻭������ڵڶ��ε�����ִ��`i++`������`i`�� 3������ǵ���`i <= 2`����Ϊ�ٲ�����ѭ����ԭ�����`i`��δ�ﵽ3��ѭ������Զ���С�

�������Ϥ JavaScript��������֪��ʹ��`var`�����ı�������ʽ*����*����Χ�ĺ����򶥼���Χ���ͺ��������д�������

```js
var closures = [];
var i;
for (i = 1; i <= 2; i++) {
  closures.push(function () { console.log(i); });
}

closures[0]();
closures[1]();
```

����һ���ϣ�ֻ��һ��`i`.���ڿ����Ƿ���ĳ�����ʹ���µ�`let`�ؼ��֣�

```js
var closures = [];
for (let i = 1; i <= 2; i++) {
  closures.push(function () { console.log(i); });
}

closures[0]();
closures[1]();
```

����³������Ϊ�Ƿ���ͬ��û�С�����������£����ȴ�ӡ��1����Ȼ���ӡ��2����ÿ���հ������Լ���`i`.�����뵽��ʱ�����е���֡������Ӿ���`i++`.�⿴�������������ڷ���͸ı�һ�����б����������Ǵ���һ���±�����

�����ǳ���һЩ�������ԡ�����Python��

```py
closures = []
for i in range(1, 3):
  closures.append(lambda: print(i))

closures[0]()
closures[1]()
```

Python ��û�������Ŀ������򡣱�������ʽ�����ģ����Զ��޶�����Χ�������������ڡ��е����� JS ������������Ϊ�����������������հ���������ͬ�ı��������ǣ��� C ��ͬ���ǣ����ǲ���ͨ������`i`*����*���һ��ֵ���˳�ѭ������������ӡ���Ρ�2����

Ruby�أ�Ruby �����ֵ��͵����ֵ�����ʽ�����Ǿ��������ʽ���

```ruby
closures = []
for i in 1..2 do
  closures << lambda { puts i }
end

closures[0].call
closures[1].call
```

��� Python һ������ӡ��2�����Ρ��������õ� Ruby ������ڷ�Χ������ʹ�ø߽�`each()`������

```py
closures = []
(1..2).each do |i|
  closures << lambda { puts i }
end

closures[0].call
closures[1].call
```

���������Ϥ Ruby����`do |i| ... end`���ֻ�������һ�����������ݸ�`each()`�����ıհ���`|i|`�Ǳհ��Ĳ���ǩ����`each()`�������øñհ����Σ�`i`��һ�δ��� 1���ڶ��δ��� 2��

����������£���ѭ��������ʵ������һ���������������ң�����ѭ����ÿ�ε������ǶԺ����ĵ������ã���˶���ÿ�ε��ö��ԣ���Щ�����ǵ����ı������������ӡ��1��Ȼ��2����

���һ�������и��߼��Ļ��ڵ�������ѭ���ṹ`foreach`���� C#��Java �ġ���ǿ�� for����`for-of`JavaScript��`for-in`Dart �ȣ���ô����Ϊ���ߺ���Ȼ��ϣ��ÿ�ε���������һ���±���������*������*��һ���±�������Ϊѭ��ͷ��������һ����������������û���������ʽ�����������ڸı�ñ�����ǰ������һ����

����������˽� StackOverflow �������ط�����ᷢ��֤�ݱ��������û��������ģ���Ϊ������*û��*�õ���ʱ�����ǻ�ǳ����ȡ��ر��ǣ�C# �����*û��*Ϊѭ����ÿ�ε��������µ�ѭ������`foreach`�������û������ĳ�����Դ�����������ǲ�ȡ�˷ǳ������Ĳ��裬�����Խ������ش���ġ��� C# 5 �У�ÿ�ε������ᴴ��һ���±�����

�ɵ� C ���`for`ѭ�����ѡ�increment �Ӿ�ȷʵ��������ͻ�䡣����ζ��ÿһ���������һ������������ÿ�ε�������һ��ѭ����������û��*��*��Ψһһ�����������Լ�⵽�����ڱհ���������ʱ��ӵ��һ�����ñ����ıհ�����û�а������ñ�����ֵ�ǵ������˳�ѭ�����κ�ֵ��

ʵ�õĴ𰸿������� JavaScript`let`��`for`ѭ�������������顣������������ͻ�䵫ʵ����ÿ�ζ�����һ���±�������Ϊ�����û���Ҫ�ġ����ǣ�����������ʱ�����е���֡�