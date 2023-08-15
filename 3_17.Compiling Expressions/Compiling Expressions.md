# ������ʽ

> ������������;����;���ҷ����Լ�������һƬ��ڵ������У������Ѿ���ʧ����·��
> 
> -- Dante Alighieri,*Inferno*

��һ�������˷ܵ�ԭ����һ����Ҳ��������������*����*�����ȣ����ṩ������ VM ִ�йܵ������һ���֡�һ����λ�����ǾͿ��Դ�ɨ��һֱ��ִ�����Ĺ�����̽���û���Դ���롣

![](./assets/b7e41281e677a3da44726dc52324ce9a48d0a4ea.png)

��Σ����ǿ�ʼ��дһ�������ġ���ʵ��*������*��������Դ���벢���һϵ�еͼ�������ָ���Ȼ�������ֽ��������ĳЩоƬ�ı���ָ��������� jlox ���ӽ������롣���Ǽ�����Ϊ���������Ժڿ͡�

> �ֽ���� Niklaus Wirth ��˵�Ѿ��㹻���ˣ�û�����������Ľ�ͷ������

������Ҳ�������Ҫ����չʾ����ϲ�����㷨֮һ��Vaughan Pratt �ġ����϶��µ���������ȼ�����������������֪���Ľ������ʽ�������ŵķ�ʽ�����������ŵش���ǰ׺���������׺����׺��*mixfix*�Լ�����õ��κ����͵�-fix �������������ش������ȼ��͹����ԡ���ϲ������

> Pratt ����������ҵ�е�һ�ֿ�ͷ��ͳ���Ҷ����ı������������鼮��û�н̹����ǡ�ѧ����ǳ���ע���ɵĽ��������� Pratt �ļ����������д�ģ���˱������ˡ�
> 
> �����������������У��ֶ��������ܳ�������ᾪ���ж�����֪�������������Ǵ�����ѧ���ģ���������˵����Ŷ���Ҽ���ǰ�о���������������ҵ�ͬ��˵�����Ǵ�����ɵ�ǰ��ѧ����..."�� 

������һ�����ڿ�ʼ��Ȥ������֮ǰ����Ҫ��һЩ׼���������ڳ����֮ǰ��������ȳ��߲ˡ����ȣ������Ƿ���Ϊ����ɨ��������д����ʱ���ּܣ����ø����õĶ���ȡ����֮��

```c
InterpretResult interpret(const char* source) {
  Chunk chunk;
  initChunk(&chunk);

  if (!compile(source, &chunk)) {
    freeChunk(&chunk);
    return INTERPRET_COMPILE_ERROR;
  }

  vm.chunk = &chunk;
  vm.ip = vm.chunk->code;

  InterpretResult result = run();

  freeChunk(&chunk);
  return result;
}
// vm.c, in interpret(), replace 2 lines
```

���Ǵ���һ���µ�chunk�����䴫�ݸ�������������������ȡ�û��ĳ������ֽ������chunk�����٣��������û���κα���������ͻ������������ȷʵ����������`compile()`����`false`���Ҷ��������õ�chunk��

���򣬽���ɵ�chunk���͵� VM ��ִ�С��� VM ���ʱ���ͷ�chunk��������ˡ��������������ڵ�`compile()`ǩ����ͬ�ˡ�

```c
#define clox_compiler_h

#include "vm.h"

bool compile(const char* source, Chunk* chunk);

#endif
// compiler.h, replace 1 line
```

���Ǵ����������д�����Ŀ飬Ȼ��`compile()`�����Ƿ����ɹ�����ʵ���ж�ǩ����������ͬ�ĸ��ġ�

```c
#include "scanner.h"

bool compile(const char* source, Chunk* chunk) {
  initScanner(source);
// compiler.c, function compile(), replace 1 line
```

��`initScanner()`�����Ǳ���Ψһ�Ҵ������Ĵ����С�ɾ������Ϊ����ɨ��������д����ʱ���룬�������滻Ϊ�������У�

```c
  initScanner(source);
  advance();
  expression();
  consume(TOKEN_EOF, "Expect end of expression.");
}
// compiler.c, in compile(), replace 13 lines 
```

����`advance()`����ɨ���������Ǻܿ�ͻῴ���������á�Ȼ�����һ�����ʽ�����ǻ�û����䣬������������֧�ֵ��﷨��Ψһ�Ӽ���[�������ڼ�����������](http://craftinginterpreters.com/global-variables.html)ʱ��������������һ�㡣�ڱ�����ʽ֮��Ӧ����Դ�����ĩβ�����Լ���ڱ� EOF ��ǡ�

���ǽ��ñ��µ����ಿ����ʹ������������ã��������Ǹ�С`expression()`���á�ͨ������ֱ�����뵽�ú��������У������ϵ������ʵ�֡�

��һ�²�һ����Pratt �Ľ��������ǳ��򵥣�һ��������ȫ�����ص������Ժ��У����ֽ��С��Ƭ�ξ��е㼬�֡���Ȼ�����ǵݹ�ģ����������һ���֡���Ҳ������һ�������ݱ��������㷨ʱ���ñ�����Ӷ�����С�

> ������²�������θ�ڣ�������ϣ������һ���Ƕ������Щ�����д��һƪ���£�������ͬ���㷨����ʹ�� Java ���������ķ��[��Pratt Parsing���ñ��ʽ������ü򵥡�](http://journal.stuffwithstuff.com/2011/03/19/pratt-parsers-expression-parsing-made-easy/)��

�Ҳ���ÿ����չ���ʱ�����·��� 40 ���д��롣��ˣ����ǽ����ⲿ����������ĺ��ģ����ڵ����֭������֮ǰ����������Χ�Ĳ��֡��������½���ȣ��⽫��Ҫ��������ĺ������洢�ռ䣬����������������õġ�

## 17.1 ���˱���  Single-pass Compilation

�����������������������������û���Դ�������˽��京�塣Ȼ������ȡ��Щ֪ʶ�����������ͬ����ĵͼ�ָ����������ʵ���н���������ɫ�ֳ����������Ľ׶Ρ�����������AST�������� jlox ��������������Ȼ��������������� AST �����Ŀ����롣

> ��ʵ�ϣ���������ӵ��Ż�����������ֹ���α���������Ҫȷ���Ż�������*��Щ*����Ҫȷ������������Ǵӱ������л��������ܡ�����Ϊ�Ż�ͨ���Ը��ӵķ�ʽ�໥���á������ڡ������о����򡱺͡��ڰ�������֮�䡣

�� clox �У����ǲ�����һ�����ɵķ������������˺ϲ�Ϊһ�ˡ���ȥ�����Ժڿ�֮����������������Ϊ�����ʵ����û���㹻���ڴ����洢����Դ�ļ��� AST����������������Ϊ��ʹ���ǵı��������򵥣������� C ���ʱ��һ�����������ơ�

������Ҫ�����ĵ��˱����������������������ԡ����ڱ����������ɴ���ʱֻ�ܿ����û��ĳ���������Ե���Ʊ���ʹ������Ҫ̫����Χ�������������һ���﷨�����˵��ǣ�΢�͡���̬���͵� Lox�ǳ��ʺ����������

> ������˵��Ӧ�����˸е����⡣�Ͼ���ȷʵר��Ϊ�Ȿ����������ԡ�

![](./assets/3386068ed32f4dfafc3b17bd70731e98ea9bf54d.png)

��ʵ������ζ�����ǵġ���������C ģ����������� jlox ��ʶ������ڽ����Ĺ��ܡ���ʹ��token��ƥ��Ԥ�ڵ�token���͵ȡ��������д������ɹ��ܡ��������ֽ��벢��������ӵ�Ŀ��顣������ζ���ҽ��ڱ��ºͺ�����½��н���ʹ�á��������͡����롱����

���ǽ����ȹ��������ʹ������ɲ��֡�Ȼ�����ǽ��������м�Ĵ���ƴ����һ���м�ʹ�� Pratt �ļ��������� Lox ���ض��﷨�������ȷ���ֽ��롣

## 17.2 ����tokens

���ȣ���������ǰ�벿�֡��������������Ӧ������������Ϥ��

```c
#include "scanner.h"

static void advance() {
  parser.previous = parser.current;

  for (;;) {
    parser.current = scanToken();
    if (parser.current.type != TOKEN_ERROR) break;

    errorAtCurrent(parser.current.start);
  }
}
// compiler.c
```

������ jlox ��һ����������token����ǰ�ƽ�������ɨ����ѯ����һ��token�����䱣���Ա����á��ڴ�֮ǰ�������ȡ��`current`  token������洢��һ��`previous`�ֶ��С����Ժ�������ó����������ǾͿ�����ƥ��token���ȡ���ء�

��ȡ��һ��token�Ĵ��������һ��ѭ���С����ס��clox ��ɨ�������ᱨ��ʷ������෴�������������*������*�������������������������ǡ�������������������

����ѭ������ȡtoken���������ֱ������һ���Ǵ���Ļ򵽴��յ㡣�����������������ಿ��ֻ�ῴ����Ч��token����ǰ��֮ǰ��token�洢������ṹ�У�

```c
#include "scanner.h"

typedef struct {
  Token current;
  Token previous;
} Parser;

Parser parser;

static void advance() {
// compiler.c
```

��������������ģ������������������һ�����ֽṹ���͵�ȫ�ֱ�����������ǲ���Ҫ�ڱ������н�״̬��һ���������ݵ���һ��������

### 17.2.1 �����﷨����

���ɨ����������һ������token��������Ҫ�����û���ʹ������ᷢ�����������

```c
static void errorAtCurrent(const char* message) {
  errorAt(&parser.current, message);
}
// compiler.c, add after variable parser
```

���Ǵӵ�ǰtoken����ȡλ�ã��Ա�����û���������λ�ò�����ת����`errorAt()`�����������ǣ����ڸո�ʹ�õ�tokenλ�ñ�������������Ϊ������������ṩ���̵����ƣ�

```c
static void error(const char* message) {
  errorAt(&parser.previous, message);
}
// compiler.c, add after variable parser
```

ʵ�ʹ������������

```c
static void errorAt(Token* token, const char* message) {
  fprintf(stderr, "[line %d] Error", token->line);

  if (token->type == TOKEN_EOF) {
    fprintf(stderr, " at end");
  } else if (token->type == TOKEN_ERROR) {
    // Nothing.
  } else {
    fprintf(stderr, " at '%.*s'", token->length, token->start);
  }

  fprintf(stderr, ": %s\n", message);
  parser.hadError = true;
}
// compiler.c, add after variable parser
```

���ȣ����Ǵ�ӡ�������ĵط��������������ɶ��ģ����ǻ᳢����ʾ���ء�Ȼ���ӡ������Ϣ����֮�������������`hadError`��־����¼����������Ƿ���ִ��󡣸��ֶ�Ҳ�����ڽ������ṹ�С�

```c
  Token previous;
  bool hadError;
} Parser;
// compiler.c, in struct Parser
```

��Щʱ����˵�������������`compile()`Ӧ�÷���`false`���������ǿ�������������һ�㡣

```c
  consume(TOKEN_EOF, "Expect end of expression.");
  return !parser.hadError;
}
// compiler.c, in compile() 
```

������һ����־Ҫ���ڴ�������������������������û������ǵĴ������д����ҽ������������﷨�е�λ�øе��������ǲ�ϣ�����ڵ�һ������֮�����һ������������������

�� jlox ������ʹ�ÿֻ�ģʽ����ָ��޸��˸����⡣�� Java �������У������׳�һ���쳣�Դ����н�����������չ������������token������ͬ������ C ��û���쳣���ơ���Ϊ������������һ����־�����ٵ�ǰ�Ƿ��ڿֻ�ģʽ��

> ��`setjmp()`��`longjmp()`�����Ҳ���ȥ�����Щʹ���ڴ�й©������ά������������������ʽ�ȹ��ǳ�����һ����̫�����ˡ�

```c
  bool hadError;
  bool panicMode;
} Parser;
// compiler.c, in struct Parser
```

����������ʱ��������������

```c
static void errorAt(Token* token, const char* message) {
  parser.panicMode = true;
  fprintf(stderr, "[line %d] Error", token->line);
// compiler.c, in errorAt()
```

֮�󣬼����������룬�ͺ�������δ������һ�����ֽ�����Զ���ᱻִ�У����Լ������޺��ġ������ǣ��������˿ֻ�ģʽ��־ʱ������ֻ�Ǽ򵥵����Ƽ�⵽���κ���������

```c
static void errorAt(Token* token, const char* message) {
  if (parser.panicMode) return;
  parser.panicMode = true;
// compiler.c, in errorAt()
```

�������ܿ��ܻ����Ӳ�����ʧ�����û�����֪������Ϊ���󶼱���û�ˡ�������������ͬ����ʱ���ֻ�ģʽ���������� Lox������ѡ�������߽磬��˵������Ժ�������ӵ�������ʱ�����ǽ��������ı�־��

��Щ���ֶ���Ҫ��ʼ����

```c
initScanner(source);

  parser.hadError = false;
  parser.panicMode = false;

  advance();
// compiler.c, in compile()  
```

Ϊ����ʾ����������Ҫһ����׼��ͷ�ļ�stdlib.h��

```c
#include <stdio.h>
#include <stdlib.h>

#include "common.h"
// compiler.c
```

���һ���������������� jlox ����һ�������ѡ�

```c
static void consume(TokenType type, const char* message) {
  if (parser.current.type == type) {
    advance();
    return;
  }

  errorAtCurrent(message);
}
// compiler.c, add after advance()
```

��������`advance()`��ȡ��һ�����ơ�����Ҳ��֤token�Ƿ����Ԥ�ڵ����͡����û�У����ᱨ�������������Ǳ������д�����﷨����Ļ�����

�õģ�����ǰ����ɵĲ���ˡ�

## 17.3 �����ֽ���

�����ǽ��������һ���û�����֮����һ�����ǽ��䷭���һϵ���ֽ���ָ�������򵥵Ĳ��迪ʼ���������ֽڸ��ӵ����С�

```c
static void emitByte(uint8_t byte) {
  writeChunk(currentChunk(), byte, parser.previous.line);
}
// compiler.c, add after consume()
```

��������ΰ���������������һ���򵥵ĺ�������д������ֽڣ����ֽڿ�����ָ��Ĳ�������������������ǰһ��token������Ϣ���Ա�����ʱ����������������

���ڱ�д���Chunk��Ҫ���ݸ�`compile()`����������Ҫ����`emitByte()`��Ϊ�ˣ���������������н麯����

```c
Parser parser;
Chunk* compilingChunk;

static Chunk* currentChunk() {
  return compilingChunk;
}

static void errorAt(Token* token, const char* message) {
// compiler.c, add after variable parser
```

���ڣ�Chunkָ��洢��ģ�鼶�����У��������Ǵ洢����ȫ��״̬һ������ߣ������ǿ�ʼ�����û�����ĺ���ʱ����current chunk���ĸ����ø��Ӹ��ӡ�Ϊ�˱��ⷵ�ز����Ĵ������룬�ҽ����߼���װ��`currentChunk()`�����С�

�����ڱ�д�κ��ֽ���֮ǰ��ʼ������µ�ģ�������

```c
bool compile(const char* source, Chunk* chunk) {
  initScanner(source);
  compilingChunk = chunk;

  parser.hadError = false;
// compiler.c, in compile()
```

Ȼ������󣬵���ɿ�ı���ʱ����һ�𶼽�����

```c
  consume(TOKEN_EOF, "Expect end of expression.");
  endCompiler();
  return !parser.hadError;
// compiler.c, in compile() 
```

�������ν�ģ�

```c
static void endCompiler() {
  emitReturn();
}
// compiler.c, add after emitByte()
```

�ڱ����У������ֻ������ʽ���������� clox ʱ�����������������ִ�е������ʽ��Ȼ���ӡ�����Ҫ��ӡ��ֵ��������ʱʹ�ø�`OP_RETURN`ָ����������ñ�����������һ����ӵ����ĩβ��

```c
static void emitReturn() {
  emitByte(OP_RETURN);
}
// compiler.c, add after emitByte()
```

�������ں��ʱ�����ǲ��������ǵ���������ɡ�

```c
static void emitBytes(uint8_t byte1, uint8_t byte2) {
  emitByte(byte1);
  emitByte(byte2);
}
// compiler.c, add after emitByte()
```

����ʱ������ƣ����ǽ����㹻��������Ҫд��һ�������룬���һ�����ֽڲ����������ֵ�ö����������ĺ�����

## 17.4 ����ǰ׺���ʽ

�����Ѿ���װ�˽����ʹ�������ʵ�ó�������ȱ�ٵĲ������м佫����������һ��Ĵ��롣

![](./assets/67bcc0f3dd8b56031dc8f07afa12ebcad28889ef.png)

Ҫ`compile()` ��ʣ�µ�Ψһ��Ҫʵ�ֵ������������

```c
static void expression() {
  // What goes here?
}
// compiler.c, add after endCompiler()
```

���ǻ�û��׼������ Lox ��ʵ��ÿһ�ֱ��ʽ��ѽ����������û�в���ֵ�����ڱ��£�����ֻ��Ҫ�����ĸ���

- ����        ��`123`
- �������ţ�`(123)`
- һԪȡ����`-123`
- ����        ��`+`,`-`,`*`,`/`

��ͨ������������ÿһ�ֱ��ʽʱ������ͨ��table-driven�Ľ����������������ܡ�

### 17.4.1 ����token

���ڣ������ǹ�ע Lox ���ʽ��ÿ�����ʽ��ֻ��һ��token���ڱ����У���ֻ�����֣����Ժ���и������ݡ���������α������ǣ�

��ÿ��token����ӳ�䵽��ͬ���͵ı��ʽ��Ϊÿ������ʵ��ֽ���ı��ʽ����һ��������Ȼ�󹹽�һ������ָ�����顣�����е�������Ӧ��`TokenType`ö��ֵ��ÿ���������ĺ����Ǳ����token���͵ı��ʽ�Ĵ��롣

Ϊ�˱������֣���ָ�����º�����ָ��洢�������`TOKEN_NUMBER`��������

```c
static void number() {
  double value = strtod(parser.previous.start, NULL);
  emitConstant(value);
}
// compiler.c, add after endCompiler()
```

�������ֵ�token�Ѿ������Ѳ��洢��`previous`��.���ǻ�ȡ�ô��ز�ʹ�� C ��׼�⽫��ת��Ϊ˫����ֵ��Ȼ��ʹ�ô˺������ɼ��ظ�ֵ�Ĵ��룺

```c
static void emitConstant(Value value) {
  emitBytes(OP_CONSTANT, makeConstant(value));
}
// compiler.c, add after emitReturn()
```

���ȣ����ǽ�ֵ��ӵ�������Ȼ�󷢳�һ��`OP_CONSTANT`ָ�������ʱ����ѹ���ջ��Ҫ�ڳ������в���һ����Ŀ�������ڣ�

```c
static uint8_t makeConstant(Value value) {
  int constant = addConstant(currentChunk(), value);
  if (constant > UINT8_MAX) {
    error("Too many constants in one chunk.");
    return 0;
  }

  return (uint8_t)constant;
}
// compiler.c, add after emitReturn()
```

�󲿷ֹ���������`addConstant()`������[ǰ����½�](http://craftinginterpreters.com/chunks-of-bytecode.html)�ж���ĵط����⽫����ֵ��ӵ���ĳ������ĩβ���������������º����Ĺ�����Ҫ��ȷ������û��̫�ೣ�������ڸ�`OP_CONSTANT`ָ��ʹ�õ����ֽ���Ϊ���������������ֻ����һ�����д洢�ͼ���256��������

> �ǵģ�������ƺܵ͡��������һ������������ʵ�֣�����ϣ�������һ��ָ�����`OP_CONSTANT_16`�������洢Ϊһ��˫�ֽڲ��������Ա����ǿ�������Ҫʱ������ೣ����
> 
> ֧�����Ĵ��벢�����ر��������ԣ������Ҵ� clox ��ʡ��������������ϣ������ VM ��չ������ĳ���

������Ͼ���ȫ���������һЩ����ʹ��`TOKEN_NUMBER` token���ں���ָ�������в���`number()`��Ȼ������������ڿ��Խ����ֱ���Ϊ�ֽ��롣

### 17.4.2 ����

���ÿ�����ʽ��ֻ��һ��token������ô���������еĽ�������ָ�������ܰ���Ȼ����������ܳ���**���ǣ������ʽ���ض�token��ͷ�����ǳ���ЩΪǰ׺���ʽ��** ���磬������һ�����ʽ���ҵ�ǰtoken��`(`ʱ������֪�����ǿ϶��ڲ鿴һ�������ŵķ�����ʽ��

��ʵ֤�����ǵĺ���ָ������Ҳ���Դ�����Щ�����ʽ���͵Ľ�����������ʹ������Ҫ���κζ���token�������ڳ���ݹ��½���������һ�������������ŵĹ���ԭ��

```c
static void grouping() {
  expression();
  consume(TOKEN_RIGHT_PAREN, "Expect ')' after expression.");
}
// compiler.c, add after endCompiler()
```

ͬ�������Ǽ����ʼֵ`(`�Ѿ������ĵ������ǵݹ�ػص� `expression()`����������֮��ı��ʽ��Ȼ��������Ľ�β`)`��

> Pratt ���������ǵݹ�*�½�*��������������Ȼ�ǵݹ�ġ���������֮�еģ���Ϊ�﷨�����ǵݹ�ġ�

�ͺ�˶��ԣ�������ʽʵ����û���κ����塣����Ψһ�������﷨-��������Ԥ�ھ��нϸ����ȼ��ĵط�����һ���ϵ����ȼ��ı��ʽ����ˣ�������û������ʱ���壬��˲��ᷢ���κ��ֽ��롣�ڲ�����`expression()`����Ϊ�����ڵı��ʽ�����ֽ��롣

### 17.4.3 һԪȡ������

һԪȡ��Ҳ��һ��ǰ׺���ʽ��������Ҳ���������ǵ�ģ�͡�

```c
static void unary() {
  TokenType operatorType = parser.previous.type;

  // Compile the operand.
  expression();

  // Emit the operator instruction.
  switch (operatorType) {
    case TOKEN_MINUS: emitByte(OP_NEGATE); break;
    default: return; // Unreachable.
  }
}
// compiler.c, add after number()
```

ǰ�ߵ�`-`token�ѱ����Ĳ�λ��`parser.previous`.���Ǵ��л�ȡtoken�����Լ�¼�������ڴ�������ĸ�һԪ�����������û�б�Ҫ�����ǵ�������[��һ��](http://craftinginterpreters.com/types-of-values.html)��ʹ����ͬ�ĺ���������`!`�����ʱ���⽫�������塣

��`grouping()`һ�����ݹ����`expression()`�����������֮�����Ƿ����ֽ�����ִ��ȡ��������`-`��������࣬������������֮��д��ȡ��ָ���ƺ��е���֣����밴��ִ��˳���ǣ�

1. ��������������������ֵ���ڶ�ջ�С�

2. Ȼ������pop ֵ��ȡ����Ȼ��push�����

> ����`OP_NEGATE`ָ��Ӧ����󷢳������Ǳ�����������һ���֡������ճ�����Դ�����г��ֵ�˳��������򣬲�������������Ϊִ�з�����˳��
> 
> �ڲ�����֮�󷢳�`OP_NEGATE`ָ��ȷʵ��ζ��д���ֽ���ʱ�ĵ�ǰ��ǲ���`-`token����������޹ؽ�Ҫ����������ʹ�øñ����Ϊ�к������ָ���������
> 
> ����ζ���������һ�����з񶨱��ʽ�����磺
> 
> ```
> print -
>   true
> ```

> ��ô���ڴ�����б�����ʱ��������������ڵ� 2 ����ʾ���󣬼�ʹ��`-`�ڵ� 1 �С����Ƚ��ķ������ڱ��������֮ǰ�洢token���У�Ȼ���䴫�ݸ�`emitByte()`���������ñ��鱣�ּ�.

���ǣ���δ������һ�����⡣�����õ�`expression()`�������������������κα��ʽ�������������ȼ���һ��������Ӷ�Ԫ������������﷨���ǽ������¡����ǣ�

```c
-a.b + c;
```

�����`-`�Ĳ�����Ӧ��ֻ��`a.b`���ʽ������������`a.b + c`.�����`unary()`����`expression()`�����߽�������������ʣ��Ĵ��룬����`+`.�������ؽ� `-`�����ȼ�����`+`�����ȼ�.

�ڽ���һԪ`-`�Ĳ�����ʱ��ֻ��Ҫ���뵱ǰ���ȼ���������ȼ��ı��ʽ���� jlox �ĵݹ��½��������У�ͨ����������ϣ�������������ȼ����ʽ�Ľ����������ڱ�����Ϊ`call()`����ʵ����һ�㡣���ڽ�����ǰ���ȼ����ʽ��ÿ������Ҳ�������κθ������ȼ��ı��ʽ����˰��������ȼ�������ಿ�֡�

clox �еĽ������� �磺`number()`��`unary()` �ǲ�ͬ�ġ�ÿ��ֻ����һ�����͵ı��ʽ������Ҳ���ἶ���԰����������ȼ��ı��ʽ���͡�������Ҫһ����ͬ�Ľ������������������������

```c
static void parsePrecedence(Precedence precedence) {
  // What goes here?
}
// compiler.c, add after unary()
```

�����������һ������ʵ���������ӵ�ǰtoken��ʼ������ǰ���ȼ���������ȼ����κα��ʽ���ڱ�д�˺���֮ǰ�����ǻ���һЩ����������Ҫ��ɣ��������ܲµ�����ʹ����һֱ��̸�۵Ľ�������ָ������ڣ���Ҫ̫����������ι����ġ�Ϊ�˽������ȼ�����Ϊ��������������������������

```c
} Parser;

typedef enum {
  PREC_NONE,
  PREC_ASSIGNMENT,  // =
  PREC_OR,          // or
  PREC_AND,         // and
  PREC_EQUALITY,    // == !=
  PREC_COMPARISON,  // < > <= >=
  PREC_TERM,        // + -
  PREC_FACTOR,      // * /
  PREC_UNARY,       // ! -
  PREC_CALL,        // . ()
  PREC_PRIMARY
} Precedence;

Parser parser;
// compiler.c, add after struct Parser
```

��Щ���� Lox �����ȼ��𣬴ӵ͵������С����� C ��ʽ��Ϊö�ٸ���������������֣�����ζ����`PREC_CALL`����ֵ�ϴ���`PREC_UNARY`.���磬������������ڴ������´���飺

```c
-a.b + c
```

�������`parsePrecedence(PREC_ASSIGNMENT)`����ô���������������ʽ����Ϊ`+`�����ȼ����ڸ�ֵ���෴���������`parsePrecedence(PREC_UNARY)`����������`-a.b`��ͣ��������������ִ�У�`+`��Ϊ�ӷ������ȼ�����һԪ�������

��������������֣����Ժܿ����`expression()`ȱʧ�Ĳ���.

```c
static void expression() {
  parsePrecedence(PREC_ASSIGNMENT);
}
// compiler.c, in expression(), replace 1 line
```

��ʼֻ�ǽ���������ȼ�����Ҳ�������и������ȼ��ı��ʽ�����ڣ�ҪΪһԪ���ʽ�������������������º����������������ʵ��ļ���

```c
  // Compile the operand.
  parsePrecedence(PREC_UNARY);

  // Emit the operator instruction.
// compiler.c, in unary(), replace 1 line 
```

����ʹ��һԪ������Լ����ڵ����ȼ�`PREC_UNARY`������Ƕ�׵�һԪ���ʽ������`!!doubleNegative`.����һԪ����������൱�ߵ����ȼ�����˿�����ȷ�ų���Ԫ�����֮��Ķ�����

> ������˵Ƕ��һԪ���ʽ�� Lox ���ر����á�����������������������������������Ҳ��������

## 17.5 ������׺���ʽ

��Ԫ�������ǰ��ı��ʽ��ͬ����Ϊ��������׺�������������ʽ�����Ǵӵ�һ��token��֪�����ڽ���ʲô��ʹ����׺���ʽ�����ǲ�֪�����Ǵ��ڶ�Ԫ��������м䣬ֱ�������������������Ȼ��żȻ�����м�������token��

��������ӣ�

```
1 + 2
```

������

����������Ŀǰ��֪��������������

1. ��`expression()`�л����`parsePrecedence(PREC_ASSIGNMENT)`.

2. �ú�����һ������ʵ����������ǰ������token��ʶ�������ڽ������֡���������Ȩ����`number()`.

3. `number()`����һ������������һ��`OP_CONSTANT`��Ȼ�󷵻ص�`parsePrecedence()`��

������ô�죿��`parsePrecedence()`�ĵ���Ӧ�����������ӷ����ʽ���������Ҫ��ĳ�ַ�ʽ�������С����˵��ǣ��������ʹ���������Ҫ���ĵط��������Ѿ�������ǰ�����ֱ��ʽ����һ��token��`+`��������`parsePrecedence()`��Ҫ����token�� ���token��ʾ��������������׺���ʽ�м� �� ����ǰ���Ѿ�����ı��ʽʵ����������һ����������

�����������ĺ���ָ�����鲻ֻ���г������������Ը���token��ͷ�ı��ʽ���෴������һ������ָ��*��*����һ�н�ǰ׺������������token������������ڶ��н���׺������������token�����������

����������`TOKEN_PLUS`,`TOKEN_MINUS`,`TOKEN_STAR`, ��`TOKEN_SLASH`����׺�������ĺ�����

```c
static void binary() {
  TokenType operatorType = parser.previous.type;
  ParseRule* rule = getRule(operatorType);
  parsePrecedence((Precedence)(rule->precedence + 1));

  switch (operatorType) {
    case TOKEN_PLUS:          emitByte(OP_ADD); break;
    case TOKEN_MINUS:         emitByte(OP_SUBTRACT); break;
    case TOKEN_STAR:          emitByte(OP_MULTIPLY); break;
    case TOKEN_SLASH:         emitByte(OP_DIVIDE); break;
    default: return; // Unreachable.
  }
}
// compiler.c, add after endCompiler()
```

������ǰ׺����������ʱ��ǰ�ߵ�token�Ѿ���ʹ�á���׺����������ʵ��������ֱ�ӡ���������߲��������ʽ�Ѿ������벢�Һ�������׺����������ġ�

����������ȱ����롣����ζ��������ʱ���������ȱ�ִ�С���������ʱ����������ֵ���ջ�����ڶ�ջ�С���������׺�������Ҫ���ĵط���

Ȼ��`binary()`���������������������������������ȷ�Ĳ�����������`unary()`�������Լ���β��������һ�������������ִ�ж����Ʋ������ֽ���ָ�

����ʱ��VM ����˳��ִ�����Ҳ��������룬�����ǵ�ֵ���ڶ�ջ�С�Ȼ����ִ�в�����ָ���� pop ����ֵ�����������push�����

�˴�����������ע��Ĵ�������һ`getRule()`�С������ǽ����ұߵĲ�����ʱ�������ٴ���Ҫ�������ȼ�����ȡ�����ı�

```
2 * 3 + 4
```

������`*`���ʽ���Ҳ�����ʱ��ֻ��Ҫ����`3`, ������`3 + 4`����Ϊ`+`���ȼ�����`*`�����ǿ���Ϊÿ����Ԫ���������һ�������ĺ�����ÿ���������`parsePrecedence()`��������ȷ�����ȼ���

�����е㷦ζ��ÿ����Ԫ��������Ҳ��������ȼ��������Լ���һ��������+�� / ��˵, PREC_FACTOR/ ���ȼ�����PREC_TERM+���������� ������`getRule()` ��̬�ز��� ��ǰ����(rule)��ʹ�õ�ǰ�������ȼ��ĸ�һ����Ϊ��������`parsePrecedence()`�������Ҳ�������

���������ǿ���`binary()`�����ж�Ԫ�����ʹ�õ�����������ʹ���Ǿ��в�ͬ�����ȼ���

> ���Ƕ��Ҳ�����ʹ��*����*��������ȼ�����Ϊ��Ԫ����������ϵġ�����һϵ��*��ͬ*������������磺
> 
> 1 + 2 + 3 + 4
> 
> ��������������������
> 
> (( 1 + 2 ) + 3 ) + 4
> 
> ��ˣ������Ҳ����������Ϊ��һ��`+`ʱ������ϣ��ʹ��`2`�����������ಿ�֣��������ʹ��`+`����һ�����ȼ������ǣ�������ǵ��������*��*��ϵģ���ô����Ǵ���ġ����ڣ�
> a = b = c = d
> 
> ���ڸ�ֵ���ҽ�ϵģ������뽫�����Ϊ��
> 
> a = ( b = ( c = d ))
> 
> Ϊ�ˣ����ǽ�`parsePrecedence()`ʹ���뵱ǰ�������ͬ�����ȼ����е��á�

## 17.6 Pratt������

�����Ѿ�����˱������Ĵ󲿷֡�����ÿ���﷨����ʽ����һ��������`number()`,`grouping()`,`unary()`, ��`binary()`����Ȼ��Ҫʵ��`parsePrecedence()`, ��`getRule()`������Ҳ֪��������Ҫһ��������һ��token���ͣ��������ҵ�

- �����Ը����͵�token��ͷ��ǰ׺���ʽ�ĺ�����

- ������׺���ʽ�ĺ����������������������͵�token���Լ�

- ʹ�ø�token��Ϊ���������׺���ʽ�����ȼ���

> ���ǲ���Ҫ�����Ը���token��ͷ��*ǰ׺���ʽ�����ȼ�����Ϊ Lox �е�����ǰ׺�������������ͬ�����ȼ���*

�����������԰�װ��һ��С�ṹParseRule�У��ýṹ������������е�һ�С�

```c
} Precedence;

typedef struct {
  ParseFn prefix;
  ParseFn infix;
  Precedence precedence;
} ParseRule;

Parser parser;
// compiler.c, add after enum Precedence
```

�� ParseFn ������һ���򵥵�typedef�����ڲ������κβ����Ҳ������κ����ݵĺ������͡�

C ����ָ�����͵��﷨�ǳ���⣬�����ǽ��������� typedef ���档������﷨�������ͼ����������������ӳʹ�á��Ķ�������������Ϊ����һ��ʧ�ܵ��﷨ʵ�顣

```c
} Precedence;

typedef void (*ParseFn)();

typedef struct {
// compiler.c, add after enum Precedence
```

���������������ı���һ�� ParseRules ���顣����һֱ��̸�����������ῴ������

```c
ParseRule rules[] = {
  [TOKEN_LEFT_PAREN]    = {grouping, NULL,   PREC_NONE},
  [TOKEN_RIGHT_PAREN]   = {NULL,     NULL,   PREC_NONE},
  [TOKEN_LEFT_BRACE]    = {NULL,     NULL,   PREC_NONE}, 
  [TOKEN_RIGHT_BRACE]   = {NULL,     NULL,   PREC_NONE},
  [TOKEN_COMMA]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_DOT]           = {NULL,     NULL,   PREC_NONE},
  [TOKEN_MINUS]         = {unary,    binary, PREC_TERM},
  [TOKEN_PLUS]          = {NULL,     binary, PREC_TERM},
  [TOKEN_SEMICOLON]     = {NULL,     NULL,   PREC_NONE},
  [TOKEN_SLASH]         = {NULL,     binary, PREC_FACTOR},
  [TOKEN_STAR]          = {NULL,     binary, PREC_FACTOR},
  [TOKEN_BANG]          = {NULL,     NULL,   PREC_NONE},
  [TOKEN_BANG_EQUAL]    = {NULL,     NULL,   PREC_NONE},
  [TOKEN_EQUAL]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_EQUAL_EQUAL]   = {NULL,     NULL,   PREC_NONE},
  [TOKEN_GREATER]       = {NULL,     NULL,   PREC_NONE},
  [TOKEN_GREATER_EQUAL] = {NULL,     NULL,   PREC_NONE},
  [TOKEN_LESS]          = {NULL,     NULL,   PREC_NONE},
  [TOKEN_LESS_EQUAL]    = {NULL,     NULL,   PREC_NONE},
  [TOKEN_IDENTIFIER]    = {NULL,     NULL,   PREC_NONE},
  [TOKEN_STRING]        = {NULL,     NULL,   PREC_NONE},
  [TOKEN_NUMBER]        = {number,   NULL,   PREC_NONE},
  [TOKEN_AND]           = {NULL,     NULL,   PREC_NONE},
  [TOKEN_CLASS]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_ELSE]          = {NULL,     NULL,   PREC_NONE},
  [TOKEN_FALSE]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_FOR]           = {NULL,     NULL,   PREC_NONE},
  [TOKEN_FUN]           = {NULL,     NULL,   PREC_NONE},
  [TOKEN_IF]            = {NULL,     NULL,   PREC_NONE},
  [TOKEN_NIL]           = {NULL,     NULL,   PREC_NONE},
  [TOKEN_OR]            = {NULL,     NULL,   PREC_NONE},
  [TOKEN_PRINT]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_RETURN]        = {NULL,     NULL,   PREC_NONE},
  [TOKEN_SUPER]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_THIS]          = {NULL,     NULL,   PREC_NONE},
  [TOKEN_TRUE]          = {NULL,     NULL,   PREC_NONE},
  [TOKEN_VAR]           = {NULL,     NULL,   PREC_NONE},
  [TOKEN_WHILE]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_ERROR]         = {NULL,     NULL,   PREC_NONE},
  [TOKEN_EOF]           = {NULL,     NULL,   PREC_NONE},
};
// compiler.c, add after unary()
```

> �����Ҳ���ÿ����Ҫ����ʱ�����·��ʱ�����˼��������һͷҰ�ޡ�
> 
> �������û�п���`[TOKEN_DOT] =`C ���������е��﷨���Ǿ��� C99 ��ָ����ʼ�������﷨�����ȱ����ֶ��������������������

�����Կ�����ν�`grouping`��`unary`���뵽����token���͵�ǰ׺���������С�����һ���У�`binary`���ӵ��ĸ�������׺���������Щ��׺�����Ҳ�����һ�������������ǵ����ȼ���

������Щ���������ಿ�ֳ�����`NULL`��`PREC_NONE`��������յ�Ԫ������Ϊû������Щ��ǹ����ı��ʽ�����ʽ�����ԣ��ȷ�˵`else`��`}` ��ͷ����ᵼ�·ǳ����ҵ���׺�������

���ǣ����ǻ�û����д�����﷨���ں�����½��У�������µı��ʽ����ʱ����������һЩ������д��������ϲ�����ֽ���������һ��ԭ���ǣ������Ժ����׵ز鿴�﷨����ʹ����Щtoken�Լ���Щtoken���á�

�����������˱�����׼���ñ�дʹ�����Ĵ����ˡ������Pratt �������������õĵط��������׶���ĺ�����`getRule()`.

```c
static ParseRule* getRule(TokenType type) {
  return &rules[type];
}
// compiler.c, add after parsePrecedence()
```

��ֻ�Ƿ��ظ����������Ĺ�������`binary()`�����Բ��ҵ�ǰ����������ȼ����˺��������ڴ��� C �����е�ѭ��������*�ڹ����֮ǰ*`binary()`���壬�Ա�ñ���Դ洢ָ������ָ�롣����ζ��`binary()`���岻��ֱ�ӷ��ʱ�

�෴�����ǽ����Ұ�װ��һ�������С��������ǿ����� `binary()`�Ķ���֮ǰ����ǰ������`getRule()`��Ȼ���ڱ�֮�����*����`getRule()`�� ������ҪһЩ������ǰ���������������ǵ��﷨�ǵݹ����һ��ʵ�����������ǰ����Ƕ�����һ�ߡ�

> ����ʹ����������� PDP-11 �ϱ�������Ա�д VM ʱ���ͻᷢ�����������

```c
 emitReturn();
}

static void expression();
static ParseRule* getRule(TokenType type);
static void parsePrecedence(Precedence precedence);

static void binary() {
// compiler.c, add after endCompiler()
```

��������ڸ����Լ�ʵ�� clox��������ע�����������Щ����Ƭ�η��������Сע�͡��������ģ������Ū���ˣ�C �����������������㡣

### 17.6.1 �������ȼ�

�������ǿ�ʼ������Ȥ�������ˡ�Э�����Ƕ�������н��������Ĵ�ʦ��`parsePrecedence()`.�����Ǵӽ���ǰ׺���ʽ��ʼ��

```c
static void parsePrecedence(Precedence precedence) {
  advance();
  ParseFn prefixRule = getRule(parser.previous.type)->prefix;
  if (prefixRule == NULL) {
    error("Expect expression.");
    return;
  }

  prefixRule();
}
// compiler.c, in parsePrecedence(), replace 1 line
```

���Ƕ�ȡ��һ��token��������Ӧ�� ParseRule�����û��ǰ׺����������tokenһ�����﷨�������Ǳ��沢���ظ������ߡ�

�������ǵ��ø�ǰ׺�����������������������顣��ǰ׺����������ǰ׺���ʽ�����ಿ�֣�ʹ������Ҫ���κ�����token��Ȼ�󷵻ش˴����Դ����ȼ��������ú���׺���ʽ�ͱ����Ȥ�ˡ�ʵ�ַǳ��򵥡�

```c
  prefixRule();

  while (precedence <= getRule(parser.current.type)->precedence) {
    advance();
    ParseFn infixRule = getRule(parser.previous.type)->infix;
    infixRule();
  }
}
// compiler.c, in parsePrecedence() 
```

���������ʵ�֡���ġ����������Ĺ���ԭ�����£� ��`parsePrecedence()`�Ŀ�ͷ�����ҵ�ǰtoken��ǰ׺�����������ݶ��壬��һ��token*����*����ĳ��ǰ׺���ʽ�������ܻ���Ϊһ��������Ƕ����һ��������׺���ʽ�У����ǵ����������Ķ�����ʱ���������ĵ�һ�����ʼ������ǰ׺���ʽ��

����֮�󣬿��ܻ����ĸ����token��ǰ׺���ʽ������ˡ���������Ѱ����һ��token����׺����������������ҵ�һ��������ζ�������Ѿ������ǰ׺���ʽ����������һ����������`parsePrecedence()`�����Ĳ���`precedence`,  ������ȼ�`precedence`������������׺��������ȼ� (�ο����� while (precedence <= getRule(parser.current.type)->precedence) )��

�����һ��token�����ȼ�̫�ͣ����߸���������׺����������������������Ѿ������ܶ�ؽ����˱��ʽ����������ʹ���������������Ȩ�ƽ��������ҵ�����׺������������������Ҫ���κ�����token��ͨ������ȷ�Ĳ������������ص�`parsePrecedence()`.Ȼ��ѭ������������*��һ��* token�Ƿ�Ҳ��һ����Ч����׺������������Խ�����ǰ��ı��ʽ��Ϊ�������������һֱ����ѭ����������׺����������������ֱ����������һ��������׺����������ȼ�̫�͵�token��ֹͣ��

����д�˺ܶ࣬�������������� Vaughan Pratt �ںϲ���ȫ�����㷨�����ڵ���������ִ�н���������Ϊ���ᴦ��һЩ���ʽ��Ҳ��ͼƬ������������ֻ����������������������ؽ�֯��һ��

![](./assets/d7d9b367bf41cbf0264501ffeb1ff702f26235cd.png)

> ��ͷ��![ʵ�ļ�ͷ��](http://craftinginterpreters.com/image/compiling-expressions/calls.png)�������ӵ���ֱ�ӵ��õ���һ��������![һ�����ŵļ�ͷ��](http://craftinginterpreters.com/image/compiling-expressions/points-to.png)��ͷ��ʾ����ָ�����������ָ�롣

�Ժ�������Ҫ���������еĴ���������ֵ�����ǣ�����֮�⣬������д�����ݺ����˱������ಿ�ֵ����б��ʽ�������󡣵�������������͵ı��ʽʱ�����ǽ��ڱ��в������Ľ�������������`parsePrecedence()`�������ġ�

## 17.7  ת����

����д�������ĺ���ʱ������Ӧ�ü���һЩ���ߡ�Ϊ�˰����������ɵ��ֽ��룬���ǽ���ӶԱ�������ɺ�ת�����֧�֡��������ֶ���д��ʱ��������Щʱ����һЩ��ʱ��־��¼���������ǽ�����һЩ��ʵ�Ĵ��룬�Ա����ǿ�����ʱ��������

�����ⲻ����������û��ģ���˽��������ڱ�־���档

```c
#include <stdint.h>

#define DEBUG_PRINT_CODE
#define DEBUG_TRACE_EXECUTION
// common.h
```

����ñ�־��ʹ�����еġ����ԡ�ģ���ӡ������ֽ��롣

```c
  emitReturn();
#ifdef DEBUG_PRINT_CODE
  if (!parser.hadError) {
    disassembleChunk(currentChunk(), "code");
  }
#endif
}
// compiler.c, in endCompiler()
```

ֻ�е�����û�д���ʱ�����ǲŻ����������﷨����󣬱������������У���������һ����ֵ�״̬�����ܻ�����𻵵Ĵ��롣�����޺��ģ���Ϊ�����ᱻִ�У������������ͼ�Ķ���������ֻ��е�����

���Ҫ����`disassembleChunk()`����Ҫ��������ͷ�ļ���

```c
#include "scanner.h"

#ifdef DEBUG_PRINT_CODE
#include "debug.h"
#endif

typedef struct {
// compiler.c
```

���������ˣ�����װ�������� VM �ı����ִ�йܵ��е����һ����Ҫ���֡����ǵĽ�����*����������ô*��������������ɨ�衢������������ֽ��롢ִ�С�

���������������һ�����ʽ�������������һ�ж���ȷ����Ӧ�ü��㲢��ӡ���������������һ����ƹ��ȵ������������������ڽ��������½����кܶ���������Ҫ��ӣ��������Ѿ���λ��

---

## [��ս](http://craftinginterpreters.com/compiling-expressions.html#challenges)

1. Ҫ������������������Ҫ�˽�ִ���߳����ͨ����Ȥ�Ľ�����������`parsePrecedence()`�Լ��洢�ڱ��еĽ����������������������ֵģ����ʽ��
   
   ```
   (- 1 + 2 ) * 3 - - 4
   ```
   
   д����Щ��������α����õġ���ʾ���Ǳ����õ�˳���ĸ������ĸ����Լ����ݸ����ǵĲ�����

2. ParseRule �� `TOKEN_MINUS`�о���ǰ׺����׺����ָ�롣������Ϊ`-`����ǰ׺�������һԪ�񶨣�������׺���������������
   
   �������� Lox �����У�������Щ��ǿ���ͬʱ����ǰ׺����׺λ�ã��� C ����ѡ�������������ô����

3. �������֪�����ӵġ�mixfix�����ʽ���������ϵĲ�������token�ָ���C ����������Ԫ�������`?:`��һ����Ϊ��֪���������
   
   ���������ӶԸ��������֧�֡������������κ��ֽ��룬ֻ��չʾ��ν������ӵ��������������������

## [���˵����ֻ�ǽ���](http://craftinginterpreters.com/compiling-expressions.html#design-note)

��Ҫ����������һЩ��������������Ա��ϲ������������ͬ��Ҳû��ϵ���͸��˶��ԣ���Ӽ�ҳ�޶��ʺ�ģ�����ɵ���������ȣ��Ҵ�ǿ�ұ����Ҳ�ͬ��Ĺ۵���ѧ���˸��ࡣ�ҵ�������*�����޹ؽ�Ҫ*��

�����������������ר�ң�������ѧ�����ר�ң��Ѿ�*����*�����˽��������ǳ��������ǡ�������Ǳ�������Ա������compiler-compilers�� LALR ���������ƵĶ����������ǰ�벿����һ��д���������������漣�ĳ����顣

���������˶����С�����ֻ��һ�Ѵ���ʱ��һ�п����������ӡ��Ķ�ϰ����Ҳ��û�б�������Ա��ô���ԡ�һ������������ڿ�Ѱ����������Ͳ��������������Ĺ���漣����ƺ���Ҫ�����ǵĽ��������ʹ��һ���µ�С���ԡ�

Yacc ������������-�����������������ĵݹ�ʾ�������ۣ���д��������һ������¡���֪���������Ǳ�дһ����������Ϊ���Ǳ�д���ǵı���������

֣���������Ҳ�δ���ƶ����ֲ�ʹ������������

����������ʽ�����Ա��ʼ�о��������������Packrat ���������������͵Ķ�������Ϊ�������ԣ�����������ʽ����Աһ�����⣬����Ҫ���ĵ�һ���¾����ó�һ��Ѹ߽׺�����

����ѧ���㷨�������򣬳�������һֱ���о�֤�����ֽ���������ʱ����ڴ�ʹ�����������������ת��Ϊ�������Ⲣ���أ��Լ������������������ͬ���﷨��

��ĳ�̶ֳ��ϣ���Щ��������Ҫ�����������ʵ��һ�����ԣ�����ҪһЩ��֤��ȷ����Ľ����������ָ�������������Ҳ��Ứ�� 7,000 ���ʱ���������﷨����ֵı�Ե��������������۸�����������ޡ���Ϊһ��������ϰ��ѧϰ��������Ҳ����Ȥ��Ҳ�����ջ�

���ǣ��������Ŀ��ֻ��ʵ��һ�����Բ�����������û���ǰ����ô����������Щ�������޹ؽ�Ҫ�������ױ������ڴ˵����ǵ���������ŭ*��*����Ϊ����ǰ��?*��Ҫ*һЩ�������ɵ������-������-�����Ķ������Ҽ������ǻ��Ѵ���ʱ��ʹ�õ������ŵĿ��������д����д���ǵĽ�������

���ǲ���Ϊ�û������������κμ�ֵ��ʱ�䡣�����ֻ������ɽ���������ѡ��һ�ֱ�׼������ʹ������Ȼ��������ݹ��½���Pratt ���������еĽ��������������� ANTLR �� Bison�����ܺá�

�����ʡ�����Ķ���ʱ�仨�ڲ���д��Ľ��������ϣ����������ڸĽ���ı��������û���ʾ�ı��������Ϣ�ϡ����õĴ�����ͱ�����û���˵����������ǰ��Ͷ��ʱ��ļ����κ��������鶼���м�ֵ��