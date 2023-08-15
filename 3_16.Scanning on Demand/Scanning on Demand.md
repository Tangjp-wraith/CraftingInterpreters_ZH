# ����ɨ��

> ��ѧ����ֻ�ж�ʮ����ע�����š�ʮ�����������ֺʹ�Լ�˸������ŵ�ˮƽ���Ͻ��е��������С�
> 
> -- Kurt Vonnegut,*Like Shaking Hands With God: A Conversation about Writing*

���ǵĵڶ��������� clox ���������׶Ρ���ɨ������������������������ݽṹ����ÿ�Խ׶Ρ�token��ɨ����������������ֽ����ӱ��������� VM�����ǵ�[��](http://craftinginterpreters.com/chunks-of-bytecode.html)��[VM](http://craftinginterpreters.com/a-virtual-machine.html)ʵ�ֽӽ�β�������ڣ�����Ҫ�ص���ʼ������һ������token��ɨ��������[��һ��](http://craftinginterpreters.com/compiling-expressions.html)�У������ֽ��������������������һ��

![](./assets/b7e41281e677a3da44726dc52324ce9a48d0a4ea.png)

�ҳ��ϣ��ⲻ�Ǳ�����������ĵ��½ڡ�����ͬһ�����Ե�����ʵ�֣���Ȼ����һЩ���ࡣ�� jlox ��ɨ������ȣ���ȷʵ������һЩ��Ȥ�Ĳ��졣�����Ķ����˽�������ʲô��

## 16.1 ����������

�����������ڹ���ǰ�ˣ������� clox �������Ľ�����һ�����С���������д���ֽ���顣��ʱ����� REPL �ͽű��ˡ�������Ĵ󲿷ִ���ɾ��������`main()`��

```c
int main(int argc, const char* argv[]) {
  initVM();

  if (argc == 1) {
    repl();
  } else if (argc == 2) {
    runFile(argv[1]);
  } else {
    fprintf(stderr, "Usage: clox [path]\n");
    exit(64);
  }

  freeVM();
  return 0;
}
// main.c, in main(), replace 26 lines
```

����������ִ���ļ������κβ����������� REPL�����������в��������ΪҪ���еĽű���·����

> �������һ�����������������������һ������Ϊ��һ������`argv`ʼ�����������еĿ�ִ���ļ������ơ�

������ҪһЩϵͳͷ�ļ����������롣

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#include "common.h"
// main.c, add to top of file
```

���������������� REPL ������ REPL-ing��

```c
#include "vm.h"

static void repl() {
  char line[1024];
  for (;;) {
    printf("> ");

    if (!fgets(line, sizeof(line), stdin)) {
      printf("\n");
      break;
    }

    interpret(line);
  }
}
// main.c
```

�������� REPL �������ŵش������е����룬����û��Ӳ������г������ơ������ REPL �е���ӣ��������ǵ�Ŀ����˵û���⡣

�����Ĺ���������`interpret()`.���Ǻܿ�ͻ�̸����һ�㣬��������������������ؽű���

```c
static void runFile(const char* path) {
  char* source = readFile(path);
  InterpretResult result = interpret(source);
  free(source); 

  if (result == INTERPRET_COMPILE_ERROR) exit(65);
  if (result == INTERPRET_RUNTIME_ERROR) exit(70);
}
// main.c, add after repl()
```

���Ƕ�ȡ�ļ���ִ�����ɵ� Lox Դ�����ַ�����Ȼ�󣬸��ݽ���������ʵ��������˳����룬��Ϊ������һ˿�����Ĺ��߹����ߣ����ҹ���������Сϸ�ڡ�

���ǻ���Ҫ�ͷ�Դ�����ַ�������Ϊ`readFile()`��̬��������������Ȩ���ݸ����ĵ����ߡ��ú���������ʾ��

> C Ҫ�����ǲ���Ҫ��ʽ�ع����ڴ棬��Ҫ��*������*�����ڴ档���ǳ���Ա�����ס����Ȩ�����������������ֶ�ʵ�����ǡ�Java ֻ��Ϊ����������¡�C++ Ϊ�����ṩ��ֱ�ӶԲ��Խ��б���Ĺ��ߣ��Ա������Ϊ������֤����
> 
> ��ϲ�� C �ļ��ԣ�������Ϊ�˸����������Ĵ��ۡ�����������Ҫ�����Ǹ������档

```c
static char* readFile(const char* path) {
  FILE* file = fopen(path, "rb");

  fseek(file, 0L, SEEK_END);
  size_t fileSize = ftell(file);
  rewind(file);

  char* buffer = (char*)malloc(fileSize + 1);
  size_t bytesRead = fread(buffer, sizeof(char), fileSize, file);
  buffer[bytesRead] = '\0';

  fclose(file);
  return buffer;
}
// main.c, add after repl()
```

������ C ����һ�������ȿ�����Ӧ�ø��������Ŭ���������Ƕ���һ��ר��Ϊ����ϵͳ��Ƶ����ԡ����ѵĲ�����������Ҫ����һ���㹻����ַ�������ȡ�����ļ��������ڶ�ȡ��֮ǰ��֪���ļ��ж��

����Ĵ����ǽ���������ľ��似�ɡ����ļ������ڶ�ȡ��֮ǰ��ʹ��`fseek()`.Ȼ�����`ftell()`�����ߴ��ļ���ʼ�����ж����ֽڡ���Ȼ����һֱѰ�ң�Ѱ�󣩵�����Ǿ��Ǵ�С�����ص���ͷ������һ��size���ַ���������һ�������ж�ȡ�����ļ���

> �ðɣ��Ǹ��ߴ�*��һ*����ԶҪ��סΪ���ֽ��ڳ��ռ䡣

������������ˣ��԰ɣ�����ȫ�ġ��� C ��׼���еĴ��������һ������Щ�������ÿ��ܻ�ʧ�ܡ�������� Java��ʧ�ܽ���Ϊ�쳣�׳����Զ�չ����ջ���������*ʵ����*����Ҫ�������ǡ��� C �У�������ǲ�������ǣ����Ǿͻ�ĬĬ�ر����ԡ�

�Ⲣ����һ���������� C ���ʵ�����飬���������������������������Ǽ������������������кô���������߲˻�ʹ������һ����

���˵��ǣ�����������ϣ����ǲ���Ҫ���κ��ر���������顣���������ȷ��ȡ�û��Ľű������������ľ��Ǹ����û������ŵ��˳������������ȣ����ǿ����޷����ļ���

```c
FILE* file = fopen(path, "rb");
  if (file == NULL) {
    fprintf(stderr, "Could not open file \"%s\".\n", path);
    exit(74);
  }

  fseek(file, 0L, SEEK_END);
// main.c, in readFile() 
```

����ļ������ڻ��û���Ȩ���������ͻᷢ�������������ܳ������������������·����

����ʧ�ܸ�Ϊ������

```c
  char* buffer = (char*)malloc(fileSize + 1);
  if (buffer == NULL) {
    fprintf(stderr, "Not enough memory to read \"%s\".\n", path);
    exit(74);
  }

  size_t bytesRead = fread(buffer, sizeof(char), fileSize, file);
// main.c, in readFile() 
```

��������������ܷ����㹻���ڴ�����ȡ Lox �ű����û����ܻᵣ�ĸ�������⣬������Ӧ�þ����Ŭ������������֪����

��󣬶�ȡ������ܻ�ʧ�ܡ�

```c
 size_t bytesRead = fread(buffer, sizeof(char), fileSize, file);
  if (bytesRead < fileSize) {
    fprintf(stderr, "Could not read file \"%s\".\n", path);
    exit(74);
  }

  buffer[bytesRead] = '\0';
// main.c, in readFile() 
```

��Ҳ��̫���ܡ�ʵ���ϣ��� `fseek()`��`ftell()` �� `rewind()`�ĵ�����������Ҳ���ܻ�ʧ�ܣ������ǲ�Ҫ����̫Զ������

> ��ʹ���Ϻ���`printf()`Ҳ��ʧ�ܡ��ǵġ����������˶��ٴ�*��*����

### 16.1.1 �򿪱���ܵ�

�����Ѿ������Lox Դ���ַ�����������������׼��������һ���ܵ���ɨ�衢�����ִ���������� `interpret()`���������ڣ��ú������оɵ�Ӳ������Կ顣�����ǽ�������Ϊ���ӽ������ջ��Ķ�����

```c
void freeVM();
InterpretResult interpret(const char* source);
void push(Value value);
// vm.h, function interpret(), replace 1 line
```

֮ǰ���Ǵ������ Chunk���������Ǵ������Դ�����ַ����������µ�ʵ�֣�

```c
InterpretResult interpret(const char* source) {
  compile(source);
  return INTERPRET_OK;
}
// vm.c, function interpret(), replace 4 lines
```

�ڱ��������ǲ��ṹ��ʵ�ʵı������������ǿ��Կ�ʼ�������Ľṹ����������һ����ģ���С�

```c
#include "common.h"
#include "compiler.h"
#include "debug.h"
// vm.c
```

���ڣ����е�һ�������������£�

```c
#ifndef clox_compiler_h
#define clox_compiler_h

void compile(const char* source);

#endif
// compiler.h, create new file
```

���ǩ����ı䣬���������Ǽ���ǰ����

����ĵ�һ�׶���ɨ�衪�������ڱ����������������顪���������ڱ����������ľ�����������

```c
#include <stdio.h>

#include "common.h"
#include "compiler.h"
#include "scanner.h"

void compile(const char* source) {
  initScanner(source);
}
// compiler.c, create new file
```

����ȻҲ���ں�����½�����ɡ�

### 16.1.2 ɨ��

�����ǿ�ʼ��д���õĴ���֮ǰ������Ҫ�һЩ���ּܡ����ȣ�һ���µ�ͷ�ļ���

```c
#ifndef clox_scanner_h
#define clox_scanner_h

void initScanner(const char* source);

// #endif
```

�����Ӧ��ʵ�֣�

```c
#include <stdio.h>
#include <string.h>

#include "common.h"
#include "scanner.h"

typedef struct {
  const char* start;
  const char* current;
  int line;
} Scanner;

Scanner scanner;
// scanner.c, create new file
```

��ɨ������ϸ����û���Դ����ʱ��������������˶�Զ������� VM ����������������**״̬��װ��һ���ṹ��**��Ȼ�󴴽������͵ĵ�������ģ������������Ͳ��ؽ������ݸ����в�ͬ�ĺ�����

���˾��ȵ��ǣ�ֻ�к����ֶΡ�`start`ָ���ǵ�ǰ����ɨ��Ĵ��صĿ�ʼ����ָ��`current`���ڲ鿴�ĵ�ǰ�ַ���

![](./assets/a995a4037368993f7c527378e54901c256ae2b18.png)

> �������������ɨ���ʶ��`bacon`����ǰ�ַ���`o`������������ѵ��ַ���`c`��

��һ��`line`�ֶ������ٵ�ǰ��λ����һ���Խ��д��󱨸档��������������������ָ��Դ�����ַ�����ͷ��ָ�롣ɨ����ͨ������һ�Σ�Ȼ����ɡ�

��Ϊ��һЩ״̬������Ӧ�ó�ʼ������

```c
void initScanner(const char* source) {
  scanner.start = source;
  scanner.current = source;
  scanner.line = 1;
}
// scanner.c, add after variable scanner
```

���Ǵӵ�һ�еĵ�һ���ַ���ʼ������һ�������������ϵ��ܲ��ߡ�

## 16.2 һ��һ��token

�� jlox �У�������ǹ����ʱ��ɨ��������һ�������е�ɨ���������򣬷���һ��token list����� clox ��˵��һ����ս��������Ҫĳ�ֿ�������������б����洢token��������Ҫ���������ͷ�token�Լ����ϱ������Ǻܶ���룬���˷Ѻܶ��ڴ档

���κ�ʱ�򣬱�����ֻ��Ҫһ����token������ס���ǵ��﷨ֻ��Ҫһ��ǰհ��ǡ�������*����*����Ҫͬʱ�������ǡ��෴����򵥵Ľ���������ڱ�������Ҫ֮ǰ��ɨ��token����ɨ�����ṩһ��ʱ������ֵ����token��������Ҫ��̬�����κζ�������������ֻ�� C ��ջ�ϴ���token��

���ҵ��ǣ����ǻ�û��һ��������ɨ����ѯ��token�ı����������ɨ��������������ʲô��������Ϊ�����������ж������ǽ���дһЩ��ʱ��������������

```c
initScanner(source);
  int line = -1;
  for (;;) {
    Token token = scanToken();
    if (token.line != line) {
      printf("%4d ", token.line);
      line = token.line;
    } else {
      printf("   | ");
    }
    printf("%2d '%.*s'\n", token.type, token.length, token.start); 

    if (token.type == TOKEN_EOF) break;
  }
}
// compiler.c  in compile()
```

> ��ʽ�ַ����е��Ǹ�`%.*s`��һ������Ĺ��ܡ�ͨ����������ͨ���ڸ�ʽ�ַ����з���һ������������������ȡ���Ҫ��ʾ���ַ�����ʹ��`*`���� ����������������Ϊ�������ݡ���˸�`printf()`���û��� ����ӡ`token.length`�ַ����ĵ�һ���ַ�`token.start`��������Ҫ�������Ƴ��ȣ���Ϊ����ָ��ԭʼԴ�ַ�������ĩβû����ֹ����

��������ѭ����ÿ����һ��ѭ�������ͻ�ɨ��һ��token����ӡ��������������һ������ġ��ļ���β����ǻ����ʱ������ֹͣ�����磬���������������������н�������

```js
print 1 + 2;
```

����ӡ������

```
   1 31 'print'
   | 21 '1'
   |  7 '+'
   | 21 '2'
   |  8 ';'
   2 39 '' 
```

��һ�����кţ��ڶ����Ǳ�����͵���ֵ������Ǵ��ء��� 2 �е����һ���մ����� EOF ��ǡ�

> �ǵģ�token���͵�ԭʼ����������ȫ������ɶ��ģ������� C �����ǵ�ȫ����

����ʣ�ಿ�ֵ�Ŀ����ͨ��ʵ������ؼ�������ʹ����鹤����

```c
void initScanner(const char* source);
Token scanToken();

#endif
// scanner.h, add after initScanner()
```

ÿ�ε��ö���ɨ�貢����Դ�����е���һ��token��token��������������

```c
#define clox_scanner_h

typedef struct {
  TokenType type;
  const char* start;
  int length;
  int line;
} Token;

void initScanner(const char* source);
// scanner.h
```

���� jlox �� Token ��ǳ����ơ�������һ��ö������ʶ����ʲô���͵�token�������֡���ʶ����`+`������ȡ�ö��ʵ������ jlox �е���ͬ�������������ö��������顣

```c
#ifndef clox_scanner_h
#define clox_scanner_h

typedef enum {
  // Single-character tokens.
  TOKEN_LEFT_PAREN, TOKEN_RIGHT_PAREN,
  TOKEN_LEFT_BRACE, TOKEN_RIGHT_BRACE,
  TOKEN_COMMA, TOKEN_DOT, TOKEN_MINUS, TOKEN_PLUS,
  TOKEN_SEMICOLON, TOKEN_SLASH, TOKEN_STAR,
  // One or two character tokens.
  TOKEN_BANG, TOKEN_BANG_EQUAL,
  TOKEN_EQUAL, TOKEN_EQUAL_EQUAL,
  TOKEN_GREATER, TOKEN_GREATER_EQUAL,
  TOKEN_LESS, TOKEN_LESS_EQUAL,
  // Literals.
  TOKEN_IDENTIFIER, TOKEN_STRING, TOKEN_NUMBER,
  // Keywords.
  TOKEN_AND, TOKEN_CLASS, TOKEN_ELSE, TOKEN_FALSE,
  TOKEN_FOR, TOKEN_FUN, TOKEN_IF, TOKEN_NIL, TOKEN_OR,
  TOKEN_PRINT, TOKEN_RETURN, TOKEN_SUPER, TOKEN_THIS,
  TOKEN_TRUE, TOKEN_VAR, TOKEN_WHILE,

  TOKEN_ERROR, TOKEN_EOF
} TokenType;

typedef struct {
// scanner.h
```

����Ϊ�������Ƽ���ǰ׺`TOKEN_`����Ϊ C �ڶ������ƿռ����׳�ö�����ƣ�֮�⣬Ψһ�������Ƕ����`TOKEN_ERROR`���͡�����ʲô��

��ɨ�������ֻ���⵽��������δ��ֹ���ַ������޷�ʶ����ַ����� jlox �У�ɨ���������б�����Щ��Ϣ���� clox �У�ɨ����Ϊ�ô�������һ���ϳɵġ����󡱱�ǣ������䴫�ݸ�����������������������֪�������˴��󣬲��ҿ����ڱ������֮ǰ��������ָ���

clox �� Token ���͵���ӱ֮����������α�ʾ���ء��� jlox �У�ÿ�� Token �����ش洢Ϊ�Լ�������С Java �ַ������������Ϊ clox �����������Ǿͱ���Ū�����ι�����Щ�ַ������ڴ档���������ѣ���Ϊ���ǰ�ֵ����token�������token����ָ��ͬһ�������ַ���������Ȩ�����֡�

���ԣ�����ʹ��ԭʼԴ�ַ�����Ϊ�ַ��洢��������һ��ָ�����ĵ�һ���ַ���ָ������������ַ�������ʾһ�����ء�����ζ�����Ǹ�������Ҫ���Ĺ�����ص��ڴ棬���ǿ������ɵظ���token��ֻҪ��ҪԴ�����ַ���������token������һ��������

> �Ҳ����Ե����ʡ�����ȷʵ��Ҫ���ǲ�ȷ��Զ�ڡ�����ģ���д�����Դ�ַ��������㹻�����������ڡ������Ϊʲô��`interpret()`��ɴ���ִ�в�����`runFile()`֮ǰ���ͷ��ַ��� ��ԭ��

### 16.2.1 ɨ��tokens

������׼����ɨ��һЩtoken�����ǽ�Ŭ�����������ʵ�֣������￪ʼ��

```c
Token scanToken() {
  scanner.start = scanner.current;

  if (isAtEnd()) return makeToken(TOKEN_EOF);

  return errorToken("Unexpected character.");
}
// scanner.c, add after initScanner()
```

����ÿ�ε��ô˺�������ɨ��һ��������token����˵�����ú���ʱ������֪������������һ����token�Ŀ�ͷ����ˣ�����`scanner.start`Ϊָ��ǰ�ַ����Ա��ס��Ҫɨ��Ĵ��ش����￪ʼ��

Ȼ�����Ƿ��Ѿ�����Դ�����ĩβ�����������������һ�� EOF token��ֹͣ������һ��tokenֵ���������������ֹͣ�������token���źš�

������ǻ�û���ߵ�������ǻ���һЩ...����...ɨ����һ��token�����ǻ�û�б�д�ô��롣���ܿ�ͻ�̸����һ�㡣����ô���δ�ɹ�ɨ�貢����token����ô�����ﺯ����ĩβ����һ����ζ�Ŵ���ɨ�����޷�ʶ����ַ��������Ϊ�˷���һ�������ǡ�

������������ڸ��麯�������д󲿷ֶ��� jlox ��Ϥ�ġ���һ��

```c
static bool isAtEnd() {
  return *scanner.current == '\0';
}
// scanner.c, add after initScanner()
```

����Ҫ��Դ�ַ��������õ��� null ��β�� C �ַ����������ǰ�ַ��ǿ��ֽڣ���ô�Ѿ����˽�β��

Ҫ����token����������ƹ��캯���ĺ�����

```c
static Token makeToken(TokenType type) {
  Token token;
  token.type = type;
  token.start = scanner.start;
  token.length = (int)(scanner.current - scanner.start);
  token.line = scanner.line;
  return token;
}
// scanner.c, add after isAtEnd()
```

��ʹ��ɨ����`start`��`current`ָ��������token�Ĵ�λ���������˼����������Ե��ֶΣ�Ȼ�󷵻�token������һ�����ڷ��ش����ǵ���ú�����

```c
static Token errorToken(const char* message) {
  Token token;
  token.type = TOKEN_ERROR;
  token.start = message;
  token.length = (int)strlen(message);
  token.line = scanner.line;
  return token;
}
// scanner.c, add after makeToken()
```

> ���µ���һ���ַǳ������������һ��������Ƭ��

![](./assets/21df9fe54e7c330ecda0282b3f71363bff1f7f3a.png)

Ψһ�������ǡ�lexeme��ָ�������Ϣ�ַ���������ָ���û���Դ���롣ͬ����������Ҫȷ��������Ϣͣ����ʱ���㹻�����Ա�������ܹ���ȡ����ʵ���ϣ�����ֻʹ�� C �ַ������ֵ��ô˺�������Щ�ǲ���ĺ�����ģ����Ի��á�

����������ӵ�еĻ�������һ�����ھ��пմʷ��﷨�����ԵĹ���ɨ�����������ķ�û�в���ʽ������ÿ���ַ�����һ�������Ⲣ����һ����Ȥ�ı�����ԣ�������������д����

## 16.3 Lox �Ĵʷ�

��򵥵ı��ֻ��һ���ַ�������������ʶ�����ǣ�

```c
if (isAtEnd()) return makeToken(TOKEN_EOF);

  char c = advance();

  switch (c) {
    case '(': return makeToken(TOKEN_LEFT_PAREN);
    case ')': return makeToken(TOKEN_RIGHT_PAREN);
    case '{': return makeToken(TOKEN_LEFT_BRACE);
    case '}': return makeToken(TOKEN_RIGHT_BRACE);
    case ';': return makeToken(TOKEN_SEMICOLON);
    case ',': return makeToken(TOKEN_COMMA);
    case '.': return makeToken(TOKEN_DOT);
    case '-': return makeToken(TOKEN_MINUS);
    case '+': return makeToken(TOKEN_PLUS);
    case '/': return makeToken(TOKEN_SLASH);
    case '*': return makeToken(TOKEN_STAR);
  }

  return errorToken("Unexpected character.");
// scanner.c, in scanToken()  
```

���Ǵ�Դ�����ж�ȡ��һ���ַ���Ȼ��ֱ���ж�(switch to)�Բ鿴���Ƿ��� Lox ���κε��ַ���λƥ�䡣Ϊ�˶�ȡ��һ���ַ���ʹ����һ���µĸ��������������ĵ�ǰ�ַ������䷵�ء�

```c
static char advance() {
  scanner.current++;
  return scanner.current[-1];
}
// scanner.c, add after isAtEnd()
```

�������������ַ��ı����ţ�����`!=`��`>=`������ÿһ������һ����Ӧ�ĵ��ַ�token������ζ�ŵ����ǿ�����`!` �������ַ�ʱ���ڲ鿴��һ���ַ�֮ǰ�����ǲ�֪������`!`����л����� a`!=`�С�������������

```c
    case '*': return makeToken(TOKEN_STAR);
    case '!':
      return makeToken(
          match('=') ? TOKEN_BANG_EQUAL : TOKEN_BANG);
    case '=':
      return makeToken(
          match('=') ? TOKEN_EQUAL_EQUAL : TOKEN_EQUAL);
    case '<':
      return makeToken(
          match('=') ? TOKEN_LESS_EQUAL : TOKEN_LESS);
    case '>':
      return makeToken(
          match('=') ? TOKEN_GREATER_EQUAL : TOKEN_GREATER);
  }
// scanner.c, in scanToken()
```

ʹ�����һ���ַ�������Ѱ��һ��`=`.����ҵ������ǽ�ʹ������������Ӧ��˫�ַ�token���������ǲ���ᵱǰ�ַ�����������Գ�Ϊ*��һ��* token��һ���֣��������ʵ��ĵ��ַ�token��

��������ʹ�õڶ����ַ����߼������

```c
static bool match(char expected) {
  if (isAtEnd()) return false;
  if (*scanner.current != expected) return false;
  scanner.current++;
  return true;
}
// scanner.c, add after advance()
```

�����ǰ�ַ���������ַ�������ǰ��������`true`���������Ƿ���`false`�Ա�������ƥ�䡣

�������ǵ�ɨ����֧�����еı����š��ڿ�ʼ���۸������ַ�֮ǰ����������΢�˽�һ�¸���������token���ַ���

### 16.3.1 �ո�

���ǵ�ɨ������Ҫ����ո��Ʊ���ͻ��з�������Щ�ַ������Ϊ�κ�token���ص�һ���֡������� `scanToken()`�� ��switch�����Ҫ�ַ�����Ҫȷ����������������ʱ������ȷ�ҵ��ո�����һ��token�����е㼬�֡����Ǳ��뽫���������������װ��һ��ѭ�������������С�

�෴��������token֮ǰ�����Ƿ�����һ�������ĺ�����

```c
Token scanToken() {
  skipWhitespace();
  scanner.start = scanner.current;
// scanner.c, in scanToken()
```

���ʹɨ����ǰ�����κ�ǰ���հס��˵��÷��غ�����֪����һ���ַ���������ģ�����������Դ�����ĩβ����

```c
static void skipWhitespace() {
  for (;;) {
    char c = peek();
    switch (c) {
      case ' ':
      case '\r':
      case '\t':
        advance();
        break;
      default:
        return;
    }
  }
}
// scanner.c, add after errorToken()
```

����һ�ֶ���������ɨ��������ѭ����������������ÿ���հ��ַ���������Ҫע����*����*�����κ�*��*�հ��ַ���Ϊ��֧����һ�㣬����ʹ�������

```c
static char peek() {
  return *scanner.current;
}
// scanner.c, add after advance()
```

��ֻ�Ƿ��ص�ǰ�ַ�������ʹ������ǰ��Ĵ��봦������з�֮������пհ��ַ���

```c
        break;
      case '\n':
        scanner.line++;
        advance();
        break;
      default:
        return;
// scanner.c, in skipWhitespace()
```

������ʹ������һ��ʱ��Ҳ�����ӵ�ǰ�кš�

### 16.3.2 ע��

ע���ڼ����ϲ��ǡ��հס���������������������ȫ׼ȷ������ Lox ���ԣ�����Ҳ�����ǣ�����Ҳ�������ǡ�

```c
         break;
      case '/':
        if (peekNext() == '/') {
          // A comment goes until the end of the line.
          while (peek() != '\n' && !isAtEnd()) advance();
        } else {
          return;
        }
        break;
      default:
        return;
// scanner.c, in skipWhitespace()
```

`//`ע����Lox��ͷ�����Ժ�`!=`   һ������Ҫ�ڶ����ַ� lookahead�����ǣ�����`!=`����ʹû���ҵ� `!`��������Ȼ������`=`��ע�Ͳ�ͬ���������û���ҵ��ڶ���`/`����ô`skipWhitespace()`Ҳ����Ҫ���ĵ�*һ��*б�� `/`��

Ϊ�˴���������⣬������ӣ�

```c
static char peekNext() {
  if (isAtEnd()) return '\0';
  return scanner.current[1];
}
// scanner.c, add after peek()
```

�����`peek()`�����ڵ�ǰ�ַ�֮���һ���ַ��������ǰ�ַ�����һ���ַ�����`/`�����ǻ�ʹ�����ǣ�Ȼ�����κ������ַ���ֱ����һ�����з���Դ���������

������`peek()`����黻�з�����ʹ���������������з�(newline) ����Ϊ`skipWhitespace()`��һ����ѭ���ĵ�ǰ�ַ������ǽ�ʶ����������`scanner.line`��

### 16.3.3 ����ֵ Literal token

���ֺ��ַ���token������ģ���Ϊ���Ǿ�����֮����������ʱֵ�����ǽ����ַ�����ʼ����Ϊ���Ǻ�����ʶ�𡪡�����������˫���ſ�ͷ��

```c
    match('=') ? TOKEN_GREATER_EQUAL : TOKEN_GREATER);
    case '"': return string();
  }
// scanner.c, in scanToken()
```

�������һ���º�����

```c
static Token string() {
  while (peek() != '"' && !isAtEnd()) {
    if (peek() == '\n') scanner.line++;
    advance();
  }

  if (isAtEnd()) return errorToken("Unterminated string.");

  // The closing quote.
  advance();
  return makeToken(TOKEN_STRING);
}
// scanner.c, add after skipWhitespace()
```

�� jlox ���ƣ����������ַ�ֱ������������������ַ��������еĻ��з�����Lox ֧�ֶ����ַ����������ң�һ��������������ҵ���������֮ǰ���ŵش�����Դ������������⡣

clox �е���Ҫ�仯��*��*���ڵġ�ͬ���������ڴ�����йء��� jlox �У�Token ����һ�� Object ���͵��ֶ����洢�����ֱ�ǵĴ�λת������������ʱֵ��

�� C ��ʵ������Ҫ����������������Ҫĳ������(union)�����ͱ�����ж�token�Ƿ�����ַ�����˫����ֵ���������һ���ַ�����������Ҫ��ĳ�ַ�ʽ�����ַ������ַ�������ڴ档

����û�н����ָ�������ӵ�ɨ�����У������Ƴٵ��Ժ����ִ���ת��Ϊ����ʱֵ���� clox �У�token���洢��λ�������û�Դ�����г��ֵ��ַ�������ȫ��ͬ���Ժ��ڱ������У�������׼���ý����洢�ڿ�ĳ�������ʱ���Ὣ�������ת��Ϊ����ʱֵ��

�ڱ������н��д��ص�ֵ��ת��ȷʵ������һЩ���ࡣɨ���������ֵĹ����뽫�����ַ�����ת��Ϊ����ֵ����Ĺ����ǳ����ơ�����û��*��ô*������࣬��������û���κ�Ӱ�죬��ʹ���ǵ�ɨ���Ǹ��򵥡�

�����������֡����������ﴦ�����ǣ�������Ϊ���Կ�ʼ���ֵ�ʮ�������е�ÿһ�����һ�� switch case��

```c
  char c = advance();
  if (isDigit(c)) return number();

  switch (c) {
// scanner.c, in scanToken() 
```

��ʹ����������ߺ�����

```c
static bool isDigit(char c) {
  return c >= '0' && c <= '9';
}
// scanner.c, add after initScanner()
```

����ʹ��������ɨ�����֣�

```c
static Token number() {
  while (isDigit(peek())) advance();

  // Look for a fractional part.
  if (peek() == '.' && isDigit(peekNext())) {
    // Consume the ".".
    advance();

    while (isDigit(peek())) advance();
  }

  return makeToken(TOKEN_NUMBER);
}
// scanner.c, add after skipWhitespace()
```

��ʵ������ jlox �İ汾��ͬ��ֻ�ǻ�û�н���λת��Ϊ˫���ȡ�

## 16.4 ��ʶ���͹ؼ���

���һ��token�Ǳ�ʶ���������û�����ĺͱ����ġ��ⲿ��Ӧ�ú���Ȥ���������� clox ��ʶ��ؼ��ֵķ�ʽ�������� jlox �еķ�ʽ��ȫ��ͬ�������漰һЩ��Ҫ�����ݽṹ��

���������ȣ����Ǳ���ɨ���λ����������ĸ���»��߿�ͷ��

```c
  char c = advance();
  if (isAlpha(c)) return identifier();
  if (isDigit(c)) return number();
// scanner.c, in scanToken() 
```

ʹ�ô˷���ʶ����Щ��

```c
static bool isAlpha(char c) {
  return (c >= 'a' && c <= 'z') ||
         (c >= 'A' && c <= 'Z') ||
          c == '_';
}
// scanner.c, add after initScanner()
```

�ҵ���ʶ�������ǽ��ڴ˴�ɨ�����ಿ�֣�

```c
static Token identifier() {
  while (isAlpha(peek()) || isDigit(peek())) advance();
  return makeToken(identifierType());
}
// scanner.c, add after skipWhitespace()
```

�ڵ�һ����ĸ֮������Ҳ����ʹ�����֣���������һֱʹ����ĸ���֣�ֱ������Ϊֹ��Ȼ����������һ��������ȷ���͵�token��ȷ�������ʵġ������Ǳ��µĶ��ز��֡�

```c
static TokenType identifierType() {
  return TOKEN_IDENTIFIER;
}
// scanner.c, add after skipWhitespace()
```

�ðɣ������⻹���Ǻ������˷ܡ�������Ǹ���û�б����֣�������������ӡ�����Ӧ�����ʶ��ؼ��֣���jlox�У����ǰ����Ƕ�����һ��Java Map�У������ֲ��ҡ������� clox ��û���κ����͵Ĺ�ϣ��ṹ����������û�С�

������Σ���ϣ��̫�����ˡ�Ҫ�ڹ�ϣ���в���һ���ַ�����������Ҫ�������ַ����������ϣ�룬�ڹ�ϣ�����ҵ���Ӧ��Ͱ��Ȼ���ǡ���ҵ����κ��ַ�������ַ�������ȱȽ�.

> ������Դ˲���Ϥ���벻Ҫ���ġ�������[��ͷ��ʼ���������Լ��Ĺ�ϣ��ʱ](http://craftinginterpreters.com/hash-tables.html)�����ǽ���ϸ�˽������������ݡ�

��������ɨ���˱�ʶ����gorgonzola����������Ҫ�����ٹ��������ж����Ƿ��Ǳ����֣��ðɣ�û�� Lox �ؼ����ԡ�g����ͷ����˲鿴��һ���ַ���������ȷ�ش𡰷񡱡���ȹ�ϣ�����Ҫ�򵥵öࡣ

��cardigan���أ������� Lox ��ȷʵ��һ���ԡ�c����ͷ�Ĺؼ��֣���class��������cardigan���еĵڶ����ַ���a���ų�����һ�㡣��forest���أ���Ϊ��for����һ���ؼ��֣��������ַ������ߵø�Զ����ȷ������û�б����֡����ǣ��ڴ��������£���һ�����ַ������Ը�������������һ���û���������ơ�����Ӧ���ܹ���ʶ����һ�㲢����ʧ�ܡ�

���Ƿ�֧�ַ�����߼��Ŀ��ӻ���ʾ��

![](./assets/17c9fc74ef537478333abcd097db5b0371155944.png)

�����Ķ�ÿ���ڵ��������ῴ�� Lox �Ĺؼ��ֳ��֡�

���ǴӸ��ڵ㿪ʼ�������һ���ӽڵ����ĸ������еĵ�һ���ַ�ƥ�䣬���Ǿ��ƶ����Ǹ��ڵ㡣Ȼ���ظ���λ�е���һ����ĸ���������ơ�������κ�ʱ������е���һ����ĸ���ӽڵ㲻ƥ�䣬��ô��ʶ��һ�����ǹؼ��ֲ���ֹͣ��������ǵ���һ��˫�߿򣬲��������ڴ��ص����һ���ַ�������ô�����ҵ���һ���ؼ��֡�

### 16.4.1 Tires��״̬��

�����ͼ��һ������[**trie**](https://en.wikipedia.org/wiki/Trie)�Ķ��������ӡ�trie �洢һ���ַ�����������������ڴ洢�ַ��������ݽṹ������ԭʼ�ַ����飬Ȼ�����ǰ�װ��һЩ����Ľṹ�У��԰���������ؽ��������������ǲ�ͬ�ġ��� trie �����Ҳ����������ַ�����

> ��Trie���� CS �������׻���������֮һ��Edward Fredkin �����ӵ��ʡ�retrieval�����м�����������ζ����Ӧ����tree��һ�����������ǣ������Ѿ���һ���ǳ���Ҫ�����ݽṹ����Ϊ��tree��?*������ ������*�����Գ�����Ӳ�����˵����Щ����������û��֪������˵�ĸ�����ˣ��������Ǿ����������ɡ�try��������ͷʹ��

�෴��trie����������ÿ���ַ�������ʾΪͨ���ַ��ڵ�����*·��*������������ı�����ʾ��ƥ���ַ��������һ���ַ��Ľڵ���һ������ı�ǡ���ͼ�е�˫�߿�������������� trie ������banquet���͡�ban�����������ж�����*����*��banque��������e���ڵ㲻���иñ�ǣ�����n���� ��t���ڵ�ᡣ

Tries��һ�ָ����������ݽṹ��������[**ȷ���������Զ���**](https://en.wikipedia.org/wiki/Deterministic_finite_automaton)(**DFA**)�������ܻ�֪�����ǵ��������ƣ�**����״̬��**������Ϊ**״̬��**��״̬���ܰ������������ڴ�[��Ϸ���](http://gameprogrammingpatterns.com/state.html)��ʵʩ����Э������з��涼���á�

�� DFA �У�����һ��*״̬*������֮����*ת��*���γ�һ��ͼ�����κ�ʱ��㣬������ǡ�á����ڡ�һ��״̬����ͨ������ת����������״̬������ʹ�� DFA ���дʷ�����ʱ��ÿ��ת�����Ǵ��ַ�����ƥ���һ���ַ���ÿ��״̬����һ��������ַ���

���ǵĹؼ���������ʶ�� Lox �ؼ��ֵ� DFA���� DFA �ȼ�����ǿ����Ϊ���ǿ���������*ͼ*��ת��������״̬֮���γ�ѭ������ʹ������ʶ�����ⳤ���ַ��������磬����һ��ʶ���������ֵ� DFA��

![ʶ�������͸������ֵ��﷨ͼ��](http://craftinginterpreters.com/image/scanning-on-demand/numbers.png)

> ����ͼ����ʽ��Ϊ[**�䷨ͼ**](https://en.wikipedia.org/wiki/Syntax_diagram)�����������**��·ͼ**����һ����������Ϊ���������е���𳵵ĵ�������
> 
> �� Backus-Naur ��ʽ����֮ǰ�����Ǽ�¼�����﷨����Ҫ��ʽ֮һ��?���������Ҫʹ���ı�������������*ͼ���**�ı�����*�Ĺٷ��淶��һЩ�������ĵط���

���ѽ�ʮλ���ֵĽڵ��۵���һ����ʹ����߿ɶ��ԣ���������������ͬ�ġ�����ͨ��·��������ÿ�����ڴ�����ʹ����Ӧ���ַ�ʱ����ڵ㡣�������Ը�⣬���ǿ��Թ���һ���޴�� DFA��Ϊ Lox ����*����*�ʷ�����������һ��ʶ���³�������Ҫ�����б�ǵĵ�һ״̬����

Ȼ�����ֹ��������� DFA��������ս�ԡ�����Ǵ���[Lex](https://en.wikipedia.org/wiki/Lex_(software))��ԭ�������һ��������Ĵʷ��﷨�ļ��ı���������һ��������ʽ���������Զ�Ϊ������һ�� DFA��������һ��ʵ������ C ���롣

> ��Ҳ�Ǵ����������Ժ��ı��༭���е�������ʽ�����ڵײ�Ĺ�����ʽ�����ǻ�ȡ����������ʽ�ַ���������ת��Ϊ DFA��Ȼ��ʹ������ƥ���ַ�����
> 
> �������ѧϰ������ʽת��ΪDFA���㷨��[����](https://en.wikipedia.org/wiki/Compilers:_Principles,_Techniques,_and_Tools)���С�

���ǲ���������·�������Ѿ�����һ���������õ���ҡɨ����������ֻ��Ҫһ��С�� trie ��ʶ��ؼ��֡�����Ӧ����ν���ӳ�䵽���룿

������򵥵Ľ��������Ϊÿ���ڵ�ʹ��һ�� switch ��䣬ÿ����֧���� case�����ǽ��Ӹ��ڵ㿪ʼ������򵥵Ĺؼ��֡�

> �򵥲�����ζ���޴���ͬ���ķ���[�����Ͼ��� V8 ����](https://github.com/v8/v8/blob/e77eebfe3b747fb315bd3baad09bec0953e53e68/src/parsing/scanner.cc#L1643)�ģ�����Ŀǰ��������ӡ���������ʵ��֮һ��

```c
static TokenType identifierType() {
  switch (scanner.start[0]) {
    case 'a': return checkKeyword(1, 2, "nd", TOKEN_AND);
    case 'c': return checkKeyword(1, 4, "lass", TOKEN_CLASS);
    case 'e': return checkKeyword(1, 3, "lse", TOKEN_ELSE);
    case 'i': return checkKeyword(1, 1, "f", TOKEN_IF);
    case 'n': return checkKeyword(1, 2, "il", TOKEN_NIL);
    case 'o': return checkKeyword(1, 1, "r", TOKEN_OR);
    case 'p': return checkKeyword(1, 4, "rint", TOKEN_PRINT);
    case 'r': return checkKeyword(1, 5, "eturn", TOKEN_RETURN);
    case 's': return checkKeyword(1, 4, "uper", TOKEN_SUPER);
    case 'v': return checkKeyword(1, 2, "ar", TOKEN_VAR);
    case 'w': return checkKeyword(1, 4, "hile", TOKEN_WHILE);
  }

  return TOKEN_IDENTIFIER;
// scanner.c, in identifierType()
```

��Щ�Ƕ�Ӧ�ڵ����ؼ��ֵĳ�ʼ��ĸ��������ǿ���һ����s������ʶ��Ψһ���ܵĹؼ�����`super`�������������ܲ��ǣ�����������Ȼ��Ҫ����������ĸ��������ͼ�У�������Ͼ��������ڡ�s���ϵ�ֱ��·����

���ǲ���Ϊÿ���ڵ�����жϡ��෴��������һ��ʵ�ú���������Ǳ�ڹؼ��ִ�λ�����ಿ�֡�

```c
static TokenType checkKeyword(int start, int length,
    const char* rest, TokenType type) {
  if (scanner.current - scanner.start == start + length &&
      memcmp(scanner.start + start, rest, length) == 0) {
    return type;
  }

  return TOKEN_IDENTIFIER;
}
// scanner.c, add after skipWhitespace()
```

���ǽ����������е������޷�֧·����һ�������ҵ�һ��ֻ����һ�����ܵı����ֵ�ǰ׺��������Ҫ��֤�����¡����ر�����ؼ���һ�����������һ����ĸ�ǡ�s������λ��Ȼ�����ǡ�sup����superb���������ַ�������ȫƥ�䡪����supar���������á�

���ȷʵ����ȷ�������ַ���������������Ҫ���ַ�����ô������һ���ؼ��֣����ع�����token���͡�������������һ����ͨ��ʶ����

�����м����ؼ��֣����ڵ�һ����ĸ���ٴη�֧�������λ�ԡ�f����ͷ����������`false`,`for`, ��`fun`����������Ϊ�ӡ�f���ڵ�����ķ�֧�����һ��switch��

```c
    case 'e': return checkKeyword(1, 3, "lse", TOKEN_ELSE);
    case 'f':
      if (scanner.current - scanner.start > 1) {
        switch (scanner.start[1]) {
          case 'a': return checkKeyword(2, 3, "lse", TOKEN_FALSE);
          case 'o': return checkKeyword(2, 1, "r", TOKEN_FOR);
          case 'u': return checkKeyword(2, 1, "n", TOKEN_FUN);
        }
      }
      break;
    case 'i': return checkKeyword(1, 1, "f", TOKEN_IF);
// scanner.c, in identifierType()
```

������switch֮ǰ��������Ҫ����Ƿ���*�ڶ�*����ĸ���Ͼ�����f������Ҳ��һ����Ч�ı�ʶ������֧����һ����ĸ�ǡ�t����

```c
    case 's': return checkKeyword(1, 4, "uper", TOKEN_SUPER);
    case 't':
      if (scanner.current - scanner.start > 1) {
        switch (scanner.start[1]) {
          case 'h': return checkKeyword(2, 2, "is", TOKEN_THIS);
          case 'r': return checkKeyword(2, 2, "ue", TOKEN_TRUE);
        }
      }
      break;
    case 'v': return checkKeyword(1, 2, "ar", TOKEN_VAR);
// scanner.c, in identifierType()
```

��������������Ƕ��`switch`��䡣��δ��벻���̣������ٶȷǳ��ǳ��졣����ɼ��ؼ�����������ٹ�����������֪����ʶ�����Ǳ�����ʶ��ʱ�����˳���

���������ǵ�ɨ����������ˡ�

> ������ʱ������Ϊ���������ڸ��ӵ����ݽṹ���������������ڵ��Ż������ǣ��ܶ�ʱ������Ҫ��ֻ�������ٵĹ����������Ҿ������ֱ�д�����ܼ򵥵Ĵ��������ʵ����һ�㡣

---

## [��ս](http://craftinginterpreters.com/scanning-on-demand.html#challenges)

1. �����µ�����֧��[**�ַ�����ֵ**](https://en.wikipedia.org/wiki/String_interpolation)�����ַ��������У�����һЩ����ķָ����������`${`�����ڿ�ͷ��`}`��β������Щ�����֮�䣬���Գ����κα��ʽ��ִ���ַ�������ʱ���������ڲ����ʽ������ת��Ϊ�ַ�����Ȼ������Χ���ַ������ֺϲ���
   
   ���磬��� Lox ֧���ַ�����ֵ����ô���...
   
   ```c
   var drink = "Tea";
   var steep = 4;
   var cool = 2;
   print "${drink} will be ready in ${steep + cool} minutes.";
   ```
   
   ���ӡ��
   
   ```c
   Tea will be ready in 6 minutes.
   ```
   
   ��ᶨ��ʲôtoken������ʵ���ַ�����ֵɨ����������������ַ������֣���ᷢ��ʲô����token���У�
   
   ��ᷢ��ʲô����token��
   
   ```c
   "Nested ${"interpolation?! Are you ${"mad?!"}"}"
   ```
   
   ���ǲ鿴����֧�ֲ�ֵ������ʵ�֣�������������δ������ġ�

2. һЩ����ʹ�ü����ű�ʾ���ͣ����һ���һ��`>>`������������⵼���� C++ ���ڰ汾�е�һ���������⣺
   
   ```c
   vector<vector<string>> nestedVectors;
   ```
   
   ���������������Ϊ`>>`���ʷ���Ϊ�������Ʊ�ǣ�����������`>`��ǡ��û�����ͨ�����Ҽ�����֮�����һ���ո����������������
   
   ���߰汾�� C++ �����ܣ����Դ����������롣Java �� C# ����û��������⡣��Щ�������ָ����ʵ����һ�㣿

3. ������ԣ����������䷢չ���ڣ������ˡ������Ĺؼ��֡�����Щ��ʶ����ĳЩ�������г䵱�����֣����������������п�������ͨ���û������ʶ����
   
   ���磬�� C# ���Ƿ���`await`�ڲ��Ĺؼ���`async`���������������У�������`await`�����Լ��ı�ʶ����
   
   ˵��һЩ�����������Ե������Ĺؼ��֣��Լ�����������������ġ�ʹ�������Ĺؼ��ֵ���ȱ����ʲô�������Ҫ������������������Ե�ǰ��ʵ�����ǣ�