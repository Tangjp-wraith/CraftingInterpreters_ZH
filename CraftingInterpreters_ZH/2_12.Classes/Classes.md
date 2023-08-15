# ��

> ���һ����û����ȫ�˽����ı��ʣ���ô����û��Ȩ��ȥ���������ΰ��İ�Դ�ڶ��İ�����������˽⣬��������֪֮���٣��㽫ֻ�ܰ���һ���������������
> 
> -- Leonardo da Vinci

�����Ѿ�����ʮһ�£��������ϵĽ�����������һ�������Ľű����ԡ�������ʹ��һЩ���õ����ݽṹ���� list �� map������Ȼ��Ҫһ�������ļ� I/O���û�����ȵĺ��Ŀ⡣�����Ա�����㹻�ˡ�������һЩ�� BASIC��Tcl��Scheme��ȥ���꣩�Լ����ڰ汾�� Python �� Lua ��ͬ�Ĺ������ԡ�

������� 80 ��������Ǿͻᵽ��Ϊֹ�����ǽ��죬������е����Զ�֧�֡���������̡���������ӵ� Lox ��Ϊ�û��ṩһ����Ϥ�Ĺ�������д����ĳ��򡣼�ʹ�����˲�ϲ��OOP�����º�[��һ��](http://craftinginterpreters.com/inheritance.html)Ҳ��������˽������������ƺ͹�������ϵͳ��

>  ���ǣ������*���*����class ����������������¡������뱾������ಿ���൱���������Ҹ��˶��ԣ��ҷ��ֶ��˽�һЩ�Ҳ�ϲ���������Ǽ����¡�������Զ���������ܼ򵥣��������߽�ʱ��ϸ�ھͻḡ�֣��һ��ø�ϸ΢���ӽǡ�

## 12.1 OOP ����

ʵ����������̵�����;������class ��[ԭ��](http://gameprogrammingpatterns.com/prototype.html)��[�෽��](https://en.wikipedia.org/wiki/Multiple_dispatch)��class ���ڵ�һλ�������ܻ�ӭ�ķ������ JavaScript���Լ���С�̶ȵ�[Lua](https://www.lua.org/pil/13.4.1.html)��������ԭ�ͱ���ǰ����Ϊ��֪��[���Ժ�](http://craftinginterpreters.com/classes.html#design-note)����ϸ������Щ������ Lox�����ǲ��õ��Ǿ��䷽����

> �෽�������������Ϥ�ķ������Һ����̸̸���ǡ���������Χ�����������[һ��ҵ������](http://magpie-lang.org/)������*�ǳ���*��������ֻ��д��ô��ҳ����������˽���࣬����[CLOS](https://en.wikipedia.org/wiki/Common_Lisp_Object_System)��Common Lisp �еĶ���ϵͳ����[Dylan](https://opendylan.org/)��[Julia](https://julialang.org/)��[Raku](https://docs.raku.org/language/functions#Multi-dispatch)��

�������Ѿ�����һ���д�˴�Լһǧ�� Java ���룬�����Ҽ���������Ҫ��ϸ�������������ҪĿ���ǽ����������������Ĵ���������һ���û�ͨ������һ��*��*��������һ�㣺

1. ����*���캯��*�Դ����ͳ�ʼ�������*ʵ��*

2. �ṩһ����ʵ���ϴ洢�ͷ���*�ֶεķ���*

3. ����һ�����������ʵ�������*��������Щ������ÿ��ʵ����״̬���в�����*

�⼸������С�ġ�����������������ԣ�һֱ׷�ݵ� Simula��Ҳ���м̳��Կ���������Ϊ�����ǽ���[��һ��](http://craftinginterpreters.com/inheritance.html)�����������ʹ�����߳�ȥ�����ǻ��кܶ�����Ҫ��������һ����Ҫ���½ڣ�������ӵ��������������֮ǰ��һ�ж�������ȫ�ں���һ�����Լ������������

![](./assets/2f4760b7aed09b025d24f312b2e9dbd2ef6bec9f.png)

> ����������ѭ����*û�а�����Լ��*��ʿ��

## 12.2 ������

��֮ǰ����һ�������ǽ����﷨��ʼ�����������һ��`class`�����ƣ������������`declaration`�﷨�����С�

```
declaration    �� classDecl
               | funDecl
               | varDecl
               | statement ;

classDecl      �� "class" IDENTIFIER "{" function* "}" ;
```

��`classDecl`��������������[֮ǰ](http://craftinginterpreters.com/functions.html#function-declarations)`function`����Ĺ���ˢ����ļ��䣺

```
function       �� IDENTIFIER "(" parameters? ")" block ;
parameters     �� IDENTIFIER ( "," IDENTIFIER )* ;
```

��Ӣ���У���������`class`�ؼ��֣����������Ȼ���ǻ��������塣�������ڲ���һ�����������б��뺯��������ͬ������û��ǰ��`fun`�ؼ��֡�ÿ����������һ�����ơ������б�����塣����һ�����ӣ�

> ע�⣬�ҳ���˵�������ͺ�����ͬ

```js
class Breakfast {
  cook() {
    print "Eggs a-fryin'!";
  }

  serve(who) {
    print "Enjoy your breakfast, " + who + ".";
  }
}
```

��������̬��������һ�����ֶ�δ��ȷ�����������С�ʵ������ɢ�����ݰ���������ʹ����ͨ����ʽ�������ɵ��������������Ϊ���ʵ��ֶΡ�

�����ǵ� AST �������У�`classDecl`�﷨�������Լ������ڵ㡣

```java
      "Block      : List<Stmt> statements",
      "Class      : Token name, List<Stmt.Function> methods",
      "Expression : Expr expression",
// tool/GenerateAst.java, in main()
```

> Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#class-statement)�С�

����������ƺͷ����洢���������С����������ں������� AST �ڵ������ Stmt.Function ���ʾ����Ϊ�����ṩ�˷������������״̬�����ơ������б�����塣

����Գ������κ��������������ĵط�����ǰ��`class`�ؼ��ִ�����

```java
    try {
      if (match(CLASS)) return classDeclaration();
      if (match(FUN)) return function("function");
// lox/Parser.java, in declaration()
```

����ã�

```java
  private Stmt classDeclaration() {
    Token name = consume(IDENTIFIER, "Expect class name.");
    consume(LEFT_BRACE, "Expect '{' before class body.");

    List<Stmt.Function> methods = new ArrayList<>();
    while (!check(RIGHT_BRACE) && !isAtEnd()) {
      methods.add(function("method"));
    }

    consume(RIGHT_BRACE, "Expect '}' after class body.");

    return new Stmt.Class(name, methods);
  }
// lox/Parser.java, add after declaration() 
```

��������������������ȣ����и������ݣ�����������ѭ�﷨���Ѿ�������`class`�ؼ��֣����Խ�����Ѱ��Ԥ�ڵ�������Ȼ���������š�һ���������壬��������������������ֱ�������Ҵ����š�ÿ������������ͨ���� �ĵ��ý��н���`function()`����[���ܺ������½���](http://craftinginterpreters.com/functions.html)����������

�����ڽ������е��κο���ʽѭ����������������������Ƿ񵽴��ļ�ĩβ���ⲻ�ᷢ������ȷ�Ĵ����У���Ϊ���ĩβӦ����һ���Ҵ����ţ�������û����﷨�����������ȷ���������壬������ȷ��������������������ѭ����

�����ƺͷ����б��װ�� Stmt.Class �ڵ��У�Ȼ�������ˡ���ǰ�����ǻ�ֱ����ת����������������������Ҫ��ͨ�������������ڵ㡣

```java
 @Override
  public Void visitClassStmt(Stmt.Class stmt) {
    declare(stmt.name);
    define(stmt.name);
    return null;
  }
// lox/Resolver.java, add after visitBlockStmt() 
```

���ǻ����õ��Ľ�������������������������Ҫ���ľ���ʹ�����������ࡣ��������Ϊ�ֲ����������������� Lox ���������������������Ҫ��ȷ����

�������ǽ�����������

```java
  @Override
  public Void visitClassStmt(Stmt.Class stmt) {
    environment.define(stmt.name.lexeme, null);
    LoxClass klass = new LoxClass(stmt.name.lexeme);
    environment.assign(stmt.name, klass);
    return null;
  }
// lox/Interpreter.java, add after visitBlockStmt()
```

�⿴��������������ִ�к��������ķ�ʽ�������ڵ�ǰ����������������ơ�Ȼ�����ǽ���*�﷨�ڵ�*��� LoxClass�����*����ʱ*��ʾ�����ǻع�ͷ���������洢������֮ǰ�����ı����С������׶α����󶨹������������Լ��ķ��������ø��ࡣ

���ǽ��������ж���������ƣ��� LoxClass �ĳ���������ʾ��

```java
package com.craftinginterpreters.lox;

import java.util.List;
import java.util.Map;

class LoxClass {
  final String name;

  LoxClass(String name) {
    this.name = name;
  }

  @Override
  public String toString() {
    return name;
  }
}
// lox/LoxClass.java, create new file
```

�������Ͽ���һ�����ֵİ�װ������������û�д洢���������Ǻ����ã�����ȷʵ��һ��`toString()`������������ǿ��Ա�дһ���򵥵Ľű�������������Ƿ���ı�������ִ�С�

```java
class DevonshireCream {
  serveOn() {
    return "Scones";
  }
}

print DevonshireCream; // Prints "DevonshireCream".
```

## 12.3 ����ʵ��

��������class �������ǻ�û�����κ����顣Lox û�п���ֱ�����౾���ϵ��õġ���̬��������������û��ʵ������ͺ����ô������ʵ������һ����

��ȻĳЩ�﷨�������� OOP ���������൱��׼�ģ���������ʵ���ķ�ʽȴ���ǡ�Ruby ��ѭ Smalltalk��ͨ�������������ķ���������ʵ��������һ�����ŵĵݹ鷽������Щ���� C++ �� Java����һ��`new`�ؼ���ר����������һ���¶���Python ��������ú���һ�������á��౾����JavaScript����������֣��е����߼����֮����

> �� Smalltalk �У�����*��*����ͨ���������ж���ͨ��������ĳ��ࣩ�ϵķ����������ġ����е����ڹ�һ·���µ����顣�����ջ���һЩ������ࣨ�� Object �� Metaclass���ϴ��ף�����ʱ���������뵽������*ex nihilo*��

�Ҷ� Lox ��ȡ����С�ķ����������Ѿ����������Ҳ���˺������ã��������ǽ����������ʹ�õ��ñ��ʽ��������ʵ�����ͺ���һ������һ����������ʵ���Ĺ����������������˵�о������ţ�Ҳ����������������`new`.��ˣ����ǿ�������ǰ��ֱ�ӽ�������ʱ��

���ڣ���������������

```js
class Bagel {}
Bagel();
```

���յ�����ʱ����`visitCallExpr()`��鱻���ö����Ƿ�ʵ��`LoxCallable`�ӿڲ����������Ϊ LoxClass û�С���*û��*������������

```java
import java.util.Map;

class LoxClass implements LoxCallable {
  final String name;
// lox/LoxClass.java, replace 1 line
```

ʵ�ָýӿ���Ҫ����������

```java
@Override
  public Object call(Interpreter interpreter,
                     List<Object> arguments) {
    LoxInstance instance = new LoxInstance(this);
    return instance;
  }

  @Override
  public int arity() {
    return 0;
  }
// lox/LoxClass.java, add after toString() 
```

��Ȥ����`call()`�����������á�һ����ʱ������Ϊ�����õ���ʵ����һ���µ� LoxInstance ������������`arity()`�����ǽ����������֤������ȷ�����Ĳ������ݸ��ɵ��ö���ķ�ʽ�����ڣ����ǻ�˵�㲻�ܴ����κβ�����������̸���û�����Ĺ��캯��ʱ�����ǽ�������������

�⽫�������� LoxInstance������ Lox ��ʵ��������ʱ��ʾ��ͬ�������ǵĵ�һ��ʵ�ִ�С�����֡�

```java
package com.craftinginterpreters.lox;

import java.util.HashMap;
import java.util.Map;

class LoxInstance {
  private LoxClass klass;

  LoxInstance(LoxClass klass) {
    this.klass = klass;
  }

  @Override
  public String toString() {
    return klass.name + " instance";
  }
}
// lox/LoxInstance.java, create new file
```

�� LoxClass һ�������ǳ��򵥣������ǲŸոտ�ʼ��������볢��һ�£������������½ű���

```js
class Bagel {}
var bagel = Bagel();
print bagel; // Prints "Bagel instance".
```

����������Ĳ��࣬������ʼ��*һЩ������*��

## 12.4 ʵ������

������ʵ��������Ӧ�����������á����������ڲ�·�ڡ����ǿ����������Ϊ�������������������ǿ��Դ�״̬��ʼ�������ԡ����ǽ����ú��ߣ���Ϊ�������ǽ�������������������һ����Ȥ�ķ�ʽ������һ����������������������ã��ͻ������������ǡ�

Lox �ڴ���״̬������ѭ JavaScript �� Python��ÿ��ʵ����������ֵ�Ŀ��ż��ϡ�ʵ����ķ������Է��ʺ��޸����ԣ����ⲿ����Ҳ���ԡ�ʹ��`.`�﷨�������ԡ�

> �������ⲿ�Ĵ���ֱ���޸Ķ�����ֶ�Υ������*��װ*״̬������������������Щ���Բ�ȡ����ԭ����������� Smalltalk �У�ʹ�ü򵥵ı�ʶ�������ֶΡ������ϣ����������෽���ķ�Χ�ڡ�Ruby ʹ��`@`������������ʶ����е��ֶΡ����﷨���ڷ����ڲ������壬����ʼ�շ��ʵ�ǰ�����״̬��
> 
> �����Ǻ��ǻ���Lox ������������̵�����������ô�ϡ�

```js
someObject.someProperty
```

���`.`һ����ʶ���ı��ʽ�ӱ��ʽ��ֵ�Ķ����ж�ȡ���и����Ƶ����ԡ��õ��뺯�����ñ��ʽ�е����ž�����ͬ�����ȼ����������ͨ��������`call`�����滻Ϊ������������������﷨�У�

```
call           �� primary ( "(" arguments? ")" | "." IDENTIFIER )* ;
```

����Ҫ���ʽ֮����������һϵ�����ŵ��ú͵����Է��ʵ�������ϡ������Է��ʡ����ֿڣ����Դ����ڿ�ʼ�����ǳ���ЩΪ��get ���ʽ����

### 12.4.1 Get���ʽ

�﷨���ڵ�Ϊ:

```java
      "Call     : Expr callee, Token paren, List<Expr> arguments",
      "Get      : Expr object, Token name",
      "Grouping : Expr expression",
// tool/GenerateAst.java, in main()
```

Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#get-expression)�С�

�����﷨���µĽ�������������е�`call()`����

```java
    while (true) { 
      if (match(LEFT_PAREN)) {
        expr = finishCall(expr);
      } else if (match(DOT)) {
        Token name = consume(IDENTIFIER,
            "Expect property name after '.'.");
        expr = new Expr.Get(expr, name);
      } else {
        break;
      }
    }
// lox/Parser.java, in call()
```

�ⲿ`while`ѭ����Ӧ���﷨�����е�`*`����������token����һ���������������ҵ����ź͵�ʱ��ȡ��������ʾ��

![](./assets/4eb893ce64b55b66f25af5eb7bf6cf5e85daf5ab.png)

�� Expr.Get �ڵ��ʵ���ṩ����������

```java
 @Override
  public Void visitGetExpr(Expr.Get expr) {
    resolve(expr.object);
    return null;
  }
// lox/Resolver.java, add after visitCallExpr() 
```

�õģ�����˵�ˡ����������Ƕ�̬���ҵģ���˲���������ǡ��ڽ����ڼ䣬���ǽ��ݹ鵽�����ı��ʽ��ʵ�ʵ����Է��ʷ����ڽ������С�

> �����Դ������Ͽ��� Lox �е����Է����Ƕ�̬�ģ���Ϊ�����ھ�̬���������в������������ơ�

```java
  @Override
  public Object visitGetExpr(Expr.Get expr) {
    Object object = evaluate(expr.object);
    if (object instanceof LoxInstance) {
      return ((LoxInstance) object).get(expr.name);
    }

    throw new RuntimeError(expr.name,
        "Only instances have properties.");
  }
// lox/Interpreter.java, add after visitCallExpr()
```

���ȣ��������������Ա����ʵı��ʽ���� Lox �У�ֻ�����ʵ���������ԡ�������������ֵ��������ͣ��������� getter ������ʱ����

��������� LoxInstance����ô����Ҫ�����������ԡ�һ����ʱ��� LoxInstance һЩʵ��״̬�ˡ�һ��Map�Ϳ����ˡ�

```java
  private LoxClass klass;
  private final Map<String, Object> fields = new HashMap<>();

  LoxInstance(LoxClass klass) {
// lox/LoxInstance.java, in class LoxInstance 
```

Map�е�ÿ��������һ���������ƣ���Ӧ��ֵ�����Ե�ֵ��Ҫ����ʵ�������ԣ�

```java
  Object get(Token name) {
    if (fields.containsKey(name.lexeme)) {
      return fields.get(name.lexeme);
    }

    throw new RuntimeError(name, 
        "Undefined property '" + name.lexeme + "'.");
  }
// lox/LoxInstance.java, add after LoxInstance() 
```

> ��ÿ���ֶη��ʽ��й�ϣ����Ҷ����������ʵ����˵�Ѿ��㹻�죬���������롣���� JavaScript �����Եĸ����� VM ʹ�ø��ӵ��Ż����硰[������](http://richardartoul.github.io/jekyll/update/2015/04/26/hidden-classes.html)�������������ֿ�����
> 
> ì�ܵ��ǣ�Ϊʹ��̬���Ը��������������Ż������������Ĺ۲졪����ʹ����Щ�����С����������������ʹ�õĶ������ͼ����ֶη��涼���൱��̬�ġ�

������Ҫ�����һ����Ȥ�ı�Ե��������ʵ��û��*����*�������Ƶ����Իᷢ��ʲô�����ǿ���ĬĬ�ط���һЩ����ֵ����`nil`�����Ҷ� JavaScript �����Եľ����ǣ�������Ϊ�������κ����õ�������������ڸǴ����෴�����ǻὫ����Ϊ����ʱ����

�����������ĵ�һ�����ǲ鿴ʵ���Ƿ������һ�����и������Ƶ��ֶΡ�ֻ���������ǲŷ��������������ǻ���������

��ע��������δ�̸�ۡ����ԡ��л���̸�ۡ��ֶΡ��ġ�����֮�����ϸ΢����ֶ���**ֱ��**�洢��ʵ���е�����״̬��������get ���ʽ���ܷ��ص�������*����*��ÿ���ֶζ���һ�����ԣ������������Ժ󽫿����ģ�����ÿ�����Զ���һ���ֶΡ�

> �Ǻǣ�Ԥʾ�������ģ�

�����ϣ����ڿ��Զ�ȡ��������ԡ����������޷����κ�״̬ʵ����䵽ʵ���У����û�пɷ��ʵ��ֶΡ��ڲ��Զ�ȡ֮ǰ������֧��д�롣

### 12.4.2 Set���ʽ

Setter ʹ���� getter ��ͬ���﷨��ֻ�����ǳ����ڸ�ֵ����ࡣ

```js
someObject.someProperty = value;
```

���﷨����������չ��ֵ���������������ʹ�õ��ʶ����

```
assignment     �� ( call "." )? IDENTIFIER "=" assignment
               | logic_or ;
```

�� getter ��ͬ��setter ������ʽ���á����ǣ���`call`�������һ����֮ǰ�����κθ����ȼ����ʽ����������������*getter*���磺

![](./assets/081c8ed28f8734759bd2ba3b93796a609eaddaac.png)

ע������ֻ��*���*һ����`.meat`��*setter*��`.omelette`��`.filling`���ֶ���get*���ʽ*��

�������������������� AST �ڵ����ڱ������ʺͱ�����ֵһ����������Ҫ�ڶ��� setter �ڵ����������ǵ� getter �ڵ㡣

```java
      "Logical  : Expr left, Token operator, Expr right",
      "Set      : Expr object, Token name, Expr value",
      "Unary    : Token operator, Expr right",
// tool/GenerateAst.java, in main()
```

> Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#set-expression)�С�

��֪�����Ƿ�ǵã������ڽ������д���ֵ�ķ�ʽ�е㻬����������`=` ֮ǰ�����������޷�ȷ����һϵ��token�Ƿ���һ����ֵ���ʽ����ֵ��  ����`call`�ڸ�ֵ�﷨������࣬������չ�������ı��ʽ������`=`���������ڽ�����ֵ�ĵ�֮���кܶ�token��

�෴������ʹ�õļ����ǽ�������Ϊ��ͨ���ʽ��Ȼ�󣬵�żȻ����������ĵȺ�ʱ�����ǲ����Ѿ������ı��ʽ������ת��Ϊ��ȷ���﷨���ڵ��Խ��и�ֵ��

�������ת�������һ���Ӿ䣬�Դ������� Expr.Get ���ʽת��Ϊ��Ӧ�� Expr.Set��

```java
        return new Expr.Assign(name, value);
      } else if (expr instanceof Expr.Get) {
        Expr.Get get = (Expr.Get)expr;
        return new Expr.Set(get.object, get.name, value);
      }
// lox/Parser.java, in assignment()
```

�Ǿ��ǽ������ǵ��﷨�����ǽ��ýڵ����͵��������С�

```java
  @Override
  public Void visitSetExpr(Expr.Set expr) {
    resolve(expr.value);
    resolve(expr.object);
    return null;
  }
// lox/Resolver.java, add after visitLogicalExpr() 
```

ͬ������ Expr.Get һ�������Ա����Ƕ�̬��ֵ�ģ����û��ʲô��Ҫ�����ġ�������Ҫ���ľ��ǵݹ鵽 Expr.Set �������ӱ��ʽ�У���Ҫ���������ԵĶ��������õ�ֵ��

�⽫���������������

```java
  @Override
  public Object visitSetExpr(Expr.Set expr) {
    Object object = evaluate(expr.object);

    if (!(object instanceof LoxInstance)) { 
      throw new RuntimeError(expr.name,
                             "Only instances have fields.");
    }

    Object value = evaluate(expr.value);
    ((LoxInstance)object).set(expr.name, value);
    return value;
  }
// lox/Interpreter.java, add after visitLogicalExpr() 
```

�����������������ԵĶ��󲢼�����Ƿ��� LoxInstance��������ǣ��Ǿ�������ʱ���󡣷����������õ�ֵ������洢��ʵ���ϡ��������� LoxInstance �е�һ���·�����

> ������һ�������Ե�������������ֲ�ͬ�Ĳ�����
> 
> 1. ��������
> 
> 2. ������������ʵ��������������ʱ����
> 
> 3. ����ֵ��
>    
>    ��Щִ�е�˳��������û��ɼ��ģ�����ζ��������Ҫ��ϸָ������ȷ�����ǵ�ʵ������ͬ��˳��ִ����Щ������

```java
 void set(Token name, Object value) {
    fields.put(name.lexeme, value);
  }
// lox/LoxInstance.java, add after get()
```

����û��������ħ�������ǽ�ֱֵ����䵽�ֶ����ڵ� Java Map�С����� Lox ������ʵ�������ɴ������ֶΣ��������鿴key�Ƿ��Ѿ����ڡ�

## 12.5 �෽��

�����Դ������ʵ������������䵽���У����౾��ʵ���ϲ�û��*��*�κ����顣ʵ��ֻ��Map������ʵ������������ͬ��Ϊ�������Ǹо������ʵ����������Ҫ��Ϊ����������

�������Ѿ������˷������������������Ѿ����úܺ��ˡ�����Ҳ����ҪΪ����*����*����κ��µĽ�����֧�֡������Ѿ�����`.`(getters) ��`()`(function calls)������������ (method call)��ֻ�ǽ�����������һ��

![](./assets/75a6cd73d981ee41dac1c7626c6bb4df0809eb77.png)

��������һ����Ȥ�����⡣�����������ʽ�ֿ�ʱ�ᷢ��ʲô������`method`����������������ϵķ���?`object`������ʵ���ϵ��ֶΣ�������δ���Ӧ����ʲô��

```js
var m = object.method;
m(argument);
```

�ó��򡰲��ҡ��÷���(method)������������۽����ʲô���洢��һ�������У�Ȼ���Ժ���øö��������������������Դ�ʵ���ϵĺ���һ���Դ�������

��һ�������أ�

```js
class Box {}

fun notMethod(argument) {
  print "called function with " + argument;
}

var box = Box();
box.function = notMethod;
box.function("argument");
```

�ó��򴴽�һ��ʵ����Ȼ�������ϵ�һ���ֶ��д洢һ��������Ȼ����ʹ���뷽��������ͬ���﷨���øú���������������

��ͬ�����Զ���Щ�����в�ͬ�Ĵ𰸡����ǿ���дһƪ�����������ġ����� Lox�����ǻ�˵����������Ĵ𰸶��ǿ϶��ģ���ȷʵ��Ч�������м���������֤����һ�㡣���ڵڶ���ʾ���������ô洢���ֶ��еĺ�����������ϣ��֧��������Ϊfirst-class���������ã������Ǵ洢���ֶ�����һ���ǳ����������顣

��һ�����ӱȽϻ�ɬ��һ���������û�ͨ��ϣ���ܹ��ڲ��ı�����������½��ӱ��ʽ�������ֲ������С������������

```js
breakfast(omelette.filledWith(cheese), sausage);
```

�������������

```js
var eggs = omelette.filledWith(cheese);
breakfast(eggs, sausage);
```

����ͬ�������顣ͬ�������ڷ��������е�`.`�� `()`�����������ı��ʽ��������ƺ�Ӧ���ܹ���*����*����������һ�������У�Ȼ���ٵ�������������Ҫ��ϸ���ǲ��ҷ���ʱ�õ��Ľ����ʲô���Լ�������Ϊ��ʽ����ʹ������ֵ�����£����磺

> �Դ˵�һ����������;�ǻص���ͨ������ϣ������һ���ص���������ֻ�ǵ���ĳ�������ϵ�һ���������ܹ����ҷ�����ֱ�Ӵ���������ʡȥ�ֶ�������������װ���ķ����������Ƚ�һ�£�
> 
> ```js
> fun callback(a, b, c) {
>   object.method(a, b, c);
> }
> 
> takeCallback(callback);
> ```

> ���������
> 
> ```js
> takeCallback(object.method);
> ```

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

�������ȡĳ��ʵ����ĳ�������ľ�����Ժ�����������ᡰ��ס����������ȡ��ʵ���𣿷����ڲ�`this`�Ƿ���Ȼ�����Ǹ�ԭʼ����

����һ������̬��������������Ĵ��ԣ�

```js
class Person {
  sayName() {
    print this.name;
  }
}

var jane = Person();
jane.name = "Jane";

var bill = Person();
bill.name = "Bill";

bill.sayName = jane.sayName;
bill.sayName(); // ?
```

���һ�д�ӡ��Bill������Ϊ��������*����*�÷�����ʵ�������Ǵ�ӡ��Jane������Ϊ�����������Ȼ�ȡ�÷�����ʵ����

Lua �� JavaScript �еĵ�Ч���뽫��ӡ��Bill������Щ���Բ�û�������ġ����������һ�ж����ֶ��ڵĺ�����functions-in-fields������˲������ `jane`��ӵ�С�`sayName`��` bill`��һЩ��

���ǣ�Lox �������������﷨����������ȷʵ֪����Щ�ɵ��õĶ����Ƿ�������Щ�Ǻ�������ˣ��� Python��C# ����������һ�����������״α�ץȡʱ�����ǻὫ������`this`���󶨡���ԭʼʵ����Python ����Щ��**�󶨷���**��

> ��֪�������������������֣��԰ɣ�

ʵ���ϣ���ͨ����������Ҫ�ġ����������ĳ�������ϵķ����Ա��Ժ��������ص�������Ҫ��ס��������ʵ������ʹ�ûص�ǡ�ô洢��ĳ������������ֶ��С�

�ðɣ��кܶ�����Ҫ���ص������Ժ��С���ʱ���Ǳ�Ե��������ǻ�ص���Щ�����ڣ������ǿ�ʼʹ�û����ķ������á������Ѿ��������ڽ����˷���������������һ���ǽ������ǡ�

```java
    define(stmt.name);

    for (Stmt.Function method : stmt.methods) {
      FunctionType declaration = FunctionType.METHOD;
      resolveFunction(method, declaration); 
    }

    return null;
// lox/Resolver.java, in visitClassStmt()
```

> ���������ʹ洢�ھֲ����������ں������壬�����Ǻܿ�ͻ���չ��δ��룬����������塣

���Ǳ���  class�����е�method ������`resolveFunction()`�����������Ѿ���д�����ڴ����������ķ�����Ψһ�������Ǵ�����һ���µ� FunctionType ö��ֵ��

```java
    NONE,
    FUNCTION,
    METHOD
  }
// lox/Resolver.java, in enum FunctionType, add ��,�� to previous line
```

�����ǽ���`this`���ʽʱ���⽫����Ҫ�����ڣ����õ��ġ���Ȥ�Ķ����ڽ�������

```java
    environment.define(stmt.name.lexeme, null);

    Map<String, LoxFunction> methods = new HashMap<>();
    for (Stmt.Function method : stmt.methods) {
      LoxFunction function = new LoxFunction(method, environment);
      methods.put(method.name.lexeme, function);
    }

    LoxClass klass = new LoxClass(stmt.name.lexeme, methods);
    environment.assign(stmt.name, klass);
// lox/Interpreter.java, in visitClassStmt(), replace 1 line
```

������һ�����������ʱ��������﷨��ʾ�������� AST �ڵ㡪��ת��Ϊ��������ʱ��ʾ�����ڣ�Ҳ��Ҫ�����а����ķ���ִ�д˲�����ÿ����������������һ�� LoxFunction ����

���ǲ���������Щ�������ǰ�װ��һ��Map�У��Է���������Ϊ�������洢�� LoxClass �С�

```java
  final String name;
  private final Map<String, LoxFunction> methods;

  LoxClass(String name, Map<String, LoxFunction> methods) {
    this.name = name;
    this.methods = methods;
  }

  @Override
  public String toString() {
// lox/LoxClass.java, in class LoxClass, replace 4 lines  
```

ʵ���Ǵ洢״̬�ĵط�������洢��Ϊ��LoxInstance �������ֶ�Map���� LoxClass ��һ������Map����ʹ��������ӵ�У�������Ȼ����ͨ�������ʵ�����ʡ�

```java
   Object get(Token name) {
    if (fields.containsKey(name.lexeme)) {
      return fields.get(name.lexeme);
    }

    LoxFunction method = klass.findMethod(name.lexeme);
    if (method != null) return method;

    throw new RuntimeError(name, 
        "Undefined property '" + name.lexeme + "'.");
// lox/LoxInstance.java, in get()
```

��ʵ���ϲ�������ʱ������Ҳ���ƥ����ֶΣ�����ʵ�������в��Ҿ��и����Ƶķ���������ҵ������䷵�ء�����ǡ��ֶΡ��͡����ԡ�֮�������������˼�ĵط���������һ������ʱ�������ܻ�õ�һ���ֶΡ����洢��ʵ���ϵ�һЩ״̬�������������Է���ʵ�����϶���ķ�����

ʹ�ô˷������Ҹ÷�����

> ���Ȳ����ֶ���ζ���ֶ��ڱη���������һ��΢���Ҫ������㡣

```java
  LoxFunction findMethod(String name) {
    if (methods.containsKey(name)) {
      return methods.get(name);
    }

    return null;
  }
// lox/LoxClass.java, add after LoxClass()
```

�����ܻ�µ��˷����Ժ���ø���Ȥ�����ڣ�����ķ�������м򵥵�Map���Ҿ����������ǿ�ʼ�ˡ���һ�ԣ�

```js
class Bacon {
  eat() {
    print "Crunch crunch crunch!";
  }
}

Bacon().eat(); // Prints "Crunch crunch crunch!".
```

> �������ϲ���н���������������ɴ��������������Ǹ�⡣��������Լ���ϲ�õ����ű���

## 12.6 This

���ǿ����ڶ�����ͬʱ������Ϊ��״̬�������ǻ�û�а���һ���ڷ����ڲ����޷����ʡ���ǰ�����󣨵��ø÷�����ʵ�������ֶΣ�Ҳ�޷���ͬһ�����ϵ�������������

Ҫ��ȡ��ʵ��������Ҫһ�����ơ�Smalltalk��Ruby �� Swift ʹ�á�self����Simula��C++��Java ����������ʹ�á�this����Python ���չ���ʹ�á�self�������Ӽ����Ͻ�������������ƺ�����

> ��I��������һ�������ѡ�񣬵��ǽ���i������ѭ���������� OOP ��һֱ׷�ݵ� Fortran������������żȻѡ����ܺ��ߡ�

���� Lox����������ͨ����ѭ Java ������Խ�ʹ�á�this�����ڷ������ڣ�`this`���ʽ����Ϊ���ø÷�����ʵ�������ߣ��������˵�����ڷ����ķ��ʺ͵��÷��������У�����������ô���*����*�÷����Ķ���

��ʹ���ǵĹ����������ѡ����´��룺

```js
class Egotist {
  speak() {
    print this;
  }
}

var method = Egotist().speak;
method();
```

�ڵ����ڶ��У����Ǵ����ʵ���л�ȡ��`speak()`�÷��������á�������һ�����������Ҹú�����Ҫ��ס��������ʵ�����Ա�*�Ժ�*�����һ�У�����Ȼ�����ڵ��øú���ʱ�ҵ�����

�����ڷ��������ʵ���һ����Ҫ`this`������ĳ�ַ�ʽ���丽�ӵ��������Ա�������Ҫ��ʱ��һֱ���ڡ���? һ�ִ洢������Χ��һЩ�������ݵķ���������������������*�հ�*��������

����ڲ��ҷ���ʱ���صĺ�����Χ�Ļ����ж���`this`Ϊһ�����ر�������ô`this`�Ժ���������ʹ�� ���ܹ��ҵ�����LoxFunction �Ѿ��߱�������Χ�������������������ӵ������Ļ��ơ�

ͨ��һ������������������ι����ģ�

```js
class Cake {
  taste() {
    var adjective = "delicious";
    print "The " + this.flavor + " cake is " + adjective + "!";
  }
}

var cake = Cake();
cake.flavor = "German chocolate";
cake.taste(); // Prints "The German chocolate cake is delicious!".
```

����һ�������ඨ��ʱ������Ϊ`taste()`����һ�� LoxFunction�����ıհ�������Χ�Ļ������ڱ�������ȫ�ֻ������������Ǵ洢����ķ���Map�е� LoxFunction ��������������

![](./assets/47b096bb40ac998afc1ff60f688100e7aa7b1a41.png)

����������`cake.taste`get ���ʽʱ�����Ǵ�����һ���»������û�����`this`�����ʸ÷����Ķ��󣨴˴�Ϊ`cake`����Ȼ������ʹ����ԭʼ������ͬ�Ĵ��봴��һ��*�µ� LoxFunction����ʹ���»�����Ϊ��հ���*

![](./assets/4832399ab25d00dd42e53fc7f00f3197b4dcad10.png)

�����������������Ƶ� get ���ʽʱ���ص� LoxFunction�����ú����Ժ�`()`���ʽ����ʱ������������һ��Ϊ�����崴��һ��������

![](./assets/4775bab5f20116f2a89f7afb2458226cc59fc81b.png)

�������廷���ĸ�����֮ǰ�����İ�`this`����ǰ����Ļ�������ˣ��������ڲ����κ�ʹ�ö�`this`�ɹ�����Ϊ��ʵ����

���û���������ʵ��`this`�����Դ������ͺ�����������Ȥ��������磺

```js
class Thing {
  getCallback() {
    fun localFunction() {
      print this;
    }

    return localFunction;
  }
}

var callback = Thing().getCallback();
callback();
```

���磬�� JavaScript �У��ӷ����ڲ����ػص��Ǻܳ����ġ��ûص�������Ҫ���𲢱����Ը÷���������ԭʼ����`this`ֵ���� ���ʡ����еĶԱհ��ͻ�������֧��Ӧ�ÿ�����ȷ�����������Щ������

�����ǿ�ʼд���롣��һ����Ϊ`this`.

```java
      "Set      : Expr object, Token name, Expr value",
      "This     : Token keyword",
      "Unary    : Token operator, Expr right",
// tool/GenerateAst.java, in main()
```

> Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#this-expression)�С�

�����ܼ򵥣���Ϊ�������ǵĴʷ��������Ѿ�ʶ��Ϊ�����ֵĵ���token��

```java
      return new Expr.Literal(previous().literal);
    }

    if (match(THIS)) return new Expr.This(previous());

    if (match(IDENTIFIER)) {
// lox/Parser.java, in primary()
```

�����ǵ��������ʱ�������Կ�ʼ�˽�`this`��������һ��������

```java
  @Override
  public Void visitThisExpr(Expr.This expr) {
    resolveLocal(expr, expr.keyword);
    return null;
  }

// lox/Resolver.java, add after visitSetExpr()  
```

���ǰѡ�this�� ��Ϊ ��ͨ�ֲ���������������������Ȼ���������в�ͨ����Ϊ��this��*û��*���κ�����������������������`visitClassStmt()`������������.

```java
    define(stmt.name);

    beginScope();
    scopes.peek().put("this", true);

    for (Stmt.Function method : stmt.methods) {
// lox/Resolver.java, in visitClassStmt()
```

�����ǽ��벢��ʼ����������֮ǰ����������һ���µ������������ж��塰this�����ͺ�������һ������һ����Ȼ�󣬵���ɺ󣬶�����Χ��������

```java
    }

    endScope();

    return null;
// lox/Resolver.java, in visitClassStmt()
```

���ڣ����ۺ�ʱ`this`�������ʽ�������ڷ����ڲ��������������Ϊ�ڷ���������ⲿ����ʽ�������ж���ġ��ֲ���������

������Ϊ`this`�������µ�*������*����˽�������ҪΪ�䴴����Ӧ��*����*�����ס��**����ʼ�ձ��뱣�ֽ����������������ͽ������Ļ������˴�ͬ��**��������ʱ����ʵ�����ҵ������󴴽���������ǰһ�м򵥵ط��ط����� LoxFunction �Ĵ����滻Ϊ��

```java
    LoxFunction method = klass.findMethod(name.lexeme);
    if (method != null) return method.bind(this);

    throw new RuntimeError(name, 
        "Undefined property '" + name.lexeme + "'.");
// lox/LoxInstance.java, in get(), replace 1 line
```

ע��� `bind()`���µ��á���������������

```java
  LoxFunction bind(LoxInstance instance) {
    Environment environment = new Environment(closure);
    environment.define("this", instance);
    return new LoxFunction(declaration, environment);
  }
// lox/LoxFunction.java, add after LoxFunction()
```

ûʲô��˵�ġ������ڷ�����ԭʼ�հ��ڴ�����һ���»������е���հ��еıհ���������������ʱ��������Ϊ�������廷���ĸ�����

���ǽ���this������Ϊ�û����е�һ��������������󶨵�������ʵ���������з��ʸ÷�����ʵ����*Et* voil�������ص� LoxFunction ���ڴ������Լ���С�־����磬���С�this���󶨵�����

ʣ�µ������ǽ�����Щ`this`���ʽ�������ڽ�������������ͱ������ʽ��ͬ��

```java
  @Override
  public Object visitThisExpr(Expr.This expr) {
    return lookUpVariable(expr.keyword, expr);
  }
// lox/Interpreter.java, add after visitSetExpr()
```

��������ʹ��֮ǰ�ĵ���ʾ����ʹ�ò��� 20 �еĴ��룬���������Դ���`this`�ڲ���������ʹ��������Ƕ���ࡢ�����ڲ���������������Ƚ��н�����������ַ�ʽ��

### 12.6.1  this ����Чʹ��

��һ�¡�����������ڷ���֮��ʹ��`this`�ᷢ��ʲô�����ڣ�

```js
print this;
```

���ߣ�

```js
fun notAMethod() {
  print this;
}
```

��������ڷ����У���û��ָ��`this`��ʵ�������Ը���һЩĬ��ֵ��`nil`����������Ϊһ������ʱ���󣬵��û���Ȼ����һ����������Խ�緢�ֲ������������Ǿͻ�Խ���ġ�

���������ı�������(resolution pass)�Ǿ�̬���˴���ĺõط������Ѿ���⵽`return`����֮�����䡣���ǽ�Ϊ `this`�����Ƶ����顣�����������е� FunctionType ö�٣����Ƕ�����һ���µ� ClassType ö�١�

```java
  }

  private enum ClassType {
    NONE,
    CLASS
  }

  private ClassType currentClass = ClassType.NONE;

  void resolve(List<Stmt> statements) {
// lox/Resolver.java, add after enum FunctionType 
```

�ǵģ���������һ������ֵ��������ʵ�ּ̳�ʱ�������������ֵ�����������ö�١����ǻ������һ����Ӧ���ֶ�`currentClass`������ֵ���������ڱ����﷨��ʱ��ǰ�Ƿ����������С���ʼֵ`NONE`������ζ�����ڲ����������С�

�����ǿ�ʼ����һ��������ʱ�����ǻ�ı�����

```java
  public Void visitClassStmt(Stmt.Class stmt) {
    ClassType enclosingClass = currentClass;
    currentClass = ClassType.CLASS;

    declare(stmt.name);
// lox/Resolver.java, in visitClassStmt()
```

�� `currentFunction`һ�������ǽ�currentClass�ֶ�֮ǰ��ֵ�洢�ھֲ������С��������ǿ��Խ��� JVM ������`currentClass`��������ջջ�С����������һ����Ƕ������һ�����У��Ͳ�������֮ǰ��ֵ��

һ������������ɣ����Ǿ�ͨ���ָ���ֵ������������ջ

```java
    endScope();

    currentClass = enclosingClass;
    return null;
// lox/Resolver.java, in visitClassStmt()
```

�����ǽ���һ��`this`���ʽʱ��������ʽû�г����ڷ������ڣ�`currentClass`�ֶλ�Ϊ�����ṩ���������������ݡ�

```java
   public Void visitThisExpr(Expr.This expr) {
    if (currentClass == ClassType.NONE) {
      Lox.error(expr.keyword,
          "Can't use 'this' outside of a class.");
      return null;
    }

    resolveLocal(expr, expr.keyword);
// lox/Resolver.java, in visitThisExpr()
```

��Ӧ�ÿ��԰����û�`this`��ȷʹ�ã����������ǲ���������ʱ�ڽ������д������á�

## 12.7 ���캯���ͳ�ʼ��

���ڼ��������������κ����飬���ӽ����µĽ�βʱ�����Ƿ����Լ���ֵ�רע�ڿ�ͷ���������ֶν�״̬����Ϊ��װ��һ��Ϊ��ʹ����ʼ��*����*��Ч���á������������ȷ��һ��ȫ�µĶ��������õ�״̬��ʼ�أ�

Ϊ�ˣ���Ҫ���캯�����ҷ��������������������ֵĲ���֮һ���������ϸ�۲������������ԣ���ᷢ�� ��������Χ�����ѷ죬��ƵĽӷ첻�������ؽ����һ��Ҳ���������һ�̱�������Щ���ҡ�

> �ټ������ӣ� �� Java �У���ʹ final �ֶα��뱻��ʼ������Ȼ��������*֮ǰ*��ȡһ�����쳣����һ���޴�����ӵ����ԡ�������ӵ� C++ ����Ҫ����Ϊ�ӹ��캯���з��������һ�ַ�ʽ��

�����족һ������ʵ������һ�Բ�����

1. ����ʱΪ��ʵ��*����*������ڴ档�ڴ���������У��˲��������û������ܹ����ʵĻ�������
   
   > C++ �ġ�[placement new](https://en.wikipedia.org/wiki/Placement_syntax)?����һ�����������ӣ����з�����ڲ��ṹ�Ǳ�¶������Ա�ġ�

2. Ȼ�󣬵����û��ṩ�Ĵ������*��ʼ��*δ���εĶ���

���������������������캯����ʱ�������뵽�ģ��������ǵ����Ǹ���֮ǰ�����Ա���ͨ���Ѿ�Ϊ��������һЩ������������ʵ�ϣ�Lox �������ڴ����µ� LoxInstance ����ʱ�Ѿ���������һ�㡣

�������ڽ����ʣ�µĲ��֡����û�����ĳ�ʼ��������Ϊ�������¶���Ĵ���飬�����ж��ֱ�ʾ����C++��Java �� C# ʹ������������ƥ��ķ�����Ruby �� Python ��֮Ϊ`init()`.���ߺܺö��Һ̣ܶ��������ǻ���������

�� LoxClass �� LoxCallable ʵ���У���������˼���

```java
                      List<Object> arguments) {
    LoxInstance instance = new LoxInstance(this);
    LoxFunction initializer = findMethod("init");
    if (initializer != null) {
      initializer.bind(instance).call(interpreter, arguments);
    }

    return instance;
// lox/LoxClass.java, in call()
```

��һ���౻����ʱ���ڴ��� LoxInstance ֮��Ѱ��һ����init������������ҵ�һ����������ͨ��������һ�������󶨲��������������б�һ��ת����

�ò����б���ζ�Ż���Ҫ������������Ԫ���ķ�ʽ��

```java
 public int arity() {
    LoxFunction initializer = findMethod("init");
    if (initializer == null) return 0;
    return initializer.arity();
  }
// lox/LoxClass.java, in arity(), replace 1 line
```

����г�ʼ��������÷�����Ԫ��(arity)�����˵����౾��ʱ���봫�ݵĲ���������������Ϊ�������������*����Ҫ*���������ʼ���������û�г�ʼ������arity ��Ȼ���㡣

�����Ͼ������������������ڵ���֮ǰ��`init()`��������������Է���`this`�������ڲ������봫�ݸ���Ĳ���һ������������Ҫ������ʵ�������ȫ�����ݡ�

### 12.7.1 ֱ�ӵ��� init()

������һ����̽������µ���������ἤ��һЩ��ֵ�������ǣ�

```js
class Foo {
  init() {
    print this;
  }
}

var foo = Foo();
print foo.init();
```

����ͨ��ֱ�ӵ�������`init()`�����������³�ʼ����һ����������������������᷵��ʲô��һ������Ĵ���`nil`����Ϊ�����init��������ֵ��

Ȼ��������ͨ����ϲ��Ϊ������ʵ�ֶ���Э�����������˵`init()`�������Ƿ���`this`����ʹֱ�ӵ��ã�Ҳ��ʹ clox �Ĺ��캯��ʵ�ֱ�����׵öࡣΪ�˱��� jlox ��֮���ݣ������� LoxFunction �������һЩ����������롣

> Ҳ����ϲ�������˵��̫��ǿ���ˡ�ʵ�ֵ�Լ������ԴӰ�����Ե�����Ǻ���ġ�һ��ֻ����ô��Сʱ�����������������߽ݾ����������ڸ��̵�ʱ����Ϊ�û��ṩ���๦�ܣ���ܿ��������ǿ��ֺ��������ľ�Ӯ��������Ū���*��Щ*���䲻���������û���δ�����Լ��������Ķ��ӡ�

```java
      return returnValue.value;
    }

    if (isInitializer) return closure.getAt(0, "this");
    return null;
// lox/LoxFunction.java, in call()
```

��������ǳ�ʼ���������ǻḲ��ʵ�ʵķ���ֵ��ǿ�Ʒ���`this`����������һ���µ�`isInitializer`�ֶΡ�

```java
  private final Environment closure;

  private final boolean isInitializer;

  LoxFunction(Stmt.Function declaration, Environment closure,
              boolean isInitializer) {
    this.isInitializer = isInitializer;
    this.closure = closure;
    this.declaration = declaration;
// lox/LoxFunction.java, in class LoxFunction, replace 1 line 
```

���ǲ��ܼ򵥵ز鿴 LoxFunction �������Ƿ�Ϊ��init������Ϊ�û������Ѿ�ʹ�ø����ƶ�����һ��*����*�������Ļ�����û��`this`���Է����ˡ�Ϊ�˱���*����*��ֵı�Ե��������ǽ�ֱ�Ӱ� LoxFunction �Ƿ��ʾ��ʼ��������������������ζ����Ҫ���ز��޸����� LoxFunctions �ļ����ط���

```java
  public Void visitFunctionStmt(Stmt.Function stmt) {
    LoxFunction function = new LoxFunction(stmt, environment,
                                           false);
    environment.define(stmt.name.lexeme, function);
// lox/Interpreter.java, in visitFunctionStmt(), replace 1 line
```

����ʵ�ʵĺ���������`isInitializer`ʼ��Ϊ false�����ڷ��������Ǽ�����ơ�

```java
    for (Stmt.Function method : stmt.methods) {
      LoxFunction function = new LoxFunction(method, environment,
          method.name.lexeme.equals("init"));
      methods.put(method.name.lexeme, function);
// lox/Interpreter.java, in visitClassStmt(), replace 1 line
```

Ȼ����`bind()`�У���������`this`�������ıհ��ĵط�������ԭʼ������ֵ��

```java
    environment.define("this", instance);
    return new LoxFunction(declaration, environment,
                           isInitializer);
  }
// lox/LoxFunction.java, in bind(), replace 1 line
```

### 12.7.2 �� init() ����

���ǻ�û���߳�����������һֱ�����û���д�ĳ�ʼ�����򲻻���ʽ����ֵ����Ϊ��������캯�����ᡣ����û����ԣ�

```js
class Foo {
  init() {
    return "something else";
  }
}
```

�϶�������������Ը�����Բ���������Ϊһ����̬���󡣻ص��������������� FunctionType �����һ�� case��

```java
    FUNCTION,
    INITIALIZER,
    METHOD
// lox/Resolver.java, in enum FunctionType
```

ʹ�ñ����ʷ�����������ȷ���Ƿ����ڽ�����ʼ������

```java
      FunctionType declaration = FunctionType.METHOD;
      if (method.name.lexeme.equals("init")) {
        declaration = FunctionType.INITIALIZER;
      }

      resolveFunction(method, declaration); 
// lox/Resolver.java, in visitClassStmt()
```

���Ժ����`return`���ʱ��������ֶβ�ʹ���`init()`�����ڲ�����ֵ��Ϊ����

```java
   if (stmt.value != null) {
      if (currentFunction == FunctionType.INITIALIZER) {
        Lox.error(stmt.keyword,
            "Can't return a value from an initializer.");
      }

      resolve(stmt.value);
// lox/Resolver.java, in visitReturnStmt()
```

������*��*û����ɡ����Ǿ�̬�ز�����ӳ�ʼ��������һ��*ֵ*��������Ȼ����ʹ��һ���յ� `return`��

```js
class Foo {
  init() {
    return;
  }
}
```

��ʱ��ʵ���Ϻ����ã��������ǲ�����ȫ��ֹ�����෴����Ӧ�÷���`this`������`nil`. ���� LoxFunction �к����׽��

```java
    } catch (Return returnValue) {
      if (isInitializer) return closure.getAt(0, "this");

      return returnValue.value;
// lox/LoxFunction.java, in call()
```

��������ڳ�ʼ��������ִ��`return`��䣬�����Ƿ���ֵ����Զ��`nil`���������ٴη���`this`��

�ۣ�����һ�������������б������ǵĻر������ǵ�С�������Ѿ��γ���һ�������ı�̷�ʽ���ࡢ�������ֶΡ�`this`�͹��캯�������ǵ�Ӥ�����Կ������ǳ����졣

--

## [��ս](http://craftinginterpreters.com/classes.html#challenges)

1. ������ʵ�����з���������û�а취�������ֱ������������ϵ��õġ���̬����������Ӷ����ǵ�֧�֡��ڷ���֮ǰʹ��`class`�ؼ�����ָʾ�����ľ�̬������
   
   ```js
   class Math {
     class square(n) {
       return n * n;
     }
   }
   
   print Math.square(3); // Prints "9". 
   ```
   
   ���������������ؽ��������⣬���� Smalltalk �� Ruby ʹ�õġ�[Ԫ��](https://en.wikipedia.org/wiki/Metaclass)����һ���ر����ŵķ�����*��ʾ��ʹ LoxClass ��չ LoxInstance �������￪ʼ��*

2. ������ִ����Զ�֧�֡�getters���͡�setters���������еĳ�Ա���������ֶζ�д����ʵ����ִ���û�����Ĵ��롣��չ Lox ��֧�� getter ��������Щ����û�в����б������������ġ������ʾ��и����Ƶ�����ʱ����ִ�� getter �����塣
   
   ```js
   class Circle {
     init(radius) {
       this.radius = radius;
     }
   
     area {
       return 3.141592653 * this.radius * this.radius;
     }
   }
   
   var circle = Circle(4);
   print circle.area; // Prints roughly "50.2655".
   ```

3. Python �� JavaScript �������Ӷ�������ķ���֮�����ɷ��ʶ�����ֶΡ�Ruby �� Smalltalk ��װ��ʵ��״̬��ֻ�����ϵķ������Է���ԭʼ�ֶΣ�����������������ĸ�״̬���������̬�������Զ��ṩ���η�����`private`��`public`�����������Щ���ֿ�����ÿ����Ա�Ļ����ϴ��ⲿ���ʡ�
   
   ��Щ����֮���Ȩ����ʲô��Ϊʲôһ�����Կ��ܸ�ϲ������һ�֣�

## [���˵����ԭ�ͺ�����](http://craftinginterpreters.com/classes.html#design-note)

�ڱ����У����ǽ����������µ�����ʱʵ�� LoxClass �� LoxInstance��**ǰ���Ƕ�����Ϊ���ڣ�������״̬����**������������� LoxInstance �ڲ��ĵ���������ֱ�Ӷ��巽��������������������£����Ǹ�������Ҫ LoxClass��LoxInstance ����һ�������İ������ڶ���������Ϊ��״̬��

������Ȼ��Ҫĳ�ַ�ʽ�������࣬���ɿ���ʵ��������Ϊ�����ǿ�����һ�� LoxInstanceֱ��[*ί��*](https://en.wikipedia.org/wiki/Prototype-based_programming#Delegation)����һ�� LoxInstance �����������ֶκͷ������е���̳С�

�û��Ὣ���ǵĳ���ģΪһ���������һЩ�����໥ί���Է�ӳ���ԡ�����ί�еĶ����ʾ�����˸Ľ��ġ��淶����ԭ�͡����󡣽����һ�����򵥵�����ʱ��ֻ��һ���ڲ����� LoxInstance��

����Ǵ˷�������**[ԭ��](https://en.wikipedia.org/wiki/Prototype-based_programming)**����Դ�������� David Ungar �� Randall Smith ��һ�ֽ���[Self](http://www.selflanguage.org/)�����Է����ġ�����ͨ���� Smalltalk ��ʼ����������������ϰ����������������ǿ��Խ����������١�

�ܳ�һ��ʱ��������ԭ�Ͷ���һ��ѧ���ϵĺ����ģ�һ��������ʤ�ĺ����ģ�����������Ȥ���о�������δ�Ը���ı���������Ӱ�졣Ҳ����˵��ֱ�� Brendan Eich ��ԭ������ JavaScript��Ȼ����Ѹ�ٽӹ������硣���� JavaScript �е�ԭ�ͣ��Ѿ�д�˺ܶࣨ�ࣩܶ�ʡ����Ƿ����ԭ���ǳ�ɫ�Ļ�����������ġ����������߼����֮��-��һ������δ�������⡣

����������[������](http://gameprogrammingpatterns.com/prototype.html)��

�Ҳ����������Ƿ���Ϊԭ�Ͷ���һ��������˵�Ǹ������⡣��������[ԭ��](http://finch.stuffwithstuff.com/)���Ժ�[�������](http://wren.io/)���ԣ��Ҷ����������ԵĿ������ܸ��ӡ��������۵���*����*�������е����á�

ԭ�ͱ�����򵥡�������ʵ���߱�д�Ĵ�����٣��û�ѧϰ�����ĸ���Ҳ���١���������Ǳ�ø������������������������������������򡣾͸��˶��ԣ�����Ϊ��ֻ�ǵ�ʽ��һ���֡�������������û�����*����*���Ҷ���Ϊ��

```
power = breadth �� ease �� complexity
```

��Щ�����Ǿ�ȷ�����ֶ�������������ʹ����ѧ��Ϊ��ȣ�������ʵ�ʵ�������

- **��� Breadth** ���������������Ĳ�ͬ����ķ�Χ��C ��Ӧ�÷�Χ�ܹ㡪���������ڴӲ���ϵͳ���û�Ӧ�ó����ٵ���Ϸ�ķ������档AppleScript �� Matlab �������ض����Ե�Ӧ�÷�Χ��С��

- **������ ease**������������������������Ҫ��������Ŭ�����������ԡ���������һ��������������صİ���������Ҫ��Ҫ�ࡣ���߼������������ȡ��ͼ������Ը����ס���������Զ���һ��������������Щ�������������������ױ�

- **������ complexity**�����ԣ�����������ʱ�����Ŀ⡢���ߡ���̬ϵͳ�ȣ��Ĺ�ģ������̸��һ�����ԵĹ淶�ж���ҳ���������ж��ٹؼ��֡������û���ϵͳ�й���֮ǰ������ص����ǵ�ʪ���е��������Ǽ򵥵ķ���ʡ�

���͸�����*ȷʵ��*������������ĸԽС�����ֵ��Խ������������Ϊ�򵥾��Ǻõ�ֱ������ȷ�ġ����ǣ��ڽ��͸�����ʱ�����Ǳ���ע�ⲻҪ���������еĹ�Ȼ������ԣ������ܹ��ʿ��ܻ��½������Java ɾ�����ַ�����������һ�ַǳ�*��*�����ԣ����������޷��ܺõش����ı���������Ҳ������ô���׵��������

��ô����������Ѱ�ҿ��Ժ��Ե�*żȻ������*�������������ͽ�������ͨ���������ԵĹ�Ȼ����������������ǵ�������

����û�����ݶ��������������ǵĳ�����ô����決�������л������������ı����ԣ�ϣ�����㹻��������֧�����ӵĸ����ԡ����ǣ�����ⲻ���û�ʹ���������Եķ�ʽ����ô����ؽ��γ��ų����⡣