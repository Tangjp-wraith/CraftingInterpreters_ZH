# ֵ������

> ������һֻ�Դ���С����ʱ������˼������ʱ����ʱ��ᷢ��һ���������Ŀ�����������������ﵱ���������������˿�����ʱ����ȫ��ͬ�ġ�
> 
> -- A. A. Milne,*Winnie-the-Pooh*

��ȥ�ļ���ƪ���ܴ󣬳����˸��ӵļ�����һҳҳ�Ĵ��롣�ڱ����У�ֻ��һ���¸�����Ҫѧϰ������ɢ����һЩ�����˵Ĵ��롣�����˴�Ϣ�Ļ��ᡣ

Lox �Ƕ�̬���͵ġ��������������ڲ�ͬʱ�䱣�沼��ֵ�����ֻ��ַ�������Ŀǰ����ֻ���뷨�����ڣ��� clox �У�����ֵ�������֡������½���ʱ��������֧�ֲ���ֵ��`nil`.��Ȼ��Щ���Ǻ���Ȥ����������ʹ����Ū������ǵ�ֵ��ʾ��ζ�̬����ͬ�����͡�

> ���˾�̬���ͺͶ�̬����֮�⻹�е����ࣺ**unityped**���ڸ÷����У����б��������е�һ���ͣ�ͨ���ǻ����Ĵ���������Unityped �����ڽ��첢����������һЩ Forths �� BCPL������ C �����ԣ������������ġ�
> 
> ����Ŀǰ��clox ��ͳһ���͵ġ�

## 18.1  ����ǩ�� union ��Tagged Unions��

�� C �й����ĺô��ǿ��Դ�ԭʼλ��ʼ�������ݽṹ��������*����*��������C �ڱ���ʱ��û�и���̫����ѵĶ�����������ʱ�͸����ˡ��� C ���ԣ����е�������һ���޲����ֽ����顣�����Ǿ���ʹ�ö����ֽ��Լ����ǵĺ��塣

Ϊ��ѡ��ֵ��ʾ��������Ҫ�ش������ؼ����⣺

1. **��α�ʾֵ�����ͣ�** ���磬��������Խ�һ�����ֳ���`true`����Ҫ������ʱ���ô��󲢱����������ԣ���Ҫ֪��ֵ��������ʲô��

2. **��δ洢ֵ����?**   ������Ҫ�ܹ��ֱ��3��һ�����֣�������Ҫ֪����������4��ͬ����֪���������������ԣ��԰ɣ������ǵ�˼άˮƽ���Ժܺõ�˵����Щ���顣

�������ǲ�ֻ������������ԣ�������Ҫ�Լ�ʵ����������ڻش�����������ʱ�����ǻ������μ�ʵ���������׷��*��Ч*���������

�����������Ժڿ�����˸�������ķ�������������Ϣ����ɾ������ٵı��ء����ڣ����ǽ�����򵥡�����Ľ��������ʼ����**��ǩ�� union**��ֵ���������֣����͡���ǩ����ʵ��ֵ����Ч���ء�Ϊ�˴洢ֵ�����ͣ�����Ϊ VM ֧�ֵ�ÿ��ֵ������һ��ö�١�

```c
#include "common.h"

typedef enum {
  VAL_BOOL,
  VAL_NIL, 
  VAL_NUMBER,
} ValueType;

// typedef double Value;
```

> ����İ�������*�� VM ������֧�ֵ�*����ֵ�������ǿ�ʼ�����������ʱ���û������ÿ���඼����Ҫ�ڴ�ö�������Լ�����Ŀ������������ԣ����ÿ��ʵ������ͬһ���ͣ���ʵ������
> 
> ���仰˵������ VM �ġ����͡�����������û��ġ�

Ŀǰ������ֻ�м���ValueType���������� clox ����ַ������������࣬ValueType��Խ��Խ�ࡣ��������֮�⣬���ǻ���Ҫ�洢ֵ�����ݡ�������`double`��`true`����`false`ֵ�����ǿ���Ϊÿ�ֿ��ܵ����Ͷ���һ�������ֶεĽṹ��

![](./assets/837dbe1be1d4fab7eaad5b0ffb24dad0f0043375.png)

�������˷��ڴ档һ��ֵ����ͬʱ�����ֺͲ���ֵ��������κ�ʱ��㣬ֻ��ʹ������һ���ֶΡ�C ������ͨ������union���Ż��������Ͽ�������һ���ṹ���������������ֶ����ڴ����ص���

> �������Ϥ ML �����е����ԣ�C �еĽṹ�����ϴ��·�ӳ�˳˻����������֮�䡢Ԫ��ʹ�����������֮�������

![](./assets/4b9d61e0ad227840a1c99401d71aa6217488390a.png)

Union�Ĵ�С��������ֶεĴ�С��������Щ�ֶζ��ظ�ʹ����ͬ��λ�������ʹ������ʱ����ǳ�С�ġ������ʹ��һ���ֶδ洢���ݣ�Ȼ��ʹ����һ���ֶη��������������½��͵ײ�λ�ĺ��塣

> ʹ�����Ͻ�λ����Ϊ��ͬ������ C �ľ��衣�����������������Ż��������������ڴ氲ȫ���Բ�����ķ�ʽ���ڴ��ÿ���ֽڽ�����Ƭ���п顣����Ҳ�ǳ�����ȫ������㲻С�ģ�����ܸ��˵ذ������ָ�����

�������ơ�tagged union������ʾ�����������ǵ���ֵ��ʾ��������������ϵ�һ���ṹ�С�

```c
} ValueType;

typedef struct {
  ValueType type;
  union {
    bool boolean;
    double number;
  } as; 
} Value;

typedef struct {
// value.h, add after enum ValueType, replace 1 line
```

Value����һ��type�ֶΣ�Ȼ���ǰ������л���ֵ��union�ĵڶ����ֶΡ��ھ��е��� C �������� 64 λ�����ϣ�����������ʾ��

> һλ���������Ժڿ͸�����ʹ�á�as����Ϊ�����ֶ����Ƶ��뷨����Ϊ������ȡ����ֵʱ�����������ܺã���������һ��ǿ��ת����

![](./assets/69e5a1ac43ab472b345d7f7dda0cf5b1fd755daf.png)

���ֽ�type  tag��ǰ��Ȼ����union��������ܹ���ϲ����ֵ�����С���롣���������ֶΰ���һ�����ֽ�˫����������˱������������ֶκ�����ĸ��ֽڵ���䣬�Խ���˫����������������İ��ֽڱ߽��ϡ�����ζ������ʵ����������tag�ϻ����� 8 ���ֽڣ���ֻ��Ҫ��ʾ 0 �� 3 ֮������֡����ǿ��Խ�ö����䵽��С�Ĵ�С����������ֻ�����Ӷ������(padding)��

> *���ǿ����� union֮��*�ƶ� tag �ֶΣ�����Ҳ�޼����¡�ÿ�����Ǵ���һ�� Values ����ʱ�����������Ƕ� Values �Ĵ󲿷��ڴ�ʹ�õĵط�����C����������ÿ�� Value*֮��*������ͬ������Ա���˫���ȶ��롣

�������ǵ�Values��16���ֽڣ��������е�������Ժ��Ľ�[��](http://craftinginterpreters.com/optimization.html)�����ͬʱ��������Ȼ�㹻С������**�洢�� C ��ջ�в���ֵ����**��Lox ��������������������Ϊ��ĿǰΪֹ����֧�ֵ�Ψһ������**���ɱ�**�ġ�������ǽ��������� 3 �� Value �ĸ������ݸ�ĳ���������Ͳ��ص��ĵ����߻ῴ���Ը�ֵ���޸ġ��㲻�ܡ��޸ġ�3������Զ��3��

## 18.2 Lox ֵ�� C ֵ

���������µ�ֵ��ʾ������û����ɡ����ڣ�clox �����ಿ�ּٶ� Value ��`double`�ı����������д������ֱ�Ӵ�һ�� C ת������һ�����ô���������ȫ���������ˡ��ñ��ߡ�

ʹ���µı�ʾ��ʽ��һ��ֵ����*����*һ��˫����ֵ��������*��ͬ*��������һ������һ����һ��ǿ���Ե�ת�����衣��Ҫ�����벢������Щת����ʹ clox �ٴι�����

���ǽ�����Щת��ʵ��ΪһЩ�꣬ÿ�����Ӧһ�����ͺͲ��������ȣ���ԭ�� C ֵ����Ϊ clox ֵ��

```c
} Value;

#define BOOL_VAL(value)   ((Value){VAL_BOOL, {.boolean = value}})
#define NIL_VAL           ((Value){VAL_NIL, {.number = 0}})
#define NUMBER_VAL(value) ((Value){VAL_NUMBER, {.number = value}})

typedef struct {
// value.h, add after struct Value
```

����ÿһ���������ʵ����͵� C ֵ�������ɾ�����ȷtype tag����������ֵ��ֵ���⽫��̬���͵�ֵ������ clox �Ķ�̬���͵������С����ǣ�Ϊ�˶� Value*��*�κ����飬������Ҫ��������ȡ�� C ֵ��

```c
} Value;

#define AS_BOOL(value)    ((value).as.boolean)
#define AS_NUMBER(value)  ((value).as.number)

#define BOOL_VAL(value)   ((Value){VAL_BOOL, {.boolean = value}})
// value.h, add after struct Value
```

> û��`AS_NIL`�꣬��Ϊֻ��һ��`nil`ֵ����������Ϊ Value`VAL_NIL`��Я���κζ������ݡ�

��Щ��ķ����෴������һ����ȷ���͵�ֵ�����ǽ�������������Ӧ��ԭʼ C ֵ������ȷ���͡����ֺ���Ҫ����Щ��ֱ�ӷ��������ֶΡ��������Ҫ�����Ƶ����飺

```c
Value value = BOOL_VAL(true);
double number = AS_NUMBER(value);
```

Ȼ�����ǾͿ��Դ�һ��ͨ��Ӱ�������Ĵ����š�����֪��ֵ�����ʵ������ͣ�����ʹ���κ�`AS_`�궼�ǲ���ȫ�ġ�Ϊ�ˣ����Ƕ����˼����������ֵ�����͡�

```c
} Value;

#define IS_BOOL(value)    ((value).type == VAL_BOOL)
#define IS_NIL(value)     ((value).type == VAL_NIL)
#define IS_NUMBER(value)  ((value).type == VAL_NUMBER)

#define AS_BOOL(value)    ((value).as.boolean)
// value.h, add after struct Value
```

��� Value ���и����ͣ�����Щ�귵��`true`���κ�ʱ�����ǵ�������һ��`AS_`�꣬����Ҫ���ȵ�������һ��`IS__`������֤������ȷ���͡�������˸��꣬���ڿ��԰�ȫ���� Lox �Ķ�̬����� C �ľ�̬����֮�䴩�����ݡ�

![](./assets/41b6623fc41071d651ff2bca5d3a26fb7894af1c.png)

> `_VAL`�꽫C ֵ���������á�`AS_`�꽫��ָ���

## 18.3 ��̬���͵�����

�Ѿ�����ֵ��ʾ��ת�����Ĺ��ߡ��� clox �ٴ�������ʣ�µľ�����ϸ�о����벢�޸����ݿ���ñ߽��ÿ���ط������Ǳ����в�����ȫ�����˷ܵĲ���֮һ�����ұ�֤�һ�����չʾÿһ�д��룬���ڿ�ʼ�ɡ�

���Ǵ����ĵ�һ��ֵ�Ǳ�������ʱ���ɵĳ�����������ת��Ϊ C double ������ֻ�轫���װ��һ�� Value �У�Ȼ���ٽ���洢�ڳ������С�

```c
  double value = strtod(parser.previous.start, NULL);
  emitConstant(NUMBER_VAL(value));
}
// compiler.c, in number(), replace 1 line
```

������ʱ��������һ����ӡֵ�ĺ�����

```c
void printValue(Value value) {
 printf("%g", AS_NUMBER(value));
}
// value.c, in printValue(), replace 1 line
```

�������ǽ�ֵ���͵�`printf()`֮ǰ���������ȡ˫����ֵ�����ǽ��ܿ����·��ʴ˺���������������ͣ����������������д������С�

### 18.3.1 һԪȡ��������ʱ����

��һ����򵥵Ĳ�����һԪȡ�������Ӷ�ջ�е���һ��ֵ��ȡ����Ȼ��ѹ�������������������������͵�ֵ�����ǲ����ټ���������������ˡ��û�Ҳ������������

```
print -false; // Uh...
```

������Ҫ���ŵش�����������ζ����ʱ�����*����ʱ����*�ˡ���ִ����Ҫ�ض����͵Ĳ���֮ǰ��������Ҫȷ�� Value*��*�����͡�

����һԪȡ�������������ʾ��

```c
      case OP_DIVIDE:   BINARY_OP(/); break;
      case OP_NEGATE:
        if (!IS_NUMBER(peek(0))) {
          runtimeError("Operand must be a number.");
          return INTERPRET_RUNTIME_ERROR;
        }
        push(NUMBER_VAL(-AS_NUMBER(pop())));
        break;
      case OP_RETURN: {
// vm.c, in run(), replace 1 line
```

���ȣ����Ǽ���ջ������ֵ�Ƿ�Ϊ���֡�������ǣ���������ʱ����ֹͣ�����������򣬽���ִ�С�ֻ���������֤֮�󣬲Ż�򿪲�������ȡ������װ�����push����

> Lox �Ĵ��������൱...*����*�����д�����������������ֹͣ���������û������޷��Ӵ����лָ������ Lox ��һ�����������ԣ�����������Ҫ���ȵ�����֮һ��

Ϊ�˷���ֵ������ʹ����һ���µ�С������

```c
static Value peek(int distance) {
  return vm.stackTop[-1 - distance];
}
// vm.c, add after pop()
```

���Ӷ�ջ����һ��ֵ����������������`distance`�ǴӶ�ջ�������¿���Զ��0 �Ƕ�����1 ������һ���ۣ��ȵȡ�

> Ϊʲô������������Ȼ����֤���أ����ǿ����������ں�����½��У���Ҫ���ǽ����������ڶ�ջ�У���ȷ������ڲ����м䴥���ռ��������ռ��������ҵ����ǡ�����������ͬ����������Ҫ�ǳ���ϰ�ߡ�

����ʹ��һ���º�����������ʱ�������ǽ��ӱ�������ಿ���������ǳ��

```c
static void runtimeError(const char* format, ...) {
  va_list args;
  va_start(args, format);
  vfprintf(stderr, format, args);
  va_end(args);
  fputs("\n", stderr);

  size_t instruction = vm.ip - vm.chunk->code - 1;
  int line = vm.chunk->lines[instruction];
  fprintf(stderr, "[line %d] in script\n", line);
  resetStack();
}
// vm.c, add after resetStack()
```

��֮ǰ�϶��� C ��*����*���ɱ��������������Щ���ܲ�ͬ���������ĺ�����`printf()`��һ��������������û��*����*���Լ��ġ��Ȿ�鲻�� C�̳̣������һ��������Թ�����������`...`��`va_list`���������ǿ��Խ����������Ĳ������ݸ�`runtimeError()`.������Щ����ת����`vfprintf()`������`printf()`��Ҫ��ʽ`va_list`�������.

> ���������Ѱ�� C �̳̣���ϲ��*[The C Programming Language](https://www.cs.princeton.edu/~bwk/cbook.html)*��ͨ����Ϊ��K&R���Լ��������ߡ�������ȫ�����µģ���д�������������ֲ�����

�����߿��Խ���ʽ�ַ������ݸ�`runtimeError()`����������������`printf()`ֱ�ӵ���ʱһ����Ȼ��`runtimeError()`��ʽ������ӡ��Щ���������ǲ����ڱ���������������������½ڽ����ɰ����������ݵĸ�ʽ������ʱ������Ϣ��

������ϣ����ʾ���õĴ�����Ϣ�󣬸����û�������ʱ����ִ�����ǵĴ������һ�С����������ڱ�������������token����������ڱ��뵽���еĵ�����Ϣ�в��Ҹ��С������������ȷ��������Ĺ�������ô����Ӧ�ڴ��б����ֽ����Դ�����С�

����ʹ�õ�ǰ�ֽ���ָ������*��һ*���鿴��ĵ��������顣������Ϊ��������ִ��ÿ��ָ��֮ǰ��ͨ�����������ԣ������ǵ��õ���һ�㣬`runtimeError()`ʧ�ܵ�ָ�����ǰһ��ָ�

> ����ʾ���������ֱ���в������ṩ̫�������ġ�����������Ķ�ջ���١���������������û�к������Ե��ã�����û�е��ö�ջ���Ը��١�

Ϊ��ʹ��`va_list`�ʹ������ĺ꣬������Ҫ����һ����׼��ͷ�ļ���

```c
#include <stdarg.h>
#include <stdio.h>
// vm.c, add to top of file
```

������������ǵ� VM ��������������ȡ������ʱ����ȷ�����飨���������ƻ���֮ǰ�����������������������ŵش������ĳ�����ȡ���������ͣ����ǻ�û�У�����Ȼ�� .

### 18.3.2 ��Ԫ���������

����������������ʱ������ƣ�����޸���Ԫ����������ף���Ȼ���Ǹ����ӡ�����Ŀǰ֧�����ֶ�Ԫ�������`+`��`-`��`*`��`/`������֮���Ψһ����������ʹ���ĸ��ײ� C �������Ϊ������޶ȵؼ����ĸ������֮���������룬���ǽ�ͨ���Է�װ��һ���������token��Ϊ�����Ĵ���Ԥ���������С�

[�Ǹ����ڼ���ǰ](http://craftinginterpreters.com/a-virtual-machine.html#binary-operators)�������е�������������������Ǵ������档�������ǿ�����һ���ط���ӱ�Ҫ�����ͼ���ת����

```c
#define READ_CONSTANT() (vm.chunk->constants.values[READ_BYTE()])
#define BINARY_OP(valueType, op) \
    do { \
      if (!IS_NUMBER(peek(0)) || !IS_NUMBER(peek(1))) { \
        runtimeError("Operands must be numbers."); \
        return INTERPRET_RUNTIME_ERROR; \
      } \
      double b = AS_NUMBER(pop()); \
      double a = AS_NUMBER(pop()); \
      push(valueType(a op b)); \
    } while (false)

  for (;;) {
// vm.c, in run(), replace 6 lines
```

�ǵģ�����ʶ�����Ǹ��ܴ�ĺꡣ�ⲻ����ͨ����Ϊ������ C ʵ�����������ǿ�ʼ�ɡ���Щ�������������Ƕ�һԪȡ�������ĸ��ġ����ȣ����Ǽ�������������Ƿ������֡�������߶����ǣ����ǻᱨ������ʱ���������������θˡ�

���������û���⣬�����Ƕ�pop�������Ȼ��Ӧ�ø��������������װ�����������push��ջ��ע�⣬���ǲ���ֱ��ʹ��`NUMBER_VAL()`.�෴��Ҫʹ�õİ�װ����Ϊ��������롣�����������е�����������������һ�����֣��������Ǵ���`NUMBER_VAL`�ꡣ

> ��֪�����Խ�����Ϊ�������ݸ����������ڰ쵽�ˣ�

```c
}
      case OP_ADD:      BINARY_OP(NUMBER_VAL, +); break;
      case OP_SUBTRACT: BINARY_OP(NUMBER_VAL, -); break;
      case OP_MULTIPLY: BINARY_OP(NUMBER_VAL, *); break;
      case OP_DIVIDE:   BINARY_OP(NUMBER_VAL, /); break;
      case OP_NEGATE:
// vm.c, in run(), replace 4 lines
```

�ܿ죬�ҽ�����չʾΪʲô����װ����Ϊ������

## 18.4 ����������

�����������е� clox ���붼�ָ����������������ʱ�����һЩ�������ˡ�������һ���������е����ּ������������ڿ���ִ��һЩ���������ƫִ����ʱ���ͼ�顣���ǿ������ڲ���ʾ�������ͣ����û��ĳ����޷���������һ�����͵�ֵ��

���ڣ��������������ǽ�����Ӷ���������ֵ�ı�����֧�ֿ�ʼ��`true`��`false`��`nil`�����Ƕ��ܼ򵥣����Խ���һ���������������������

�������֣����Ǳ��봦�������ʮ���ֿ�����ֵ����ʵ������ͨ��������ֵ�洢�ڿ�ĳ������в�����һ���򵥵ؼ��ظó������ֽ���ָ�������������⡣���ǿ���Ϊ��������ͬ�������顣���ǻ��ڳ������д洢���ȷ�˵`true`����ʹ�� an`OP_CONSTANT`���������

���ǿ��ǵ���Щ������ʵ���ϣ��٣�ֻ���������ܵ�ֵ������Ҫ���ģ�����û�б�Ҫ�ġ������Һ��������������������˷�һ�����ֽڵ�ָ���һ����������Ŀ���෴�����ǽ���������ר��ָ��������Щ����ֵ�е�ÿһ��ѹ���ջ��

> �Ҳ����ڿ���Ц˵ĳЩ����ֵ��ר�ò�������졣�ֽ��� VM ���Ѵ󲿷�ִ��ʱ������ȡ�ͽ���ָ����ڸ�������Ϊ������Ҫ��ָ��Խ�١�Խ�򵥣����ͻ�Խ�졣ר���ڳ��������ļ��ָ���Ǿ�����Ż���
> 
> ���磬Java �ֽ���ָ���ר�ŵ�ָ�����ڼ��� 0.0��1.0��2.0 �ʹ� -1 �� 5 ������ֵ�������ڴ��������� JVM ���ڽ��ֽ��� JIT ����Ϊ�����������ճ�Ϊһ�ֲ������Ż��������ִ��֮ǰ�Ĵ��롣��

```c
  OP_CONSTANT,
  OP_NIL,
  OP_TRUE,
  OP_FALSE,
  OP_ADD,
// chunk.h, in enum OpCode
```

���ǵ�ɨ�����Ѿ���`true`��`false`��`nil`��Ϊ�ؼ��֣�������ǿ���ֱ��������������ʹ�����ǻ��ڱ�� Pratt ��������ֻ��Ҫ��������������������Щ�ؼ���token���͹��������С����ǽ����������������ʹ����ͬ�ĺ��������

```c
  [TOKEN_ELSE]          = {NULL,     NULL,   PREC_NONE},
  [TOKEN_FALSE]         = {literal,  NULL,   PREC_NONE},
  [TOKEN_FOR]           = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 1 line 
```

���

```c
  [TOKEN_THIS]          = {NULL,     NULL,   PREC_NONE},
  [TOKEN_TRUE]          = {literal,  NULL,   PREC_NONE},
  [TOKEN_VAR]           = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 1 line 
```

�����

```c
  [TOKEN_IF]            = {NULL,     NULL,   PREC_NONE},
  [TOKEN_NIL]           = {literal,  NULL,   PREC_NONE},
  [TOKEN_OR]            = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 1 line 
```

����������ǰ׺λ������`false`,`nil`, ��`true`, ʱ�������������µĽ�����������

```c
static void literal() {
  switch (parser.previous.type) {
    case TOKEN_FALSE: emitByte(OP_FALSE); break;
    case TOKEN_NIL: emitByte(OP_NIL); break;
    case TOKEN_TRUE: emitByte(OP_TRUE); break;
    default: return; // Unreachable.
  }
}
// compiler.c, add after binary()
```

��Ȼ`parsePrecedence()`�Ѿ������˹ؼ���token����ô������Ҫ���ľ��������ȷ��ָ����Ǹ��ݽ�����token���������㡣ǰ�����ڿ��Խ� Boolean �� nil ����ֵ����Ϊ�ֽ��롣����ִ�йܵ������ƶ������ǵ����������

> ���Ǳ�����Ϊÿ������ʹ�õ����Ľ�������������Ϊ�Լ���ʡһ�����أ���������˵����о�����Ҫ���߳�������Ϊ����Ҫ��Ʒζ���⡣

```c
     case OP_CONSTANT: {
        Value constant = READ_CONSTANT();
        push(constant);
        break;
      }
      case OP_NIL: push(NIL_VAL); break;
      case OP_TRUE: push(BOOL_VAL(true)); break;
      case OP_FALSE: push(BOOL_VAL(false)); break;
      case OP_ADD:      BINARY_OP(NUMBER_VAL, +); break;
// vm.c, in run()
```

����������ס�ÿ��ָ�������ʵ���ֵ������ѹ���ջ������Ҳ��Ӧ�����Ƿ�������

```c
   case OP_CONSTANT:
      return constantInstruction("OP_CONSTANT", chunk, offset);
    case OP_NIL:
      return simpleInstruction("OP_NIL", offset);
    case OP_TRUE:
      return simpleInstruction("OP_TRUE", offset);
    case OP_FALSE:
      return simpleInstruction("OP_FALSE", offset);
    case OP_ADD:
// debug.c, in disassembleInstruction()
```

������������ǾͿ�������������춯�صĳ����ˣ�

```c
true
```

���˵���������ͼ��ӡ���ʱ�����ᱬը������Ҳ��Ҫ��չ`printValue()`�Դ��������ͣ�

```c
void printValue(Value value) {
  switch (value.type) {
    case VAL_BOOL:
      printf(AS_BOOL(value) ? "true" : "false");
      break;
    case VAL_NIL: printf("nil"); break;
    case VAL_NUMBER: printf("%g", AS_NUMBER(value)); break;
  }
}
// value.c, in printValue(), replace 1 line
```

���ڿ����ˣ���������һЩ�����͡����ǻ����Ǻ����á���������ֵ֮�⣬�����ܶ�����*��*�κ����顣����Ҫһ��ʱ����ܷ������ã������ǿ��Կ�ʼ������ֵ�����߼��������

### 18.4.1 �߼��Ǻͼ�

��򵥵��߼��������һԪȡ����

```c
print !true; // "false"
```

����²����õ�һ����ָ�

```c
  OP_DIVIDE,
  OP_NOT,
  OP_NEGATE,
// chunk.h, in enum OpCode  
```

���ǿ�������`unary()`ΪһԪȡ����д�Ľ���������������һ���Ǳ��ʽ��ֻ��Ҫ��������������м��ɡ�

```c
  [TOKEN_STAR]          = {NULL,     binary, PREC_FACTOR},
  [TOKEN_BANG]          = {unary,    NULL,   PREC_NONE},
  [TOKEN_BANG_EQUAL]    = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 1 line 
```

��Ϊ��֪��Ҫ�����������Ը�`unary()`�����Ѿ���һ��token���͵�switch��ȷ������ĸ��ֽ���ָ�����ֻ�������һ��case��

```c
 switch (operatorType) {
    case TOKEN_BANG: emitByte(OP_NOT); break;
    case TOKEN_MINUS: emitByte(OP_NEGATE); break;
    default: return; // Unreachable.
  }
// compiler.c, in unary()  
```

�����ǰ�ˡ�������ת�� VM ��������ָ���Ϊ��ʵ��

```c
      case OP_DIVIDE:   BINARY_OP(NUMBER_VAL, /); break;
      case OP_NOT:
        push(BOOL_VAL(isFalsey(pop())));
        break;
      case OP_NEGATE:
// vm.c, in run()
```

��������֮ǰ��һԪ�����һ������popһ����������ִ�в�����Ȼ��push��������ң������������������������������ǲ��ò����Ķ�̬���͡���ȡ`true`���߼��Ǻ����ף�����û��ʲô����ֹ���ع�صĳ���Աд�������Ķ�����

```c
print !nil;
```

����һԪȡ�������κη����ֵĶ���ȡ�����Ǵ��󡣵��ǣ��������ű�����һ����Lox���漰����ֵ��������Ҫ����ֵ��������ʱ��Ϊ���ɡ������������͵Ĺ����Ϊ��falsiness��������������ʵ������

> �������̲�ס��Ū������������͵�ֵ��ζ��ʲô��`nil`���������Լ��ķ񶨣��е���һ����ֵ�α�㡣��һ���ַ������ԣ�������ת����

```c
static bool isFalsey(Value value) {
  return IS_NIL(value) || (IS_BOOL(value) && !AS_BOOL(value));
}
// vm.c, add after peek()
```

Lox ���ⷽ����ѭ Ruby��`nil`��`false`Ϊ�٣�����ֵ����`true`.������һ���������ɵ���ָ���˻���Ҫ�ܹ��ڷ�������з���ࡣ

```c
case OP_DIVIDE:
      return simpleInstruction("OP_DIVIDE", offset);
    case OP_NOT:
      return simpleInstruction("OP_NOT", offset);
    case OP_NEGATE:
// debug.c, in disassembleInstruction()
```

### 18.4.2 ��ȺͱȽ������

�ǻ����������Ǽ��������㶨��ȺͱȽ��������`==`,`!=`,`<`,`>`,`<=`, ��`>=`���⺭���˳��߼������`and`��`or`֮������з��ز�����������������Ϊ��Щ��Ҫ��·����������һ��������̣����ǻ�û��׼���á�

�����������Щ����Ա����˵����

```c
  OP_FALSE,
  OP_EQUAL,
  OP_GREATER,
  OP_LESS,
  OP_ADD,
// chunk.h, in enum OpCode  
```

�ȵȣ���ôֻ������?��ôû��`!=`��`<=`��`>=`�� ����Ҳ����Ϊ��Щ����ָ���ʵ˵�����������������VM ��ִ�еø��죬�������Ŀ�������ܣ�����Ӧ����������

���ҵ���ҪĿ���ǽ����й��ֽ����������֪ʶ����ϣ������ʼ��ʶ���ֽ���ָ���Ҫ�����û�Դ���롣VM ������ȫ���ɵ�ʹ������Ҫ���κ�ָ��ʹ������У�ֻҪ���Ǿ�����ȷ���û��ɼ���Ϊ��

`a != b`�ñ��ʽ�� `!(a == b)`������ͬ�����壬��˱�����������������һ�����ɵر���ǰ�ߡ�����ר��`OP_NOT_EQUAL`ָ����������һ��`OP_EQUAL`���һ��`OP_NOT`.ͬ����`a <= b`��`!(a > b)`��ͬ��`a >= b`��`!(a < b)`��ͬ����ˣ�����ֻ��Ҫ������ָ�

> `a <= b`������`!(a > b)`һ���𣿸���[IEEE 754](https://en.wikipedia.org/wiki/IEEE_754)����������Ϊ NaN ʱ�����бȽ������������ false������ζ��`NaN <= 1`�Ǽٵģ�`NaN > 1`Ҳ�Ǽٵġ��������ǵ����Ǽ������ʼ����ǰ�ߵķ񶨡�
> 
> �����Ȿ�飬���ǲ���̫��ע��Щ������Щϸ����������ʵ����ʵ���к���Ҫ��

�������ڽ������У�����ȷʵ�������µ���������Է���������С�����ʹ����֮ǰ��ͬ�Ľ���������`binary()`������`!=`���У�

```c
  [TOKEN_BANG]          = {unary,    NULL,   PREC_NONE},
  [TOKEN_BANG_EQUAL]    = {NULL,     binary, PREC_EQUALITY},
  [TOKEN_EQUAL]         = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 1 line
```

���������������ں�ߡ�

```c
  [TOKEN_EQUAL]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_EQUAL_EQUAL]   = {NULL,     binary, PREC_EQUALITY},
  [TOKEN_GREATER]       = {NULL,     binary, PREC_COMPARISON},
  [TOKEN_GREATER_EQUAL] = {NULL,     binary, PREC_COMPARISON},
  [TOKEN_LESS]          = {NULL,     binary, PREC_COMPARISON},
  [TOKEN_LESS_EQUAL]    = {NULL,     binary, PREC_COMPARISON},
  [TOKEN_IDENTIFIER]    = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 5 lines 
```

��`binary()`�ڲ��������Ѿ���һ��switch����Ϊÿ��token����������ȷ���ֽ��롣����Ϊ���������������case��

```c
switch (operatorType) {
    case TOKEN_BANG_EQUAL:    emitBytes(OP_EQUAL, OP_NOT); break;
    case TOKEN_EQUAL_EQUAL:   emitByte(OP_EQUAL); break;
    case TOKEN_GREATER:       emitByte(OP_GREATER); break;
    case TOKEN_GREATER_EQUAL: emitBytes(OP_LESS, OP_NOT); break;
    case TOKEN_LESS:          emitByte(OP_LESS); break;
    case TOKEN_LESS_EQUAL:    emitBytes(OP_GREATER, OP_NOT); break;
    case TOKEN_PLUS:          emitByte(OP_ADD); break;
// compiler.c, in binary() 
```

`==`,`<`��`>`��������һ��ָ��������һ��ָ�һ���������������㣬Ȼ��һ��`OP_NOT`�Խ��ȡ��������������ֻ��Ҫ����ָ�

����ζ���� VM �У����ǵĹ������򵥡��������һ��Ĳ�����

```c
      case OP_FALSE: push(BOOL_VAL(false)); break;
      case OP_EQUAL: {
        Value b = pop();
        Value a = pop();
        push(BOOL_VAL(valuesEqual(a, b)));
        break;
      }
      case OP_ADD:      BINARY_OP(NUMBER_VAL, +); break;
// vm.c, in run()
```

��������`==`�����κ�һ�Զ��������ǲ�ͬ���͵Ķ�����Ƚϸ��ӣ����Խ��߼�������һ�������ĺ�����������ġ��ú������Ƿ���һ�� C `bool`���������ǿ��԰�ȫ�ؽ������װ��һ��`BOOL_VAL`.�ú����� Values ��أ�����������ڡ�value��ģ���С�

```c
} ValueArray;

bool valuesEqual(Value a, Value b);
void initValueArray(ValueArray* array);
// value.h, add after struct ValueArray
```

����ʵ�֣�

```c
bool valuesEqual(Value a, Value b) {
  if (a.type != b.type) return false;
  switch (a.type) {
    case VAL_BOOL:   return AS_BOOL(a) == AS_BOOL(b);
    case VAL_NIL:    return true;
    case VAL_NUMBER: return AS_NUMBER(a) == AS_NUMBER(b);
    default:         return false; // Unreachable.
  }
}
// value.c, add after printValue()
```

���ȣ����Ǽ�����͡���� Values �в�ͬ�����ͣ����ǿ϶��ǲ���ȵġ��������ǽ������ֵ��ֱ�ӱȽ����ǡ�

> ĳЩ���Ծ��С���ʽת�������������һ������ת��Ϊ��һ�����ͣ���ͬ���͵�ֵ���ܱ���Ϊ����ȵġ����磬���� 0 ��ͬ�� JavaScript �е��ַ�����0����������ɢ��һ���㹻���ʹ����Դ�������� JS �����һ�������ġ��ϸ���ȡ��������`===`.
> 
> PHP ��Ϊ�ַ�����1���͡�01���ǵȼ۵ģ���Ϊ���Ƕ�����ת��Ϊ�ȼ۵����֣��������յ�ԭ������Ϊ PHP ���� Lovecraftian eldritch god ������ݻ�˼��ġ�
> 
> ��������ж��������͸��������͵Ķ�̬����������Ϊ�������ֵ��ͬ�����磬1.0 ���� 1������ͬ�������͵�ֵ���.

����ÿ��ֵ���ͣ����Ƕ���һ��������case������Ƚ�ֵ�������ǵ�case�����Ƴ̶ȣ���������֪��Ϊʲô���ǲ��ܼ򵥵�`memcmp()`ʹ������ Value �ṹ����������������������Ͳ�ͬ��С�������ֶΣ�ֵ����δʹ�õ�λ��C ����֤���е����ݣ����������ȵ�ֵ������δʹ�õ��ڴ���ʵ���ϲ�ͬ��

![](./assets/807f8a1a27a7be7714dd2f3c475151eaf70137e1.png)

���㲻�������ڵ�֪�����ʵ֮ǰ�Ҿ����˶���ʹ�ࡣ��

������Σ����������� clox ��Ӹ������ͣ��˺����������µ�case��Ŀǰ�����������㹻�ˡ������Ƚ�����������ף���Ϊ���ǽ�����������

```c
        push(BOOL_VAL(valuesEqual(a, b)));
        break;
      }
      case OP_GREATER:  BINARY_OP(BOOL_VAL, >); break;
      case OP_LESS:     BINARY_OP(BOOL_VAL, <); break;
      case OP_ADD:      BINARY_OP(NUMBER_VAL, +); break;
// vm.c, in run()
```

�����Ѿ���չ��`BINARY_OP`���������ط��������͵���������������ǿ�ʼʹ���������Ǵ�����`BOOL_VAL`��Ϊ���ֵ�����ǲ���ֵ����������ӺŻ����û������

һ�����������ӽ̾���Ľ�β�Ƿ�������ָ�

```c
    case OP_FALSE:
      return simpleInstruction("OP_FALSE", offset);
    case OP_EQUAL:
      return simpleInstruction("OP_EQUAL", offset);
    case OP_GREATER:
      return simpleInstruction("OP_GREATER", offset);
    case OP_LESS:
      return simpleInstruction("OP_LESS", offset);
    case OP_ADD:
// debug.c, in disassembleInstruction()
```

���������ǵ���ֵ�������͸��ӽ���ͨ�ñ��ʽ��ֵ���ˡ����� clox �����룺

```c
!(5 - 4 > 3 * 2 == !nil)
```

�ðɣ��ҳ�������ܲ�����*����*�ı�﷽ʽ������������ȡ�ý�չ������ȱ��һ�־����Լ����ַ���ʽ���������ͣ��ַ�������ЩҪ���ӵö࣬��Ϊ�ַ����Ĵ�С���ܲ�ͬ����ʵ֤��������΢С�Ĳ���������˾޴��Ӱ�죬����������Ϊ�ַ�������д[��һ��](http://craftinginterpreters.com/strings.html)��

---

## [��ս](http://craftinginterpreters.com/types-of-values.html#challenges)

1. ���ǿ��Ը���һ���ؼ��ٶ�Ԫ���������������ɾ����Щ����ָ�����������δ������ǵ�ȱʧ��

2. �෴�����ǿ���ͨ���������߼���������Ӧ�ĸ������ָ��������ֽ��� VM ���ٶȡ���ᶨ��ʲôָ�������������ڱ��������֧�ֵ������û����룿