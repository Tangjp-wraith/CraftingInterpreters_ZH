# ȫ�ֱ���

> Ҫ����һ�ַ������Է�����ͺ��ˣ�������ζ������Զ������ɫ��Ҳ��Զ����¾ɡ�Ȼ�󣬵�������Ҫ����ʱ�򣬿��Դ�ƿ�ӵ����ӣ����������������һ��һ����
> 
> -- Daphne du Maurier,?*Rebecca*

[��һ��](http://craftinginterpreters.com/hash-tables.html)�Ƕ�һ���Ӵ����롢�����ļ������ѧ���ݽṹ�ĳ���̽���������ۺ͸��������һЩ���ڴ� O ���ź��㷨�����ۡ����µ�ѧ�����۽��١�û�д���뷨����ѧϰ���෴������һЩ�򵥵Ĺ�������һ��������ǣ����ǵ��������֧�ֱ�����

ʵ���ϣ���ֻ֧��*ȫ��*�������ֲ���������[��һ��](http://craftinginterpreters.com/local-variables.html)�������� jlox �У������跨�����Ƕ�������һ���½��У���Ϊ���Ƕ����б���ʹ������ͬ��ʵ�ּ��������ǹ�����һϵ�л�����ÿ��������һ����һֱ������������һ��ѧϰ��ι���״̬�ļ򵥡��ɾ��ķ�����

����Ҳ*����*��ÿ������һ��������һ������ʱ����һ���µĹ�ϣ������ͨ������ VM �ĵ�·�������ж��ٴ�����ʹ�ñ����йأ��������������һ�ж������������ clox�����ǽ�ͨ���Ծֲ�����ʹ�ø���Ч�Ĳ������Ľ�������ȫ�ֱ���������ô�����Ż���

> ���Ǹ�������ʵ���еĳ���Ԫ���ԡ�ͨ������ͬ���������Ի��ж���ʵ�ּ�����ÿ�ּ�������Բ�ͬ��ʹ��ģʽ�����˵��������磬JavaScript �����ͨ�����Ը���ر�ʾ������ʵ���Ķ��󣬶�������������Լ����Ը����ɵ��޸ġ�C �� C++ ������ͨ���ж��ַ��������� case �������� case ֵ���ܼ��̶�������`switch`��䡣

���ٻع�һ�� Lox ���壺Lox �е�ȫ�ֱ����ǡ����ڰ󶨡��ģ�����˵�Ƕ�̬�����ġ�����ζ���������ڶ���ȫ�ֱ���֮ǰ����һ������ȫ�ֱ����Ĵ��롣ֻҪ���벻�ڶ��巢��֮ǰִ�У�һ�ж��ܺá�ʵ���ϣ�����ζ���������ں����������ú���ı�����

```js
fun showVariable() {
  print global;
}

var global = "after";
showVariable();
```

�������Ĵ�����ܿ���������֣��������ڶ����໥�ݹ麯���ܷ��㡣�������Ը��õ��� REPL ���ʹ�á���������һ���б�дһ��С������Ȼ����������һ����ʹ�õı�����

�ֲ������Ĺ�����ʽ��ͬ�����ھֲ�����������*����*��ʹ��֮ǰ��������� VM �����ڱ���ʱ�������ǣ���ʹ�ڼ򵥵ĵ����������Ҳ����ˡ��⽫ʹ�����ܹ�Ϊ�ֲ�����ʹ�ø����ܵı�ʾ����������һ�µ����ˡ����ڣ�������ֻ����ȫ�ֱ�����

## 21.1 ��� statements

������ʹ�ñ������������ģ�����ζ������Ҳ��ʱ��Ϊ��������Ӷ�����֧���ˡ���������ǵõĻ���Lox ������Ϊ���ࡣ��������䡱�ǽ������ư󶨵�ֵ����Щ��䡣�������͵���䡪������������ӡ�ȡ������Ϊ����䡱�����ǲ�����ֱ���ڿ��������ʹ��������䣬������ʾ��

```c
if (monday) var croissant = "yes"; // Error.
```

��������������Χ������Χ��������������⡣��ˣ�����������һ��������ͨ��Ϊ����������������*��*����Ӽ����õ������﷨���������﷨�Ͻ�ֹ����

```c
statement      �� exprStmt
               | forStmt
               | ifStmt
               | printStmt
               | returnStmt
               | whileStmt
               | block ;
```

Ȼ�������ڽű��Ķ���Ϳ���ʹ�õ����Ĺ���

```c
declaration    �� classDecl
               | funDecl
               | varDecl
               | statement ;
```

��`declaration`��������������Ƶ���䣬������`statement`��������������͡�����`block` �� `statement`�У�������ͨ��������Ƕ���ڿ�������������������������С�

> ��������е������ŶԱ��ʽ�����á������������������ȼ�������������ֻ���������ȼ������������ĵط���

�ڱ����У����ǽ�ֻ���ܼ�������һ��������

```
statement      �� exprStmt
               | printStmt ;

declaration    �� varDecl
               | statement ;
```

��ĿǰΪֹ�����ǵ� VM ����������Ϊ�������ʽ����Ϊ�������ǿ��Խ����ͱ����ȫ�����ݡ��������� Lox ʵ���У�������һϵ����������������׼��֧������

```c
  advance();

  while (!match(TOKEN_EOF)) {
    declaration();
  }

  endCompiler();
// compiler.c, in compile(), replace 2 lines 
```

����һֱ�ڱ�������(declaration)��ֱ������Դ�ļ���ĩβ������ʹ���������һ��������

```c
static void declaration() {
  statement();
}
// compiler.c, add after expression()
```

���ǽ��ڱ��º�����ܱ����������������ڣ�����ֻ��ת��`statement()`.

```c
static void statement() {
  if (match(TOKEN_PRINT)) {
    printStatement();
  }
}
// compiler.c, add after declaration()
```

����԰��������������������԰���������䡣����ζ���������������ս��ǵݹ�ġ��������ڲ���д��ǰ��������

```c
static void expression();
static void statement();
static void declaration();
static ParseRule* getRule(TokenType type);
// compiler.c, add after expression()
```

### 21.1.1 ��ӡ��� print statements

�ڱ������������������Ҫ֧�֡������Ǵ�`print`��俪ʼ������Ȼ�أ������`print`token��ͷ������ʹ���������������⣺

```c
static bool match(TokenType type) {
  if (!check(type)) return false;
  advance();
  return true;
}
// compiler.c, add after consume()
```

�����ܻ�� jlox ���ϳ����������ǰtoken���и������ͣ�����ʹ��token������`true`���������ǲ����token������`false`��������������Ǹ�����һ����������ʵ�ֵģ�

> һ·�������Ǹ��麯����

```c
static bool check(TokenType type) {
  return parser.current.type == type;
}
// compiler.c, add after consume()
```

�����ǰtoken���и������ͣ����`check()`��������`true`��������װ��һ���������ƺ��е�ɵ���������Ժ������ʹ��������������Ϊ�������Ķ̶�����������ʹ�������������Ķ���

> ��������΢������������ڷ�������Ե���д����������൱�Ӵ󡣵�������ǧ�д���ʱ�������б��һ�в�ʹ������׶���ʵ�ú���������Ӯ�����ı�����

�������ȷʵƥ����`print`���ƣ���ô���ǽ��ڴ˴������������ಿ�֣�

```c
static void printStatement() {
  expression();
  consume(TOKEN_SEMICOLON, "Expect ';' after value.");
  emitByte(OP_PRINT);
}
// compiler.c, add after expression()
```

`print`��������ʽ����ӡ���������������Ƚ���������ñ��ʽ���﷨����֮����Ҫһ���ֺţ���������ʹ��������󣬷���һ����ָ������ӡ�����

```c
  OP_NEGATE,
  OP_PRINT,
  OP_RETURN,
// chunk.h, in enum OpCode
```

������ʱ��������ִ������ָ�

```c
        break;
      case OP_PRINT: {
        printValue(pop());
        printf("\n");
        break;
      }
      case OP_RETURN: {
// vm.c, in run()
```

����������������ָ��ʱ�����Ѿ�ִ���˱��ʽ�Ĵ��룬�����ֵ����ջ�����������Ǽ򵥵�pop����ӡ����

��ע�⣬����֮�����ǲ���push�κ��������ݡ����� VM �б��ʽ(expressions)�����(statements)֮���һ���ؼ�����ÿ���ֽ���ָ�������ָ������޸Ķ�ջ��**��ջЧӦ**�����磬`OP_ADD`��������ֵ��ѹ��һ��ֵ��ʹ��ջ����ǰСһ��Ԫ�ء�

> ��ִ��`OP_ADD`��ջ�� ֮ǰ��һ��Ԫ�أ���������Ч���� -1��

![](./assets/d3901a1efaaa51b85003c6014474c40a20b035b7.png)

�����Զ�һϵ��ָ��Ķ�ջЧ������Ի�����ǵ���Ч����������Ӵ��κ��������ʽ(expression)�����һϵ��ָ��Ķ�ջЧ��ʱ�����ܼ�Ϊһ����ÿ�����ʽ(expression)�ڶ�ջ������һ�����ֵ��

�������(statement)���ֽ�����ܶ�ջЧӦΪ�㡣�������(statement)�������κ�ֵ����������ձ��ֶ�ջ���䣬��������ִ�в���ʱ��Ȼ��ʹ�ö�ջ�������Ҫ��**��Ϊ�����ǿ�ʼ�������̺�ѭ��ʱ��������ܻ�ִ��һ������䡣���ÿ������������С��ջ�������տ��ܻ����������**��

�������ڽ�����ѭ����ʱ������Ӧ��ɾ��һЩ����

```c
      case OP_RETURN: {
        // Exit interpreter.
        return INTERPRET_OK;
// vm.c, in run(), replace 2 lines
```

�� VM ֻ����������������ʽʱ��������һЩ��ʱ����`OP_RETURN`�����ֵ����������������� `print`�����ǲ�����Ҫ���ˡ������� clox ������ʵ���ֽ���һ����

> ����������ֻ��һ�������ǻ�����Ӻ���ʱ�ٴη���`OP_RETURN`�����ڣ����˳�������������ѭ����

������һ������ָ����Ҫ���������֧�֡�

```c
      return simpleInstruction("OP_NEGATE", offset);
    case OP_PRINT:
      return simpleInstruction("OP_PRINT", offset);
    case OP_RETURN:
// debug.c, in disassembleInstruction()
```

�������ǵ�`print`��䡣�����Ը�⣬��һ�ԣ�

```c
print 1 + 2;
print 3 * 4;
```

�������ģ��ðɣ�Ҳ���������˷ܣ����������ڿ��Թ������������������Ľű�����о����ǽ�����

### 21.1.2 ���ʽ��� expression-statements

�ȵ��㿴����һ����䡣�������*û��*����`print`�ؼ��֣���ô����һ�����ڲ鿴���ʽ��䡣

```c
    printStatement();
  } else {
    expressionStatement();
  }
// compiler.c, in statement()
```

�������������ģ�

```c
static void expressionStatement() {
  expression();
  consume(TOKEN_SEMICOLON, "Expect ';' after expression.");
  emitByte(OP_POP);
}
// compiler.c, add after expression()
```

�����ʽ��䡱ֻ��һ�����ʽ���һ���ֺš�������������Ҫ�����������б�д���ʽ�ķ�ʽ��ͨ����������Ϳ��Ե��ú�������㸳ֵ�ĸ����ã�������ʾ��

```c
brunch = "quiche";
eat(brunch);
```

�������Ͻ������ʽ��������ʽ�����������������ֱ�ӶԸ���Ϊ���б��롣��������ʽ��Ȼ�󷢳�һ��`OP_POP`ָ�

```c
  OP_FALSE,
  OP_POP,
  OP_EQUAL,
// chunk.h, in enum OpCode
```

����˼�壬��ָ�ջ��ֵ��������������

```c
      case OP_FALSE: push(BOOL_VAL(false)); break;
      case OP_POP: pop(); break;
      case OP_EQUAL: {
// vm.c, in run()
```

����Ҳ���Է���������

```c
      return simpleInstruction("OP_FALSE", offset);
    case OP_POP:
      return simpleInstruction("OP_POP", offset);
    case OP_EQUAL:
// debug.c, in disassembleInstruction()
```

���ʽ��仹���Ǻ����ã���Ϊ���ǲ��ܴ����κ��и����õı��ʽ�����ǵ�����[�Ժ���Ӻ���](http://craftinginterpreters.com/calls-and-functions.html)ʱ���ǽ��Ǳز����ٵġ���ʵ��������еĴ������䣬�� C ���ԣ����Ǳ��ʽ��䡣

> ����ͳ�ƣ��ڱ���ĩβ�ġ�compiler.c���汾�У�149 ��������� 80 ���Ǳ��ʽ��䡣

### 21.1.3 ����ͬ��

�������ڱ���������������������ʱ�����ǿ��Խ��������©[�˼���](http://craftinginterpreters.com/compiling-expressions.html#handling-syntax-errors)�����⡣�� jlox һ����clox ʹ�ý���ģʽ����ָ�����С��������ļ������������������������ڵ���ͬ����ʱ�˳��ֻ�ģʽ������ Lox������ѡ�����߽���Ϊ�Ǹ��㡣��������������䣬���ǿ���ʵ��ͬ���ˡ�

```c
  statement();

  if (parser.panicMode) synchronize();
}
// compiler.c, in declaration()
```

��������ڽ���ǰ������ʱ��������������Ǿͻ����ֻ�ģʽ���������������ʱ��������֮�����ǿ�ʼͬ����

```c
static void synchronize() {
  parser.panicMode = false;

  while (parser.current.type != TOKEN_EOF) {
    if (parser.previous.type == TOKEN_SEMICOLON) return;
    switch (parser.current.type) {
      case TOKEN_CLASS:
      case TOKEN_FUN:
      case TOKEN_VAR:
      case TOKEN_FOR:
      case TOKEN_IF:
      case TOKEN_WHILE:
      case TOKEN_PRINT:
      case TOKEN_RETURN:
        return;

      default:
        ; // Do nothing.
    }

    advance();
  }
}
// compiler.c, add after printStatement()
```

���ǲ������������token��ֱ�����ǵ��￴���������߽�Ķ���������ͨ��Ѱ�ҿ��Խ�������ǰ��token��ʶ��߽磬����ֺš��������ǽ����ҿ�ʼ���ĺ���token��ͨ���ǿ������������ؼ���֮һ��

## 21.2 ��������

�����ܹ�*��ӡ*������ʹ��������ڱ�����Բ�������Ӯ���κν�����������Ǽ�����һЩ�����Ĳ��������飬�ñ���������ȥ����Ҫ֧�����ֲ�����

> ���̲�ס����һЩ���� 4H ֮��ġ����Բ����ᡱ��һ�������ŵ��ݵ�̯λ�ϰ�����Ӥ�����ԣ�*����*��*�С�*

- ʹ��`var`�������һ���±�����
- ʹ�ñ�ʶ�����ʽ���ʱ�����ֵ��
- ʹ�ø�ֵ���ʽ����ֵ�洢�����б����С�

��������һЩ����֮ǰ����������������е��κ�һ�������Դ�������ʼ��

```c
static void declaration() {
  if (match(TOKEN_VAR)) {
    varDeclaration();
  } else {
    statement();
  }

  if (parser.panicMode) synchronize();
// compiler.c, in declaration(), replace 1 line
```

����Ϊ�����﷨�����ճ���ռλ���������������Ѿ�����ʵ�ʵ�ʵ�֡����ƥ��һ��`var` token�����������

```c
static void varDeclaration() {
  uint8_t global = parseVariable("Expect variable name.");

  if (match(TOKEN_EQUAL)) {
    expression();
  } else {
    emitByte(OP_NIL);
  }
  consume(TOKEN_SEMICOLON,
          "Expect ';' after variable declaration.");

  defineVariable(global);
}
// compiler.c, add after expression()
```

�ؼ��ֺ����������������`parseVariable()`����ģ������Ժ�ὲ����Ȼ��Ѱ��һ��`=`���һ����ʼ�����ʽ������û�û�г�ʼ����������������ͨ������`OP_NIL`ָ����ʽ�ؽ����ʼ��Ϊ`nil`���������ַ�ʽ�����Ƕ�ϣ������ԷֺŽ�����

> �����ϣ��������Ա��������������Ǵ������磺
> 
> var a;
> 
> ʵ������
> 
> var a = nil;
> 
> ǰ����Ϊ�������ɵĴ�����ͬ

�����������º������ڴ�������ͱ�ʶ�������ǵ�һ����

```c
static void parsePrecedence(Precedence precedence);

static uint8_t parseVariable(const char* errorMessage) {
  consume(TOKEN_IDENTIFIER, errorMessage);
  return identifierConstant(&parser.previous);
}
// compiler.c, add after parsePrecedence()
```

��Ҫ����һ��token��һ����ʶ�����������ﱻ���ĺ�ת���±ߣ�

```c
static void parsePrecedence(Precedence precedence);

static uint8_t identifierConstant(Token* name) {
  return makeConstant(OBJ_VAL(copyString(name->start,
                                         name->length)));
}
// compiler.c, add after parsePrecedence()
```

**�ú�����ȡ������token�����������Ϊ�ַ�����ӵ���ĳ������С�Ȼ�������س������иó���������**��

ȫ�ֱ���������ʱ*������*���ҡ�����ζ�� VM���ֽ��������ѭ������Ҫ���ʸ����ơ������ַ���̫����޷���Ϊ��������䵽�ֽ������С��෴�����ǽ��ַ����洢�ڳ������У�Ȼ��ָ��ͨ�����е��������ø����ơ�

�˺������ظ����� һֱ��`varDeclaration()`���Ժ����ƽ����˴���

```c
static void defineVariable(uint8_t global) {
  emitBytes(OP_DEFINE_GLOBAL, global);
}
// compiler.c, add after parseVariable()
```

�⽫��������±������洢���ʼֵ���ֽ���ָ��������б������Ƶ�������ָ��Ĳ�����������ڶ�ջ�� VM һ����������󷢳�����ָ�������ʱ����������ִ�б�����ʼ�����Ĵ��롣��Ὣֵ���ڶ�ջ�ϡ�Ȼ���ָ���ȡ��ֵ������洢�����Ա����á�

> ��֪������һЩ�������ڿ������������塣���ǣ�����������Ӹ������ڴ������Ƶ����Թ��ܣ����ǽ����л�ø���ĺô����������������������±����������͸�ֵ���ʽ�������ǡ�

������ʱ�����Ǵ�������ָ�ʼ��

```c
  OP_POP,
  OP_DEFINE_GLOBAL,
  OP_EQUAL,
// chunk.h, in enum OpCode
```

��������ǵ���Ӧ�ֵĹ�ϣ��ʵ�����������ѡ�

```c
      case OP_POP: pop(); break;
      case OP_DEFINE_GLOBAL: {
        ObjString* name = READ_STRING();
        tableSet(&vm.globals, name, peek(0));
        pop();
        break;
      }
      case OP_EQUAL: {
// vm.c, in run()
```

���Ǵӳ������л�ȡ���������ơ�Ȼ�����Ǵ�ջ��ȡ��ֵ�������洢��һ���Ը�����Ϊ���Ĺ�ϣ���С�

> ��ע�⣬�ڽ�ֵ��ӵ���ϣ��֮ǰ��*���ǲ���*������ֵ������ڽ�����ӵ���ϣ��Ĺ����д����������գ����ȷ�� VM ��Ȼ�����ҵ���ֵ������һ�����ԵĿ����ԣ���Ϊ��ϣ���ڵ�����Сʱ��Ҫ��̬���䡣

�˴��벻�����Ƿ����ڱ��С�Lox ��ȫ�ֱ����ǳ����ɣ������������¶������Ƕ������������ REPL �Ự�к����ã���������ǡ���Ѿ��ڹ�ϣ���У��� VM ͨ���򵥵ظ��Ǹ�ֵ��֧������

������һ��С���ֺ꣺

```c
#define READ_CONSTANT() (vm.chunk->constants.values[READ_BYTE()])
#define READ_STRING() AS_STRING(READ_CONSTANT())
#define BINARY_OP(valueType, op) \
// vm.c, in run()
```

�����ֽ�����ж�ȡһ�����ֽڲ���������������Ϊ�鳣�����е������������ظ����������ַ�������������ֵ*�Ƿ�*Ϊ�ַ���������ֻ�ǲ��������ǿ��ת���������ǰ�ȫ�ģ���Ϊ��������Զ���ᷢ�����÷��ַ���������ָ�

��Ϊ���ǹ��Ĵʷ����࣬��������Ҳ�ڽ��ͺ�����ĩβȡ����������ꡣ

```c
#undef READ_CONSTANT
#undef READ_STRING
#undef BINARY_OP
// vm.c, in run()
```

��һֱ��˵����ϣ������ʵ�������ǻ�û�С���Ҫһ���ط����洢��Щȫ�ֱ�������Ϊϣ�������� clox ����ʱһֱ���ڣ��������ǽ�����ֱ�Ӵ洢�� VM �С�

```c
  Value* stackTop;
  Table globals;
  Table strings;
// vm.h, in struct VM
```

������ַ�������������������Ҫ�� VM ����ʱ����ϣ���ʼ��Ϊ��Ч״̬��

```c
  vm.objects = NULL;

  initTable(&vm.globals);
  initTable(&vm.strings);
// vm.c, in initVM()
```

���˳�ʱ��������ͷŵ���

�ý��̽����˳�ʱ�ͷ��������ݣ���Ҫ�����ϵͳ�������ǵ���̯�Ӹо��ܲ����档

```c
void freeVM() {
  freeTable(&vm.globals);
  freeTable(&vm.strings);
// vm.c, in freeVM()
```

������һ��������Ҳϣ���ܹ��������ָ�

```c
      return simpleInstruction("OP_POP", offset);
    case OP_DEFINE_GLOBAL:
      return constantInstruction("OP_DEFINE_GLOBAL", chunk,
                                 offset);
    case OP_EQUAL:
// debug.c, in disassembleInstruction()
```

�������Ϳ��Զ���ȫ�ֱ�����������˵�û�����*˵*�����Ѿ��������ˣ���Ϊ����ʵ�����޷�*ʹ��*���ǡ���ô���������������⡣

## 21.3 ��ȡ����

�����������б������һ��������ʹ�ñ��������Ʒ��ʱ�����ֵ�������ｫ��ʶ��token ���ӵ����ʽ��������

```c
  [TOKEN_LESS_EQUAL]    = {NULL,     binary, PREC_COMPARISON},
  [TOKEN_IDENTIFIER]    = {variable, NULL,   PREC_NONE},
  [TOKEN_STRING]        = {string,   NULL,   PREC_NONE},
// compiler.c, replace 1 line
```

�����������µĽ�����������

```c
static void variable() {
  namedVariable(parser.previous);
}
// compiler.c, add after string()
```

������һ�����м���С�ĸ����������ڿ������������壬���ں�����½��л��ø����á��ұ�֤��

```c
static void namedVariable(Token name) {
  uint8_t arg = identifierConstant(&name);
  emitBytes(OP_GET_GLOBAL, arg);
}
// compiler.c, add after string()
```

������֮ǰ����ͬ`identifierConstant()`��������ȡ�����ı�ʶ����ǲ����������Ϊ�ַ�����ӵ���ĳ������С�ʣ�µľ��Ƿ���һ��ָ��ø����Ƽ���ȫ�ֱ���������˵����

```c
  OP_POP,
  OP_GET_GLOBAL,
  OP_DEFINE_GLOBAL,
// chunk.h, in enum OpCode
```

�ڽ������У�ʵ��ͬ`OP_DEFINE_GLOBAL`�෴.

```c
      case OP_POP: pop(); break;
      case OP_GET_GLOBAL: {
        ObjString* name = READ_STRING();
        Value value;
        if (!tableGet(&vm.globals, name, &value)) {
          runtimeError("Undefined variable '%s'.", name->chars);
          return INTERPRET_RUNTIME_ERROR;
        }
        push(value);
        break;
      }
      case OP_DEFINE_GLOBAL: {
// vm.c, in run()
```

���Ǵ�ָ��Ĳ���������ȡ��������������ȡ��������Ȼ������ʹ������Ϊ����ȫ�ֹ�ϣ���в��ұ�����ֵ��

�����ϣ���в����ڼ�������ζ�Ŵ�δ�����ȫ�ֱ��������� Lox �е�����ʱ����������Ǳ��������ڷ����������ʱ�˳�������ѭ������������ȡֵ������ѹ���ջ��

```c
      return simpleInstruction("OP_POP", offset);
    case OP_GET_GLOBAL:
      return constantInstruction("OP_GET_GLOBAL", chunk, offset);
    case OP_DEFINE_GLOBAL:
debug.c, in disassembleInstruction()
```

һ��㷴�����룬���Ǿ�����ˡ����ǵĽ��������ڿ������������Ĵ��룺

```c
var beverage = "cafe au lait";
var breakfast = "beignets with " + beverage;
print breakfast;
```

ֻʣ��һ������ˡ�

## 21.4 ��ֵassignment

�ڱ����У���һֱ��ͼ��������һ���൱��ȫ�����׵ĵ�·���Ҳ��ر����ѵ�*����*�����Ҿ�������*�������*��ñ�������Ҫ�ĸ����ӡ����ǣ��ֽ���������е��������ѡ�� ʹ��ֵ����ʵ�֡�

> ��������ǵõĻ�����ֵ�� jlox �зǳ��򵥡�

�ֽ��� VM ʹ�õ��̱�����������û���κ��м� AST ������¼�ʱ�����������ֽ��롣һ����ʶ���һ���﷨�����ͻ�Ϊ���������롣��ܲ��ʺϸ�ֵ���ʽ�����ǣ�

```c
menu.brunch(sunday).beverage = "mimosa";
```

�ڴ˴����У�������`menu.brunch(sunday).beverage`ֱ������`=`��һ��֮������token�����ʶ���Ǹ�ֵ��Ŀ���������ͨ���ʽ`menu`������ʱ���������Ѿ�Ϊ�������鷢�����ֽ��롣

���������Ⲣ����������ô���¡�������������ο���������ӣ�

![](./assets/081c8ed28f8734759bd2ba3b93796a609eaddaac.png)

���ܸ�`.beverage`���ֲ��ñ���Ϊ get ���ʽ����`.`������������ݶ���һ�����ʽ�����������ı��ʽ���塣��`menu.brunch(sunday)`���ֿ���������һ�������ִ�С�

��������˵���˵��ǣ���ֵ����Ψһ������������token�����Ҷˣ�������`=`. ��ʹ setter �Ľ����߿��������ⳤ�ı��ʽ������Ϊ�� get ���ʽ��ͬ�Ĳ���ֻ��β���ʶ����������`=`֮ǰ.���ǲ���Ҫ̫���ǰհ�ԾͿ�����ʶ��`beverage`Ӧ�ñ�����Ϊһ��set���ʽ������һ�� getter��

���������ף���Ϊ����ֻ��`=` ǰ�ߵı�ʶ�� �� ����ʵ�ַ����ǣ����� ����������ֵĿ��ı��ʽ֮ǰ������Ѱ�Һ���`=`  token��������ǿ���һ�������ǽ������Ϊ��ֵ�� setter �����Ǳ������ʻ� getter��

���ǻ�����Ҫ����setter������������Ҫ�����ֻ�Ǳ�����

```c
  uint8_t arg = identifierConstant(&name);

  if (match(TOKEN_EQUAL)) {
    expression();
    emitBytes(OP_SET_GLOBAL, arg);
  } else {
    emitBytes(OP_GET_GLOBAL, arg);
  }
}
// compiler.c, in namedVariable(), replace 1 line
```

�ڱ�ʶ�����ʽ�Ľ��������У������ڱ�ʶ������Ѱ�ҵȺš�����ҵ�һ�������Ƿ����������ʵĴ��룬���� ������Ҫ����ֵȻ�󷢳���ֵָ�

����������Ҫ�ڱ�������ӵ����һ��ָ�

```c
  OP_DEFINE_GLOBAL,
  OP_SET_GLOBAL,
  OP_EQUAL,
// chunk.h, in enum OpCode
```

�������ϣ���������ʱ��Ϊ�����ڶ���һ���±�����

```c
      }
      case OP_SET_GLOBAL: {
        ObjString* name = READ_STRING();
        if (tableSet(&vm.globals, name, peek(0))) {
          tableDelete(&vm.globals, name); 
          runtimeError("Undefined variable '%s'.", name->chars);
          return INTERPRET_RUNTIME_ERROR;
        }
        break;
      }
      case OP_EQUAL: {
// vm.c, in run()
```

��Ҫ�������ڵ�����������ȫ�ֹ�ϣ����ʱ�ᷢ��ʲô�������δ����ñ��������Ը�ֵ�������������ʱ����Lox[������ʽ��������](http://craftinginterpreters.com/statements-and-state.html#design-note)��

> ����`tableSet()`��ֵ�洢��ȫ�ֱ������У���ʹ�ñ���֮ǰδ���塣�����ʵ�� REPL �Ự���ǿɼ��ģ���Ϊ��ʹ�ڱ�������ʱ��������Ի�������С���������Ҳע��ӱ���ɾ���Ǹ���ʬֵ��

��һ�����������ñ�������Ӷ�ջ�е���ֵ�����ס����ֵ��һ�����ʽ���������Ҫ����ֵ��������Է���ֵǶ����ĳ������ı��ʽ�С�

���һЩ����ࣺ

```c
      return constantInstruction("OP_DEFINE_GLOBAL", chunk,
                                 offset);
    case OP_SET_GLOBAL:
      return constantInstruction("OP_SET_GLOBAL", chunk, offset);
    case OP_EQUAL:
// debug.c, in disassembleInstruction()
```

������������ˣ��԰ɣ���?... ����ȫ�ġ����Ƿ���һ�����󣡿�����

```
a * b = c + d ;
```

���� Lox ���﷨��`=`������͵����ȼ������Ӧ�ô��Եؽ���Ϊ��

![](./assets/ba2313394144ca37df822271b28f6675d7636c50.png)

��Ȼ��`a * b`����һ����Ч�ĸ�ֵĿ�꣬������Ӧ����һ���﷨���󡣵��������ǵĽ����������ģ�

> ���ǣ����a * b��һ����Ч�ķ���Ŀ�꣬�����Ǻܷ��������������һЩ���ƴ�����������ͼ��ĳ�ֺ���ķ�ʽ�������ֵ�ֿ�����������`a`��`b`... �ǿ����Ǹ��������⡣

1. ���ȣ�`parsePrecedence()`ʹ��`variable()`ǰ׺���������н���`a`��
2. ֮�������׺����ѭ����
3. ������`*`������`binary()`��
4. ��ݹ�ص���`parsePrecedence()`�����ұߵĲ�������
5. ���ٴε���`variable()`����`b`��
6. �ڵ��� `variable()`�ڲ����������β���`=`.������һ������˽��е����ಿ�ֽ���Ϊ��ֵ��

���仰˵��������������Ĵ�����Ϊ��

![](./assets/0e7fd0f9137e2cabc2cfd2c21dccb76138a87177.png)

���Ǹ��������ȼ�������Ϊ`variable()`û�п��ǰ�����������Χ���ʽ�����ȼ����������ǡ������׺��������Ҳ࣬������һԪ������Ĳ���������������ʽ�����ȼ�̫�߶�������`=`.

Ҫ��������⣬`variable()`Ӧ���ڵ����ȼ����ʽ���������в��Ҳ�ʹ��`=`�����߼��Ͻ���֪����ǰ���ȼ��Ĵ�����`parsePrecedence()`.��`variable()`��������Ҫ֪��ʵ�ʼ�����ֻ�������ȼ��Ƿ��㹻��������ֵ��������ǽ�����ʵ��Ϊ����ֵ���ݡ�

```c
    error("Expect expression.");
    return;
  }

  bool canAssign = precedence <= PREC_ASSIGNMENT;
  prefixRule(canAssign);

  while (precedence <= getRule(parser.current.type)->precedence) {
// compiler.c, in parsePrecedence(), replace 1 line
```

���ڸ�ֵ��������ȼ��ı��ʽ���������Ψһ����ֵ��ʱ�����ڽ�����ֵ���ʽ�򶥼����ʽʱ�����ڱ��ʽ����У����ñ�־�ڴ˴����������������

```c
static void variable(bool canAssign) {
  namedVariable(parser.previous, canAssign);
}
// compiler.c, function variable(), replace 3 lines
```

��ͨ��һ���²������ݸ�����

```c
static void namedVariable(Token name, bool canAssign) {
  uint8_t arg = identifierConstant(&name);
// compiler.c, function namedVariable(), replace 1 line
```

���������ʹ������

```c
  uint8_t arg = identifierConstant(&name);

  if (canAssign && match(TOKEN_EQUAL)) {
    expression();
// compiler.c, in namedVariable(), replace 1 line 
```

�������Ͽ�����һλ���ݴ��͵��������е���ȷλ����Ҫ�������ڣ������Ѿ������ˡ��������Ƕ����ĳ�����и������ȼ��ı��ʽ�У�`canAssign`����`false`����ʹ������һ��`=`����Ҳ�����`=`��Ȼ��`namedVariable()`���أ�ִ�����շ��ص�`parsePrecedence()`.

Ȼ���أ���������δ�������֮ǰ�𻵵�ʾ�������ڣ�`variable()`��������`=`����⽫�ǵ�ǰtoken��`parsePrecedence()`��������ǰ׺����������`variable()`��Ȼ���Խ�����׺����ѭ����û����`=`�����Ľ����������������������ѭ����

Ȼ��`parsePrecedence()`ĬĬ�ط��ص������ߡ���Ҳ���ԡ���� `=`û����Ϊ���ʽ��һ���ֱ����ģ���û������������������������һ����������Ӧ�ñ�������

```c
    infixRule();
  }

  if (canAssign && match(TOKEN_EQUAL)) {
    error("Invalid assignment target.");
  }
}
// compiler.c, in parsePrecedence()
```

������֮ǰ�Ĵ�������ڱ���ʱ��ȷ�صõ���һ�����󡣺õģ�*����*����������𣿻��ǲ���Ȼ�������������ڽ��������ݸ�����һ������������������Щ�����洢�ں���ָ����У�������н�����������Ҫ������ͬ�����͡����ܴ��������������֧��������ֵĿ�ꡪ��setters��Ψһ����һ�����������Ѻõ� C ������Ҫ������*��*���ܲ�����

> ��� Lox ��������±��������`array[index]`��ô��׺`[`Ҳ������ֵ��֧��`array[index] = value`.

��ˣ����ǽ���һЩ���صĹ������������¡����ȣ������Ǽ�������־���ݸ���׺����������

```c
    ParseFn infixRule = getRule(parser.previous.type)->infix;
    infixRule(canAssign);
  }
// compiler.c, in parsePrecedence(), replace 1 line
```

������setters����Ҫ����Ȼ�����ǽ��޸�����ĺ���ָ�����͡�

```c
} Precedence;

typedef void (*ParseFn)(bool canAssign);

typedef struct {
// compiler.c, add after enum Precedence, replace 1 line
```

�Լ�һЩ��ȫ��ζ�Ĵ��������������еĽ��������н���������������

```c
static void binary(bool canAssign) {
  TokenType operatorType = parser.previous.type;
// compiler.c, function binary(), replace 1 line
```

�����

```c
static void literal(bool canAssign) {
  switch (parser.previous.type) {
// compiler.c, function literal(), replace 1 line
```

�����

```c
static void grouping(bool canAssign) {
  expression();
// compiler.c, function grouping(), replace 1 line
```

�����

```c
static void number(bool canAssign) {
  double value = strtod(parser.previous.start, NULL);
// compiler.c, function number(), replace 1 line
```

����Ҳ�ǣ�

```c
static void string(bool canAssign) {
  emitConstant(OBJ_VAL(copyString(parser.previous.start + 1,
// compiler.c, function string(), replace 1 line
```

���

```c
static void unary(bool canAssign) {
  TokenType operatorType = parser.previous.type;
// compiler.c, function unary(), replace 1 line
```

�������ǻص��˿��Ա���� C �����������������������������

```js
var breakfast = "beignets";
var beverage = "cafe au lait";
breakfast = "beignets with " + beverage;

print breakfast;
```

����ʼ��������ʵ�����Ե���ʵ���룡

---

## [��ս](http://craftinginterpreters.com/global-variables.html#challenges)

1. ÿ��������ʶ��ʱ�����������Ὣȫ�ֱ�����������Ϊ�ַ�����ӵ��������С���ÿ�ζ��ᴴ��һ���³�������ʹ�ñ������Ѿ��ڳ������ǰһ�����С���ͬһ��������ͬһ������������õ�����£�����һ���˷ѡ��ⷴ��������������䳣���������۵ļ��ʣ���Ϊ����ֻ������һ�������� 256 ��������
   
   �Ż������������ʱ��ȣ������Ż����Ӱ������������ܣ�������ȷ��Ȩ����

2. ÿ��ʹ��ʱ�ڹ�ϣ���а����Ʋ���ȫ�ֱ����ǳ�������ʹ��һ���õĹ�ϣ��Ҳ����ˡ��������һ�ָ���Ч�ķ������洢�ͷ���ȫ�ֱ��������ı�������

3. �� REPL ������ʱ���û����ܻ��д����δ֪ȫ�ֱ����ĺ�����Ȼ������һ���У����������˱�����Lox Ӧ��ͨ�����״ζ��庯��ʱ�����桰δ֪������������������ŵش���������⡣
   
   ���ǵ��û����� Lox*�ű�*ʱ�������������������κδ���֮ǰ�������������ȫ�ġ������������
   
   ```c
   fun useVar() {
     print oops;
   }
   
   var ooops = "too many o's!";
   ```
   
   ��������ǿ��Ծ�̬�ظ�����`oops`���ᱻ���壬��Ϊ�����е��κεط���*û��*������ȫ�ֱ�������ע�⣬`useVar()`����δ�����ù�����˼�ʹδ����ñ�����Ҳ���ᷢ������ʱ������Ϊ��Ҳ��δ��ʹ�ù���
   
   ���ǿ��Խ������Ĵ��󱨸�Ϊ������������ڴӽű�����ʱ������������Ϊ����Ӧ����֤����Ĵ𰸡���֪���������ű�������ʲô���ã�