# ����

> �����ѡ��������ѣ����㲻��ѡ����ļ��ˣ��������Ƿ�������ǣ�������Ȼ��������ˣ�����㲻���ϣ�������㿴������ɵ��
> 
> -- Harper Lee,*To Kill a Mockingbird*

���������� VM ����¹��ܵ����һ�¡������Ѿ����������� Lox ���Դ�������ʣ�µľ��Ǽ̳з����͵��ó��෽��������һ��֮�����ǻ���[��һ��](http://craftinginterpreters.com/optimization.html)������û�������µ���Ϊ����**ֻ��ʹ**���еĶ������졣�����һ��������ӵ��һ�������� Lox ʵ�֡�

> �����ֻ��ʹ������ʾ�ö�����ø��첻��Ҫ���Ͼ������������ڶ����������ȫ��Ŀ���Ǳ� jlox �и��õ����ܡ����������˵��ȥʮ���¶��ǡ��Ż�����

�����е�һЩ���ϻ��������� jlox�����ǽ�� super ���õķ�ʽ������ͬ����Ȼ clox ��ͨ����ջ�ϴ洢״̬�ĸ��ӻ�����ʵ�ֵġ��������������һ����ȫ��ͬ�ġ�����ķ���������̳еķ������á�

## 29.1 �̳з���

���ǽ��ӷ����̳п�ʼ����Ϊ�����򵥡���ϰһ�£�Lox �̳��﷨������ʾ��

```js
class Doughnut {
  cook() {
    print "Dunk in the fryer.";
  }
}

class Cruller < Doughnut {
  finish() {
    print "Glaze with icing.";
  }
}
```

�����Cruller ��̳��� Donut����� Cruller ��ʵ���̳��˸�`cook()`�������Ҳ�֪����ΪʲôҪǿ���������֪���̳�����ι����ġ������ǿ�ʼ�������﷨��

```c
  currentClass = &classCompiler;

  if (match(TOKEN_LESS)) {
    consume(TOKEN_IDENTIFIER, "Expect superclass name.");
    variable(false);
    namedVariable(className, false);
    emitByte(OP_INHERIT);
  }

  namedVariable(className, false);
// compiler.c, in classDeclaration()
```

�����Ǳ�������֮�������һ������� `<`����ô�ҵ���һ�������Ӿ䡣���ѵ�����ı�ʶ��token��Ȼ�����`variable()`�ú���������ǰʹ�õ�token��������Ϊ�������ã������������Լ��ر�����ֵ�����仰˵���������Ʋ��ҳ��ಢ����ѹ��ջ��

֮�����ǵ���`namedVariable()`�����м̳е�������ص�ջ�ϣ�Ȼ����һ��`OP_INHERIT`ָ���ָ��������ӵ��µ����ࡣ����һ���У����Ƕ�����һ��`OP_METHOD`ָ�ͨ�����䷽���������һ���������ı����е������������ơ�����`OP_INHERIT`ָ����������ಢ���̳е�Ч��Ӧ��������

��ǰ���ʾ���У���������ͨ������﷨����ʱ��

```js
class Cruller < Doughnut {
```

���������ֽ��룺

![](./assets/bc4f1f68363da3ea04413f5a004264964e376429.png)

������ʵ����`OP_INHERIT`ָ��֮ǰ����Ҫ���һ����Ե�����

```c
    variable(false);

    if (identifiersEqual(&className, &parser.previous)) {
      error("A class can't inherit from itself.");
    }

    namedVariable(className, false);
// compiler.c, in classDeclaration()
```

һ���಻�������Լ��ĳ��ࡣ�������ܽӴ���һ��������ҵĺ�����ѧ�Һ�һ�����������޸ĵ� DeLorean���������޷����Լ�����̳С�

> ��Ȥ���ǣ�ͨ������ʵ�ַ����̳еķ�ʽ������Ϊ����ѭ��ʵ���ϲ����� clox �������κ����⡣���������κ�*����*�����飬���Ҳ���Ϊ���ᵼ�±���������ѭ����

### 29.1.1 ִ�м̳�

���ڽ�����ָ�

```c
  OP_CLASS,
  OP_INHERIT,
  OP_METHOD
// chunk.h, in enum OpCode
```

���赣�Ĳ�������������Ҫ������ֵ������������ࡪ����������ջ���ҵ�������ζ�ŷ���������ס�

```c
      return constantInstruction("OP_CLASS", chunk, offset);
    case OP_INHERIT:
      return simpleInstruction("OP_INHERIT", offset);
    case OP_METHOD:
// debug.c, in disassembleInstruction()
```

�������Ƕ��������ĵط���

```c
        break;
      case OP_INHERIT: {
        Value superclass = peek(1);
        ObjClass* subclass = AS_CLASS(peek(0));
        tableAddAll(&AS_CLASS(superclass)->methods,
                    &subclass->methods);
        pop(); // Subclass.
        break;
      }
      case OP_METHOD:
// vm.c, in run()
```

��ջ�Ķ������£������࣬Ȼ���ǳ��ࡣץס��������Ȼ����м̳С������ clox ������ jlox ��ͬ��·���ĵط��������ǵĵ�һ���������У�ÿ�����඼�洢�˶��䳬������á��ڷ��������ϣ����û��������ķ��������ҵ��÷�������ͨ���̳����ݹ����ÿ�����ȵķ�����ֱ���ҵ�Ϊֹ��

���磬����Cruller ��һ��ʵ�� `cook()`������ jlox�ĵ���·����

![](./assets/082a52684966fc058c6ba720d35611654891c09b.png)

�ڷ��������ڼ���Ҫִ�д��������������������Ҹ�����ǣ��̳еķ�������������ԽԶ������Խ��������һ���ܰ��ı��ݹ��¡�

�·���Ҫ��öࡣ��������ʱ�������м̳���ķ������Ƶ������Լ��ķ������С��Ժ�*����*һ������ʱ���ӳ���̳е��κη����������������Լ��ķ��������ҵ����̳и�������Ҫ���������ʱ���������౻����ʱ�������Ѿ���ɡ�����ζ�ż̳еķ�����������ͨ��������һ���졪��һ�ι�ϣ����ҡ�

![](./assets/9043ca24aa826d5cf75184c068a8a36fec5e139e.png)

> �ðɣ��Ҳ������ι�ϣ����ҡ���Ϊ�������Ǳ���ȷ��ʵ���ϵ��ֶβ������ظ÷�����

����ʱ��˵�����ֳ�Ϊ�����¸��Ƽ̳С��ļ��������ȼ��ֿ��٣����������Ż�һ������ֻ����ĳЩ����������ʹ���������� Lox ����Ч����Ϊ Lox ����**���**�ġ�һ�����������ִ�У�����ķ���������Զ����ı䡣

�� Ruby��Python �� JavaScript �������У������ƽ������ಢ��һЩ�·����������У�����ɾ�����ǡ�*����ƻ����ǵ��Ż�����Ϊ�������������ִ�к�*��Щ�޸ķ����ڳ����ϣ����ཫ���������Щ���ġ���������û������������̳����Ƿ�ӳ����ĵ�ǰ״̬��

> ��������������ʱ����������ʽ����ķ�������ʹ����������������һ���ǳ�ǿ��Ĺ��ߣ���Ҳ��һ��Σ�յĹ��ߡ�
> 
> ��Щ����������߿����е�*̫*Σ�յ��˸���ȡ��һ����ǡ�������֡����Ӳ����������߸�����ò�ġ�Ѽ�Ӵ�ס���

![](./assets/50101884390b6acc54ac970a53d57618366b91b4.png)

��������˵���˵��ǣ����Ҳ¶�ϲ���ù��ܵ��û���˵���ǣ���Lox ���������޲����ӻ��Ѽ�ӣ�������ǿ��԰�ȫ��Ӧ�ô��Ż���

���������أ�������ķ������Ƶ�����ķ������в����������Լ��ķ���������ͻ�����˵��ǣ�û�С������ڴ�������`OP_CLASS`��ָ��֮�����κη���������`OP_METHOD`ָ�����֮ǰ����`OP_INHERIT`�����������¸��Ƴ���ķ���ʱ������ķ������ǿյġ����า�ǵ��κη����������Ǳ�����Щ�̳е���Ŀ��

### 29.1.2 ��Ч�ĳ���

���ǵ�ʵ�ּ򵥶����٣���������ϲ�� VM ����ķ�ʽ������������׳��û��ʲô������ֹ�û��Ӹ���������Ķ���̳У�

```c
var NotClass = "So not a class";
class OhNo < NotClass {}
```

��Ȼ��û������ĳ���Ա������д�������������Щû�������Ǳ�� Lox �û���һ���򵥵�����ʱ�����Խ��������⡣

```c
        Value superclass = peek(1);
        if (!IS_CLASS(superclass)) {
          runtimeError("Superclass must be a class.");
          return INTERPRET_RUNTIME_ERROR;
        }

        ObjClass* subclass = AS_CLASS(peek(0));
// vm.c, in run()
```

������Ǵӳ����Ӿ��еı�ʶ�����ص�ֵ���� ObjClass������������ʱ�������û�֪�����Ƕ����Ǽ������Ŀ�����

## 29.2 �洢����

����û��ע�⵽����������ӷ����̳�ʱ��ʵ���ϲ�û������κδ����ൽ���ĳ�������ã������Ǹ��Ƽ̳еķ���֮��������ȫ�����˳��ࡣ���ǲ���Ҫ��������ľ�����������ǲ���Ҫ��

�ⲻ����֧��super ���á�����������ܻḲ�ǳ��෽�������������Ҫ�ܹ��Ӵ������෽�������������۸û���֮ǰ����������һ��super��������ξ�̬�����ġ�

> �����ܡ�����ʿ��ܲ������������Ʋ�÷���*��*�����ǡ�������ΪʲôҪ����ȥʹ��`super`������ֱ�ӵ������أ�

�ص� jlox ��̫ƽʢ����������չʾ��[������ֵ�ʾ��](http://craftinginterpreters.com/inheritance.html#semantics)������super���õĵ��ȷ�ʽ��

```c
class A {
  method() {
    print "A method";
  }
}

class B < A {
  method() {
    print "B method";
  }

  test() {
    super.method();
  }
}

class C < B {}

C().test();
```

��`test()`�������ڣ�`this`�� C ��һ��ʵ�������super�����������*������*�ĳ�������ģ���ô���ǽ��� C �ĳ��� B �в��ҡ�����super�������������Χ��ĳ�������ġ�����������£������� B ��`test()`�����У����Գ����� A������Ӧ�ô�ӡ��A method����

����ζ��super���ò����������ʱʵ����̬���������ڲ��ҷ����ĳ����ǵ��÷���λ�õľ�̬��ʵ�����Ǵʷ������ԡ��������� jlox ��Ӽ̳�ʱ�����������˴ʷ�������ľ�̬��,������洢����ʷ���������ͬ�Ļ����С������ͺ����������������ĳ����������ģ�

```c
class A {
  method() {
    print "A method";
  }
}

var Bs_super = A;
class B < A {
  method() {
    print "B method";
  }

  test() {
    runtimeSuperCall(Bs_super, "method");
  }
}

var Cs_super = B;
class C < B {}

C().test();
```

ÿ�����඼��һ�����ر����洢���䳬������á�ÿ����Ҫִ��super����ʱ���Ӹñ������ʳ��ಢ��������ʱ��ʼ������Ѱ�ҷ�����

���ǽ������� clox ��ͬ�ķ�������֮ͬ�����ڣ�����ӵ���ֽ��� VM ��ֵջ����ֵϵͳ�������� jlox �Ķѷ��价���ࡣ�����е㲻ͬ��������Ч����һ���ġ�

### 29.2.1 һ������ľֲ�����

���ǵı������Ѿ�������������������ص�ջ�ϡ�����û�н��ò�۱���Ϊ��ʱ���������Ǵ�����һ���������򲢽�����Ϊ�ֲ�������

```c
    }

    beginScope();
    addLocal(syntheticToken("super"));
    defineVariable(0);

    namedVariable(className, false);
    emitByte(OP_INHERIT);
// compiler.c, in classDeclaration()
```

����һ���µĴʷ�������ȷ�����������ͬһ�����������������࣬ÿ���඼��һ����ͬ�ı��ز����洢�䳬�ࡣ�����������ǽ������������Ϊ��super����������ǲ�Ϊÿ�����ഴ��һ�������򣬱����ͻᷢ����ͻ��

���ǽ���������Ϊ��super����ԭ����ʹ�á�this����Ϊ���ʽ����Ϊ�����ؾֲ��������Ƶ�ԭ����ͬ����super����һ�������֣�����֤�����������ر����������û������ͻ��

��֮ͬ�����ڣ��ڱ���`this`���ʽʱ�����ǿ��Է����ʹ��һ������Ϊ��this����token������������û��ô���ˡ��෴�����������һ��С����������Ϊ�����ĳ����ַ�������һ���ϳɱ�ǡ�

```c
static Token syntheticToken(const char* text) {
  Token token;
  token.start = text;
  token.length = (int)strlen(text);
  return token;
}
// compiler.c, add after variable()
```

> ��˵�������ַ���������Ϊtoken��������ؽ����κ��ڴ����������ǳ���Ϊ��ʹ�öѷ�����ַ������������ջ�й©�ڴ棬��Ϊ����Զ���ᱻ�ͷš����� C �ַ������ֵ��ڴ�λ�ڿ�ִ���ļ��ĳ������ݲ��֣���Զ����Ҫ�ͷţ��������Ǻܺá�

��������Ϊ����������˾ֲ������������Ҫ�ر�����

```c
  emitByte(OP_POP);

  if (classCompiler.hasSuperclass) {
    endScope();
  }

  currentClass = currentClass->enclosing;
// compiler.c, in classDeclaration()
```

�ڱ��������弰�䷽�������ǵ��������򲢶�����super���������������ñ����Ϳ�������������з����з��ʡ�����һ���е����������Ż���������ֻ�����г����Ӿ�ʱ�Ŵ���*��Χ*����ˣ�ֻ���ڴ���һ����Χʱ�����ǲ���Ҫ�رշ�Χ��

Ϊ�˸�����һ�㣬���ǿ�����`classDeclaration()`�ж���һ���ֲ����������Ǻܿ죬�������е�������������Ҫ֪����Χ�����Ƿ������ࡣ�������ǲ�������δ�����Լ������ڽ������ʵ��Ϊһ���ֶδ洢�� ClassCompiler �С�

```c
typedef struct ClassCompiler {
  struct ClassCompiler* enclosing;
  bool hasSuperclass;  //<<
} ClassCompiler;
// compiler.c, in struct ClassCompiler
```

�����ǵ�һ�γ�ʼ��һ�� ClassCompiler ʱ������������һ�����ࡣ

```c
  ClassCompiler classCompiler;
  classCompiler.hasSuperclass = false;
  classCompiler.enclosing = currentClass;
// compiler.c, in classDeclaration()
```

Ȼ���������һ�������Ӿ䣬��֪�����ڱ���һ������

```c
    emitByte(OP_INHERIT);
    classCompiler.hasSuperclass = true;
  }
// compiler.c, in classDeclaration()
```

���ֻ����ṩ��һ��������ʱ��������κη����з�����Χ����ĳ������Ļ��ơ���ֻ�跢��������������Ϊ��super���ı������ñ����Ƿ��������ⲿ�ľֲ����������������е���ֵ֧��ʹ VM �ܹ����񷽷������ڲ��ľֲ�������������Ƕ���ڸ÷����ڵĺ����еľֲ�������

## 29.3 ���ó��෽��

��������ʱ֧�֣��Ϳ���ʵ��super�����ˡ�������һ�������Ǵ�ǰ���󣬴����﷨��ʼ��super ������Ȼ���Թؼ��ֿ�ʼ��`s

> �������������ѡ���Ҫ��ӵ��������е����һ����Ŀ��

```c
  [TOKEN_RETURN]        = {NULL,     NULL,   PREC_NONE},
  [TOKEN_SUPER]         = {super_,   NULL,   PREC_NONE},
  [TOKEN_THIS]          = {this_,    NULL,   PREC_NONE},
// compiler.c, replace 1 line
```

�����ʽ�������ҵ�һ��`super`tokenʱ������Ȩ��ת��һ���µĽ�������������������ʼ��

```c
static void super_(bool canAssign) {
  consume(TOKEN_DOT, "Expect '.' after 'super'.");
  consume(TOKEN_IDENTIFIER, "Expect superclass method name.");
  uint8_t name = identifierConstant(&parser.previous);
}
// compiler.c, add after syntheticToken()
```

�������Ǳ���`this`���ʽ�ķ�ʽ�кܴ�ͬ���� `this`��ͬ��`super` token���Ƕ����ı��ʽ���෴�����������ķ����������﷨���ɷָ�Ĳ��֡����ǣ������ڵĲ����б��Ƿֿ��ġ�����ͨ��������һ����Lox ֧���ڲ�������������½��Գ��෽����������Ϊ�հ���ȡ��

> �������⣺���һ����`super`token��һ�����ʽ����������ʲô���Ķ���

```js
class A {
  method() {
    print "A";
  }
}

class B < A {
  method() {
    var closure = super.method;
    closure(); // Prints "A".
  }
}
```

���仰˵��Lox ��û��������super���ñ��ʽ������super*����*���ʽ�������Ը�����Ҫѡ���������á���ˣ�������������һ��`super`tokenʱ�����ǻ�ʹ�ú���`.`��ǣ�Ȼ����ҷ������ơ������Ƕ�̬���ҵģ���������ʹ��`identifierConstant()`��������token �Ĵ�λ������洢�ڳ������У��������Ƕ����Է��ʱ��ʽ������������

�����Ǳ�������ʹ����Щ��Ǻ����������飺

```c
  uint8_t name = identifierConstant(&parser.previous);

  namedVariable(syntheticToken("this"), false);
  namedVariable(syntheticToken("super"), false);
  emitBytes(OP_GET_SUPER, name);
}
// compiler.c, in super_()
```

Ϊ�˷��ʵ�ǰʵ���ĳ��෽��������ʱ��Ҫ�����ߺ���Χ����������ĳ��ࡣ��һ�ε���`namedVariable()`���ɴ��������Ҵ洢�����ر�����this���еĵ�ǰ������������ѹ��ջ���ڶ��ε���`namedVariable()`���������Դ��䡰super�������в��ҳ��ಢ����push��ջ����

������Ƿ���һ����`OP_GET_SUPER`ָ����н������������Ƶĳ�����������Ϊ�������������������̫���ˡ�Ϊ��ʹ����廯���뿼������ʾ������

```c
class Doughnut {
  cook() {
    print "Dunk in the fryer.";
    this.finish("sprinkles");
  }

  finish(ingredient) {
    print "Finish with " + ingredient;
  }
}

class Cruller < Doughnut {
  finish(ingredient) {
    // No sprinkles, always icing.
    super.finish("icing");
  }
}
```

Ϊ���� `super.finish("icing")`  ���ʽ���ֽ��뿴�����͹�����ʽ���£�

![](./assets/5d163767eefe05e78193f2f36260ee14ca3adde4.png)

ǰ����ָ��������ʱ����ִ�з���super�����������Ϣ��

1. ��һ��ָ�**ʵ��**���ص�ջ�ϡ�

2. �ڶ���ָ�����**�����÷����ĳ���**��

3. Ȼ����`OP_GET_SUPER`ָ�**Ҫ���ʵķ���������**����Ϊ��������

����ָ�����������������б�͵��ú�������ͨ�ֽ��롣

���Ǽ���׼�����ڽ�������ִ����ָ��`OP_GET_SUPER`�ˡ�����������֮ǰ���������Ḻ�𱨸�һЩ����

```c
static void super_(bool canAssign) {
  if (currentClass == NULL) {
    error("Can't use 'super' outside of a class.");
  } else if (!currentClass->hasSuperclass) {
    error("Can't use 'super' in a class with no superclass.");
  }

  consume(TOKEN_DOT, "Expect '.' after 'super'.");
// compiler.c, in super_()
```

super ���ý��ڷ������ڣ���Ƕ���ڷ����ڵĺ����У������壬���ҽ��ھ��г������ķ����ڲ������塣����ʹ�� `currentClass`��ֵ�������������������`NULL`��ָ��һ��û�г�����࣬��������Щ����

### 29.3.1 ִ�� super����

�����û�û���ڲ�����ĵط�����`super`���ʽ������ͻ�ӱ��������ݵ�����ʱ�����Ǹ��Լ�һ����ָ�

```c
  OP_SET_PROPERTY,
  OP_GET_SUPER,
  OP_EQUAL,
// chunk.h, in enum OpCode
```

�������������ó����������������Ĳ�����һ���������

```c
      return constantInstruction("OP_SET_PROPERTY", chunk, offset);
    case OP_GET_SUPER:
      return constantInstruction("OP_GET_SUPER", chunk, offset);
    case OP_EQUAL:
// debug.c, in disassembleInstruction()
```

�����ܻ�Ԥ�ϵ�һЩ���ѵ����飬��������ָ��������ִ�����������Է��ʡ�

```c
      }
      case OP_GET_SUPER: {
        ObjString* name = READ_STRING();
        ObjClass* superclass = AS_CLASS(pop());

        if (!bindMethod(superclass, name)) {
          return INTERPRET_RUNTIME_ERROR;
        }
        break;
      }
      case OP_EQUAL: {
// vm.c, in run()
```

������һ�����ӳ������ж�ȡ�������ơ�Ȼ�����ǽ������ݸ�`bindMethod()`�����ڸ�����ķ������в��ҷ���������һ�� ObjBoundMethod �Խ����ɵıհ����󵽵�ǰʵ����

�ؼ������������Ǵ����ĸ��� ��`bindMethod()`��ͨ�����������Է��ʣ�����ʹ�� ObjInstances �Լ����࣬���ṩ��������Ҫ�Ķ�̬���ȡ�����super���ã����ǲ�ʹ��ʵ�����ࡣ�෴��ʹ�ð�����ľ�̬�������࣬�������ѷ����ȷ����λ��ջ�����ȴ����ǡ�

���ǵ����ó��ಢ���䴫�ݸ�`bindMethod()`�������ȷ�������ó����ʵ���Լ�����֮����κ������е��κ���д������������ȷ�ذ��������κγ���̳е��κη�����

������Ϊ��ͬ��pop ����Ὣʵ������ջ�Ķ�����`bindMethod()`�ɹ�ʱ�����ᵯ��ʵ����push�µİ󶨷������������ᱨ������ʱ���󲢷���`false`������������£�������ֹ��������

> ��`OP_GET_PROPERTY`֮��ȵ���һ�����������ǲ������ȳ���Ѱ���ڱ��ֶΡ��ֶβ��Ǽ̳еģ�����`super`���ʽ���ǽ���Ϊ������
> 
> ��� Lox ��һ�ֻ���ԭ�͵����ԣ���ʹ��*ί��*������*�̳�*����ôʵ�����̳У���ί�и���������ʵ����������һ��*��*����һ����̳С�����������£��ֶο��Ա��̳У�������Ҫ�����������ǡ�

### 29.3.2 ����super ����

���ڿ��Է��ʳ��෽�������ڷ��صĶ����������Ե��õ� ObjBoundMethod���������Ҳ����ʹ��super*����*��������һ��һ���������Ѿ��ﵽ�� VM ������������ȷ����ĵز���

���ǣ�Ҳ����һ��һ������������ͬ��������Ϊÿ��super�����ڶ��Ϸ���һ�� ObjBoundMethod����ʹ�����ʱ����һ��ָ����`OP_CALL`��������󶨷�����������Ȼ��������ָ���ʵ�ϣ��볣�淽��������ȣ�super���ø�������ˡ����ٶ��ڷ������ã��û�ʵ�����п��ܵ��ô洢���ֶ��еĺ�����ʹ�� super ���ã���*����*�ڲ��ҷ�����Ψһ�����������Ƿ�������������

����������ڳ��෽�����ƺ󿴵������ţ�����Ȼ�����Լ��ش�������⣬�������ǽ�����ִ���뷽��������ͬ���Ż���ȡ�����س���ͷ��������д��룬`OP_GET_SUPER`���������滻Ϊ��

```c
  namedVariable(syntheticToken("this"), false);
  if (match(TOKEN_LEFT_PAREN)) {
    uint8_t argCount = argumentList();
    namedVariable(syntheticToken("super"), false);
    emitBytes(OP_SUPER_INVOKE, name);
    emitByte(argCount);
  } else {
    namedVariable(syntheticToken("super"), false);
    emitBytes(OP_GET_SUPER, name);
  }
}
// compiler.c, in super_(), replace 2 lines
```

���ڣ������Ƿ����κζ���֮ǰ����Ѱ��һ�������ŵĲ����б�����ҵ�һ�����ͱ�������Ȼ����س��ࡣ֮�󣬷���һ����`OP_SUPER_INVOKE`ָ��������ָ������ `OP_GET_SUPER`��`OP_CALL` ����Ϊ����������Ҫ������������Ҫ���ҵķ������Ƶĳ�����������Ҫ���ݸ����Ĳ�����������

> ����һ���ر�*super*��superָ�����������ҵ���˼�Ļ�����...��Ϊ������µ�Ц���е���Ǹ��

�����������û���ҵ� `(`������������ǰһ�������ʽ����Ϊsuper���ʲ�����һ��`OP_GET_SUPER`.

���ű�����ˮ��Ư�������ǵĵ�һվ�����һ����ָ�

```c
  OP_INVOKE,
  OP_SUPER_INVOKE,
  OP_CLOSURE,
// chunk.h, in enum OpCode
```

���������ķ�������֧�֡�

```c
      return invokeInstruction("OP_INVOKE", chunk, offset);
    case OP_SUPER_INVOKE:
      return invokeInstruction("OP_SUPER_INVOKE", chunk, offset);
    case OP_CLOSURE: {
// debug.c, in disassembleInstruction()
```

super����ָ���� `OP_INVOKE`������ͬ�Ĳ������������������ͬ�ĸ��麯���������������󣬹ܵ�������ת�����������С�

```c
        break;
      }
      case OP_SUPER_INVOKE: {
        ObjString* method = READ_STRING();
        int argCount = READ_BYTE();
        ObjClass* superclass = AS_CLASS(pop());
        if (!invokeFromClass(superclass, method, argCount)) {
          return INTERPRET_RUNTIME_ERROR;
        }
        frame = &vm.frames[vm.frameCount - 1];
        break;
      }
      case OP_CLOSURE: {
// vm.c, in run()
```

��һ�δ����������ʵ��`OP_INVOKE`��������`OP_GET_SUPER`������ջ����֯��ʽ����һЩ���졣����δ�Ż���super���ã���ִ�е��õĲ���֮ǰ�����౻�������滻Ϊ�ѽ��������� ObjBoundMethod ����ȷ����`OP_CALL`ִ��ʱ���󶨷���λ�ڲ����б��£�����ʱ���������ڱհ����á�

ͨ�������Ż���ָ��������е���ң�

![](./assets/c0fa22f4ae5b19cc74061114c0b2413936072e7c.png)

���ڽ������෽���ǵ��õ�һ���֣����Բ�����Ҫ�����ǲ��ҷ���ʱ�Ѿ��ڶ�ջ�ϡ�����ζ�ų������λ�ڲ���֮�ϡ�

����֮�⣬��Ϊ�� `OP_GET_SUPER`�����`OP_CALL`������ͬ�����ȣ�������ȡ�������ƺͲ���������������Ȼ�󽫳����ջ���������Ա���������ķ������в��Ҹ÷������ⷽ���ʹջ����ǡ���ʺϷ������á�

���ǽ����ࡢ�������ƺͲ����������ݸ�����`invokeFromClass()`�������ú����ڸ��������ϲ��Ҹ����ķ�����������ʹ�ø������������������ĵ��á�����Ҳ������������᷵��`false`�����˳�������������`invokeFromClass()` ��Ϊ�����ıհ�(?method��s closure)   pushһ���µ� CallFrame������ջ�ϡ����ʹ����������� CallFrame ָ����Ч���������ˢ����`frame`��

## 29.4 һ�������������

�ع�һ�����Ǵ��������ݡ�����ͳ�ƣ����Ǳ�д�˴�Լ 2,500 ���൱�ɾ���ֱ�ӵ� C ���롣�Ǹ�С���������һ��������ʵ�֡����ǳ��߼�����Lox ���ԣ���һ���������ʽ���͵��������ȼ����һ�׿�������䡣����ʵ���˱������������հ����ࡢ�ֶΡ������ͼ̳С�

������ӡ����̵��ǣ����ǵ�ʵ�ֿ���ֲ���κδ��� C ��������ƽ̨�������ٶ���������ʵ������ʹ�á�������һ�������ֽ����������һ�����������ڲ�ָ��Ľ�������������������յĶ����ʾ��һ�����ڴ洢����������ѷ����ջ���Լ�һ����ȷ�������ռ�����

�������ȥ����ʼ�о� Lua��Python �� Ruby ��ʵ�֣����ᾪ�ȵط��� ���ڶ�����˵�����кܶ��Ѿ�����Ϥ�����Ѿ����������˶Ա�����Թ���ԭ����˽⣬�ⷴ������ʹ���Ա�̱������˸�������˽⡣�������������һ�������֣�������Ҳ���Դ���������������ˡ�

�����Ը�⣬�����������ͣ��������ӵ�е� Lox ������ʵ���������ҹ�����ȫ�ġ������������������ڿ��԰��������κ�����ȥ�ĵط������ǣ������ϣ�����и���Ȥ�ĵ����͵������������ϻ�ø��õ����ܣ���ô����һ�¡�����û������κ��¹��ܣ��������Ƴ���һЩ�����Ż��Լ����������ܡ����������������Ȥ��[������Ķ�](http://craftinginterpreters.com/optimization.html)...

---

## [��ս](http://craftinginterpreters.com/superclasses.html#challenges)

1. ��������̵�һ��ԭ������Ӧ��ȷ���¶�������Ч״̬���� Lox �У�����ζ�Ŷ���һ����ʼ���������ʵ�����ֶΡ��̳�ʹ��������ø��ӣ���Ϊ���ݶ���̳����е������࣬ʵ�����봦����Ч״̬��
   
   �򵥵Ĳ����Ǽ�ס��ÿ�������`init()`�����е���`super.init()`�����ѵĲ������ֶΡ�û��ʲô������ֹ�̳����е�����������������ͬ���ֶ����ơ������������ʱ�����ǻ�ȵ��˴˵��ֶΣ�������ʹ����ʵ��������״̬��
   
   ��� Lox ���������ԣ�������ν��������⣨����еĻ����������Ҫ�������ԣ���ʵ�����ĸ��ġ�

2. ���ǵ����¸��Ƽ̳��Ż�֮������Ч��ֻ����Ϊ Lox �����������������޸���ķ���������ζ�����ǲ��ص��������и��Ƶķ������Ժ�Գ���ĸ��Ĳ�ͬ����
   
   �������ԣ��� Ruby��*ȷʵ*�������º��޸��ࡣ�������Ե�ʵ������ڱ��ַ���������Ч��ͬʱ֧�����޸ģ�

3. ��[���ڼ̳е� jlox �½���](http://craftinginterpreters.com/inheritance.html)������������ʵ�� BETA ���Եķ������Ƿ�������ս���ٴν����ս����������� clox �С����Ƕ���ǰ��ս��������
   
   �� Lox �У����ڴ���������������������һ�����ڲ��ҷ���ʱ�����Ǵ����νṹ�ĵײ���ʼ��Ȼ�������ϡ�������ķ������ڳ���ķ�����Ϊ�˴Ӹ��Ƿ����л�ȡ���෽����������ʹ��`super`.
   
   ����[BETA](https://beta.cs.au.dk/)����[�෴�ķ���](http://journal.stuffwithstuff.com/2012/12/19/the-impoliteness-of-overriding-methods/)���������һ������ʱ���������νṹ��*������ʼ�����¹��������෽��ʤ�����෽����Ϊ�˵������෽�������෽�����Ե���`inner`�����е��� ��������`super`�������ӵ���νṹ�е���һ��������
   
   ���෽�����ƺ�ʱ�ε���������Ľ�����Ϊ��������෽������������`inner`����ô�����û�а취���ǻ��޸ĳ������Ϊ��
   
   ȥ�� Lox ��ǰ�� override ��`super`���ƣ����� BETA �����塣�����֮��
   
   - ������ķ���ʱ����̳�����*���*�ķ������ȡ�
   
   - �ڷ������ڣ��� `inner`�ĵ���������������в��Ҿ�����ͬ���Ƶķ���    ���Ű��� ��`inner` �����`this`����֮��ļ̳��������û��ƥ��ķ�������`inner`���ò�ִ���κβ�����
   
   ���磺
   
   ```js
   class Doughnut {
     cook() {
       print "Fry until golden brown.";
       inner();
       print "Place in a nice box.";
     }
   }
   
   class BostonCream < Doughnut {
     cook() {
       print "Pipe full of custard and coat with chocolate.";
     }
   }
   
   BostonCream().cook();
   ```
   
   ��Ӧ�ô�ӡ��
   
   ```
   Fry until golden brown.
   Pipe full of custard and coat with chocolate.
   Place in a nice box.
   ```
   
   ���� clox ����Ҫʵ�� Lox������Ҫ�����õ�����ʵ�֣�������һ�γ���������Ч���������ս��