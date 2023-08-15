# ��������

> �������ѧ�е��κ����ⶼ����ͨ��һ����Ӳ�����������˼�Ӳ�̫������⡣
> 
> -- David Wheeler

��һ����һ�������ҳ��Խ����Էֽ��С��Ƭ�Σ�����ʱ������������ٷ�����һ�������Ǻ��������ǿ���ֻ�Ӻ���������ʼ�����ǵ����ܵ�������ʱ��Ͳ��Ǻ������ˡ����ǿ��Ե��ã���û��ʲô�ɵ��õġ���� VM û�����ӵ������Կ������κ����ݣ���ô VM ��֧���������������������ʱ֧�ֶ����Ǻ��м�ֵ����������Ҫȫ���Ը�������ܶ࣬����ɺ����ǻ�о��ܺá�

> �ԡ������ѡ����ǶԴ�������Ϊ����ֱ��������ǣ�������������������������̶�����һ�㣬�ȿȣ����ɡ�

## 24.1 ��������

VM ������Ȥ�Ľṹ�仯���ǹ���ջ�ġ������Ѿ����˾ֲ���������ʱ������ջ�������Ѿ������һ�롣��������û�е���ջ(*call*?stack)�ĸ��������ȡ�úܴ��չ֮ǰ��������������⡣�����ȣ������Ǳ�дһЩ���롣һ���ҿ�ʼ�˶��������Ǹо��ö��ˡ����û��ĳ�ֶ�����ʾ���������ǽ�����Ϊ�������Խ������￪ʼ���� VM �ĽǶ�������ʲô�Ǻ�����

һ��������һ������ִ�е����壬��������ζ��һЩ�ֽ��롣���ǿ��Խ��������������к������������һ���������顣ÿ����������һ��ָ�룬ָ����������ĵ�һ��ָ�

����¾��Ǳ���Ϊ��������Ĺ�����ʽ�������ջ�õ�һ�������Ļ�������顣���Ƕ����ֽ��� VM�����ǿ�����һЩ���߲�ε����顣����Ϊ������ģ����Ϊÿ�������������Լ��� Chunk������ҪһЩ����Ԫ���ݣ����������Ǽ�������ȫ����䵽һ���ṹ�С�

```c
  struct Obj* next;
};

typedef struct {
  Obj obj;
  int arity;
  Chunk chunk;
  ObjString* name;
} ObjFunction;

struct ObjString {
// object.h, add after struct Obj
```

������ Lox ����һ�ȹ���ģ�������Ǳ�Ȼ�� Lox ������� ObjFunction ������ͬObj ��ͷ,����������������������һ���ġ�`arity`�ֶδ洢���������Ĳ���������Ȼ�󣬳��˿�֮�⣬���洢���������ơ�����ڱ���ɶ�������ʱ����ǳ����㡣

> �����ƺ�û�з��������ֽ���ƫ�����ڱ���ת�����ر��������ԡ�

���ǡ�object��ģ���һ����Ҫ���� Chunk��������Ҫ include��

```c
#include "common.h"
#include "chunk.h"
#include "value.h"
// object.h
```

������ַ������������������Ƕ�����һЩ����������ʹ Lox ������������ C ��ʹ�á��е����������������ȣ����ǽ�����һ�� C ����������һ���µ� Lox ������

```c
  uint32_t hash;
};

ObjFunction* newFunction();
ObjString* takeString(char* chars, int length);
// object.h, add after struct ObjString
```

ʵ�������

```c
ObjFunction* newFunction() {
  ObjFunction* function = ALLOCATE_OBJ(ObjFunction, OBJ_FUNCTION);
  function->arity = 0;
  function->name = NULL;
  initChunk(&function->chunk);
  return function;
}
// object.c, add after allocateObject()
```

ʹ��`ALLOCATE_OBJ()`�������ڴ沢��ʼ������ı�ͷ���Ա� VM ֪������ʲô���͵Ķ�������û����ʹ�� ObjString ���������������ʼ�����������ǽ���������Ϊһ�ֿհ�״̬��0 arity�������ƺ��޴��롣�Ժ��ڴ�����������д��

��Ϊ������һ���µĶ���������Ҫ��ö������һ���µĶ������͡�

```c
typedef enum {
  OBJ_FUNCTION,
  OBJ_STRING,
} ObjType;
// object.h, in enum ObjType
```

������һ����������ʱ�����뽫��ռ�õ��ڴ滹��������ϵͳ��

```c
  switch (object->type) {
    case OBJ_FUNCTION: {
      ObjFunction* function = (ObjFunction*)object;
      freeChunk(&function->chunk);
      FREE(ObjFunction, object);
      break;
    }
    case OBJ_STRING: {
// memory.c, in freeObject()
```

��� switch case�����ͷ� ObjFunction �����Լ���ӵ�е��κ������ڴ档����ӵ�����ǵĿ飬������Ҫ���ÿ��������������

> ����Ҫ��ʽ�ͷź��������ƣ���Ϊ����һ�� ObjString������ζ�����ǿ����������ռ���Ϊ���ǹ��������������ڡ����ߣ����٣�һ������[ʵ���������ռ���](http://craftinginterpreters.com/garbage-collection.html)�����Ǿ��ܹ�������

Lox �����ӡ�κζ��󣬶�������һ�ȹ���Ķ�������Ҳ��Ҫ�������ǡ�

```c
  switch (OBJ_TYPE(value)) {
    case OBJ_FUNCTION:
      printFunction(AS_FUNCTION(value));
      break;
    case OBJ_STRING:
// object.c, in printObject()
```

�����ã�

```c
static void printFunction(ObjFunction* function) {
  printf("<fn %s>", function->name->chars);
}
// object.c, add after copyString()
```

��Ȼһ������֪���������֣���Ҳ����˵������

��������м��������ڽ�ֵת��Ϊ���������ȣ�ȷ��ֵʵ������һ��������

```c
#define OBJ_TYPE(value)        (AS_OBJ(value)->type)

#define IS_FUNCTION(value)     isObjType(value, OBJ_FUNCTION)
#define IS_STRING(value)       isObjType(value, OBJ_STRING)
// object.h
```

���������Ϊ true��Ȼ��������ʹ�����·�����ȫ�ؽ� Value ת��Ϊ ObjFunction ָ�룺

```c
#define IS_STRING(value)       isObjType(value, OBJ_STRING)

#define AS_FUNCTION(value)     ((ObjFunction*)AS_OBJ(value))
#define AS_STRING(value)       ((ObjString*)AS_OBJ(value))
// object.h
```

���������ǵĶ���ģ�;�֪����α�ʾ�����ˡ�������ϡ���׼���ý��ܸ��ѵ���ս����

## 24.2 ���뺯������

���ڣ����ǵı��������������Ǳ����һ�������Ŀ顣����ÿ�������Ĵ��붼λ�ڲ�ͬ�Ŀ��У���˱�ø��Ӹ��ӡ���������������������ʱ������Ҫ�ڱ��뺯����ʱ�����뷢�䵽�������С��ں������ĩβ����������Ҫ���ص������ڴ����ǰһ���顣

����ں������ڵĴ�����˵�ܺã�����������벻�������أ�Lox ����ġ����㡱Ҳ������ʽ���룬Ҳ��Ҫ��������һ���顣����Ҳ����ͨ����������������Զ�����ĺ��������򻯱������� VM��������������������ĳ�ֺ������У�VM ����ͨ�����ú��������д��롣�ͺ����������򶼰�����һ����ʽ`main()`�����С�

> ����ȫ�ֱ��������ʧЧ�����Ǿ��в�ͬ�ھֲ���������������������������������£��ű��Ķ��㲻�����塣

��ʹ���û�����ĺ���֮ǰ�������ǽ����ع���֧����ʽ�������������� Compiler �ṹ��ʼ��������ֱ��ָ�������д��Ŀ飬�����������ڹ����ĺ�������

```c
typedef struct {
  ObjFunction* function;
  FunctionType type;

  Local locals[UINT8_COUNT];
// compiler.c, in struct Compiler
```

����һЩ FunctionType ö�١����ñ�����֪�������ڱ��붥����뻹���ڱ��뺯���塣���������������������һ�㡪�������Ϊʲô����һ�����õĳ��󡪡�����һ�����ط�������������������ġ������Ժ��ٽ���

```c
typedef enum {
  TYPE_FUNCTION,
  TYPE_SCRIPT
} FunctionType;
// compiler.c, add after struct Local
```

��������д����ÿ���ط����ڶ���Ҫͨ��`function`ָ�롣���˵��ǣ�������½�֮ǰ�����ǽ��Կ�ķ��ʷ�װ��`currentChunk()`�����С�����ֻ��Ҫ�޸���������ı������ͻ�ܸ��ˡ�

> �⼸����������һ������Ԥ��δ����ˮ���򣬲���֪�������Ժ���Ҫ���Ĵ��롣���ǣ�ʵ���ϣ�������Ϊ����д��֮ǰ�ͱ�д�˱�������д��롣

```c
Compiler* current = NULL;

static Chunk* currentChunk() {
  return & current->function->chunk;
}

static void errorAt(Token* token, const char* message) {
// compiler.c, add after variable current, replace 5 lines
```

��ǰ��ʼ�������ڱ���ĺ�����ӵ�еĿ顣��������������Ҫʵ�ʴ����ú�������ǰ��VM ��һ���鴫�ݸ����������������ô�����������෴��������������������һ���������ú��������û�������ѱ��붥�����롪����������Ŀǰ֧�ֵ����д��롣

### 24.2.1 �ڱ���ʱ��������

���ǿ�ʼ��`compile()`���䰴˳����д������Ǳ���������Ҫ��ڵ㡣

```c
  Compiler compiler;
  initCompiler(&compiler, TYPE_SCRIPT);

  parser.hadError = false;
// compiler.c, in compile(), replace 2 lines
```

�������ĳ�ʼ����ʽ�кܶ�仯�����ȣ����ǳ�ʼ���������µ��ֶΡ�

```c
static void initCompiler(Compiler* compiler, FunctionType type) {
  compiler->function = NULL;
  compiler->type = type;
  compiler->localCount = 0;
// compiler.c, function initCompiler(), replace 1 line
```

Ȼ�����Ƿ���һ���µĺ������������롣

```c
  compiler->scopeDepth = 0;
  compiler->function = newFunction();
  current = compiler;
// compiler.c, in initCompiler()
```

> �����ף���`function`�ֶ�����Ϊ nullȻ���ڼ���֮������Ϊ�丳ֵ���������޴��������������ռ���ص�ƫִ��

�ڱ������д��� ObjFunction ���ܿ������е���֡����������Ǻ���������ʱ��ʾ�����������ڱ���ʱ�����������������룬�����������ַ��������������������ڱ���ʱ������ʱ����֮�������һ��������������̸����������ʱ������ʵ����������������������һ�ֲ�����������ֵ�ķ��š����Ա������ڱ����ڼ䴴����������Ȼ��������ʱ�����Ǳ��򵥵ص��á�

> ���ǿ����ڱ���ʱ������������Ϊ����ֻ��������ʱ���õ����ݡ������Ĵ��롢���ƺ�Ԫ��(arity)���ǹ̶��ġ���������[��һ��](http://craftinginterpreters.com/closures.html)�����������ʱ��������ıհ�ʱ��������ø��Ӹ��ӡ�

������һ����ֵĴ��룺

```c
  current = compiler;

  Local* local = & current->locals[current->localCount++];
  local->depth = 0;
  local->name.start = "";
  local->name.length = 0;
}
// compiler.c, in initCompiler()
```

���ס����������`locals`����������Щ��ջ������Щ�ֲ���������ʱ����������������ڿ�ʼ����������ʽ������ջ��Ϊ 0 �� VM �Լ��ڲ�ʹ�á����Ǹ���һ�������ƣ������û��Ͳ���дһ���������ı�ʶ���������������ʱ���һ�������ǹ���ʲô�ġ�

���ǳ�ʼ�����档�����һЩ����ı���ʱ�����ǻ���Ҫ����һ�˽���һЩ���ġ�

```c
static ObjFunction* endCompiler() {
  emitReturn();
// compiler.c, function endCompiler(), replace 1 line
```

��ǰ����`interpret()`���ñ�����ʱ�����ᴫ��Ҫд��Ŀ顣���ڱ������Լ������˺������󣬷����Ǹ�����������������ӵ�ǰ�ı������л�ȡ����

```c
  emitReturn();
  ObjFunction* function = current->function;

#ifdef DEBUG_PRINT_CODE
// compiler.c, in endCompiler()
```

Ȼ�����������ص�`compile()`��

```c
#endif

  return function;
}
// compiler.c, in endCompiler()
```

�����ǶԸú���������һ�ε����ĺ�ʱ������Щʱ�����������һЩ��ϴ����� VM ת���������ֽ��룬�Ա����ǿ��Ե��Ա�����������Ӧ���޸����Լ�����������Ϊ���ɵĿ鱻��װ��һ�������С�

```c
#ifdef DEBUG_PRINT_CODE
  if (!parser.hadError) {
    disassembleChunk(currentChunk(), function->name != NULL
        ? function->name->chars : "<script>");
  }
#endif
// compiler.c, in endCompiler(), replace 1 line
```

��ע��˴��ļ���Բ鿴�����������Ƿ�Ϊ`NULL`?�û�����ĺ��������ƣ�������Ϊ������봴������ʽ����û�У��������Լ�����ϴ����У�Ҳ��Ҫ���ŵش����������£�

```c
static void printFunction(ObjFunction* function) {
  if (function->name == NULL) {
    printf("<script>");
    return;
  }
  printf("<fn %s>", function->name->chars);
// object.c, in printFunction()
```

�û��޷���öԶ������������ò����Դ�ӡ����������`DEBUG_TRACE_EXECUTION`��ӡ������ջ����ϴ�����Զ���ȷʵ��ˡ�

> ��������������Ҵ������ϴ��뱾��ᵼ�� VM �����δ����ǿɾ�û��˼�ˣ�

��������ߵ�`compile()`�����ǵ����亯��ǩ����

```c
#include "vm.h"

ObjFunction* compile(const char* source);

#endif
// compiler.h, function compile(), replace 1 line
```

����������һ�������������Ƿ���һ���顣ʵ�����£�

```c
ObjFunction* compile(const char* source) {
  initScanner(source);
// compiler.c, function compile(), replace 1 line
```

������ǵõ�һЩʵ�ʵĴ��롣���ǽ�������������Ϊ��

```c
  while (!match(TOKEN_EOF)) {
    declaration();
  }

  ObjFunction* function = endCompiler();
  return parser.hadError ? NULL : function;
}
// compiler.c, in compile(), replace 2 lines
```

���Ǵӱ������л�ȡ�����������û�б�����󣬾ͷ�����������ͨ�����������������ź�`NULL`��������VM �Ͳ��᳢��ִ�п��ܰ�����Ч�ֽ���ĺ�����

���գ����ǽ�����`interpret()`�Դ���`compile()`��������������������Ҫ����һЩ�������ġ�

## 24.3 ����ջ֡

��ʱ����и����ϵ��ش��Ծ�ˡ������ǿ���ʵ�ֺ��������͵���֮ǰ����Ҫ�� VM ׼���ô������ǡ���Ҫ���ĵ���Ҫ������������

### 24.3.1 ����ֲ�����

������Ϊ�ֲ���������ջ�ۡ��������еľֲ��������ֲ��ڶ��������ʱ����Ӧ����ι�����

һ��ѡ���ǽ�������ȫ�ֿ���ÿ������������ VM ջ�л���Լ���һ��ר�ò�ۣ�������Զӵ����Щ��ۣ���ʹδ���øú���Ҳ����ˡ����������е�ÿ���ֲ������� VM �ж�����һЩ�ڴ湩�Լ�ʹ�á�

> ������������ C ������ʹ��`static` ����ÿ���ֲ�����.

�Ų������㣬���ڵı������ʵ�־������������ġ���һ�� Fortran ������Ϊÿ��������̬�����ڴ档���Ե�����������ĺܵ�Ч��������������κ�ʱ��㶼���ڱ����õ��м䣬���Ϊ����ռ��δʹ�õ��ڴ���һ���˷ѡ�

Ȼ�����������������ǵݹ顣ͨ���ݹ飬������ͬʱ�����롱��ͬһ�����Ķ�ε��á�ÿ������Ҫ�Լ����ڴ����洢��ֲ��������� jlox �У�����ͨ����ÿ�ε��ú����������ʱΪ������̬�����ڴ������������⡣�� clox �У����ǲ�ϣ��ÿ�κ������ö����������ܳɱ���

> Fortran ͨ����ȫ��ֹ�ݹ������������⡣�ݹ��ڵ�ʱ����Ϊ��һ�ָ߼��ġ���µ����ԡ�

���ڣ����ǵĽ���������� Fortran �ľ�̬����� jlox �Ķ�̬����֮�䡣VM �е�ֵ��ջ���ڹ۲�ֲ���������ʱ�����Ժ���ȳ��ķ�ʽ���С���������˵���˵��ǣ���ʹ�����������û��ʹ�ã������Ȼ��ˡ�����һ�����ӣ�

```c
fun first() {
  var a = 1;
  second();
  var b = 2;
}

fun second() {
  var c = 3;
  var d = 4;
}

first();
```

��ִ�г��򲢲鿴ÿ��ʱ����ڴ�������Щ������

![](./assets/9272b64430c0cf2f4f1e17b42104fefa1b663c15.png)

��ִ����������������ʱ��ÿ���ֲ���������ѭ����ԭ�򣺺�������ı�������ǰ�������ı���֮ǰ����(any variable declared after it will be discarded before the first variable needs to be)����ʹ�ڿ�Խ�����������Ҳ����ˣ�����֪��������`a`֮ǰ ���ȶ���`c`��`d`������ȥ����Ӧ������ VM ��ջ�Ϸ���ֲ�������

��������£�������Ȼ�ڱ���ʱȷ��ÿ���������ڶ�ջ�е�λ�á���ʹ�����ڴ���������ֽ���ָ��򵥿��١��������ʾ���У����ǿ���"������һ��ֱ�ӵķ�ʽ�����������Ⲣ�����ǿ��С����ǣ�

> ��˵����������Ϊ������ʵ�����޷����������⡣��Ϊ������ Lox ����һ�ȹ���ģ����������޷��ڱ���ʱȷ����Щ������������Щ����������

```c
fun first() {
  var a = 1;
  second();
  var b = 2;
  second();
}

fun second() {
  var c = 3;
  var d = 4;
}

first();
```

�ڶ�`second()` �ĵ�һ�ε����У�c��d������� 1 �� 2�������ڵڶ��ε����У�������ҪΪ b �ڳ��ռ�,�����c��d��Ҫλ�ڲ�� 2 �� 3 �С���˱������޷�Ϊ�纯�����õ�ÿ���ֲ�����ȷ��һ��ȷ�еĲ�ۡ�������һ�������ĺ����У�ÿ���ֲ����������λ���ǹ̶��ġ�����d������c֮��Ĳ��С����ǹؼ��ĵط���

��һ������������ʱ�����ǲ�֪��ջ���������Ϊ�����ԴӲ�ͬ���������е��á����ǣ�����ջ��ǡ����������Ƕ�֪�����������оֲ���������ڸ�����λ�á���ˣ����������һ��������ͨ����Ӽ������������⡣

��ÿ���������õĿ�ʼ��VM ��¼�ú����Լ��ĵ�һ���ֲ�������ʼ�Ĳ۵�λ�á�ʹ�þֲ�������ָ��ͨ������ھֲ������Ĳ������������ǣ���������������������ջ�ײ����ڱ���ʱ�����Ǽ�����Щ��Բۡ�������ʱ������ͨ����Ӻ������õ���ʼ�۽���Բ�ת��Ϊ����ջ������

�ͺ������ڸ���Ķ�ջ�л����һ�������ڡ��򡰿�ܡ��������������д洢��ֲ�������**����֡(call frame)** ��λ����������ʱȷ���ģ��������ڲ�ʹ�����λ�ã�����֪������������ҵ�������

![](./assets/50de8229a829a1101ca7bde51a50659edc108a78.png)

��ʷ�ϣ������ֲ�������ʼ��λ�� ������ **ָ֡��**����Ϊ��ָ��������֡�Ŀ�ͷ����ʱ�������**base pointer**����Ϊ��ָ�����к����������ڵĻ���ַջ��(base stack slot)��

����������Ҫ���ٵĵ�һ�����ݡ�ÿ�����ǵ���һ������ʱ��VM ����ȷ���ú���������ʼ�ĵ�һ��ջ�ۡ�

### 24.3.2 ���ص�ַ

���ڣ������ͨ������`ip`�ֶ�������ָ������Ψһ��Ȥ����Ϊ��Χ�ƿ�����ָ���Щָ���`ip`ʵʩ�˸����������ƫ�ơ����ú����ǳ��򵥡���ֻ�轫��`ip`����Ϊָ��ú������еĵ�һ��ָ��ɡ����ǵ��������ʱ�أ�

VM ��Ҫ���ص��ú����Ŀ飬���ڵ��ú�ָ�ִ�к�߸��ŵ�ָ���ˣ�����ÿ���������ã�������Ҫ���ٵ������ʱ���ص�λ�á����Ϊ**���ص�ַ**����Ϊ���� VM �ڵ��ú󷵻ص�ָ���ַ��

ͬ�������ڵݹ飬�������������ж�����ص�ַ���������ÿ��*����*�����ԣ������Ǻ�������

> ���� Fortran ��������������һ��ʵ�ַ��ص�ַ������ɡ���������*��*֧�ֵݹ飬����κθ����ĺ������κ�ʱ��㶼ֻ��Ҫһ�����ص�ַ����ˣ���������ʱ���ú���ʱ�������*�޸��Լ��Ĵ���*��������ĩβ����תָ�����Ϊ��ת��������ߡ���ʱ��������֮��Ľ��޺̡ܶ�

### 24.3.3 ����ջ

��ˣ�����ÿ��ʵʱ�������á���ÿ����δ���صĵ��á�����Ҫ���ٺ����ֲ�������ջ�еĺδ���ʼ���Լ�������Ӧ�Ӻδ��ָ������ǽ�����������һЩ����һ�����һ���µĽṹ�С�

```c
#define STACK_MAX 256

typedef struct {
  ObjFunction* function;
  uint8_t* ip;
  Value* slots;
} CallFrame;

typedef struct {
// vm.h
```

CallFrame ��ʾ�������ڽ��еĺ������á���`slots`�ֶ�ָ��˺�������ʹ�õĵ�һ����۴��� VM ֵ��ջ���Ҹ���ȡ��һ���������ƣ���Ϊ��������� C���Թֵ� ��ָ����һ�����顱�������ǻ���������������Դ���

���ص�ַ��ʵ�����������������е㲻ͬ�������߲��ǽ����ص�ַ�洢�ڱ������ߵ�֡�У����Ǵ洢�������Լ���`ip`.�����ǴӺ�������ʱ��VM ����ת�������� CallFrame �� `ip`�� ��������ָ���

�һ������������һ��ָ�򱻵��ú�����ָ�롣���ǽ�ʹ���������ҳ���������һЩ������

ÿ�ε��ú���ʱ�����Ƕ��ᴴ��һ�������Ľṹ�������ڶ��϶�̬�������ǣ�������������������Ǻ��Ĳ����������Ҫ�����ܿ졣���˵��ǣ��۲����ͱ�����ͬ���������þ���ջ���塣���`first()`����`second()`����`second()`���ý��� `first()`����֮ǰ��ɡ�

> ��� Lisp ʵ�ֶ�̬����ջ֡����Ϊ������[����(continuations)](https://en.wikipedia.org/wiki/Continuation)��ʵ�֡������������֧����������ô��������*����*���Ǿ��ж�ջ���塣

��ˣ��� VM �У�����Ԥ�ȴ�������Щ CallFrame �ṹ�����飬��������Ϊջ���������Ǵ���ֵ����һ����

```c
typedef struct {
  CallFrame frames[FRAMES_MAX];
  int frameCount;

  Value stack[STACK_MAX];
// vm.h, in struct VM, replace 2 lines
```

�������ȡ���˹�ȥֱ���� VM ��ӵ�е�`chunk`��`ip`�ֶΡ�����ÿ�� CallFrame �����Լ�`ip`��ָ�룬ָ��������ִ�е� ObjFunction����������ǿ����ҵ������Ŀ顣

VM �е���`frameCount`�ֶδ洢 CallFrame ��ջ�ĵ�ǰ�߶ȡ������ڽ��еĺ������õ�������Ϊ��ʹ clox �򵥣�����������ǹ̶��ġ�����ζ�ţ����������ʵ��һ�������ǿ��Դ�����������ȡ����� clox�����ڴ˴����壺

```c
#include "value.h"

#define FRAMES_MAX 64
#define STACK_MAX (FRAMES_MAX * UINT8_COUNT)

typedef struct {
// vm.h, replace 1 line
```

���ǻ����������¶�����ֵջ�Ĵ�С����ȷ����ʹ�ڷǳ���ĵ�����������Ҳ���㹻��ջ�ۡ�VM ����ʱ��CallFrame ��ջΪ�ա�

> ������㹻��ĺ�������ʹ�����㹻�����ʱ���������Ǳ��ر�������Ȼ�п��������ջ��һ����׳��ʵ�ֿ��Է�ֹ�������������������ͼ�������ü򵥡�

```c
  vm.stackTop = vm.stack;
  vm.frameCount = 0;
}
// vm.c, in resetStack()
```

��vm.h��ͷ�ļ���Ҫ���� ObjFunction����������ͷ�ļ���

```c
#define clox_vm_h

#include "object.h"
#include "table.h"
// vm.h, replace 1 line
```

��������׼����ת�� VM ��ʵ���ļ�������ǰ����һЩ���صĹ����������Ѿ���`ip`��VM �ṹ�Ƶ��� CallFrame��������Ҫ�޸� VM ���漰`ip`����������ÿһ�д��롣���⣬��Ҫ����ͨ��ջ�۷��ʾֲ�������ָ�������ڵ�ǰ CallFrame ��`slots`�ֶ�ִ�д˲�����

���ǽ���ͷ��ʼ�����ƽ���

```c
static InterpretResult run() {
  CallFrame* frame = &vm.frames[vm.frameCount - 1];

#define READ_BYTE() (*frame->ip++)

#define READ_SHORT() \
    (frame->ip += 2, \
    (uint16_t)((frame->ip[-2] << 8) | frame->ip[-1]))

#define READ_CONSTANT() \
    (frame->function->chunk.constants.values[READ_BYTE()])

#define READ_STRING() AS_STRING(READ_CONSTANT())
// vm.c, in run(), replace 4 lines
```

���ȣ�����ǰ���� CallFrame �洢���������ڵľֲ�������(��CallFrame* frame=...)��Ȼ����ͨ��frame����`ip`�İ汾�滻֮ǰ���ֽ�����ʺꡣ

> ����ÿ�ζ�����ͨ�� CallFrame ���������ʵ�ǰ֡��������߳�������Ҫ���ǣ�**��֡�洢�ھֲ������л���� C ����������ָ�뱣���ڼĴ�����**����ӿ���frame�� `ip`�ķ���.����*��֤*���������������������п��ܻ���������

���ڿ�ʼ��ϸ΢���漰����ÿ��ָ�

```c
      case OP_GET_LOCAL: {
        uint8_t slot = READ_BYTE();
        push(frame->slots[slot]);
        break;
// vm.c, in run(), replace 1 line
```

��ǰ��`OP_GET_LOCAL`ֱ�Ӵ� VM ��ջ�����ж�ȡ�����ı��زۣ�����ζ������ջ�ײ���ʼ�����ۡ����ڣ������ʵ�ǰ֡��`slots`���飬����ζ������������ڸ�֡��ͷ�ĸ�����Ųۡ�

���þֲ������Ĺ�����ʽ��ͬ��

```c
      case OP_SET_LOCAL: {
        uint8_t slot = READ_BYTE();
        frame->slots[slot] = peek(0);
        break;
// vm.c, in run(), replace 1 line
```

�����޸�VM`ip`�ֶε���תָ����ڣ��Ե�ǰ֡��`ip`Ҳ��ͬ�����޸�.

```c
      case OP_JUMP: {
        uint16_t offset = READ_SHORT();
        frame->ip += offset;
        break;
// vm.c, in run(), replace 1 line
```

������תҲ��ͬ��

```c
      case OP_JUMP_IF_FALSE: {
        uint16_t offset = READ_SHORT();
        if (isFalsey(peek(0))) frame->ip += offset;
        break;
// vm.c, in run(), replace 1 line
```

�Լ����ǵ������תѭ��ָ�

```c
      case OP_LOOP: {
        uint16_t offset = READ_SHORT();
        frame->ip -= offset;
        break;
// vm.c, in run(), replace 1 line
```

������һЩ��ϴ��������ִ��ʱ��ӡÿ��ָ��԰������ǵ��� VM����Ҳ��Ҫ���½ṹһ������

```c
    printf("\n");
    disassembleInstruction(&frame->function->chunk,
        (int)(frame->ip - frame->function->chunk.code));
#endif
// vm.c, in run(), replace 2 lines
```

�������Ǵӵ�ǰ�� CallFrame �ж�ȡ�������Ǵ��� VM��`chunk`��`ip`�ֶΡ�

��֪����ʵ�����ǲ�����̫��⡣�����ָ��ֻʹ�ú꣬��˲���Ҫ�޸ġ���������������������`run()`.

```c
InterpretResult interpret(const char* source) {
  ObjFunction* function = compile(source);
  if (function == NULL) return INTERPRET_COMPILE_ERROR;

  push(OBJ_VAL(function));
  CallFrame* frame = &vm.frames[vm.frameCount++];
  frame->function = function;
  frame->ip = function->chunk.code;
  frame->slots = vm.stack;

  InterpretResult result = run();
// vm.c, in interpret(), replace 10 lines
```

�������ڿ��Խ����ڵı������������ӵ��ո������ĺ�˸��ġ����ȣ���Դ���봫�ݸ����������������Ƿ���һ���µ� ObjFunction�����а����ѱ���Ķ������롣�������`NULL`������ζ�ű������Ѿ�������һЩ����ʱ��������������£����˳�����Ϊ�޷������κζ�����

���򣬽������洢�ڶ�ջ�ϲ�׼��һ����ʼ CallFrame ��ִ������롣�������������Ϊʲô��������������ջ���㡪�����洢�����õĺ��������µ� CallFrame �У�����ָ�����������ʼ��`ip`Ϊָ�����ֽ���Ŀ�ͷ���������ջ��������Ϊ�� VM ֵ��ջ����ײ���ʼ��

��ʹ������׼���ÿ�ʼִ�д��롣��ɺ�VM �����ͷ�Ӳ����顣���� ObjFunction ӵ�иô��룬����Ҫ���������ˣ����Ծ�����������`interpret()`��

```c
  frame->slots = vm.stack;

  return run();
}
// vm.c, in interpret(), replace 4 lines
```

���þ� VM �ֶε����һ�δ�����`runtimeError()`.���ǽ��ڱ��º��������������������ڽ������Ϊ��

```c
  fputs("\n", stderr);

  CallFrame* frame = &vm.frames[vm.frameCount - 1];
  size_t instruction = frame->ip - frame->function->chunk.code - 1;
  int line = frame->function->chunk.lines[instruction];
  fprintf(stderr, "[line %d] in script\n", line);
// vm.c, in runtimeError(), replace 2 lines
```

������ֱ�Ӵ� VM ��ȡ���ip�����Ǵ�ջ������ CallFrame �ж�ȡ����Ӧ��ʹ�ù����ٴ����в�����ǰһ����

����������ȷ�������������Щ���� clox �ص��˿�����״̬�������������ͻ�����...������֮ǰ�����ġ����ǻ�û������κ��¹��ܣ��������е�����ʧ�����������еĻ�����ʩ������������Ѿ�׼�����ˡ���������������

## 24.4 ��������

�ڿ�����ɵ��ñ��ʽ֮ǰ����Ҫ��һЩ�������Ե��ã���������������������fun�ؼ��ֿ�ʼ��

> `fun`�ǵģ�ÿ�γ��ֹؼ���ʱ���Ҷ��Ὺһ���޴�����Ц��

```c
static void declaration() {
  if (match(TOKEN_FUN)) {
    funDeclaration();
  } else if (match(TOKEN_VAR)) {
    varDeclaration();
// compiler.c, in declaration(), replace 1 line
```

������Ȩ���ݸ����

```c
static void funDeclaration() {
  uint8_t global = parseVariable("Expect function name.");
  markInitialized();
  function(TYPE_FUNCTION);
  defineVariable(global);
}
// compiler.c, add after block()
```

������һ�ȹ��񣬺�������ֻ�Ǵ���һ��ֵ������洢���������ı����С���������������κ�������������һ���������ơ�����ĺ��������Ὣ�����󶨵�ȫ�ֱ������ڿ�����������ڲ���������������һ���ֲ�������

��ǰ����½��У��ҽ��������[�������׶ζ���](http://craftinginterpreters.com/local-variables.html#another-scope-edge-case)��������ȷ����**�޷��ڱ����Լ��ĳ�ʼ�������з����Լ���ֵ**���ǻ����⣬��Ϊ������û��ֵ��

������������������⡣�������������������Լ��������ǰ�ȫ�ġ��ں�����ȫ����֮ǰ�������ܵ��ú�����ִ�к����壬�������Զ���ῴ������δ��ʼ��״̬�ı�����ʵ���ϣ�Ϊ��֧�ֵݹ鱾�غ��������������������á�

Ϊ�������������ڱ��뺯����֮ǰ���ڱ�������ʱ���������������ı������Ϊ���ѳ�ʼ�����������Ϳ������������������ƶ������������

����������ȷʵ��Ҫ������¡�

```c
static void markInitialized() {
  if (current->scopeDepth == 0) return;
  current->locals[current->localCount - 1].depth =
// compiler.c, in markInitialized()
```

֮ǰ������ֻ�ڱ�����������ʱ�ŵ���`markInitialized()`�����ڣ�������������Ҳ�����ô˺������������������ʱ��û�оֲ��������Ա��Ϊ�ѳ�ʼ�������ú������󶨵�һ��ȫ�ֱ�����

�����������Ǳ��뺯�����������Ĳ����б�ͺ����塣Ϊ�ˣ�ʹ��һ�������ĸ����������ø����������ɵĴ��뽫���ɵĺ����������ڶ�ջ������֮�����ǵ���`defineVariable()`���ú����洢������Ϊ�������ı����С�

�Ҳ�ֳ���������ͺ�����Ĵ��룬��Ϊ�Ժ����������������ڵķ����������������𲽹��������������ʼ��

```c
static void function(FunctionType type) {
  Compiler compiler;
  initCompiler(&compiler, type);
  beginScope(); 

  consume(TOKEN_LEFT_PAREN, "Expect '(' after function name.");
  consume(TOKEN_RIGHT_PAREN, "Expect ')' after parameters.");
  consume(TOKEN_LEFT_BRACE, "Expect '{' before function body.");
  block();

  ObjFunction* function = endCompiler();
  emitBytes(OP_CONSTANT, makeConstant(OBJ_VAL(function)));
}
```

> ��`beginScope()`û����Ӧ��`endScope()`���á���Ϊ�����ǵ��ﺯ�����ĩβʱ������ȫ������ Compiler������û�б�Ҫ�رռ��������������������

���ڣ��Ȳ��ù��Ĳ��������ǽ���һ�Կ����ţ�Ȼ�������ġ������������ſ�ʼ�������������������Ȼ��������е�`block()`��������֪����α���һ��������ಿ�֣����������š�

### 24.4.1 ��������ջ

��Ȥ�Ĳ����Ǳ������Ķ����͵ײ���Compiler�ṹ �洢���ݣ�������Щ������Щ�ֲ�����ӵ�У���ǰ�ж���Ƕ�׿飬�ȵȡ�������Щ�����ض��ڵ��������ġ���������ǰ����Ҫ�����໥Ƕ�׵Ķ�������ı��롣

> ���ס�������������������Ϊ��ʽ���������壬���һ���������*�κ�*�������������Ǿͽ�����Ƕ�׺��������硣

�������ļ�����Ϊÿ�����ڱ���ĺ�������һ�������ı�����������ʼ���뺯������ʱ���� C ��ջ�ϴ���һ���µ� Compiler ��������г�ʼ����`initCompiler()`���ñ���������Ϊ��ǰ��������Ȼ�󣬵����뺯����ʱ�����з����ֽ���ĺ�������д���±���������ӵ�еĿ顣

���ﺯ���������ĩβ�����ǵ���`endCompiler()`.�������±���ĺ������󣬽�����Ϊ�����洢��*��Χsurrounding* �����ĳ������С����ǣ��ȵȣ�������λص���Χ������������`initCompiler()`���� ��ǰ������ָ��ʱ��ʧ������

����ͨ����һϵ��Ƕ�׵� Compiler �ṹ��Ϊջ�����������⡣�� VM �е� Value �� CallFrame ��ջ��ͬ�����ǲ���ʹ�����顣�෴������ʹ������ÿ����������ָ��������ĺ����ı�������һֱָ�򶥼�����ĸ���������

```c
} FunctionType;

typedef struct Compiler {
  struct Compiler* enclosing;
  ObjFunction* function;
// compiler.c, add after enum FunctionType, replace 1 line
```

�� Compiler �ṹ�У������޷����� Compiler �����Ͷ��� *typedef*����Ϊ��������δ��ɡ��෴������Ϊ�ṹ������������������enclosing�ֶε����͡�C����֡�

����ʼ��һ���µı�����ʱ���ڸ�ָ���в��񼴽������ǵ�ǰ�ı�������

```c
static void initCompiler(Compiler* compiler, FunctionType type) {
  compiler->enclosing = current;
  compiler->function = NULL;
// compiler.c, in initCompiler()
```

Ȼ�󣬵����������ʱ����ͨ������ǰ�ı������ָ�Ϊ�µĵ�ǰ�����������Լ��Ӷ�ջ�е�����

```c
#endif

  current = current->enclosing;
  return function;
// compiler.c, in endCompiler()
```

��ע�⣬������������Ҫ��̬����Compiler�ṹ��ÿ������Ϊ�ֲ������洢�� C ջ�С���`compile()`��`function()`�С������������� C ��ջ�����ǿ��Ի���������������ǵ�ԭ������Ϊ������ʹ�õݹ��½�������`function()`������Ƕ�׺�������ʱ���ջ�ݹ��������

> ��������ջ���ڱ������ṹȷʵ��ζ�ű�������Ƕ�׺��������������ʵ�����ơ��ߵ�̫Զ�����ܻ���� C ��ջ���������ϣ���������Բ�̬�����������������ӽ�׳�������� JavaScript VM �ȹ����������ĵ����⡪����������ǵı�������Ϊ������������ĺ���Ƕ��������

### 24.4.2 ��������

��������������ݲ����������Ͳ��Ǻ����ã����Խ��������������������

```c
 consume(TOKEN_LEFT_PAREN, "Expect '(' after function name.");
  if (!check(TOKEN_RIGHT_PAREN)) {
    do {
      current->function->arity++;
      if (current->function->arity > 255) {
        errorAtCurrent("Can't have more than 255 parameters.");
      }
      uint8_t constant = parseVariable("Expect parameter name.");
      defineVariable(constant);
    } while (match(TOKEN_COMMA));
  }
  consume(TOKEN_RIGHT_PAREN, "Expect ')' after parameters.");
// compiler.c, in function()  
```

�������Ͻ�������ֻ���ں�����������ʷ���Χ�������ľֲ�����������ʹ�����еı�����֧�������������ֲ������������ͱ������������г�ʼֵ�趨��ľֲ�������ͬ���˴�û�����ڳ�ʼ������ֵ�Ĵ��롣�������ں��������д��ݲ���ʱ�����ǽ�������������γ�ʼ���ġ�

������������ʱ��ͨ�������Ĳ�����������¼������arity�������ú����洢����һ��Ԫ�������������ơ����뺯������ʱ������`initCompiler()`���ڽ����������ƺ��������á�����ζ�ſ�����������ǰ��token�л�ȡ���ơ�

```c
  current = compiler;
  if (type != TYPE_SCRIPT) {
    current->function->name = copyString(parser.previous.start,
                                         parser.previous.length);
  }

  Local* local = & current->locals[current->localCount++];
// compiler.c, in initCompiler()
```

ע�⣬����С�ĵش����������ַ����ĸ��������ס������ֱ��ָ��ԭʼԴ�����ַ�����һ��������ɱ��룬���ַ������ܻᱻ�ͷš������ڱ������д����ĺ�������ȱ�������������������һֱ���ڵ�����ʱ����������Ҫ���Լ��Ķѷ��������ַ�����������һֱ������

�á��������ǿ������������뺯��������

```c
fun areWeHavingItYet() {
  print "Yes we are!";
}

print areWeHavingItYet;
```

����ֻ�ǲ��ܶ��������κ����õ����顣

> ���ǿ��Դ�ӡ���ǣ������������ⲻ�Ǻ����á�

## 24.5 ��������

�����ڽ���ʱ�����ǽ���ʼ����һЩ��Ȥ����Ϊ����һ���ǵ��ú���������ͨ�����������룬���Ǻ������ñ��ʽ��һ����׺`(`����������ڱ����õ���������һ�������ȼ����ʽ����ͨ��ֻ��һ����ʶ����Ȼ�����м��`(` ���������ö��ŷָ��Ĳ������ʽ�������һ�� `)`������������

�Ǹ���ֵ��﷨�ӽǽ�������ν��﷨���ص��������С�

```c
ParseRule rules[] = {
  [TOKEN_LEFT_PAREN]    = {grouping, call,   PREC_CALL},
  [TOKEN_RIGHT_PAREN]   = {NULL,     NULL,   PREC_NONE},
// compiler.c, add after unary(), replace 1 line
```

���������������ʽ���������ʱ��������ɵ�һ���µĽ�����������

```c
static void call(bool canAssign) {
  uint8_t argCount = argumentList();
  emitBytes(OP_CALL, argCount);
}
// compiler.c, add after binary()
```

�����Ѿ�ʹ����`(`token�����Խ�����ʹ�õ�����`argumentList()`�����������������ú�������������Ĳ���������ÿ���������ʽ�������ɴ��룬����ֵ����ջ����׼�����á�֮�󣬷���һ����`OP_CALL`ָ�������øú�����ʹ�ò���������Ϊ��������

����ʹ������������������������

```c
static uint8_t argumentList() {
  uint8_t argCount = 0;
  if (!check(TOKEN_RIGHT_PAREN)) {
    do {
      expression();
      argCount++;
    } while (match(TOKEN_COMMA));
  }
  consume(TOKEN_RIGHT_PAREN, "Expect ')' after arguments.");
  return argCount;
}
// compiler.c, add after defineVariable()
```

�ô����� jlox �п�����Ӧ�ú���Ϥ��ֻҪ������ÿ�����ʽ���ҵ����ţ��ͻ���ϸ�о��������ڽ���ʱ�����������������ţ�������ˡ�

�ţ���ࡣ�ص� jlox�����������һ������ʱ��飬��ȷ�������Ὣ���� 255 ���������ݸ����á���ʱ����˵������Ϊ clox ��Ҫ���Ƶ����ơ�����������Ϊʲô�ˡ�����Ϊ���ǽ�����������Ϊ���ֽڲ�������䵽�ֽ����У��������ֻ�ܴﵽ 255������Ҳ��Ҫ���������������֤��һ�㡣

```c
      expression();
      if (argCount == 255) {
        error("Can't have more than 255 arguments.");
      }
      argCount++;
// compiler.c, in argumentList()
```

����ǰ�ˡ�������������ˣ����м����ͣ����������ָ�

```c
  OP_LOOP,
  OP_CALL,
  OP_RETURN,
// chunk.h, in enum OpCode
```

### 24.5.1 ���󶨲���  Binding arguments to parameters

�����ǿ�ʼʵ��֮ǰ��Ӧ�ÿ��ǵ���ʱ��ջ�������Լ�������Ҫ��������ʲô�������� call ָ��ʱ���Ѿ�ִ���˱����ú����ı��ʽ�����������Ĳ������������ǵĳ���������������

```c
fun sum(a, b, c) {
  return a + b + c;
}

print 4 + sum(5, 6, 7);
```

�����`OP_CALL`���� `sum()`��ָ���ͣ VM����ջ������ʾ��

![](./assets/eacc6621bb0beb2c0468b5030d6291ce45a1e364.png)

��`sum()`������ĽǶ�����һ�¡�����������`sum()`ʱ���Զ�������㡣Ȼ��֮������˳��Ϊ����`a`��`b`��`c`���䱾�زۡ�Ҫִ�ж� `sum()`�ĵ��ã���Ҫ   ��ʼ��ɱ����ú�����CallFrame �Լ�������ʹ�õ�ջ������Ȼ����Ҫ�ռ����ݸ������Ĳ����������Ƿ����������Ӧ���С�

�� VM ��ʼִ�е�����ʱ`sum()`������ϣ������ջ���ڿ�������������

![](./assets/e8a05347acdc6636a37a560953c99aca1f821dee.png)

���Ƿ�ע�⵽���������õĲ����ۺͱ���������Ҫ�Ĳ��������������ȫ��ȷ��˳�����еģ��෽�㰡���Ⲣ���ɺϡ�����̸��ÿ�� CallFrame ��ջ�ж����Լ��Ĵ���ʱ���Ҵ�δ˵����Щ���ڱ�����*���ཻ*�ġ�û��ʲô����ֹ�����ص����ǣ�����������

![](./assets/d174fbecef70881418565e8c9abeed5b6c8059a7.png)

������(caller)ջ�Ķ������������õĺ��������������ǲ���������֪����������ʹ�õĲ�֮��û���κ������ۣ���Ϊ�����Ѿ������������������ʽʱ������κ���ʱ���󡣱�������(callee��s)��ջ�ĵײ��ص�����˲����������ֵ�Ѿ����ڵ�λ����ȫ���롣

> ��ͬ���ֽ���VM����ʵ��CPU�ܹ��в�ͬ�ĵ���Լ�������������������ݲ������洢���ص�ַ�ȵľ�����ơ�������ʹ�õĻ����ǻ���Lua�ĸɾ������ٵ��������

����ζ�Ų���Ҫ��*�κ�*���������������󶨵������������֮��򻷾�֮��û�и���ֵ�������Ѿ�ǡ����������Ҫ�ĵط��������ܷ�����ѳ�Խ����

��ʱ��ִ�е���ָ���ˡ�

```c
      }
      case OP_CALL: {
        int argCount = READ_BYTE();
        if (!callValue(peek(argCount), argCount)) {
          return INTERPRET_RUNTIME_ERROR;
        }
        break;
      }
      case OP_RETURN: {
// vm.c, in run()
```

��Ҫ֪�������õĺ����Լ����ݸ����Ĳ������������Ǵ�ָ��Ĳ������еõ����ߡ���Ҳ��������ͨ����ջ���������ȥ�Ĳ��������ڶ�ջ���ҵ�������λ�á����ǽ������ݽ���һ��������`callValue()`�������������`false`�����ʾ�õ��õ�����ĳ������ʱ���󡣵������������ʱ����ֹ��������

���`callValue()`�ɹ������ú����� CallFrame ջ�Ͻ���һ����֡����`run()`�������Լ���ָ��ǰ֡�Ļ���ָ�룬���������Ҫ������

```c
          return INTERPRET_RUNTIME_ERROR;
        }
        frame = &vm.frames[vm.frameCount - 1];
        break;
// vm.c, in run()
```

�����ֽ������ѭ���Ӹ�`frame`�����ж�ȡ����˵� VM ��ʼִ����һ��ָ��ʱ������`ip`���µ��õĺ����� CallFrame �ж�ȡ����ת������롣ִ�иõ��õĹ��������￪ʼ��

```c
static bool callValue(Value callee, int argCount) {
  if (IS_OBJ(callee)) {
    switch (OBJ_TYPE(callee)) {
      case OBJ_FUNCTION: 
        return call(AS_FUNCTION(callee), argCount);
      default:
        break; // Non-callable object type.
    }
  }
  runtimeError("Can only call functions and classes.");
  return false;
}
// vm.c add after peek()
```

> ����ʹ��`switch`�������鵥�������е�����ˣ����ǵ��������case�����������ɵ�������ʱ���������ˡ�

���˳�ʼ��һ���µ� CallFrame�����ﻹ�и��������Ҫ������Ϊ Lox �Ƕ�̬���͵ģ�����û��ʲô������ֹ�û���д����Ĵ��룬���磺

```c
var notAFunction = 123;
notAFunction();
```

��������������������ʱ��Ҫ��ȫ�ر������ֹͣ�������������ĵ�һ���¾��Ǽ��������ͼ���õ�ֵ�����͡����������һ���������ͻ��������ʵ�ʵ��÷��������

```c
static bool call(ObjFunction* function, int argCount) {
  CallFrame* frame = &vm.frames[vm.frameCount++];
  frame->function = function;
  frame->ip = function->chunk.code;
  frame->slots = vm.stackTop - argCount - 1;
  return true;
}
// vm.c, add after peek()
```

��ֻ�ǳ�ʼ��ջ�ϵ���һ�� CallFrame�����洢ָ�򱻵��ú�����ָ�룬����֡��ipָ�����ֽ���Ŀ�ͷ�����������`slots`ָ���Խ���ܵĴ��ڷ���ջ��������㷨ȷ��ջ�ϵĲ����뺯���Ĳ������룺

![](./assets/06d4a725228976bb3d9a905a4fcbb205886014ed.png)

��Ȥ����`- 1`��Ҫ����ջ���㣬�������Ժ���ӷ���ʱ����������Ϊ��Ԥ���������� slot 1 ��ʼ�������ô�����ǰһ�� slot ��ʼ����ʹ������������롣

�ڼ���֮ǰ�������ǽ���ָ����ӵ����ǵķ��������С�

```c
      return jumpInstruction("OP_LOOP", -1, chunk, offset);
    case OP_CALL:
      return byteInstruction("OP_CALL", chunk, offset);
    case OP_RETURN:
// debug.c, in disassembleInstruction()
```

����һ�ο��ٵ����С����������������� CallFrame �ı�ݺ��������ǲ���ʹ����������ִ�ж������ĵ�һ����ܡ�

```c
  push(OBJ_VAL(function));
  call(function, 0);

  return run();
// vm.c, in interpret(), replace 4 lines
```

�õģ����ڻص�����...

### 24.5.2 ����ʱ������

�ص���ջ���ڵĹ���ԭ���Ǽ������Ϊ������ÿ������������һ�����������ǣ�ͬ������Ϊ Lox ���Ǿ�̬���͵ģ��޴����û����ܻᴫ��̫���̫�ٵĲ������� Lox �У����ǽ��䶨��Ϊ����ʱ���󣬻����������棺

```c
static bool call(ObjFunction* function, int argCount) {
  if (argCount != function->arity) {
    runtimeError("Expected %d arguments but got %d.",
        function->arity, argCount);
    return false;
  }

  CallFrame* frame = &vm.frames[vm.frameCount++];
// vm.c, in call()
```

�ǳ��򵥡������Ϊʲô��ÿ��������Ԫ��(arity)�洢�� ObjFunction �С�

����һ����Ҫ����Ĵ��������û����޴���ϵ���󣬶��������Լ����޴���ϵ������Ϊ CallFrame ������й̶���С������������Ҫȷ����ȵ����������������

```c
  }

  if (vm.frameCount == FRAMES_MAX) {
    runtimeError("Stack overflow.");
    return false;
  }

  CallFrame* frame = &vm.frames[vm.frameCount++];
// vm.c, in call()
```

��ʵ���У��������ӽ�������ƣ��ܿ�����ĳЩʧ�صĵݹ�����д��ڴ���

### 24.5.3 ��ӡ��ջ����

��Ȼ�������۵�������ʱ���󣬵������ǻ���ʱ�������Ǳ�ø����á�ֹͣ����ʱ������ڷ�ֹ VM ��ĳ�ֶ��岻��ȷ�ķ�ʽ��������ٺ���Ҫ�����Ǽ򵥵���ֹ�����ܰ����û��޸�*����*�ô���Ĵ��롣

������������ʱ���ϵľ��乤����**��ջ����**������ӡ��������ֹʱ����ִ�е�ÿ���������Լ�������ֹʱ��ִ��λ�á�������������һ�����ö�ջ���������Ѿ�����ش洢��ÿ�����������ƣ�������ʱ�����ƻ����û����ڵĺ�гʱ�����ǿ�����ʾ������ջ������������������

```c
  fputs("\n", stderr);

  for (int i = vm.frameCount - 1; i >= 0; i--) {
    CallFrame* frame = &vm.frames[i];
    ObjFunction* function = frame->function;
    size_t instruction = frame->ip - function->chunk.code - 1;
    fprintf(stderr, "[line %d] in ", 
            function->chunk.lines[instruction]);
    if (function->name == NULL) {
      fprintf(stderr, "script\n");
    } else {
      fprintf(stderr, "%s()\n", function->name->chars);
    }
  }

  resetStack();
}
// vm.c, in runtimeError(), replace 4 lines
```

> ���`- 1`����Ϊ IP �Ѿ�λ����һ��Ҫִ�е�ָ���ϣ�������ϣ����ջ����ָ����һ��ʧ�ܵ�ָ�

��ӡ������Ϣ��������ǽ����ö�ջ�Ӷ�����������õĺ������ƶ����ײ����������룩������ÿһ֡�������ҵ���Ӧ�ڸ�֡�����ڲ�`ip`��ǰ���кš�Ȼ���ӡ���к��Լ��������ơ�

> ����Ӧ���ڸ�������ʾ�ĸ�˳���ջ֡����һЩ���硣����������ڲ�ĺ�����Ϊ��һ�У������Ŷ�ջ�ĵײ�ǰ����Python ���෴��˳���ӡ���ǡ����Դ��ϵ����Ķ���������ĳ�������ε��������ڵ�λ�õģ����һ���Ǵ���ʵ�ʷ����ĵط���
> 
> ���ַ�������߼��ġ���ȷ����ʼ�տ��Կ���������ĺ�������ʹ��ջ����̫�����޷���ʾ��һ����Ļ�ϡ���һ���棬����ѧ�еġ�[��������](https://en.wikipedia.org/wiki/Inverted_pyramid_(journalism))����������Ӧ�ý�����Ҫ����Ϣ�����ı����*��ǰ��*���ڶ�ջ�����У�����ʵ�ʷ�������ĺ������������������ʵ�ֶ����������ġ�

���磬�������������𻵵ĳ���

```c
fun a() { b(); }
fun b() { c(); }
fun c() {
  c("too", "many");
}

a();
```

����ӡ������

```c
Expected 0 arguments but got 2.
[line 4] in c()
[line 2] in b()
[line 1] in a()
[line 7] in script
```

�⿴����������������

### 24.5.4 �Ӻ�������

����Խ��Խ���ˡ����ڿ��Ե��ú�����VM ��ִ�����ǡ��������ܴ��������ﷵ�ء�`OP_RETURN`���ںܳ�ʱ���ˣ�����������һЩ��ʱ�����������ֻ��Ϊ�������������ֽ���ѭ��������ʵ�ֵ�ʱ���Ѿ�������

```c
      case OP_RETURN: {
        Value result = pop();
        vm.frameCount--;
        if (vm.frameCount == 0) {
          pop();
          return INTERPRET_OK;
        }

        vm.stackTop = frame->slots;
        push(result);
        frame = &vm.frames[vm.frameCount - 1];
        break;
      }
// vm.c, in run(), replace 2 lines
```

����������һ��ֵʱ����ֵ��λ��ջ���������Ǽ������������ú���������ջ���ڣ�������ǵ����÷���ֵ�����浽result��Ȼ�������غ����� CallFrame������������һ�� CallFrame������ζ�������Ѿ�����˶�������ִ�С����������Ѿ���ɣ��������Ǵ�ջ�е������ű�������Ȼ���˳���������

���򣬶���������������������;ֲ����������в�ۡ�������������������ݲ�������ͬ�ۡ����ڵ�������ɣ������߲�����Ҫ���ǡ�����ζ�Ŷ�ջ�Ķ���ǡ��λ�ڷ��غ���ջ���ڵĿ�ͷ��

���ǽ�����ֵpush��ջ���Ǹ��µĽϵ�λ�á�Ȼ�����ǽ�`run()`�����Ļ���ָ�����Ϊ��ǰ֡������ʼ����ʱһ�������ֽ������ѭ������һ�ε����У�VM`ip`���Ӹ�֡��ȡ������ִ�н����ص������ߣ�������ֹͣ�ĵط�����`OP_CALL`ָ��֮��

![](./assets/b6d7b72f696991427c4ce19feee730d7980730a6.png)

ע�⣬�������������ú���*ȷʵ*������һ��ֵ�����Ǻ�������ִ�е��������ĩβ����ʽ���أ�

```js
fun noReturn() {
  print "Do stuff";
  // No return here.
}

print noReturn(); // ???
```

����Ҳ��Ҫ��ȷ��������������£����Ա�ָ��Ϊ��ʽ����`nil`��Ϊ��������һ�㣬��������������

```c
static void emitReturn() {
  emitByte(OP_NIL);
  emitByte(OP_RETURN);
}
// compiler.c, in emitReturn()
```

�����������ں������ĩβ`emitReturn()`д��`OP_RETURN`ָ����ڣ��ڴ�֮ǰ�����ᷢ��һ��ѹ��`nil`��ջ��ָ����������Ǿ����˹����������ã������������Դ����������������Ǽ���֪��������������ʲô��

## 24.6  return statements

�������Ҫһ��������ʽ����Ķ����ĺ���`nil`������Ҫһ��`return`��䡣�����ǿ�ʼ������

```c
    ifStatement();
  } else if (match(TOKEN_RETURN)) {
    returnStatement();
  } else if (match(TOKEN_WHILE)) {
// compiler.c, in statement()
```

������������һ��`return`�ؼ���ʱ��������������

```c
static void returnStatement() {
  if (match(TOKEN_SEMICOLON)) {
    emitReturn();
  } else {
    expression();
    consume(TOKEN_SEMICOLON, "Expect ';' after return value.");
    emitByte(OP_RETURN);
  }
}
// compiler.c, add after printStatement()
```

����ֵ���ʽ�ǿ�ѡ�ģ���˽��������ҷֺ�token���ж��Ƿ��ṩ��ֵ�����û�з���ֵ����������ʽ����`nil`������ͨ������ `emitReturn()`��ʵ���������ᷢ��һ��`OP_NIL`ָ��������Ǳ��뷵��ֵ���ʽ����`OP_RETURN`ָ�������

�����Ѿ�ʵ�ֵ�`OP_RETURN`ָ����ͬ��������Ҫ�κ��µ�����ʱ���롣���� jlox �кܴ�Ĳ�ͬ����jlox�У����Ǳ�����ִ��`return`���ʱʹ���쳣��չ����ջ��������Ϊ����Դ�һЩǶ�׿������ء����� jlox �ݹ�ر��� AST������ζ����Ҫ�ӱܴ��� Java �������á�

�ֽ������������ȫ����ƽ�����ڽ����ڼ���еݹ��½�����������ʱ��VM ���ֽ������ѭ������ȫƽ̹�ġ�C �������û�еݹ顣��ˣ���ʹ�Ǵ�ĳЩǶ�׿��з��أ�Ҳ��Ӻ�����ĩβ����һ���򵥡�

���������ǻ�û����ȫ��ɡ���`return`���������һ���µı�����󡣷��ض��ڴӺ������غ����ã��� Lox ����Ķ���Ҳ������ʽ���롣Ӧ�ò��ܴ����ﷵ�ء�

```c
return "What?!";
```

> ����߼�������`return`�������������������뷨������Ϊ���ṩһ����ǰ��ֹ�ű�����Ȼ��ʽ������������ʹ�÷��ص�������ָʾ���̵��˳����롣

�����Ѿ�ָ��`return`���κκ���֮����һ�������һ�����������������ʵ�֣�

```c
static void returnStatement() {
  if (current->type == TYPE_SCRIPT) {
    error("Can't return from top-level code.");
  }

  if (match(TOKEN_SEMICOLON)) {
// compiler.c, in returnStatement()
```

�������ǽ� FunctionType ö����ӵ���������ԭ��֮һ��

## 24.7 ��������

���ǵ� VM ���Խ��Խǿ�������к��������á����������ء������Զ�����಻ͬ�ĺ�������Щ������������Ȥ�ķ�ʽ�໥���á����ǣ������ף��������*ʲôҲ����*�ˡ�Lox ����Ψһ���������û��ɼ������飬�����临������Σ����Ǵ�ӡ��Ҫ��Ӹ��๦�ܣ�������Ҫ�����ǹ������û���

**�������ʵ��ͨ����������**����������������硣�����ϣ���ܹ���д���ʱ�䡢��ȡ�û����������ļ�ϵͳ�ĳ���������Ҫ��ӱ������������ɴ� Lox ���õ��� C ��ʵ�֡����Թ�����Щ���ܡ�

�����Բ��棬Lox �൱�����������бհ����ࡢ�̳к�������Ȥ�Ķ��������о���һ��������Ե�һ��ԭ������Ϊ������û��ԭ�����ܡ����ǿ���ͨ�����һ�������ǽ������һ�����������ԡ�

Ȼ�����о�һ�Ѳ���ϵͳ����ʵ���ϲ����Ǻ��н������塣һ�����˽�����ν�һ�� C ����󶨵� Lox�����������ˡ�������ȷʵ��Ҫ����*һ��*����ʹ��һ����һ�ı��غ���Ҳ��Ҫ���ǹ������еĻ������� Lox �� C �ӿڡ��������ǽ�ͨ������������м��Ĺ�����Ȼ����ɺ����ǽ����һ��΢С�ı���������֤�������Թ�����

������Ҫ�»��Ƶ�ԭ���ǣ���ʵ�ֵĽǶ�������ԭ��������ͬ�� Lox �����������Ǳ�����ʱ�����ǲ���push CallFrame����Ϊû���ֽ���ָ���֡������û���ֽ���顣�෴��������ĳ�ַ�ʽ������һ�α��� C ���롣

ͨ����������������Ϊ��ȫ��ͬ�Ķ����������� clox �д�������⡣

```c
} ObjFunction;

typedef Value (*NativeFn)(int argCount, Value* args);

typedef struct {
  Obj obj;
  NativeFn function;
} ObjNative;

struct ObjString {
// object.h, add after struct ObjFunction
```

��ʾ��ʽ�� ObjFunction ���򵥡���������һ�� Obj ��ͷ��һ��ָ��ʵ�ֱ�����Ϊ�� C ������ָ�롣�����������ò���������ָ���ջ�ϵ�һ��������ָ�롣��ͨ����ָ����ʲ�������ɺ����᷵�ؽ��ֵ��

������һ�����µĶ������ʹ���һЩ������Ҫ���� ObjNative����������һ�����ƹ��캯���ĺ�����

```c
ObjFunction* newFunction();
ObjNative* newNative(NativeFn function);
ObjString* takeString(char* chars, int length);
// object.h, add after newFunction()
```

��������ʵ�֣�

```c
ObjNative* newNative(NativeFn function) {
  ObjNative* native = ALLOCATE_OBJ(ObjNative, OBJ_NATIVE);
  native->function = function;
  return native;
}
// object.c, add after newFunction()
```

���캯������ C ����ָ���װ�� ObjNative �С������ö���ͷ���洢���������ڶ���ͷ��������Ҫһ���µĶ������͡�

```c
typedef enum {
  OBJ_FUNCTION,
  OBJ_NATIVE,
  OBJ_STRING,
} ObjType;
// object.h, in enum ObjType
```

VM ����Ҫ֪������ͷű�����������

```c
    }
    case OBJ_NATIVE:
      FREE(ObjNative, object);
      break;
    case OBJ_STRING: {
// memory.c, in freeObject()
```

����û��̫�����ݣ���Ϊ ObjNative ��ӵ���κζ�����ڴ档���� Lox ����֧�ֵ���һ�������Ǵ�ӡ��

```c
      break;
    case OBJ_NATIVE:
      printf("<native fn>");
      break;
    case OBJ_STRING:
// object.c, in printObject()
```

Ϊ��֧�ֶ�̬���ͣ�������һ�������鿴һ��ֵ�Ƿ���һ�����غ�����

```c
#define IS_FUNCTION(value)     isObjType(value, OBJ_FUNCTION)
#define IS_NATIVE(value)       isObjType(value, OBJ_NATIVE)
#define IS_STRING(value)       isObjType(value, OBJ_STRING)
// object.h
```

���緵�� true���˺�ӱ�ʾ����������ֵ����ȡ C ����ָ�룺

```c
#define AS_FUNCTION(value)     ((ObjFunction*)AS_OBJ(value))
#define AS_NATIVE(value) \
    (((ObjNative*)AS_OBJ(value))->function)
#define AS_STRING(value)       ((ObjString*)AS_OBJ(value))
object.h
```

������Щ������ VM ��Դ��κ���������һ���Դ����������������Խ����Ǵ洢�ڱ����С��������ǡ��ٰ������ɶԵȡ���Ȼ�������������ĵĲ�����*����*���ǡ���ʹ��һ����Ϊ���ñ��ʽ�е�����������

��`callValue()`���������һ�����͵�case��

```c
      case OBJ_FUNCTION: 
        return call(AS_FUNCTION(callee), argCount);
      case OBJ_NATIVE: {
        NativeFn native = AS_NATIVE(callee);
        Value result = native(argCount, vm.stackTop - argCount);
        vm.stackTop -= argCount + 1;
        push(result);
        return true;
      }
      default:
// vm.c, in callValue()
```

��������õĶ����Ǳ��غ��������ǻ��������� C ���������账�� CallFrames ���κζ���������ֻ�ǽ��� C���õ������Ȼ���������ջ�С���ʹ�ñ������������ܿ졣

����������û�Ӧ���ܹ����ñ�����������û���κοɵ��õĺ�����û�������ⲿ�����ӿ�֮��Ķ������û����޷������Լ��ı�������������������Ϊ VM ʵ���ߵĹ��������ǽ���һ������������ʼ������һ����¶�� Lox ������±���������

```c
static void defineNative(const char* name, NativeFn function) {
  push(OBJ_VAL(copyString(name, (int)strlen(name))));
  push(OBJ_VAL(newNative(function)));
  tableSet(&vm.globals, AS_STRING(vm.stack[0]), vm.stack[1]);
  pop();
  pop();
}
// vm.c, add after runtimeError()
```

������һ��ָ�� C ������ָ������� Lox �е����ơ����ǽ�������װ�� ObjNative �У�Ȼ����洢�ھ��и������Ƶ�ȫ�ֱ����С�

��������֪��Ϊʲô�ڶ�ջ��ѹ��͵������ƺͺ������⿴��������֣��԰ɣ������漰�����ռ�ʱ�����뵣�ĵ����顣`copyString()`��`newNative()`��̬�����ڴ档����ζ��һ������ GC�����Ǿ��п��ܴ������ա�����������������������Ҫȷ���ռ���֪�����ǻ�û��������ƺ� ObjFunction���������Ͳ��Ὣ���Ǵ����������ͷų����������Ǵ洢��ֵ��ջ�Ͽ���ʵ����һ�㡣

> �����û����ѭ������Щ���벻Ҫ���ġ�[һ����������ʵʩ GC](http://craftinginterpreters.com/garbage-collection.html)�����ͻ��ø������塣

�о���ɵ������������Щ����֮�����ǽ�ֻ���һ��С�ı���������

```c
static Value clockNative(int argCount, Value* args) {
  return NUMBER_VAL((double)clock() / CLOCKS_PER_SEC);
}
// vm.c, add after variable vm
```

�⽫�����Գ���ʼ��������������ʱ�䣬����Ϊ��λ�������Է���ض� Lox ������л�׼���ԡ��� Lox �У����ǽ�������Ϊ`clock()`��

```c
  initTable(&vm.strings);

  defineNative("clock", clockNative);
}
// vm.c, in initVM() 
```

Ҫ��ȡ C ��׼��`clock()`��������vm��ģ����Ҫ������

```c
#include <string.h>
#include <time.h>

#include "common.h"
// vm.c
```

�кܶ�������Ҫ���������������ˣ����벢����һ�£�

```c
fun fib(n) {
  if (n < 2) return n;
  return fib(n - 2) + fib(n - 1);
}

var start = clock();
print fib(35);
print clock() - start;
```

���ǿ��Ա�дһ���ǳ���Ч�ĵݹ�쳲��������������õ��ǣ����ǿ��Ժ�������Ч��*�ж�*�͡���Ȼ���ⲻ�Ǽ���쳲��������е�������ķ����������Ƕ�����ʵ�ֶԺ������õ�֧�ֽ���ѹ�����Եĺ÷��������ҵĻ����ϣ��� clox ������������ jlox �п��屶������һ���ܴ�Ľ�����

> ������ Ruby 2.4.3p205 �����е�ͬ�� Ruby ������һ�㣬������ Python 3.7.3 �����еĳ����Լ 3 ����������Ȼ������ VM �н������򵥵��Ż���

---

## [��ս](http://craftinginterpreters.com/calls-and-functions.html#challenges)

1. ��ȡ��д��`ip`�ֶ����ֽ���ѭ������Ƶ���Ĳ���֮һ�����ڣ�ͨ��ָ��ǰ��CallFrame ָ�������������Ҫһ�����ָ�룬����ܻ���ʹ CPU �ƹ����ٻ��沢�������ڴ档�������һ�������������½���
   
   ��������£����ǻὫ��`ip`�����ڱ��� CPU �Ĵ����С�C �����������ڲ������������������ָ�����������Թ��������Թ��������������Ż����������ֱ�ӽ� `ip`�洢�� C �ֲ������в������`register`��C ���������п��ܻ�ͬ�����ǵ���ò����
   
   ��ȷʵ��ζ��������Ҫ�ڿ�ʼ�ͽ�����������ʱС�ĵؽ�����`ip`���ز��洢����ȷ�� CallFrame �С�ʵ�ִ��Ż�����д������׼���ԣ����������Ӱ�����ܡ�����Ϊ����Ĵ��븴����ֵ����

2. �������������ٶȺܿ죬����ԭ��������û����֤���ô��ݵĲ����Ƿ��뺯��Ԥ�ڵ�һ���ࡣ�������Ӧ����������������û���㹻����������´���ص��ñ����������ܻᵼ�º�����ȡδ��ʼ�����ڴ档��� arity ��顣

3. ���ڣ����غ����޷���������ʱ�����źš���ʵ��ʵ���У�����������Ҫ֧�ֵģ���Ϊ�������������� C �ľ�̬���������У����Ӷ�̬���͵� Lox ������á����磬����û���ͼ���ַ������ݸ�`sqrt()`����ñ���������Ҫ��������ʱ����
   
   ��չ��������ϵͳ��֧�������˹������Ӱ�챾�����õ����ܣ�

4. ��Ӹ��౾��������ִ������Ϊ���õ����顣ʹ����Щ��дһЩ���������ʲô���������Ӱ�����Եĸо��Լ�����ʵ���ԣ�