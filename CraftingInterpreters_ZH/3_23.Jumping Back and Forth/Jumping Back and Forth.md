# ��ת����

> ����ͷ������Ĺ�ؾ���һ������������һ�����ӣ���Ϊ�˴ﵽĳ��Ŀ�Ķ�����ġ����Ǻ��������������ӵ�����Ϊ�㷢�֣���ʹ�����ã�Ҳû�����塣
> 
> -- Umberto Eco,*The Name of the Rose*

����������Ҫһ��ʱ�䣬����������׼���ý����������ӵ��������������Ϊ jlox �������������������У����Ǹ��� Java ʵ���� Lox �Ŀ�������Ϊ��ִ�� Lox`if`��䣬����ʹ���� Java`if`���������ѡ���ķ�֧�����е�ͨ����������ȫ�������⡣*JVM ����*�򱾻� CPU��ͨ��ʲôħ��ʵ��`if`���ģ��������������Լ����ֽ��� VM �����ƽ⣬���ǿ��Իش�������⡣

��̸�ۡ���������ʱ��ָ����ʲô��������flow����ִָ��ʱ�ƶ�ͨ�������ı��ķ�ʽ���������������ڲ���һ��С�������ڴ������ǻ�������ִ�е��εΡ�Flow �ǻ��������ߵ�·����ͨ��*����*�����ˣ�����������ִ����Щ���롣

�� jlox �У������Ĺ�ע�㡪����ǰ�Ĵ���㡪���������ģ����� �洢�ڸ��� Java ��������Щ AST �ڵ��Լ��������е� Java ���롣�� clox �У���������ȷ��VM ��`ip`�ֶδ洢��ǰ�ֽ���ָ��ĵ�ַ�����ֶε�ֵǡ���ǳ����еġ��������ڵ�λ�á���

ִ��ʱͨ������`ip`�����ǿ������������ظı��Ǹ�������Ϊ��ʵ�ֿ����������б�Ҫ�ľ����Ը���Ȥ�ķ�ʽ�ı�`ip`����򵥵Ŀ������ṹ��һ��`if`��û��`else`�Ӿ����䣺

```c
if (condition) print("condition was truthy");
```

VM ������������ʽ���ֽ��롣������Ϊ�棬��������ִ��`print`�����е���䡣��Ȥ�����������Ϊ��ʱ���������������ʱ��ִ�н����� then ��֧������ִ����һ����䡣

Ҫ����һ�δ��룬����ֻ�轫��`ip`�ֶ�����Ϊ�ô���֮����ֽ���ָ��ĵ�ַ��Ҫ������������ĳЩ���룬������Ҫһ��ָ�����鿴��ջ������ֵ��������Ǽٵģ��������Ӹ�����ƫ������`ip`����һϵ��ָ�������ʲô����������ִ���ճ����е���һ��ָ�

�����Ǳ�����ֽ���ʱ���������ʽǶ�׿�ṹ��ʧ�ˣ�ֻ����һϵ�б�ƽ��ָ�Lox ��һ��[�ṹ�����](https://en.wikipedia.org/wiki/Structured_programming)���ԣ��� clox �ֽ��벻�ǡ���ȷ�����ȡ��������ο���������һ���ֽ���ָ���������һ������м䣬���ߴ�һ����Χ������һ����Χ��

VM ������ִ��������ʹ���ʹ��ջ����δ֪�ġ���һ�µ�״̬����ˣ���ʹ�ֽ����Ƿǽṹ���ģ�����Ҳ��ע��ȷ��������ֻ���ɱ����� Lox ������ͬ�Ľṹ��Ƕ�׵ĸɾ����롣

��������ʵ CPU ����Ϊ��ʽ����ʹ���ǿ���ʹ�ýṹ���������ĸ߼����Զ����ǽ��б�̣�������Ҳ�Ὣ�併��Ϊԭʼ��ת���ڵײ㣬��ʵ֤�� goto ��Ψһ�����Ŀ�������

�����������Ҳ�����Ҫ����ѧ����Ҫ��һ���ǣ����������һ��������תָ�ֻҪ��û��`else`�Ӿ䣬������ʵ�� Lox �� `if`��䡣��ˣ������Ǽ�������ʼ�ɡ�

## 23.1 if���

��ô���½��ˣ����Ѿ�֪����·�ˡ��κ��¹��ܶ���ǰ�˿�ʼ����ͨ���ܵ��������á�һ��`if`��䣬�ţ�һ����䣬�����������ǽ����ҽӵ��������ĵط���

```c
  if (match(TOKEN_PRINT)) {
    printStatement();
  } else if (match(TOKEN_IF)) {
    ifStatement();
  } else if (match(TOKEN_LEFT_BRACE)) {
// compiler.c, in statement()
```

�����ǿ���һ��`if`�ؼ���ʱ�����ǽ����뽻�����������

```c
static void ifStatement() {
  consume(TOKEN_LEFT_PAREN, "Expect '(' after 'if'.");
  expression();
  consume(TOKEN_RIGHT_PAREN, "Expect ')' after condition."); 

  int thenJump = emitJump(OP_JUMP_IF_FALSE);
  statement();

  patchJump(thenJump);
}
// compiler.c, add after expressionStatement()
```

> ����û��ע�⵽�ؼ���`if`֮���`(`ʵ����û�����κ����õ����飿���û�����������Խ�ͬ����ȷ�����ڽ��������磺
> 
> ```c
> if condition) print("looks weird");
> ```

> �رյ�`)`�����ã���Ϊ������������ʽ�����ķֿ�����Щ����ʹ��`then`�ؼ��ִ��档���ǿ�ʼ��`(`��û�����κ����顣�����������Ϊ��ƥ������Ŷ�����������˵����������⡣

���ȱ�����������ʽ����Բ������������������ʱ����Ὣ����ֵ���ڶ�ջ��������ʹ������ȷ����ִ�� then ��֧������������

Ȼ�����Ƿ���һ����`OP_JUMP_IF_FALSE`ָ�����һ������������ʾҪƫ�ƶ���`ip`����Ҫ���������ֽڵĴ��롣�������Ϊ�٣�����`ip`���������������������ģ�

> �˴�����˺�ѱ�Ե�Ŀ��ʾͨ����������������ĳЩ�Ӿ����ɵ��ֽ���顣���ԡ���������ʽ���������Ǳ���ñ���ʽʱ����������ָ�

![](./assets/7ede8290ab3f72fd450cc162d288598d49d12bfc.png)

������һ�����⡣������д`OP_JUMP_IF_FALSE`ָ��Ĳ�����ʱ����ô֪��Ҫ����Զ����û�б��� then ��֧�����Բ�֪�������������ֽ��롣

Ϊ�˽��������⣬ʹ����һ������ļ��ɣ�����**backpatching**������ʹ��ռλ�����������������תָ��ƫ�ơ��������������һ���ָ�����ת����������������� then ���塣һ����ɣ����ھ�֪��Ҫ����Զ�����Ի�ȥ��������ƫ�����滻ռλ��ƫ��������Ϊ���ǿ��Լ������ˡ��е��񽫲����쵽�ѱ����������нṹ�ϡ�

![](./assets/257d80a13ea1c217c9a2c41ba01540da6b9302f2.png)

���ǽ�������ɱ���Ϊ��������������

```c
static int emitJump(uint8_t instruction) {
  emitByte(instruction);
  emitByte(0xff);
  emitByte(0xff);
  return currentChunk()->count - 2;
}
// compiler.c, add after emitBytes()
```

��һ������������ָ�Ϊ��תƫ��д��һ��ռλ�������������ǽ���������Ϊ�������룬��Ϊ�Ժ���������ͬ��ָ��ʹ��������麯����ʹ�������ֽ���Ϊ��תƫ�Ʋ�������16 λƫ���������ǿ���������� 65,535 �ֽڵĴ��룬��Ӧ�������������ǵ���Ҫ��

> һЩָ��е����ġ�������תָ�������Ҫ��ת��Զ�ľ���ʱ����Щָ����Ҫ����Ĳ�������

�ú������ؿ��з�����ָ���ƫ���������� then ��֧�󣬻�ȡ��ƫ���������䴫���ˣ�

```c
static void patchJump(int offset) {
  // -2 to adjust for the bytecode for the jump offset itself.
  int jump = currentChunk()->count - offset - 2;

  if (jump > UINT16_MAX) {
    error("Too much code to jump over.");
  }

  currentChunk()->code[offset] = (jump >> 8) & 0xff;
  currentChunk()->code[offset + 1] = jump & 0xff;
}
// compiler.c, add after emitConstant()
```

�⽫���ص��ֽ��룬���ü��������תƫ�����滻����λ�õĲ��������ڷ�����һ��ϣ����ת��ָ��֮ǰ��������`patchJump()`�������ʹ�õ�ǰ�ֽ��������ȷ����ת��Զ����`if`��������£�����ζ���ڱ��� then ��֧֮��ͱ�����һ�����֮ǰ��

����������ڱ���ʱ����Ҫ�ġ������Ƕ�����ָ�

```c
  OP_PRINT,
  OP_JUMP_IF_FALSE,
  OP_RETURN,
// chunk.h, in enum OpCode
```

�� VM �У���������������������

```c
        break;
      }
      case OP_JUMP_IF_FALSE: {
        uint16_t offset = READ_SHORT();
        if (isFalsey(peek(0))) vm.ip += offset;
        break;
      }
      case OP_RETURN: {
// vm.c, in run()
```

�������ӵĵ�һ������ 16 λ��������ָ�Ϊ�˴ӿ��ж�ȡ��������ʹ����һ���µĺꡣ

```c
#define READ_CONSTANT() (vm.chunk->constants.values[READ_BYTE()])
#define READ_SHORT() \
    (vm.ip += 2, (uint16_t)((vm.ip[-2] << 8) | vm.ip[-1]))
#define READ_STRING() AS_STRING(READ_CONSTANT())
// vm.c, in run()
```

���ӿ��г���������������ֽڣ������й���һ�� 16 λ�޷���������������һ�����������ȡ�����塣

```c
#undef READ_BYTE
#undef READ_SHORT
#undef READ_CONSTANT
// vm.c, in run()
```

��ȡƫ���������Ǽ��ջ����������ֵ��������Ǽٵģ����ǽ������Ծƫ��Ӧ�õ�`ip`.�������ǲ���`ip`�ᣬִ�н��Զ����е���תָ��֮�����һ��ָ�

������Ϊ�ٵ�����£�����Ҫ���κ����������������Ѿ�ƫ����`ip`����˵��ⲿָ�����ѭ���ٴ�ת��ʱ�������ڸ���ָ���ʼִ�У����� then ��֧�е����д��롣

> ��˵�˲�����C��`if`�����ʵ��Lox�Ŀ���������������ȷʵ����һ�����ж��Ƿ�ƫ��ָ��ָ�롣�����ǲ�û������ʹ�� C ��*��������*���������Ը�⣬���ǿ��Դ���������ͬ�������顣����������һ������`falsey()`��������һ�� Lox ֵ�������Ϊ���򷵻� 1�����򷵻� 0��Ȼ�����ǿ���������ʵ����תָ�
> 
> ```c
> case OP_JUMP_IF_FALSE: {
>   uint16_t offset = READ_SHORT();
>   vm.ip += falsey() * offset;
>   break;
> }
> ```

> ��`falsey()`�������ܻ�ʹ��һЩ��������������ͬ��ֵ���ͣ������Ǹú�����ʵ��ϸ�ڣ�����Ӱ�����ǵ� VM ���ִ���Լ��Ŀ�������

��ע�⣬��תָ��Ὣ����ֵ������ջ���������ﻹû����ȫ��ɣ���Ϊ����ڶ�ջ������һ�������ֵ�����Ǻܿ�ͻ���������ɾ�����ʱ������һ�㣬������ Lox ��ȷʵ��һ�����Թ�����`if`��䣬ֻ��Ҫһ��Сָ����� VM ����ʱ֧������

### 23.1.1 Else�Ӿ�

`if`���û��`else`�Ӿ�֧�־���û�� Gomez �� Morticia Addams�����ԣ��ڱ��� then ��֧֮��Ѱ��һ��`else`�ؼ��֡�����ҵ�һ��������� else ��֧��

```c
  patchJump(thenJump);

  if (match(TOKEN_ELSE)) statement();
}
// compiler.c, in ifStatement()
```

������Ϊ��ʱ�������� then ��֧������� else ��֧��`ip`��������else �Ŀ�ʼ�����⻹���������ǵ��µ����̣�

![](./assets/be3b37dc2ebbf40dad60d1adfb28320d1fd77629.png)

�������Ϊ�棬��������Ҫִ�� then ��֧��������֮��ִ�н�ֱ�ӽ��� else ��֧����ѽ��������Ϊ��ʱ���������� then ��֧����Ҫ���� else ��֧������������������������Ƕ�ִֻ��һ����֧��������ʾ��

![](./assets/5b22e69ca3ab0033fdcc2995739ef9db1dfe22da.png)

Ϊ��ʵ������������Ҫ�� then ��֧��ĩβ�ٴ���ת��

```c
  statement();

  int elseJump = emitJump(OP_JUMP);

  patchJump(thenJump);
// compiler.c, in ifStatement()
```

������ else ����������޲���ƫ������

```c
  if (match(TOKEN_ELSE)) statement();
  patchJump(elseJump);
}
// compiler.c, in ifStatement()
```

ִ���� then ��֧�������ת�� else ��֧֮�����һ����䡣��������Ծ��ͬ������Ծ���������ġ��������ǽ�������������Ҫ��һ��ָ����������һ�㡣

```c
  OP_PRINT,
  OP_JUMP,
  OP_JUMP_IF_FALSE,
// chunk.h, in enum OpCode
```

�����������ͣ�

```c
        break;
      }
      case OP_JUMP: {
        uint16_t offset = READ_SHORT();
        vm.ip += offset;
        break;
      }
      case OP_JUMP_IF_FALSE: {
// vm.c, in run()
```

����ûʲô����ֵġ���Ψһ���������������������������Ӧ��ƫ������

����then �� else ��֧�����Թ����ˣ������������һ�㡣���һ�����������ڶ�ջ�е�����ֵ�����ס��ÿ����䶼����������ջЧӦ�������ִ����Ϻ󣬶�ջӦ��֮ǰһ���ߡ�

���ǿ�����`OP_JUMP_IF_FALSE`ָ�����������������߽����� ��ϣ��pop�� �������߼��������ʹ����ͬ��ָ����ԣ����ǻ��ñ������ڱ���`if`���ʱ����������ȷ��`OP_POP`ָ���Ҫע��ͨ�����ɴ����ÿ��ִ��·����Ҫ����������

������Ϊ��ʱ�����Ǿ��� then ��֧�ڵĴ���֮ǰ��������

```c
  int thenJump = emitJump(OP_JUMP_IF_FALSE);
  emitByte(OP_POP);
  statement();
// compiler.c, in ifStatement()
```

�������ǽ��������� else ��֧�Ŀ�ͷ��

```c
  patchJump(thenJump);
  emitByte(OP_POP);

  if (match(TOKEN_ELSE)) statement();
// compiler.c, in ifStatement() 
```

���������Сָ���ζ��ÿ��`if`��䶼��һ�������� else ��֧����ʹ�û�û�б�д`else`�Ӿ䡣�������뿪������£����з�֧�����ľ��Ƕ�������ֵ��

��������ȷ����������ʾ��

![](./assets/c0330ba49c00dfc6ea01cbc8074fa44832f5253f.png)

�����ϸ�鿴�����ᷢ����ʼ��ִ�е�����֧��ȷ�����ȵ���������ʣ�µľ���һ�㷴���֧�֡�

```c
      return simpleInstruction("OP_PRINT", offset);
    case OP_JUMP:
      return jumpInstruction("OP_JUMP", 1, chunk, offset);
    case OP_JUMP_IF_FALSE:
      return jumpInstruction("OP_JUMP_IF_FALSE", 1, chunk, offset);
    case OP_RETURN:
// debug.c, in disassembleInstruction()
```

������ָ����һ�� 16 λ���������¸�ʽ���������������һ���µ�ʵ�ú�������������ǡ�

```c
static int jumpInstruction(const char* name, int sign,
                           Chunk* chunk, int offset) {
  uint16_t jump = (uint16_t)(chunk->code[offset + 1] << 8);
  jump |= chunk->code[offset + 2];
  printf("%-16s %4d -> %d\n", name, offset,
         offset + 3 + sign * jump);
  return offset + 3;
}
// debug.c, add after byteInstruction()
```

���ˣ�����һ�������Ŀ������ṹ���������һ�� 80 ����ĵ�Ӱ����̫�����־ͻῪʼ������Ŀ������﷨�ͻ����д�����l�ţ�80 ������ѽ������������Ǳ����Լ�Ŭ����

> �����Ҷ� Depeche Mode ���ų־õ��Ȱ���

## 23.2 �߼������

�����ܻ��ǵ� jlox �е���һ�㣬���߼������`and`��`or`����������һ�Զ�Ԫ�����������`+`and`-`����Ϊ���Ƕ�·���ҿ��ܲ����������������ֵ�������Ҳ���������������Ǹ����ǿ���������ʽ��

���ǻ�������`if`����`else`�Ӿ��һЩ���塣�������ǵ���򵥷�����ֻ����չʾ���������뼰�������ɵ��ֽ����в����Ŀ��������� `and`��ʼ�����ǽ���ҽӵ�����ı���ʽ�������У�

```c
  [TOKEN_NUMBER]        = {number,   NULL,   PREC_NONE},
  [TOKEN_AND]           = {NULL,     and_,   PREC_AND},
  [TOKEN_CLASS]         = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 1 line
```

�⽫����һ���µĽ�����������

```c
static void and_(bool canAssign) {
  int endJump = emitJump(OP_JUMP_IF_FALSE);

  emitByte(OP_POP);
  parsePrecedence(PREC_AND);

  patchJump(endJump);
}
// compiler.c, add after defineVariable()
```

���ⱻ����ʱ����ߵı���ʽ�Ѿ������롣����ζ��������ʱ������ֵ��λ�ڶ�ջ�Ķ���������Ǹ�ֵ�Ǽٵģ���ô��֪����������ʽ`and`һ���Ǽٵģ��������������ұߵĲ�����������ߵ�ֵ��Ϊ��������ʽ�Ľ�������򣬶�����ߵ�ֵ�������ұߵĲ�����������Ϊ����`and`����ʽ�Ľ����

�����д���������������ġ�����������ʾ��

![](./assets/c8f33bc2d27701e6d1c42e2256d817aa120e7fd2.png)

�����������˽�Ϊʲô`OP_JUMP_IF_FALSE`��ֵ�����ڶ�ջ��������`and`�����Ϊ��ʱ����ֵ�ᱣ��������Ϊ��������ʽ�Ľ����

> ���ǵĲ����뷶Χ�ڻ����㹻�Ŀռ䣬���Կ���Ϊ��ʽ�����Ͳ�������������ת�ṩ������ָ�������ͼ���Ȿ��Ķ������١��������ֽ��� VM �У�ֵ��̽�����Ӹ���ר��ָ��鿴�������Ӱ�����ܡ�

### 23.2.1 or�����

`or`�������΢����һЩ���������ǽ������ӵ��������С�

```c
  [TOKEN_NIL]           = {literal,  NULL,   PREC_NONE},
  [TOKEN_OR]            = {NULL,     or_,    PREC_OR},
  [TOKEN_PRINT]         = {NULL,     NULL,   PREC_NONE},
// compiler.c, replace 1 line
```

���ý�����ʹ��һ����׺`or` tokenʱ�������������

```c
static void or_(bool canAssign) {
  int elseJump = emitJump(OP_JUMP_IF_FALSE);
  int endJump = emitJump(OP_JUMP);

  patchJump(elseJump);
  emitByte(OP_POP);

  parsePrecedence(PREC_OR);
  patchJump(endJump);
}
// compiler.c, add after number()
```

��`or`����ʽ�У�������Ϊ*��*�������������Ҳ����������ˣ���Ҫ��ֵΪ��ʱ��ת�����ǿ�������һ��������ָ���ֻ��Ϊ��չʾ������������ɵؽ����Ե�����ӳ�䵽����Ҫ���κ�ָ�����У��Ҹ����������е���תָ����ʵ������

�����Ϊ false ʱ��������΢������һ����䡣���������������������ȷ�������Ĵ��롣��ֵΪ truthy ʱ�����С�赸��Ч����������������������ʾ��

![](./assets/ba4c46d6023bb519ad5f0dc98363e2cc871ed7c0.png)

��ʵ˵���ⲻ����õķ�������Ҫ�����ָ��Ҫִ�к͸���Ŀ�����û�г�ֵ�����֤��Ϊʲô`or`Ӧ�ñ�`and`. ������Ȥ���ǣ������ڲ������κ���ָ��������ʵ���������������ԭ���ҵķ��ݡ�

�õģ������ Lox �е�����*��֧*�ṹ��Ҳ����˵���ҵ���˼�ǣ���Щ����������ֻ���������롣��������ͨ����ĳ�ֶ�·��֧��䣬����`switch`��Ҳ��������������ʽ`?:`���� Lox ���ּ򵥡�

## 23.3 while���

�⽫���Ǵ���*ѭ��*��䣬��*���*��ת�Ա�ʹ������Զ��ִ�С�Lox ֻ������ѭ���ṹ��`while`��`for`. `while`ѭ�����򵥣��������Ǵ����￪ʼ

```c
    ifStatement();
  } else if (match(TOKEN_WHILE)) {
    whileStatement();
  } else if (match(TOKEN_LEFT_BRACE)) {
// compiler.c, in statement()
```

�����ǵ���һ��`while`  tokenʱ�����ǵ��ã�

```c
static void whileStatement() {
  consume(TOKEN_LEFT_PAREN, "Expect '(' after 'while'.");
  expression();
  consume(TOKEN_RIGHT_PAREN, "Expect ')' after condition.");

  int exitJump = emitJump(OP_JUMP_IF_FALSE);
  emitByte(OP_POP);
  statement();

  patchJump(exitJump);
  emitByte(OP_POP);
}
// compiler.c, add after printStatement()
```

�����Ϻ�`if`���һ������������������ʽ����ǿ������������������������һ����תָ��������Ϊ�٣�������������������䡣

�ڱ���������޲���ת����ע�����һ·���Ķ�ջ�е�������ֵ����`if`����Ψһ������ѭ������������������

> ��Ŀ�ʼ�����Ҷ��߼������ʹ����ͬ��תָ��ľ�����

```c
  statement();
  emitLoop(loopStart);

  patchJump(exitJump);
// compiler.c, in whileStatement()
```

������֮�����ǵ��ô˺�����������loop��ָ���ָ����Ҫ֪�����ض�Զ����ǰ��ʱ������������׶η���ָ���Ϊ�ڷ�����תָ��֮ǰ��֪��Ҫ����Զ����������û���Ǹ����⡣�����Ѿ�������������Ҫ���صĴ���㡪����������������ʽ֮ǰ��

������Ҫ���ľ����ڱ���ʱ�����λ�á�

```c
static void whileStatement() {
  int loopStart = currentChunk()->count;
  consume(TOKEN_LEFT_PAREN, "Expect '(' after 'while'.");
// compiler.c, in whileStatement()
```

ִ��`while`ѭ���������һֱ���ص�����֮ǰ����������ÿ�ε���ʱ����������������ʽ������ĵ�ǰָ������洢���ֽ����У�`loopStart`�Լ�¼���Ǽ����������������ʽ֮ǰ���ֽ���ƫ������Ȼ�������ݸ��������������

```c
static void emitLoop(int loopStart) {
  emitByte(OP_LOOP);

  int offset = currentChunk()->count - loopStart + 2;
  if (offset > UINT16_MAX) error("Loop body too large.");

  emitByte((offset >> 8) & 0xff);
  emitByte(offset & 0xff);
}
// compiler.c, add after emitBytes()
```

�е���`emitJump()`��`patchJump()`�Ľ�ϡ�������һ���µ�ѭ��ָ���ָ����������*���*��ת������ƫ����������תָ��һ����֮��������һ�� 16 λ������������ӵ�ǰ���ڵ�ָ�`loopStart`��Ҫ���صĵ��ƫ��������`+ 2`��Ϊ�˿��ǵ�`OP_LOOP` ������Ҳ��Ҫ������ָ�������������Ĵ�С��

�� VM �ĽǶ�������`OP_LOOP`��`OP_JUMP`֮��ȷʵû��������졣���߶�ֻ����`ip`����ƫ��. ���ǿ���Ϊ����ʹ��һ��ָ�������һ�������ŵ�ƫ������������������Ϊ�ܿ��ֶ��������ŵ� 16 λ��������������ֽ�����ķ��˵Ĳ���Ҫ����һЩ�����������п��õĲ�����ռ䣬��ôΪʲô��ʹ�����أ�

��ָ�������

```c
  OP_JUMP_IF_FALSE,
  OP_LOOP,
  OP_RETURN,
// chunk.h, in enum OpCode
```

��������У���������ʵ�֣�

```c
      }
      case OP_LOOP: {
        uint16_t offset = READ_SHORT();
        vm.ip -= offset;
        break;
      }
      case OP_RETURN: {
// vm.c, in run()
```

Ψһ������`OP_JUMP`�Ǽ��������Ǽӷ��������Ҳ���ơ�

```c
      return jumpInstruction("OP_JUMP_IF_FALSE", 1, chunk, offset);
    case OP_LOOP:
      return jumpInstruction("OP_LOOP", -1, chunk, offset);
    case OP_RETURN:
// debug.c, in disassembleInstruction()
```

�������ǵ�`while`��䡣������������ת-һ������������ǰ��ת��������������ʱ����ѭ�����Լ�һ�������������ѭ������ִ��������֮������������ʾ��

![](./assets/d13ee3c70e31db5cb1ca93add0eeac5ca2b6a78b.png)

## 23.4 for ���

Lox �е���һ��ѭ������ǹ��ϵ�`for`ѭ�������̳��� C����`while`ѭ����ȣ����и���Ĺ��ܡ������������Ӿ䣬�����Ӿ䶼�ǿ�ѡ�ģ�

- ��ʼֵ�趨������Ǳ������������ʽ���������Ŀ�ͷ����һ�Ρ�

- �����Ӿ���һ������ʽ��������`while`ѭ����һ���������ļ�����Ϊ����ʱ�������˳�ѭ����

- ��������ʽ��ÿ��ѭ����������ʱ����һ�Ρ�

> ������븴ϰһ�£��ڶ������е���Ӧ�½�[������ϸ�ؽ���](http://craftinginterpreters.com/control-flow.html#for-loops)���塣

�� jlox �У���������`for`ѭ������Ϊ`while`ѭ��֮ǰ������ĩβ����һЩ��������� �����AST�����ǽ������Ƶ����飬���ܲ����� AST ���������κ����顣�෴���ֽ����������ʹ�����е���ת��ѭ��ָ�

���ǽ������ʵ�֣���`for`�ؼ��ֿ�ʼ

```c
    printStatement();
  } else if (match(TOKEN_FOR)) {
    forStatement();
  } else if (match(TOKEN_IF)) {
// compiler.c, in statement()
```

�����ø����������������ֻ֧��`for`���п��Ӿ��ѭ��`for (;;)`����ô���ǿ�������ʵ������

```c
static void forStatement() {
  consume(TOKEN_LEFT_PAREN, "Expect '(' after 'for'.");
  consume(TOKEN_SEMICOLON, "Expect ';'.");

  int loopStart = currentChunk()->count;
  consume(TOKEN_SEMICOLON, "Expect ';'.");
  consume(TOKEN_RIGHT_PAREN, "Expect ')' after for clauses.");

  statement();
  emitLoop(loopStart);
}
// compiler.c, add after expressionStatement()
```

������һ��ǿ���Ա����š�Ȼ�����Ǳ������塣�������Ƕ�`while`ѭ������������������������Ķ�����¼�ֽ���ƫ������������һ��ѭ�������ص���֮����Ǹ��㡣�����Ѿ���������ѭ���Ĺ���ʵ�֡�

> ���û��`return`��䣬��������ʱ����֮��û���κη���������ֹ����

### 23.4.1 for��ʼ���Ӿ�

�������ǽ����ӵ�һ���Ӿ䣬��ʼ��������ֻ������֮ǰִ��һ�Σ���˱���ܼ򵥡�

```c
  consume(TOKEN_LEFT_PAREN, "Expect '(' after 'for'.");
  if (match(TOKEN_SEMICOLON)) {
    // No initializer.
  } else if (match(TOKEN_VAR)) {
    varDeclaration();
  } else {
    expressionStatement();
  }

  int loopStart = currentChunk()->count;
// compiler.c, in forStatement(), replace 1 line
```

�﷨�е㸴�ӣ���Ϊ���������������������ʽ��ʹ��`var`�ؼ��ֵĴ������ж�����Щ�����ڱ���ʽ��������ǵ���`expressionStatement()`����`expression()`����Ѱ��һ���ֺţ�������Ҳ��Ҫ�������һ�����һ��`OP_POP`ָ����������ֵ�����ǲ�ϣ����ʼ�����ڶ�ջ�������κζ�����

���һ��`for`���������һ����������ô�ñ�����������Ӧ����ѭ���塣����ͨ������������װ��һ������������ȷ����һ�㡣

```c
static void forStatement() {
  beginScope();
  consume(TOKEN_LEFT_PAREN, "Expect '(' after 'for'.");
// compiler.c, in forStatement()
```

Ȼ�����������ر�����

```c
  emitLoop(loopStart);
  endScope();
}
// compiler.c, in forStatement()
```

### 23.4.2 for �����Ӿ�

���������ǿ������˳�ѭ������������ʽ��

```c
  int loopStart = currentChunk()->count;
  int exitJump = -1;
  if (!match(TOKEN_SEMICOLON)) {
    expression();
    consume(TOKEN_SEMICOLON, "Expect ';' after loop condition.");

    // Jump out of the loop if the condition is false.
    exitJump = emitJump(OP_JUMP_IF_FALSE);
    emitByte(OP_POP); // Condition.
  }

  consume(TOKEN_RIGHT_PAREN, "Expect ')' after for clauses.");
// compiler.c, in forStatement(), replace 1 line
```

���ڸ��Ӿ��ǿ�ѡ�ģ�������Ҫ�鿴���Ƿ�ʵ�ʴ��ڡ�������Ӿ䱻ʡ�ԣ���һ��token�����Ƿֺţ���������Ѱ������˵�������û�зֺţ��������һ����������ʽ��

����������£����Ǳ�������Ȼ�󣬾��� while һ�����������Ϊ�٣��ᷢ��һ���˳�ѭ����������ת��������ת��ֵ���ڶ�ջ�ϣ�������ִ������֮ǰ���䵯������ȷ������������Ϊ��ʱ������ֵ��

��ѭ����֮��������Ҫ�޲��Ǹ���ת��

```c
  emitLoop(loopStart);

  if (exitJump != -1) {
    patchJump(exitJump);
    emitByte(OP_POP); // Condition.
  }

  endScope();
}
// compiler.c, in forStatement()
```

���ǽ����������Ӿ�ʱ�������������û�У��򲻻���ת����������ջ��Ҳ���ᵯ���κ�����ֵ��

### 23.4.3 for �����Ӿ�

�Ұ���õ�������������Ӿ䡣��ܸ��ӡ������ı���ʽ������ѭ����֮ǰ������ѭ����֮��ִ�С�������ǽ���Ϊ AST ���ڵ�����һ�˱��������ɴ��룬���ǿ��Լ򵥵ر���������`for`��� AST �� body �ֶ�֮ǰ�� increment �Ӿ䡣

���ҵ��ǣ����ǲ����Ժ�increment�Ӿ䣬��Ϊ������ֻ�Դ������һ�α�������Ϊ���棬���ǽ�*����*�������������壬��*�ص�*��������������Ȼ��ת����һ��������

��֪�����е���֣����ǣ��٣���ʤ���� C ���ֶ������ڴ��е� AST���԰ɣ����Ǵ��룺

```c
  }

  if (!match(TOKEN_RIGHT_PAREN)) {
    int bodyJump = emitJump(OP_JUMP);
    int incrementStart = currentChunk()->count;
    expression();
    emitByte(OP_POP);
    consume(TOKEN_RIGHT_PAREN, "Expect ')' after for clauses.");

    emitLoop(loopStart);
    loopStart = incrementStart;
    patchJump(bodyJump);
  }

  statement();
// compiler.c, in forStatement(), replace 1 line
```

ͬ�������ǿ�ѡ�ġ������������һ���Ӿ䣬���ʡ��ʱ����һ����ǽ��������š�����������ʱ��������Ҫ��������������Ӧ��ִ�С���ˣ����ȣ����Ƿ���һ����������ת����increment�Ӿ�Ĵ�������ѭ���塣

�����������Ǳ���increment����ʽ��������ͨ����һ����ֵ����������ʲô������ֻ��Ϊ�����ĸ����ö�ִ��������������Ҳ����һ�� pop ����������ֵ��

���һ�����е㼬�֡����ȣ�����һ��ѭ��ָ����ǽ����Ǵ��ص�`for`ѭ����������ѭ����������еĻ���������������ʽ֮ǰ����ѭ��������increment֮��������Ϊincrement��ÿ��ѭ����������ʱִ�С�

Ȼ�����Ǹ���`loopStart`Ϊָ��increment����ʽ��ʼ��ƫ�������Ժ󣬵��������������֮�󷢳�ѭ��ָ��ʱ���⽫����������increment����ʽ��������û��incrementʱ��������ѭ�������������������ΰ�increment֯����ѭ����֮��

��ܸ��ӣ���һ�ж�����ˡ����������Ӿ������ѭ����������������̣�

![](./assets/efa92873fb775ef4ba7b70210c1864bc2e84d6bf.png)

���� jlox ��ʵ��`for`ѭ��һ�������ǲ���Ҫ�Ӵ�����ʱ����һ�ж�������Ϊ VM �Ѿ�֧�ֵ�ԭʼ�������������ڱ����У�������ǰ������һ�󲽡���clox������ͼ���걸�ġ����ǻ��������൱������﷨������������������ʽ��ʽ��������ˣ�Ҳֻ���������򵥵���ָ��������ǵ� VM �ܹ���˵������һ���൱������Ŭ���ر��ʡ�

> ���޷�����˫�����һ���������ڡ�

---

## [��ս](http://craftinginterpreters.com/jumping-back-and-forth.html#challenges)

1. ����`if`���֮�⣬����� C ϵ�����Զ��ж�·`switch`��䡣��һ�����ӵ� clox���﷨�ǣ�
   
   ```
   switchStmt     �� "switch" "(" expression ")"
                    "{" switchCase* defaultCase? "}" ;
   switchCase     �� "case" expression ":" statement* ;
   defaultCase    �� "default" ":" statement* ;
   ```
   
   Ҫִ��һ��`switch`��䣬���ȼ��������ڵ�switch����ʽ��Ȼ��ִ��case������ÿ�������������ֵ����ʽ����� case ֵ���� switch ֵ����ִ�� case �µ���䣬Ȼ���˳�`switch`��䡣���򣬳�����һ��case�����û�� case ƥ�䲢����һ��`default`�Ӿ䣬��ִ��������䡣
   
   Ϊ����������򵥣�����ʡ���� ʧ�� ��`break`��䡣ÿ�� case ���������ɺ��Զ���ת�� switch ����ĩβ��

2. �� jlox �У��������������Ӷ�`break`����֧�ֵ���ս����һ�Σ���������`continue`��
   
   ```
   continueStmt   �� "continue" ";" ;
   ```
   
   `continue`���ֱ����ת������ķ��ѭ���Ķ���������ѭ��������ಿ�֡���`for`ѭ����`continue`������еĻ���`continue` ��ת�������Ӿ䡣û�а�����ѭ���е�����Ǳ���ʱ����
   
   һ��Ҫ���Ƿ�Χ��ִ��`continue` ʱ����ѭ�����ڻ�Ƕ����ѭ���ڵĿ��������ľֲ�����Ӧ����δ�����

3. �� Algol 68 �������������ṹ����û�б仯������ʱ�����Ե��ݻ�һֱ������ʹ������������Ժ͸��߼����������ʽ������û�еõ�̫���ע��
   
   Ϊ�˺��棬����Ϊ Lox ����һ�����õ���ӱ���������ܡ���������������ʽ�ĸĽ���ȫ�µĶ�������ʵ���У����������ֵͱ���ˮƽ������㹻���õĶ���������ǿ���û�ѧϰ����Ϥ�ķ��ź���Ϊ�ĳɱ�����������ϰ��Ƽ��ܵĺû��ᡣ

## [���˵����˼��GOTO�Ļ��� ](http://craftinginterpreters.com/jumping-back-and-forth.html#design-note)

���������� Lox ������Ư���Ľṹ��������ʵ���ϱ�����Ϊԭʼ�ķǽṹ����Ծ������ʷ����й���˺����ߵ���һ�̡�һֱ����goto����������������£�����*��*����¡����Ƕ�֪�� goto ��а��ġ�����...Ϊʲô��

ȷʵ��������ʹ�� goto ��д����ά���Ĵ��롣������Ϊ����Ĵ��������Ա��û�����ۼ��������ַ���Ѿ��ܾ�û�������ˡ���������������ԵĿֲ�������Ԯ�����ǲ����ˡ�

���Ǻ������������������ԭ������Ϊ Edsger Dijkstra ����������*ACM ͨѶ*��1968 �� 3 �£��ϵ������ż���Go To Statement Considered Harmful��ɱ��������һ��ʱ��������Χ�ƽṹ����̵�����һֱ�ܼ��ң�˫����ӵ���߶������ۣ�������Ϊ Dijkstra ��ֵ�ó��޵�����Ч�ؽ�������������Ĵ���������Զ�û�зǽṹ������ת��䡣

һ���ҳ���ż�����ǹƥ�����ƻ������Թ��ܣ���һ���Ƿǳ�����ӡ����̵Ķ���������㻹û�ж��������ҹ�����ȥ��һ�������Ǽ������ѧ֪ʶ�Ŀ�������Ʒ�������ǲ�����洫����֮һ�����⣬�����Ķ�ѧ�� CSд����һ���ܺõļ����ϰ������һ������õļ��ܡ�

Ҳ����˵��������ܿ˷� Dijkstra �����������ܵļ�ǫ�顢���ҿ���д�����

> ����ҷ�����Ϊʲôʹ�� go to ���������������Եĺ����...��ʱ�Ҳ�û��̫����������֣��������ύ�ҵĳ��濼�ǣ���Ϊ��������ָ�����������У����˶ش�����������

�����������ڶ෢���е�һ���������ֵ�Ⱥ������֮ǰ����ʲ������д��

���Ѿ�ͨ���˺ܶ�飬����һЩ��������Ӧ�����ۡ��ҳ�����ֻ�ǰٸн�������һ���ǳ��ߵ�ˮƽ�ϣ��Һ�����һ������һ���۵��������ģ�

1. ��Ϊ����Ա�����Ǳ�д���򡪡���̬�ı����������ǹ��ĵ���ʵ�����еĳ��򡪡����Ķ�̬��Ϊ��

2. ���Ǹ��ó�������̬��������Ƕ�̬�������û���ṩ�κ�֤����֧����һ˵�������ҽ����ˡ���

3. ��ˣ�����Խ��ʹ����Ķ�ִ̬�з�ӳ���ı��ṹԽ�á�

����һ���õĿ�ʼ�������ǵ�ע�������������Ǳ�д�Ĵ���������ڲ����еĴ���֮��ķ�����һ����Ȥ�ļ��⡣Ȼ������ͼ��������ı���ִ��֮��ġ���Ӧ��ϵ��������һ��������ְҵ�����ж����ᳫ���ϸ�ı�̵�����˵�����Ķ����൱���⡣��˵��

> ���������ǿ�����α������̵Ľ�չ��������Էǳ������˼��������⣺����һ�����̣�����Ϊ��һ��ʱ�������Ķ�������һ�����⶯��֮��ֹͣ�����Ǳ����޸�ʲô���ݲ�������������̣�ֱ���ǳ���ͬ�ĵ㣿��

����һ�°ɡ�������̨�����������ȫ��ͬ��������������ͬ�ĳ��򡪡���ȫȷ������������ִ�й����е��������ͣ����֮һ������Ҫ����һ̨���������ʲô���ݲ���ʹ�����һ̨�����һ��׼ȷ��ֹͣ��

�����ĳ���ֻ������ֵ�����ļ���䣬�Ǻ����ס���ֻ��Ҫ֪����ִ�е����һ�����֮��ĵ㡣��������һ���ϵ㣬`ip`�����ǵ� VM �У��������Ϣ�е��кš����������ӷ�֧������`if`����`switch`�����������κ����ݡ���ʹ���ָ���֧�ڲ���������Ȼ����֪�����������

���Ӻ������ú�������Ҫ���ණ������������ĳ������ִ�й�������ͣ��һ̨������������ܻ�Ӷ��λ�õ��øú�����*Ҫ����������*ִ�е�ͬһ����ͣ�ڶ�̨����������Ҫ��*��ȷ*���øú���ʱ��ͣ����

��ˣ���������Ҫ֪����ǰ��䣬���Ҷ�����δ���صĺ������ã�������Ҫ֪�����õ��λ�á����仰˵��һ�����ö�ջ����Ȼ�Ҳ���Ϊ Dijkstra д�����ʱ���������������ġ�

��ָ��ѭ��ʹ�����ø��ѡ��������ѭ������м���ͣ���㲻֪���Ѿ������˶��ٴε�����������˵�㻹��Ҫ���ֵ������������ң�����ѭ������Ƕ�ף��������Ҫһ����Щ����������ö�ջָ�뽻������Ϊ��Ҳ�������ⲿ������ѭ������

������������ֵĵط�������������������ڹ���һЩ��������ϣ�������� goto ��δ���������Щ���෴����ֻ��˵��

> ����ʹ�� go to ����ֱ�Ӻ���ǣ������ҵ�һ����������������������̽��ȡ�

��û��֤������ѣ�Ҳû��˵��ԭ����ֻ��˵���㡣��ȷʵ˵һ�ַ��������������⣺

> ��Ȼ��ʹ�� go to �����Ȼ����ͨ����������Ψһ���������ȣ��ü����������Գ�����������ִ�еĲ���������һ�ֱ�׼��ʱ�ӣ����������ڣ�������������Ȼ��һ�޶�����ȴ���ް�����

����...��ʵ���Ͼ���ѭ�������������ģ�������Щ�ܺá�������˵ÿ��ѭ�����Ǽ򵥵ġ����ڴ� 0 �� 10 ��ÿ������������������������`while`���и���������ѭ����

��һ���ȽϽӽ������ӣ����� clox ���ĵĺ����ֽ���ִ��ѭ����Dijkstra ��Ϊ��ѭ�������ڴ����ģ���Ϊ���ǿ��Լ򵥵ؼ���ѭ�������˶��ٴ����ƶ�����ȡ����Ƕ���ĳЩ�û������ Lox �����е�ÿ��ִ��ָ���ѭ����������һ�Ρ�֪����ִ���� 6,201 ���ֽ���ָ������ܸ������� VM ά���߹��ڽ�����״̬��*�κ�*������

��ʵ�ϣ�������������ָ��һ�������ε�������B?hm �� Jacopini[֤��](https://en.wikipedia.org/wiki/Structured_program_theorem)��*�κ�*ʹ�� goto �Ŀ�����������ת��Ϊ��ʹ������ѭ���ͷ�֧�Ŀ����������ǵ��ֽ��������ѭ���Ǹ�֤����һ�������������ӣ���ʵ���� clox �ֽ���ָ��ķǽṹ��������������ʹ���κ� gotos ������

���ƺ������� Dijkstra �����ţ���*����*ͨ���� goto ת��Ϊ��ʹ�� goto �ĳ�����Ϊ�������Ӧ��ϵ��Ȼ��ʹ�øó���Ķ�Ӧ��ϵ����������˵�������ǿ��Խ��ܵģ���Ϊ����ʹ�÷�֧��ѭ����

���ǣ���ʵ˵������������۵�Ҳ�ܱ���������Ϊ�����������϶�����������ѧ����ʹ�ü��߼������б�Ӧ�Ǿ�������ġ�����Ϊ������֤��Dijkstra �ǶԵģ�һЩʹ�� goto �Ĵ�����ĺ���⡣ͨ��ʹ�ýṹ�������������д󲿷ֿ��Զ���Ӧ�ñ�ɸ������Ĵ��롣

ͨ������������ȫ���� goto�������Կ��Ա���ʹ�� goto ��д����Ĵ��롣��ʹ�û�ʹ�ýṹ����������ʹʹ����Щ�ṹ��д���� goto �Ĵ����Ϊһ������ս�����������������������ľ�ʤ����

����ʱ��ȷʵ��֪�������Ƿ��Ӥ����ͬϴ��ˮһ�𵹵��ˡ���û�� goto ������£����Ǿ��������ڸ����ӵĽṹ��ģʽ����ѭ���ڵĿ��ء���һ����������ӡ���һ����ʹ�ñ��������˳�һϵ��Ƕ��ѭ����

```c
// See if the matrix contains a zero.
bool found = false;
for (int x = 0; x < xSize; x++) {
  for (int y = 0; y < ySize; y++) {
    for (int z = 0; z < zSize; z++) {
      if (matrix[x][y][z] == 0) {
        printf("found");
        found = true;
        break;
      }
    }
    if (found) break;
  }
  if (found) break;
}
```

����ı��±ߵĺã�

```c
for (int x = 0; x < xSize; x++) {
  for (int y = 0; y < ySize; y++) {
    for (int z = 0; z < zSize; z++) {
      if (matrix[x][y][z] == 0) {
        printf("found");
        goto done;
      }
    }
  }
}
done:
```

> ������ͨ����ÿ��ѭ���������Ӿ�Ŀ�ͷ��������û��`break`���������ִ�д˲����������Ǳ�����һ�����޵� goto-ish �ṹ��`!found &&`

������������ϲ���������ǽ�����ڿ־�����������ƺ͹��̾��ߡ�����������˶� goto ������ͺô����κ�΢����˽⡣�෴������ֻ����Ϊ��������Ϊ���к��ġ����͸��˶��ԣ��Ҵ���û�з��ֽ����Ǹ����������Թ�����������㡣