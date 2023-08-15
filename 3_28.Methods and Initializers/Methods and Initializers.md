# �����ͳ�ʼ����

> �����������ʱ����������֮�����������
> 
> -- Umberto Eco,*The Mysterious Flame of Queen Loana*

��ʱ���������ͨ����Ϊ����������������������ζ�ŷ����ͷ������á����ң�����������һ������ķ��������Գ�ʼ����Ҳ�ǡ�

������Щ��������֮ǰ�� jlox ����������Ϥ�����򡣵ڶ������е������������ǽ�ʵ�ֵ�һ����Ҫ�Ż�����ʹ�������õ��ٶȱȻ�׼���ܿ��߱����ϡ��������ǻ����Ȥ֮ǰ�������û����Ķ����������á�

## 28.1 ��������

���ǲ������з�������֮ǰ�Ż��������ã�û�з������þͲ��ܵ��÷��������Խ���������ʼ��

### 28.1.1 ��ʾ����

����ͨ���ӱ�������ʼ��������������õ�����ģ�͡�clox �з���������ʱ��ʾ������ jlox��ÿ���඼�洢һ��������ϣ�������Ƿ������ƣ�ÿ��ֵ���Ƿ�������� ObjClosure��

```c
typedef struct {
  Obj obj;
  ObjString* name;
  Table methods;
} ObjClass;
// object.h, in struct ObjClass
```

һ��ȫ�µ�����һ���յķ�������ʼ��

```c
  klass->name = name; 
  initTable(&klass->methods);
  return klass;
// object.c, in newClass()
```

ObjClass �ṹӵ����������ڴ棬���Ե��ڴ�������ͷ�һ����ʱ�������ҲӦ�ñ��ͷš�

```c
    case OBJ_CLASS: {
      ObjClass* klass = (ObjClass*)object;
      freeTable(&klass->methods);
      FREE(ObjClass, object);
// memory.c, in freeObject()
```

˵���ڴ��������GC ��Ҫͨ������ٵ��������С����һ������Ȼ���Է��ʣ�����ͨ��ĳ��ʵ��������ô�������з�����ȻҲ��Ҫ�����ȥ��

```c
      markObject((Obj*)klass->name);
      markTable(&klass->methods);
      break;
// memory.c, in blackenObject()
```

����ʹ������`markTable()`������������ÿ������Ŀ�еļ��ַ�����ֵ��

���� jlox �Ĵ洢��ķ����ǳ���Ϥ����ͬ�Ĳ�����������ñ�������֮ǰ�Ľ��������Է��������������� AST �ڵ㼰����������з�����������ʱ��������ֻ�Ǳ��������б���

���ڣ���������Ҫ����������ʱ��ÿ����Ϣ������ͨ��һϵ�б�ƽ���ֽ���ָ��Ľӿڡ�������λ�ȡ���԰��������ķ����������������������ʾΪ�ֽ��룿�������������������ҳ��𰸡�

### 28.1.2 ���뷽������

��һ�¸�����������һ��������, �����Խ����൫ֻ���������塣�������ǲ���һ����������������֮���һϵ�з���������

```c
  consume(TOKEN_LEFT_BRACE, "Expect '{' before class body.");
  while (!check(TOKEN_RIGHT_BRACE) && !check(TOKEN_EOF)) {
    method();
  }
  consume(TOKEN_RIGHT_BRACE, "Expect '}' after class body.");
// compiler.c, in classDeclaration()
```

Lox û���ֶ����������������ĩβ�Ҵ�����֮ǰ���κ����ݶ������Ƿ������������������һ��������} �򵽴��ļ�ĩβʱ����ֹͣ���뷽��������û���С���������Ҵ����ţ���һ�����ȷ��������������������ѭ����

�����������ļ��ֲ�����һ��������������������ķ���������ʱ��Ҫ��ĳ�ַ�ʽ���Ҳ���������Щ�����ܶ����ݴ����һ��`OP_CLASS`ָ��෴������Ϊ���������ɵ��ֽ���Ὣ���̲��Ϊ*һϵ��*ָ��������Ѿ�����һ��`OP_CLASS`ָ��������һ���µĿ� ObjClass ����Ȼ��������ָ�����洢��һ�������У��������������ơ�

> ���ǶԱհ��������Ƶ����顣��`OP_CLOSURE`ָ����Ҫ֪��ÿ���������ֵ�����ͺ�������������`OP_CLOSURE`��ָ��֮��ʹ��һϵ��αָ�������б��롪���������ǿɱ������Ĳ�������VM �ڽ���`OP_CLOSURE`ָ��ʱ��������������Щ������ֽڡ�
> 
> �������ǵķ����е㲻ͬ����Ϊ�� VM �ĽǶ�������ÿ�����巽����ָ��ǵ����Ķ������������ַ������е�ͨ���ɱ��С��αָ�������΢��һЩ���������������ٳ�������ѭ���У��������޹ؽ�Ҫ��

���ڣ�����ÿ���������������ᷢ��һ����`OP_METHOD`ָ����������һ�������������е�`OP_METHOD`ָ�ִ��������Ǿ͵õ���һ���������ࡣ���û�����������Ϊ����ԭ�Ӳ���ʱ��VM ����ʵ��Ϊһϵ��ͻ�䡣

Ҫ����һ���·�����VM ��Ҫ����������

1. ���������ơ�

2. ������ıհ���

3. �������󶨵����ࡣ

���ǽ��𲽱�д���������룬������������ν�������ʱ�ģ������￪ʼ��

```c
static void method() {
  consume(TOKEN_IDENTIFIER, "Expect method name.");
  uint8_t constant = identifierConstant(&parser.previous);
  emitBytes(OP_METHOD, constant);
}
// compiler.c, add after function()
```

��`OP_GET_PROPERTY`����������ʱ��Ҫ���Ƶ�ָ��һ��������������������token�Ĵ������ӵ��������У�ȡ�ر�������Ȼ�����Ƿ���һ��`OP_METHOD`�Ը�����Ϊ��������ָ�����������֡��������Ƿ����壺

```c
  uint8_t constant = identifierConstant(&parser.previous);

  FunctionType type = TYPE_FUNCTION;
  function(type);
  emitBytes(OP_METHOD, constant);
// compiler.c, in method()
```

����ʹ�����ڱ��뺯����������ͬ�������� `function()`���ù��ߺ�������������������б��ͺ����塣Ȼ�������������Դ���һ�� ObjClosure ����������ջ������������ʱ��VM ���������ҵ��հ���

����ǽ������󶨵����ࡣVM ����������ҵ��������ҵ��ǣ������ǵ���`OP_METHOD`ָ��ʱ�����ǲ�֪�������������û��ڱ��ط�Χ���������࣬�������ڶ�ջ�ϡ����Ƕ�������������ȫ�ֱ������ж��� ObjClass ��

> ��� Lox ֧�ֽ��ڶ��������࣬�� VM ���Լ������ͨ��ֱ�Ӵ�ȫ�ֱ������в������ҵ��κ��ࡣȻ������Ϊ����֧�ֱ����࣬��������Ҳ��Ҫ�������������

��Ҫ�¡�������ȷʵ֪�����*����*�����ǿ�������������token��������������

```c
  consume(TOKEN_IDENTIFIER, "Expect class name.");
  Token className = parser.previous;
  uint8_t nameConstant = identifierConstant(&parser.previous);
// compiler.c, in classDeclaration()
```

��������֪����û���������и����Ƶ����������ڱθ��ࡣ�����������˼򵥵��޸��������ǿ�ʼ�󶨷���֮ǰ���ᷢ��������ػ�ջ����������κδ��롣

```c
  defineVariable(nameConstant);

  namedVariable(className, false);
  consume(TOKEN_LEFT_BRACE, "Expect '{' before class body.");
// compiler.c, in classDeclaration()
```

�ڱ���������֮ǰ�����ǵ���`namedVariable()`.�ø����������ɴ����Խ����и������Ƶı������ص�ջ�С�Ȼ�����Ǳ��뷽����

> �ڵ��� defineVariable() ֮ǰpop class����� ����namedVariable() ����ֱ�Ӽ��ػ�ջ�ƺ����޴���Ϊʲô��ֱ�Ӱ�������ջ���أ����ǿ��ԣ�����[��һ��](http://craftinginterpreters.com/superclasses.html)�У����ǽ�������������֮����������֧�ּ̳С�����ʱ������಻��ջ�ϣ��������򵥡�

����ζ�ŵ�����ִ��ÿ��`OP_METHOD`ָ��ʱ��ջ�Ķ����Ƿ����ıհ�������������·���һ�����﷽����ĩβ�����ǾͲ�����Ҫ���ಢ���� VM ����Ӷ�ջ�е�����

```c
  consume(TOKEN_RIGHT_BRACE, "Expect '}' after class body.");
  emitByte(OP_POP);
}
// compiler.c, in classDeclaration()
```

��������Щ����һ������һ���׸���������ʾ����������

```c
class Brunch {
  bacon() {}
  eggs() {}
}
```

���ڴˣ������Ǳ��������ɵ������Լ���Щָ��������ʱ���Ӱ���ջ��

![](./assets/ee0b538efa8f9b8867f335e5d83a2c345c4ddd12.png)

ʣ�µľ���Ϊ����`OP_METHOD`ָ��ʵ������ʱ��

### 28.1.3 ִ�з�������

�������Ƕ�������롣

```c
  OP_CLASS,
  OP_METHOD
} OpCode;
// chunk.h, in enum OpCode
```

���������������ַ���������������ָ��һ�����������

```c
    case OP_CLASS:
      return constantInstruction("OP_CLASS", chunk, offset);
    case OP_METHOD:
      return constantInstruction("OP_METHOD", chunk, offset);
    default:
// debug.c, in disassembleInstruction()
```

�ڽ������У�����Ҳ������һ����case��

```c
        break;
      case OP_METHOD:
        defineMethod(READ_STRING());
        break;
    }
// vm.c, in run()
```

��������Ǵӳ������ж�ȡ�������Ʋ����䴫�ݵ��˴���

```c
static void defineMethod(ObjString* name) {
  Value method = peek(0);
  ObjClass* klass = AS_CLASS(peek(1));
  tableSet(&klass->methods, name, method);
  pop();
}
// vm.c, add after closeUpvalues()
```

�����հ�λ��ջ�����������󶨵�����֮�ϡ����Ƕ�ȡ��������ջ�۲����հ��洢����ķ������С�Ȼ�����ǵ����հ�����Ϊ�����Ѿ����������

��ע�⣬���ǲ��Ահ������������κ�����ʱ���ͼ�顣��`AS_CLASS()`�����ǰ�ȫ�ģ���Ϊ���������������˵��¸���λ�ڸö�ջ���еĴ��롣VM�����Լ��ı�������

> VM ������ִ�е�ָ������Ч�ģ���Ϊ�������ṩ���ֽ����������*Ψһ*������ͨ�� clox �Լ��ı������������ֽ������������ JVM �� CPython��֧��ִ�е���������ֽ��롣�⵼����һ����ͬ�İ�ȫ���¡������������ֽ�����ܻ�ʹ VM ��������㡣
> 
> Ϊ�˷�ֹ���������JVM ��ִ���κμ��صĴ���֮ǰִ���ֽ�����֤��CPython ��ʾ�����û���ȷ���������е��κ��ֽ��붼�ǰ�ȫ�ġ�

�����һϵ��`OP_METHOD`ָ�`OP_POP`����������ǽ�ӵ��һ���������÷��������࣬���Կ�ʼ�����ˡ���һ���ǽ���Щ�������ز�ʹ�����ǡ�

## 28.2 ��������

�����ʱ�򣬷��������ʲ����������ã�����������Ϥ���﷨��

```js
instance.method(argument);
```

�����ס���� Lox ������һЩ�����У������������ǲ�ͬ�Ĳ��ҿ��Էֿ���

```js
var closure = instance.method;
closure(argument);
```

��Ȼ�û�*����*�Ѳ����ֿ������Ǿ͵÷ֿ�ʵ�֡���һ����ʹ�����еĵ������﷨������ʵ�����϶���ķ�������Ӧ�÷���ĳ�ֶ���Ȼ���û���������һ�����øö���

�Զ��׼��ķ���������ķ������в��ҷ���������������ƹ����� ObjClosure�������ǻ���Ҫ��ס����������һ������ʱ����`this`��󶨵����ʸ÷�����ʵ��������[������ jlox ���ӷ���ʱ](http://craftinginterpreters.com/classes.html#methods-on-classes)��ʾ����

```js
class Person {
  sayName() {
    print this.name;
  }
}

var jane = Person();
jane.name = "Jane";

var method = jane.sayName;
method(); // ?
```

��Ӧ�ô�ӡ��Jane���������`.sayName`ĳ�ַ�ʽ���صĶ�����Ҫ��ס�Ժ����ʱ��������ʵ������ jlox �У�����ʹ�ý��������еĶѷ��价������ʵ�ָá����� (memory)�������ദ�����б����洢��

�ֽ��� VM ��һ�������ӵĴ洢״̬�ļܹ���[�ֲ���������ʱ����](http://craftinginterpreters.com/local-variables.html#representing-local-variables)��ջ�ϣ�[ȫ�ֱ���](http://craftinginterpreters.com/global-variables.html#variable-declarations)�ڹ�ϣ���У��հ��еı���ʹ��[upvalues](http://craftinginterpreters.com/closures.html#upvalues)������Ҫһ�������ӵĽ������������ clox �еķ��������ߣ��Լ�һ���µ�����ʱ���͡�

### 28.2.1 �󶨷��� (Bound methods)

���û�ִ�з�������ʱ�����ҵ��÷����ıհ��������װ��һ���µġ��󶨷��� bound method�������У��ö�����ٴ��з��ʸ÷�����ʵ��������󶨶����Ժ��������һ�������á�����ʱ��VM ��ִ��һЩ�������԰�`this`�����������ڵĽ�������

> �Ҵ� CPython ��ȡ��Ϊ���󶨷�������Python ���������Ϊ������ Lox����ʹ������ʵ���������С�

�����µĶ������ͣ�

```c
} ObjInstance;

typedef struct {
  Obj obj;
  Value receiver;
  ObjClosure* method;
} ObjBoundMethod;

ObjClass* newClass(ObjString* name);
// object.h, add after struct ObjInstance
```

�����������ͷ����հ���װ��һ�𡣼�ʹֻ���� ObjInstances �ϵ��÷����������ߵ�����Ҳ�� Value������ VM ����������ʲô���Ľ�������ʹ�� Value ��ζ�Ų�����ָ�봫�ݸ���ͨ�õĺ���ʱ������ָ��ת���� Value��

�½ṹ��ζ���������Ѿ�ϰ�ߵĳ������塣��������ö���е��°�����

```c
typedef enum {
  OBJ_BOUND_METHOD,
  OBJ_CLASS,
// object.h, in enum ObjType
```

���ֵ���͵ĺ꣺

```c
#define OBJ_TYPE(value)        (AS_OBJ(value)->type)

#define IS_BOUND_METHOD(value) isObjType(value, OBJ_BOUND_METHOD)
#define IS_CLASS(value)        isObjType(value, OBJ_CLASS)
// object.h
```

��һ����ֵת��Ϊ ObjBoundMethod ָ��ĺ꣺

```c
#define IS_STRING(value)       isObjType(value, OBJ_STRING)

#define AS_BOUND_METHOD(value) ((ObjBoundMethod*)AS_OBJ(value))
#define AS_CLASS(value)        ((ObjClass*)AS_OBJ(value))
// object.h
```

������ ObjBoundMethod �ĺ�����

```c
} ObjBoundMethod;

ObjBoundMethod* newBoundMethod(Value receiver,
                               ObjClosure* method);
ObjClass* newClass(ObjString* name);
// object.h, add after struct ObjBoundMethod
```

�Լ��ú�����ʵ�֣�

```c
ObjBoundMethod* newBoundMethod(Value receiver,
                               ObjClosure* method) {
  ObjBoundMethod* bound = ALLOCATE_OBJ(ObjBoundMethod,
                                       OBJ_BOUND_METHOD);
  bound->receiver = receiver;
  bound->method = method;
  return bound;
}
// object.c, add after allocateObject()
```

���ƹ��캯���ĺ���ֻ�Ǵ洢�����ıհ��ͽ����ߡ���������Ҫ�󶨷���ʱ�������ͷ�����

```c
  switch (object->type) {
    case OBJ_BOUND_METHOD:
      FREE(ObjBoundMethod, object);
      break;
    case OBJ_CLASS: {
// memory.c, in freeObject()
```

�󶨷����м������ã�������*ӵ��*���ǣ�������ֻ�ͷ��Լ������ǣ���Щ����ȷʵ�ᱻ�����ռ������١�

```c
  switch (object->type) {
    case OBJ_BOUND_METHOD: {
      ObjBoundMethod* bound = (ObjBoundMethod*)object;
      markValue(bound->receiver);
      markObject((Obj*)bound->method);
      break;
    }
    case OBJ_CLASS: {
// memory.c, in blackenObject()
```

��ȷ�������ľ���������߱������ڴ��У��Ա����Ժ����`this`���ʱ��Ȼ�����ҵ��������ǻ����ٷ����հ���

> ���ٷ����հ�������������Ҫ�ġ���������һ�� ObjInstance������һ��ָ������ ObjClass ��ָ�룬ObjClass ��һ���������з����ı��������� ObjBoundMethod ����������ĳ��ģ���ķ�ʽ���Ҹе����ɡ�

���ж���֧�ֵ����һ�������Ǵ�ӡ��

```c
  switch (OBJ_TYPE(value)) {
    case OBJ_BOUND_METHOD:
      printFunction(AS_BOUND_METHOD(value)->method->function);
      break;
    case OBJ_CLASS:
// object.c, in printObject()
```

�󶨷����Ĵ�ӡ��ʽ�뺯����ȫ��ͬ�����û��ĽǶ��������󶨷���*��*һ������������һ�����ǿ��Ե��õĶ������ǲ��ṫ�� VM ʹ�ò�ͬ�Ķ�������ʵ�ְ󶨷�����

![](./assets/5288f9a835ff78c1992981aee8c40d922d5291a9.png)

��������ɶ�ñ����Ϊ���Ǹոմﵽ��һ��С��̱���ObjBoundMethod �����ӵ� clox �����һ������ʱ���͡����Ѿ���д�� last`IS_`��`AS_`�ꡣ���Ǿ��뱾��������ֻ�м����ˣ�������һ�������� VM Խ��Խ���ˡ�

### 28.2.2 ��������

�����ǵ��¶�����������ʲô��ʹ������һ����ʵ�ֵ���ͬ���㡱�����﷨���ʷ������������Ѿ���������ȷ�ı���ʽ��Ϊ���Ƿ���`OP_GET_PROPERTY`ָ�������Ҫ����Ψһ�ı���������ʱ��

��ִ�����Է���ָ��ʱ��ʵ��λ��ջ��������ָ��Ĺ����ǲ��Ҿ��и������Ƶ��ֶλ򷽷�������ջ�Ķ����滻Ϊ���ʵ����ԡ�

�������Ѿ������ֶΣ���������ֻ��`OP_GET_PROPERTY`����һ��������չcase��

```c
          pop(); // Instance.
          push(value);
          break;
        }

        if (!bindMethod(instance->klass, name)) {
          return INTERPRET_RUNTIME_ERROR;
        }
        break;
      }
// vm.c, in run(), replace 2 lines
```

�����ڴ���֮��������Բ��ҽ�����ʵ���ϵ��ֶΡ��ֶ��������ڱη�������������Ѱ��һ���ֶΡ����ʵ��û�о��и����������Ƶ��ֶΣ�������ƿ�������һ��������

���ǻ�ȡʵ�����ಢ���䴫�ݸ��µ�`bindMethod()`���֡�����ú����ҵ�һ�����������Ὣ��������ջ������`true`������������`false`��ָʾ�Ҳ������и����Ƶķ�������������Ҳ����һ���ֶΣ�����ζ��������һ������ʱ����������ֹ��������

���Ǻö�����

```c
static bool bindMethod(ObjClass* klass, ObjString* name) {
  Value method;
  if (!tableGet(&klass->methods, name, &method)) {
    runtimeError("Undefined property '%s'.", name->chars);
    return false;
  }

  ObjBoundMethod* bound = newBoundMethod(peek(0),
                                         AS_CLOSURE(method));
  pop();
  push(OBJ_VAL(bound));
  return true;
}
// vm.c, add after callValue()
```

���ȣ���������ķ������в��Ҿ��и������Ƶķ���������Ҳ������ᱨ������ʱ�����˳������򣬻�ȡ�÷����������װ���µ� ObjBoundMethod �С����Ǵ�ջ������ȡ��������������ǵ���ʵ�����ð󶨷����滻ջ����

���磺

```c
class Brunch {
  eggs() {}
}

var brunch = Brunch();
var eggs = brunch.eggs;
```

������ VM ִ�б���ʽ`bindMethod()`����`brunch.eggs`ʱ�����������

![](./assets/33566d148bdcc3c16d4c9cc3bab36e53fda0459f.png)

��������кܶ�����������û��ĽǶ�����������ֻ�ǻ����һ�����Ե��õĺ�����

### 28.2.3 ���÷���

�û�����������������������ʵ���Ϸ������ǣ������󶨵ķ����ŵ�ջ�ϡ�����ֻ�ǲ��ܶ���Щ�󶨵ķ����������κ����õ����顣ȱ�ٵĲ����ǵ������ǡ�������`callValue()`��ʵ�֣��������������Ϊ�¶�����������һ��case��

> �󶨷���*��*һ����ֵ��������ǿ��Խ���洢�ڱ����У����䴫�ݸ�������������������value��-y ������

```c
    switch (OBJ_TYPE(callee)) {
      case OBJ_BOUND_METHOD: {
        ObjBoundMethod* bound = AS_BOUND_METHOD(callee);
        return call(bound->method, argCount);
      }
      case OBJ_CLASS: {
// vm.c, in callValue()
```

��ԭʼ�հ��� ObjBoundMethod ���أ���ʹ�����е�`call()`��������ͨ������ CallFrame push������ջ����ʼ�Ըñհ��ĵ��á�������ܹ����д� Lox ���������ȫ����

```js
class Scone {
  topping(first, second) {
    print "scone with " + first + " and " + second;
  }
}

var scone = Scone();
scone.topping("berries", "cream");
```

�����������衣���ǿ������������ʺ͵��÷���������Щ������ʧ�ˡ����ǲ����䷳�ؽ������հ���װ��һ���󶨽����ߵĶ����У����ǵ����ǵ��ø÷���ʱ�����Ǹ���û��ʹ���Ǹ������ߡ�

## 28.3 This

�󶨷�����Ҫ���������ߵ�ԭ�����������ڷ������ڷ��ʡ�Lox ͨ��`this`����ʽ���������Ľ����ߡ���ʱ��ʹ��һЩ���﷨�ˡ��ʷ��������Ѿ���`this`����Ϊһ������ı�����ͣ���˵�һ���ǽ���token���ӵ��������С�

```c
  [TOKEN_SUPER]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_THIS]          = {this_,    NULL,   PREC_NONE},
  [TOKEN_TRUE]          = {literal,  NULL,   PREC_NONE},
// compiler.c, replace 1 line 
```

> ��������������ĩβ���»�������Ϊ`this`�� C++ �еı����֣�����֧�ֽ� clox ����Ϊ C++��

������������`this` ǰ׺λ��ʱ��������ɵ�һ���µĽ�����������

```c
static void this_(bool canAssign) {
  variable(false);
} 
// compiler.c, add after variable()
```

���ǽ��� clox ��Ӧ���� jlox ��ʹ�õ���ͬʵ��`this`���������ǽ�`this`����Ϊһ���ʷ�������ľֲ���������ֵ������س�ʼ������ֲ�����һ����������ζ�����ǿ�����ѻ�úܶ���Ϊ���ر��ǣ����÷����еıհ�`this`������ȷ�����鲢����ֵ�в�������ߡ�

����������������ʱ��`this`  token �ոձ����Ѳ��洢Ϊ��ǰ�� token�����ǵ������е�`variable()`�������ú�������ʶ������ʽ����Ϊ�������ʡ������õ�������������ȷ���������Ƿ�Ӧ���Һ���`=`������������������������ܸ�ֵ��`this`�����Դ���`false` �Է�ֹ������

��`variable()`����������`this`�Ƿ�����Լ���token���Ͳ��Ҳ��Ǳ�ʶ�����ܸ��˵ؽ����ء�this����Ϊһ����������Ȼ��ʹ�����е�������������Ʋ����������ڣ��ò��ҽ�ʧ�ܣ���Ϊ��δ����������Ϊ��this���ı�������ʱ���ǽ�����Ӧ�ô�����ڴ��е�ʲôλ���ˡ�

���������Ǳ��հ�����֮ǰ��clox ��ÿ���ֲ������洢�� VM ��ջ�С�����������ٺ���ջ�����е���Щ��λ������Щ�ֲ���������������ǵã�������ͨ������һ������Ϊ���ַ����ľֲ�������Ԥ��ջ���㡣

���ں������ã��ò����ձ��汻���õĺ��������ڲ��û�����ƣ���������Զ���������������Բµ�������ô���¡�����*����*���ã����ǿ����������øò�����洢�����������㽫�洢`this`�󶨵���ʵ����Ϊ�˱���`this`����ʽ��������ֻ��ҪΪ�þֲ������ṩ��ȷ�����ơ�

```c
  local->isCaptured = false;
  if (type != TYPE_FUNCTION) {
    local->name.start = "this";
    local->name.length = 4;
  } else {
    local->name.start = "";
    local->name.length = 0;
  }
}
// compiler.c, in initCompiler(), replace 2 lines
```

����ֻ��Է���ִ�д˲�������������û��`this`.���ң���ʵ�ϣ�����*����*����һ����Ϊ��this���ı��������������ں��������б�дһ��`this`����ʽ�����ú�������������һ�������У���`this`��ȷ�ؽ���Ϊ�ⲿ�����Ľ����ߡ�

```c
class Nested {
  method() {
    fun function() {
      print this;
    }

    function();
  }
}

Nested().method();
```

�ó���Ӧ��ӡ��Ƕ��ʵ������Ϊ�˾������ֲ��� 0 ȡʲô���֣���������Ҫ֪�������ڱ���һ���������Ƿ������������������� FunctionType ö��������һ���µ� case �����ַ�����

```c
  TYPE_FUNCTION,
  TYPE_METHOD,
  TYPE_SCRIPT
// compiler.c, in enum FunctionType
```

�����Ǳ���һ������(method)ʱ������ʹ���Ǹ����͡�

```c
  uint8_t constant = identifierConstant(&parser.previous);

  FunctionType type = TYPE_METHOD;
  function(type);
// compiler.c, in method(), replace 1 line
```

�������ǿ�����ȷ��������⡰this�����������ã���������������ȷ��`OP_GET_LOCAL`ָ�������������հ��������Բ���`this`�����߲�����洢����ֵ�С��ܿᡣ

����������ʱ��������ʵ���ϲ���*��*���С���������û������Э�顣�����޸���

```c
      case OBJ_BOUND_METHOD: {
        ObjBoundMethod* bound = AS_BOUND_METHOD(callee);
        vm.stackTop[-argCount - 1] = bound->receiver;
        return call(bound->method, argCount);
      }
// vm.c, in callValue()
```

��һ������������ʱ��ջ���������еĲ�����Ȼ�������Щ���������Ǳ����÷����ıհ���������� CallFrame �е�������ڵ�λ�á����д��뽫����������ò�ۡ����磬���������ķ������ã�

```c
scone.topping("berries", "cream");
```

��������������洢�������Ĳ�ۣ�

![](./assets/6783e479028b3e642add017456485b5fa0ed3d6a.png)

`-argCount`����������`- 1`����`stackTop`�պ�ָ�򳬹����ʹ�õ�ջ�� ��

### 28.3.1 ����this

���ǵ����������֧���û�*��ȷ*ʹ��`this`�������ǻ���Ҫȷ��������ȷ�����û�����`this`��`this`Lox ˵����ʽ�����ڷ�������֮����һ���������������������÷�Ӧ�ñ�����������

```c
print this; // At top level.

fun notMethod() {
  print this; // In a function.
}
```

��ô���������֪�����Ƿ��ڷ����ڲ��أ��Զ��׼��Ĵ��ǲ鿴��ǰ Compiler �� FunctionType������ֻ��������������һ��ö�ٰ�����ר�Ŵ������������ǣ��ⲻ����ǰ���ʾ��������ȷ�ش������룬�����ʾ���У�����һ�������У��������������Ƕ����һ�������С�

���ǿ��Գ��Խ�����this����������κ���Χ�Ĵʷ���Χ�ڶ��Ҳ��������򱨸��������У�����Ҫ���Ƕ�һ�Ѵ�����д��ң���Ϊ�������û���ҵ����������ڽ��������Ĵ������ʽ�ؽ�����Ϊȫ�ַ��ʡ�

����һ���У����ǽ���Ҫ�й������������Ϣ����������У����ǿ���������ʹ������ȷ�������Ƿ���һ�������С��������ǲ���������δ�����Լ������������һЩ�����ھͰѻ�����װ��λ��

```c
Compiler* current = NULL;
ClassCompiler* currentClass = NULL;  //<<

static Chunk* currentChunk() {
// compiler.c, add after variable current
```

���ģ�����ָ��һ���ṹ����ʾ��ǰ���ڱ�������ڲ��ࡣ������������ʾ��

```c
} Compiler;

typedef struct ClassCompiler {
  struct ClassCompiler* enclosing;
} ClassCompiler;

Parser parser;
// compiler.c, add after struct Compiler
```

��������ֻ�洢һ��ָ������� ClassCompiler ��ָ�룬����еĻ�����������Ƕ����������ķ����в����������� Lox ֧���������� Compiler �ṹһ��������ζ�� ClassCompiler �γ�һ���������ӵ�ǰ���ڲ����ͨ�����з������������

������Ǹ��������κ��������У���ģ�����`currentClass`��`NULL`.����������ʼ����һ����ʱ�����Ὣһ���µ� ClassCompiler ѹ�����ʽ����ջ��

```c
  defineVariable(nameConstant);

  ClassCompiler classCompiler;
  classCompiler.enclosing = currentClass;
  currentClass = &classCompiler;

  namedVariable(className, false);
// compiler.c, in classDeclaration()
```

ClassCompiler �ṹ���ڴ���� C ��ջ�ϣ���������ͨ��ʹ�õݹ��½���д��������õı������ܡ����������ĩβ�����ǽ��ñ������Ӷ�ջ�е������ָ���յı�������

```c
  emitByte(OP_POP);

  currentClass = currentClass->enclosing;
}
// compiler.c, in classDeclaration()
```

�����������������ʱ��`enclosing`����`NULL`�����������`currentClass`Ϊ`NULL`����ˣ�Ҫ�鿴�����Ƿ���һ�����С��������һ�������С�������ֻ�����ģ�������

```c
static void this_(bool canAssign) {
  if (currentClass == NULL) {
    error("Can't use 'this' outside of a class.");
    return;
  }

  variable(false);
compiler.c, in this_()
```

����һ����`this`��ȷ�ؽ�ֹ��class֮�⡣�������ǵķ�����ĸо�����������������ϵķ��������ʽ���������������Ӱ�������ø÷�����ʵ�����������������ˣ�

## 28.4 ʵ����ʼ����

����������Խ�״̬����Ϊ��ϵ��һ���ԭ�򡪡���ʽ�ĺ���ԭ��֮һ������Ϊ��ȷ������ʼ�մ�����Ч���������״̬�����Ӵ�����״̬��Ψһ������ͨ�����ķ���ʱ����Щ��������ȷ��û���κ����⡣����ٶ�����*�Ѿ�*�����ʵ���״̬��������һ�δ���ʱ�أ�

> ��Ȼ��Lox ȷʵ�����ⲿ����ֱ�ӷ��ʺ��޸�ʵ�����ֶΣ�������ͨ���䷽�������� Ruby �� Smalltalk ��ͬ�����ǽ�״̬��ȫ��װ�ڶ����С��������ǵ���߽ű����Բ�������ô��ԭ��

������������ȷ��ͨ�����캯����ȷ����ȫ�µĶ��󣬹��캯���Ȳ�����ʵ���ֳ�ʼ����״̬���� Lox �У�����ʱ�����µ�ԭʼʵ��������һ�����������һ����ʼ�������������κ��ֶΡ���ʼ�����Ĺ�����ʽ����ͨ����������ͬ������һЩ������

1. ÿ���������ʵ��ʱ������ʱ�����Զ����ó�ʼ��������

2. ����ʵ���ĵ����������ڳ�ʼ������ɺ�ȡ��ʵ���������ܳ�ʼ����������������ʲô����ʼ����������Ҫ��ʽ����`this`��

3. ��ʵ�ϣ�һ����ʼ������*��ֹ*�����κ�ֵ����Ϊ���ֵ������ζ����ᱻ������

> �ͺ����ʼ��������ʽ��װ�������Ĵ�����У�
> 
> ```js
> fun create(klass) {
>   var obj = newInstance(klass);
>   obj.init();
>   return obj;
> }
> ```

> ��ע�ⷵ�ص�ֵ�����`init()`�������ġ�

��������֧�ַ�����Ҫ���ӳ�ʼ����������ֻ��Ҫʵ������������������ǰ�˳���ߡ�

### 28.4.1 ���ó�ʼ������

���ȣ��Զ�����`init()`��ʵ����

```c
        vm.stackTop[-argCount - 1] = OBJ_VAL(newInstance(klass));
        Value initializer;
        if (tableGet(&klass->methods, vm.initString,
                     &initializer)) {
          return call(AS_CLOSURE(initializer), argCount);
        }
        return true;
// vm.c, in callValue()
```

������ʱ������ʵ��������������Ѱ��`init()`��������������ҵ�һ�������ǻᷢ��һ�����á����Ϊ��ʼ������ıհ�����һ���µ� CallFrame���������������������

```js
class Brunch {
  init(food, drink) {}
}

Brunch("eggs", "coffee");
```

�� VM ִ�ж� �ĵ���ʱ`Brunch()`�����������ģ�

![](./assets/4cf22e5525ca7184c2c92b7fb03144a57eaab38e.png)

�����ǵ�����ʱ���ݸ�����κβ�����Ȼλ��ʵ���Ϸ��Ķ�ջ�С�`init()`�����µ� CallFrame ������ջ���ڣ������Щ������ʽ��ת������ʼ������

Lox ����Ҫ���������ʼ���������ʡ�ԣ�����ʱ�������µ�δ��ʼ��ʵ�������ǣ����û��`init()`��������ô�ڴ���ʵ��ʱ���ഫ�ݲ�����û���κ����塣������Ϊ����һ������

```c
          return call(AS_CLOSURE(initializer), argCount);
        } else if (argCount != 0) {
          runtimeError("Expected 0 arguments but got %d.",
                       argCount);
          return false;
        }
// vm.c, in callValue()
```

����*ȷʵ*�ṩ�˳�ʼ����ʱ�����ǻ���Ҫȷ�����ݵĲ����������ʼ������������ƥ�䡣���˵��ǣ�`call()`�����Ѿ�Ϊ������������¡�

Ҫ���ó�ʼ����������ʱ�ᰴ���Ʋ���`init()`����������ϣ�����ܿ죬��Ϊ������ÿ�ι���ʵ��ʱ����������ζ��������������Ѿ�ʵ�ֵ��ַ���intern��Ϊ�ˣ�VM Ϊ��init��������һ�� ObjString �������������ַ������� VM �ṹ�С�

```c
  Table strings;
  ObjString* initString;
  ObjUpvalue* openUpvalues;
// vm.h, in struct VM 
```

������ VM ����ʱ�����������ַ�����

```c
  initTable(&vm.strings);

  vm.initString = copyString("init", 4);

  defineNative("clock", clockNative);
// vm.c, in initVM()
```

����ϣ������������������ GC ��Ϊ����һ������

```c
  markCompilerRoots();
  markObject((Obj*)vm.initString);
}
// memory.c, in markRoots()
```

��ϸ���������κεȴ������Ĵ����𣿲�������һ��΢��ġ������ռ������ڶ�ȡ`vm.initString`.���ֶ��Ǹ��ݵ��õĽ����ʼ����`copyString()`�����Ǹ����ַ���������ڴ棬��ᴥ�� GC������ռ����ڴ����ʱ�����У������ڳ�ʼ��֮ǰ��ȡ`vm.initString`�����ԣ��������ǽ��ֶι��㡣

```c
  initTable(&vm.strings);

  vm.initString = NULL;
  vm.initString = copyString("init", 4);

// vm.c, in initVM()
```

������ VM �ر�ʱ���ָ�룬��Ϊ��һ�н��ͷ�����

```c
  freeTable(&vm.strings);
  vm.initString = NULL;
  freeObjects();
// vm.c, in freeVM()
```

�õģ��������ǿ��Ե��ó�ʼ������

### 28.4.2 ��ʼ��������ֵ

��һ����ȷ��ʹ�ó�ʼ�����������ʵ�����Ƿ�����ʵ����������`nil`���ʼ�������ص��������ݡ����ڣ����һ���ඨ����һ����ʼ��������ô������һ��ʵ��ʱ��VM �Ὣ�Ըó�ʼ�����ĵ������͵� CallFrame ջ�ϡ�Ȼ�����ͼ����������䡣

ÿ����ʼ����������ʱ���û����ഴ��ʵ���ĵ��þͻ���ɣ����һὫ��ʼ���������ص�ֵ����ջ�С�����ζ�ų����û����Խ�`return this;`���ڳ�ʼ�������ĩβ�����򲻻᷵���κ�ʵ�����ⲻ�Ǻ����á�

Ϊ�˽��������⣬ÿ��ǰ�˱���һ����ʼ������ʱ���������������ĩβ������ͬ���ֽ����ԴӸ÷�������`this`��������ͨ���Ĵ��������������ʽ`nil`��Ϊ������*��һ��*����������Ҫ����֪������ʱ�����ʼ����������ͨ������������ڱ���ķ����������Ƿ�Ϊ��init������⵽��һ�㡣

```c
  FunctionType type = TYPE_METHOD;
  if (parser.previous.length == 4 &&
      memcmp(parser.previous.start, "init", 4) == 0) {
    type = TYPE_INITIALIZER;
  }

  function(type);
// compiler.c, in method()
```

���Ƕ�����һ���µĺ������������ֳ�ʼ����������������

```c
  TYPE_FUNCTION,
  TYPE_INITIALIZER,
  TYPE_METHOD,
// compiler.c, in enum FunctionType
```

ÿ��������������ĩβ������ʽ����ʱ�����Ǽ�������Ծ����Ƿ�����ض��ڳ�ʼ���������Ϊ��

```c
static void emitReturn() {
  if (current->type == TYPE_INITIALIZER) {
    emitBytes(OP_GET_LOCAL, 0);
  } else {
    emitByte(OP_NIL);
  }

  emitByte(OP_RETURN);
// compiler.c, in emitReturn(), replace 1 line
```

�ڳ�ʼ�����У����ǲ����ڷ���֮ǰѹ��`nil`��ջ�����Ǽ��ذ���ʵ���Ĳ��㡣�ڱ���û��ֵ��`return`���ʱҲ�����`emitReturn()`�����������Ҳ������ȷ�����û��ڳ�ʼ����������ǰ���ص������

### 28.4.3 ��ʼ�������󷵻�

���һ����Ҳ�������ǳ�ʼ�������⹦���б��е����һ����ó��Դӳ�ʼ���������κ��������ݳ�Ϊ���󡣼�Ȼ�����������˷������ͣ���ͺܼ��ˡ�

```c
  if (match(TOKEN_SEMICOLON)) {
    emitReturn();
  } else {
    if (current->type == TYPE_INITIALIZER) {
      error("Can't return a value from an initializer.");
    }

    expression();
// compiler.c, in returnStatement()
```

�����ʼ�����е�`return`�����ֵ�����ǻᱨ�����������Ȼ��������֮������ֵ�������������Ͳ��ᱻβ�����ʽ�Ի󲢱���һ�Ѽ�������

��������[�ܿ�ͻ�](http://craftinginterpreters.com/superclasses.html)̸���ļ̳�֮�⣬���������� clox ����һ�������൱��ȫ����ϵͳ��

```js
class CoffeeMaker {
  init(coffee) {
    this.coffee = coffee;
  }

  brew() {
    print "Enjoy your cup of " + this.coffee;

    // No reusing the grounds!
    this.coffee = nil;
  }
}

var maker = CoffeeMaker("coffee and chicory");
maker.brew();
```

�ǳ��ʺ��ʺϾ����̵� C ����

> �ҳ��ϡ����̡����ܲ����ǵ�������Ա���õĴ�С�ο���Ҳ����Ӧ��˵���������ġ�֮��ġ�

## 28.5 �Ż�����

���ǵ� VM ��ȷ��ʵ���˷������úͳ�ʼ��������������塣���ǿ���������ͣ�����������Ǵ�ͷ��ʼ���� Lox �������ڶ���ʵ�ֵ���Ҫԭ���Ǳ����ǵľ� Java ������ִ�еø��졣���ڣ���ʹ�� clox �У���������Ҳ������

Lox �����彫�������ö���Ϊ���������������ʷ���Ȼ����ý�������ǵ� VM ����֧�ֽ���Щ��Ϊ�����Ĳ�������Ϊ�û�*����*�����Ƿֿ����������ڲ����÷���������·�������Ȼ���Ժ���ð󶨵ķ�������ĿǰΪֹ������ʵ�ֵ�һ�ж����ǲ���Ҫ�ġ�

����*ʼ��*��������Ϊ�����Ĳ�����ִ�л�����޴�ĳɱ���ÿ�� Lox ������ʺ͵��÷���ʱ������ʱ�Ѷ������һ���µ� ObjBoundMethod����ʼ�����ֶΣ�Ȼ���������ء��Ժ�GC ���뻨ʱ���ͷ�������Щ��ʱ�󶨷�����

�����ʱ��Lox �������һ������Ȼ���������������󶨷�����һ���ֽ���ָ�����Ȼ������һ���ֽ���ָ��ʹ�á���ʵ�ϣ��������ֱ�ӣ������ڱ������������Դ��ı���*����*�����ڷ�������һ����������Է��ʺ��һ�������źܿ�����һ���������á�

�������ǿ����ڱ���ʱʶ����Բ�������������л��ᷢ��һ���µ�����ָ����ִ���Ż��ķ������á�

���Ǵӱ���   �����Ա���ʽ�ĺ�����ʼ��

> ��������㹻��ʱ��۲��ֽ��� VM �����У����ᷢ��������һ����һ����ִ��ͬһϵ�е��ֽ���ָ�**һ�־�����Ż������Ƕ���һ����Ϊ����**ָ����µ�ָ�����Щָ���ں�Ϊһ�����������о�����ͬ��Ϊ��ָ�
> 
> �ֽ����������������������֮һ�ǽ���ͷ���ÿ��ָ��Ŀ�����������ָ���ںϵ�һ��ָ���п����������е�һЩ��
> 
> ��ս����ȷ��*��Щ*ָ�������㹻�ձ��ԴӸ��Ż��л��档ÿ���µĳ���ָ�������һ�����Լ�ʹ�õĲ����룬����ֻ����ô��Ĳ��������ʹ�á�����̫�࣬������Ҫ����Ĳ�������룬������Ӵ����С��ʹ*����*ָ��Ľ����ٶȱ�����

```c
  if (canAssign && match(TOKEN_EQUAL)) {
    expression();
    emitBytes(OP_SET_PROPERTY, name);
  } else if (match(TOKEN_LEFT_PAREN)) {
    uint8_t argCount = argumentList();
    emitBytes(OP_INVOKE, name);
    emitByte(argCount);
  } else {
// compiler.c, in dot()
```

�����������������ƺ�����Ѱ�������š��������ƥ��һ���������л���һ���µĴ���·��������������������ñ���ʽʱһ����������б���Ȼ�����Ƿ���һ����`OP_INVOKE`ָ�����Ҫ������������

1. ���������ڳ������е�������

2. ���ݸ������Ĳ���������

���仰˵����������ָ�˳������������滻`OP_GET_PROPERTY`��ָ��Ĳ�������`OP_CALL`��ʵ������������ָ����ںϡ������Ƕ�������

```c
  OP_CALL,
  OP_INVOKE,
  OP_CLOSURE,
// chunk.h, in enum OpCode
```

���������ӵ����������У�

```c
    case OP_CALL:
      return byteInstruction("OP_CALL", chunk, offset);
    case OP_INVOKE:
      return invokeInstruction("OP_INVOKE", chunk, offset);
    case OP_CLOSURE: {
// debug.c, in disassembleInstruction()
```

����һ���µ�����ָ���ʽ�������ҪһЩ�Զ��巴����߼���

```c
static int invokeInstruction(const char* name, Chunk* chunk,
                                int offset) {
  uint8_t constant = chunk->code[offset + 1];
  uint8_t argCount = chunk->code[offset + 2];
  printf("%-16s (%d args) %4d '", name, argCount, constant);
  printValue(chunk->constants.values[constant]);
  printf("'\n");
  return offset + 3;
}
// debug.c, add after constantInstruction()
```

���Ƕ�ȡ������������Ȼ���ӡ���������ƺͲ����������ڽ��������ֽ������ѭ���У������Ķ�����ʼ�ˡ�

```c
      }
      case OP_INVOKE: {
        ObjString* method = READ_STRING();
        int argCount = READ_BYTE();
        if (!invoke(method, argCount)) {
          return INTERPRET_RUNTIME_ERROR;
        }
        frame = &vm.frames[vm.frameCount - 1];
        break;
      }
      case OP_CLOSURE: {
// vm.c, in run()
```

�󲿷ֹ��������� ��`invoke()`�����ǽ����н��ܡ���������Ǵӵ�һ���������в��ҷ������ƣ�Ȼ���ȡ����������������Ȼ�����ǽ����صĹ����ƽ�`invoke()`��������óɹ����ú�������`true` ��������һ����`false`������ζ�ŷ���������ʱ���������������鲢�����ѷ���ʱ��ֹ��������

��󣬼�����óɹ������ջ����һ���µ� CallFrame�����������`frame` ��ˢ�µ�ǰ֡�Ļ��渱����

��Ȥ�Ĺ������������

```c
static bool invoke(ObjString* name, int argCount) {
  Value receiver = peek(argCount);
  ObjInstance* instance = AS_INSTANCE(receiver);
  return invokeFromClass(instance->klass, name, argCount);
}
// vm.c, add after callValue()
```

���ȣ����Ǵ�ջ�л�ȡ�����������ݸ��÷����Ĳ�����ջ���Ϸ���������ǿ������²鿴��ô��ۡ�Ȼ�󽫶���ת��Ϊʵ���������ϵ��÷�����һ���򵥵����顣

��ȷʵ�������*��*һ��ʵ������`OP_GET_PROPERTY`ָ��һ�������ǻ���Ҫ�����û�����س��ԶԴ������͵�ֵ���÷����������

```c
  Value receiver = peek(argCount);

  if (!IS_INSTANCE(receiver)) {
    runtimeError("Only instances have methods.");
    return false;
  }

  ObjInstance* instance = AS_INSTANCE(receiver);
// vm.c, in invoke()
```

����һ������ʱ�������Ա��沢�˳����������ǻ�ȡʵ�����ಢ��ת����һ���µ�ʵ�ú�����

> �����ڿ��Բµ������ǽ���δ����ֵ�һ�������ĺ����У���Ϊ�����Ժ�������������ڱ���������`super`���á�

```c
static bool invokeFromClass(ObjClass* klass, ObjString* name,
                            int argCount) {
  Value method;
  if (!tableGet(&klass->methods, name, &method)) {
    runtimeError("Undefined property '%s'.", name->chars);
    return false;
  }
  return call(AS_CLOSURE(method), argCount);
}
// vm.c, add after callValue()
```

�ú�����˳������ VM ���ʵ��`OP_GET_PROPERTY`��`OP_CALL`ָ����߼������ȣ���������ķ������а����Ʋ��ҷ���������Ҳ������ᱨ�������ʱ�����˳���

�������ǻ�ȡ�����ıհ����������ĵ���push�� CallFrame ��ջ�ϡ�����Ҫ�ѷ���ͳ�ʼ�� ObjBoundMethod����ʵ�ϣ�������������Ҫ������ջ�ϵ��κζ������������ͷ��������Ѿ���������Ҫ��λ�á�

> ��������ʹ��ջ�� 0 ���洢�����ߵ�һ���ؼ�ԭ�򡪡�����ǵ������Ѿ�Ϊ����������֯ջ�ķ�ʽ����Ч�ĵ���Լ�����ֽ��� VM ���ܹ��µ���Ҫ��ɲ��֡�

����������� VM ������һ�����÷�����С������Ӧ�ûῴ������ǰ��ȫ��ͬ����Ϊ�����ǣ�������ǵĹ������öԣ�*����*Ӧ�û�����ߡ���д��һ��С��΢��׼���ԣ�����ִ��һ�� 10,000 �η������á�Ȼ���������������� 10 ����ִ�ж������Ρ����ҵĵ����ϣ�û����`OP_INVOKE`ָ���ͨ���� 1,089 �����Ρ���������µ��Ż�����ͬʱ����� 8,324 �����Ρ��ٶ������7.6 �������ڱ�������Ż�������һ���޴�Ľ�����

> ���ǲ�Ӧ��*����*���š��������ܸĽ�������������Լ�δ�Ż��ķ�������ʵ�ֶ��Եģ����ǳ�����Ϊÿ���������ý��жѷ��䲻��Ӯ���κα�����

![](./assets/9f66101bc910119af946cf46526103045ad187f4.png)

### 28.5.1 �����ֶ�

�Ż��Ļ��������ǣ����㲻���ƻ���ȷ�ԡ��� ������ʵ�ָ����Ϊ�����ṩ��ʱ���û���ϲ��������ǰ��������*��ȷ*�Ĵ𰸡���������ʵ�ָ���ķ�������δ�ܼ����һԭ��

```js
class Oops {
  init() {
    fun f() {
      print "not a method";
    }

    this.field = f;
  }
}

var oops = Oops();
oops.field();
```

���һ�п�������һ���������á���������Ϊ���ǲ��Ҿ�ְ��Ϊ������һ��`OP_INVOKE`ָ�Ȼ������ʵ������ˡ�ʵ�ʷ�������һ��*�ֶ�*���ʣ�������һ��������Ȼ�󱻵��á����ڣ����ǵ� VM û����ȷִ�������������Ҳ�����Ϊ��field���ķ���ʱ��������ʱ����

> ��ĳЩ����£���������ʱ�᷵�ش�����Ի�ȡ�����ٶ����Լӿ�����ܵõ����õ�����ʱ���û����ܻ�е����⡣��Щ����[**���ؿ����㷨**](https://en.wikipedia.org/wiki/Monte_Carlo_algorithm)�����򡣶���ĳЩ����������һ���ܺõ�Ȩ�⡣
> 
> ��������Ҫ�����û�*ѡ��*Ӧ������һ���㷨���������Ե�ʵ���߲��ܵ���������������ǳ������ȷ�ԡ�

��Щʱ�򣬵�����ʵ��ʱ`OP_GET_PROPERTY`�����Ǵ������ֶκͷ������ʡ�Ҫ��������´���������Ҫ��`OP_INVOKE`.

```c
  ObjInstance* instance = AS_INSTANCE(receiver);

  Value value;
  if (tableGet(&instance->fields, name, &value)) {
    vm.stackTop[-argCount - 1] = value;
    return callValue(value, argCount);
  }

  return invokeFromClass(instance->klass, name, argCount);
// vm.c, in invoke()
```

�ܼ򵥵��޸����ڲ���ʵ����ķ���֮ǰ�������Ȳ��Ҿ�����ͬ���Ƶ��ֶΡ���������ҵ�һ���ֶΣ���ô�����洢��ջ�У���������ߣ�*��*�����б��¡�������Ϊ��һ��ָ�����������������ŵĲ����б�֮ǰִ�е�`OP_GET_PROPERTY`��Ϊ��

Ȼ�����ǳ��Ե��ø��ֶε�ֵ��������ϣ���Ŀɵ���һ�������ֽ�`callValue()`���ֵ�����Ͳ��ʵ��ص���������������ֶε�ֵ������հ������Ŀɵ������ͣ��򱨸�����ʱ����

�����ʹ���ǵ��Ż���ȫ��ȫ�����ȫ�������ҵ��ǣ�����ȷʵ������һ�����ܡ�����ʱ���������븶���Ĵ��ۡ�������Բ���������һЩ���˵ļ����������ż����Կ��Խ��е��Ż��е���ɥ�����ǣ���Ϊ���� ��ʵ���ߣ����Ǳ������������Ϸ��

> ��Ϊ����*�����*�����ǵĽ�ɫ�Ƿǳ���ͬ�ġ��������ȷʵ���������Ա�����������ʱ���ܻ�ѡ����֧���Ż��ķ�ʽ���ƻ�������ԡ��û���Ҫ���б����������ԣ�������Ҳ��Ҫ���ٵ�ʵʩ����ʱ��������ܸ����� perf ��Ϊ�ر�������һ��Ȩ���Ǻõ�������ơ�

�����ڴ˴���д�Ĵ�����ѭ���͵��Ż�ģʽ��

1. ʶ�������������Ҫ�ĳ���������������С�����������£�����һ���������ʣ�Ȼ����һ�����á�

2. ���Ӹ�ģʽ���Ż�ʵ�֡��������ǵ�`OP_INVOKE`ָ�

3. ʹ��һЩ�����߼��������Ż���Ĵ��룬����֤��ģʽ�Ƿ�ʵ��Ӧ�á�����������������ڿ���·���ϡ������˻ص�����������׳��δ�Ż���Ϊ�����������ζ�ż������ʵ�������ڵ��÷��������Ƿ����ֶΡ�

�����������Թ�������ʵ�ֹ�����ø��죬���ᷢ���Լ�����Խ��Խ���ʱ����Ѱ��������ģʽ��Ϊ���������ܱ������Ż���ȫְ VM ����ʦ�Ĵ󲿷�ְҵ���Ķ������ѭ���жȹ���

�����ǿ�����ʱͣ��������������clox ����֧��������������ԵĴ󲿷ֹ��ܣ����Ҿ��пɹ۵����ܡ�

## [��ս](http://craftinginterpreters.com/methods-and-initializers.html#challenges)

1. �ڹ�ϣ������ ���`init()`���� �ǳ���ʱ�䣬����Ȼ�൱���������ʵ��һЩ��������д��׼���������ܲ��졣

2. ���� Lox �����Ķ�̬���������У��������õ��������������ִ�й����е��ö����ĸ��ַ�����������ˣ���ʵ���У����������µ��õ����ջ��������ڼ����ȫ��ͬ���������ȫ��ͬ�ķ��������������ʵ���ϲ��Ƕ�̬�ģ���ʹ����˵���ǿ��ԡ�
   
   �߼�����ʵ����θ��ݸù۲�����Ż���

3. ����ִ��`OP_INVOKE`ָ��ʱ��VM ����������ι�ϣ�����ҡ����ȣ���Ѱ��һ���������ط������ֶΣ�ֻ����ʧ��ʱ���Ż�Ѱ�ҷ�����ǰһ�����������á���������ֶβ�����������������*��Ҫ*�ģ���Ϊ�����Ա�ʾʹ����ͬ���﷨�����ֶκͷ����������ֶ��ڱη���(ͬ���ֶαȷ������ȼ���)��
   
   ����һ��Ӱ������ʵ�����ܵ�����ѡ��������ȷ��ѡ������� Lox ��������ԣ������ô����

## [���˵������ӱԤ��](http://craftinginterpreters.com/methods-and-initializers.html#design-note)

����Ȼ�ǵ��ҵ�һ���� TRS-80 �ϱ�дһ��΢�� BASIC �����ü������һЩ��ǰû�����������顣�о�����һ������������ҵ�һ��ƴ�����㹻�Ľ������ͽ�������������*���Լ�������*��дһ��С�����ü������ĳ�£�����ĳ�ָ߽�Ԫ���������������ǲ���������Ȼ��һ������ĸо���

����ʶ���ҿ������һ�����ԣ�������ۺ���Ϊ����ѡ�񡣾ͺ�����һֱ����һ����Ҫ��У����˽��ѧУ��Ȼ����һ��ת��һ������ѧУ���������ҿ��Դ��κ����봩���·����Ҳ���ҪΪ��ʹ�ô��������ҿ���ʹ�õȺ�����Ķ������и�ֵ���ҿ�����û�����������������𣿶��ؼ̳�*��*���ط�����һ�ְ�������̬���صĶ�̬���ԣ�

��Ȼ�أ���ץס���������ɣ������������ܡ�������������֡�����ϵ�������ƾ��������͵�Ʋ�š�����֮��û�ж��š�����������ʱʧ�ܵ����ؽ�������ֻ��Ϊ�˲�ͬ������ͬ�����顣

����һ���ǳ���Ȥ�����飬��ǿ���Ƽ���������Ҫ������졢ǰ���ı�����ԡ����뿴��������������ԡ���ʱ����ȻΪ�˺���������Źֵ�������ԡ�

*����*���������Ŀ���ǳɹ��������ɹ���������Ϊ�����û�����ô�������ȼ����벻ͬ������������£�������ҪĿ�����þ����ܶ���˼�ס�������ԡ���*��ĺ���*�������Եľ䷨������Ӽ����ת�Ƶ������ڸ���Ԫ��Ҫ������������

����Ա���������ǵ�ʱ��ܱ��أ����Ҷ���Щ����ֵ���ϴ������ǵ�ʪ���гֽ���̬�ȡ����ǲ��뽫ʱ���˷������ն��������õ������ϡ���ˣ���Ϊ�������ʦ������Ŀ���Ǿ������ٵ�ѧϰ��Ϊ�����ṩ�����ܶ������������

һ����Ȼ�ķ�����*��*���������Ծ��еĸ���͹���Խ�٣�Ҫѧϰ�Ķ���������Խ�١�������С�ű����Ծ�����óɹ���ԭ��֮һ����ʹ���ǲ�����͹�ҵ������ôǿ�󡪡����Ǹ��������֣�����һ������ĳ�˵Ĵ��ԣ��û��ͻ�ϣ������ʹ�����ǡ�

�ر��ǣ����Ƕ�̬�������Ե�һ�����ơ���̬����Ҫ����ѧϰ*����*���ԡ�������ʱ����;�̬����ϵͳ����Ȼ����ܴﵽ�ü�������µĵز�����̬����Ҫ����ֻѧϰǰ�ߡ�

���գ��������㹻�������ھ�̬�����ļ�ֵ����֧��ѧϰ�ڶ��־�̬���Ե�Ŭ��������ֵ������һ��ʼ������ô���ԡ�

���Ե��������ڣ��򵥵���������ͨ�����������ܺͱ��������ҵ������������Ĺ�����һ����������ͨ����������ֻ�����ø��١�

������һ�ַ������Ա���󲿷����⡣������Ҫ��ʶ���û����ؽ������������Լ��ص����ǵ��Ժ��У�*ֻ��������ǻ�û�еĲ���*����������[֮ǰ�����˵��](http://craftinginterpreters.com/parsing-expressions.html#design-note)���ᵽ�ģ�ѧϰ�ǹ���ת�������Ѿ�֪���ĺ�������Ҫ֪����*֮��*�Ĳ��졣

�������Ե�����Ǳ���û��Ѿ��˽�����һЩ������ԡ���ѧϰ���棬��������������Թ������κι��ܱ����϶��ǡ���ѡ��ġ����Ѿ������ǵ��Ժ��У�����ֻ��Ҫ��ʶ������������ͬ�������顣

���仰˵��*��Ϥ*���ǽ������Բ��óɱ�����һ���ؼ����ߡ���Ȼ���������ȫ��󻯸����ԣ����ս����һ��������������ȫ��ͬ�����ԡ��ⲻ�ǳɹ����ؾ�����Ϊ����һ���ϣ��û�����û�ж����л����������ԡ�

������ȷʵ��Ҫ�ṩһЩ����עĿ�Ĳ��졣��Щ�����������Կ������������������Բ��ܣ��������ٲ�������������������������ƵĻ���ƽ����Ϊ֮һ�����������Ե������Խ�����ѧϰ�ɱ����������������������עĿ�����ơ�

�Ҵ�**��ӱԤ��**�ĽǶ�����������ƽ����Ϊ�������� Steve Klabnik ��˵����������[İ��Ԥ��](https://words.steveklabnik.com/the-language-strangeness-budget)�����û�Ϊѧϰһ�������Զ�Ը����ܵ��¶����������ż��ܵ͡������Ǹ������ǾͲ�����֡�

����ѧ�е�һ����ظ�����[**��������**](https://en.wikipedia.org/wiki/Idiosyncrasy_credit)��������ϵ���������������һ���̶���ƫ�����淶����ͨ��������������ڵ�������Ӯ��������Ȼ������԰���ЩǮ���ڹŹֵĻ�ϣ�������Щ����ܻ��������ǵ�ע�⡣���仰˵��֤�����ǡ�����֮һ���Ϳ������������Ĺ�̥���ģ��������ڴˡ�

�κ�ʱ�������������������������û�е��¶����������κ�ʱ����������Բ�ͬ�ķ�ʽ�����������������飬�㶼�Ứ��һЩԤ�㡣û��ϵ������*��Ҫ*��Ǯ�������������עĿ��������Ŀ����*���ǵ�*ʹ����������ÿ�����ܻ���죬�����Լ���Ϊ�������������˶�������עĿ��������Ȼ�������Ե��������Ƿ�ֵ�á��ı��Ƿ�����м�ֵ������ֵ�û�һЩ�����Ԥ�㣿

��ʵ���У��ҷ�������ζ�������ջ����﷨�Ϸǳ����أ����������ϸ��Ӵ󵨡����ܻ������·�����Ȥ�����������鶨����滻�����Ų�̫����Ϊ��������̫��ʵ�ʹ��ܣ�����ȷʵ������һЩ���⡣�﷨������ѷ������á�

��һ���棬�µ�������������������Ե�������Multimethods��mixin��traits��reflection��dependent types��runtime metaprogramming �ȿ��ԴӸ����������û�ʹ�����Ե�������

���ǣ����������ز���ı�һ����Ȥ�������������������Ƿ�Ҫ׷�������ɹ������ǲ���Ҫ����Ϊ�Թ㲥�Ѻõ������ֶӡ����������������������ɾ�ʿ�ֻ����˻�����һ�������ҶԱ�����С�������ܸ��ҳϣ������ڹ�ģ�е����⣬�Ǿ�ȥ�ɡ�