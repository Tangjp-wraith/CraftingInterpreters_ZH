# �̳�

> ���������Ǻ��е�һ��ˮ�飬Ȼ�����㣬Ȼ�������������Ȼ���Ǻ��ӣ��Լ���������֮��������������ֻ��������������ֻ��������צ�ӣ����ҵ�����������ǵļ��������ӵ�������ţ��ĥ�������ǵ�ѪҺ��������������Ĵ�һ���̣��������ܵ�����ʱ������Ƥ���ϵ�ë����������������������ëʱһ������������ʷ�������ڳ�Ϊ���ǵĹ����о�������һ�У�������Ȼ�ǡ�
> 
> -- Terry Pratchett,*A Hat Full of Sky*

�������������Ѿ����˵ڶ�[����](http://craftinginterpreters.com/a-tree-walk-interpreter.html)�����һ�¡���������˵�һ�� Lox ��������[��һ��](http://craftinginterpreters.com/classes.html)��һ����໥��֯������������ԡ����޷������Ǳ˴˷ֿ�������ȷʵ�跨�⿪�����е�һ���֡��ڱ����У����ǽ�ͨ����Ӽ̳������ Lox ����֧�֡�

�̳г������������������У�һֱ׷�ݵ���һ�����ԣ�[Simula](https://en.wikipedia.org/wiki/Simula)�����ڣ�Kristen Nygaard �� Ole-Johan Dahl �����Ǳ�д��ģ�������ע�⵽����Ĺ��ԡ��̳�Ϊ�����ṩ��һ��Ϊ���Ʋ������ô���ķ�����

> �����˵�����������Զ��� Simula�̳�������*��-Ŷ���һᣬ���������Լ���

## 13.1 ���������

������������ǡ��̳С������ϣ������ѡ��һ��һ�µı����������ǳ�Ϊ�����ࡱ�͡����ࡱ������̫�����ˡ��ܾ���ǰ��CAR Hoare ������������ࡱ��ָ���Ľ���һ�����͵ļ�¼���͡�Simula ���ø�������ָ������һ���̳е�*��*���Ҳ���Ϊ���� Smalltalk ����֮ǰ�����˷�ת������ǰ׺�Ի�á����ࡱ��ָ����ϵ����һ������ C++ �У����������������ࡱ�͡������ࡱ������Ҫ���ʹ�á����ࡱ�͡����ࡱ��

> ��Super-���͡�sub-�����������зֱ��ʾ�����桱�͡����桱�����̳�������ɸ��ڶ����ļ��ס���������ͼ�еĳ����·�����һ���˵�����ӡ���ָ�����������һЩ��һ��ĸ����е�����ڶ���ѧ�У������ǶԸ���������ĸ���ϸ���ࡣ
> 
> �ڼ������У�һ���Ӽ�������һ������ĳ����У��ó������и��Ӽ�������Ԫ�أ��������ܸ��ࡣ�����ۺͱ������������������������������С������͡��͡������͡���
> 
> �ھ�̬���͵�������������У�����ͨ��Ҳ���䳬��������͡�����������һ�� Donut �����һ�� BostonCream ���ࡣÿ�� BostonCream Ҳ�� Doughnut ��ʵ���������ܴ��ڲ��� BostonCreams ������Ȧ������ Crullers����
> 
> ��������Ϊ����������ֵ�ļ��ϡ����� Donut ʵ���ļ��ϰ������� BostonCream ʵ���ļ��ϣ���Ϊÿ�� BostonCream Ҳ��һ�� Doughnut������ BostonCream ��һ�����࣬һ�������ͣ�����ʵ����һ���Ӽ�����һ�ж��ų�һ�С�

![](./assets/2500bb7fc7900d2f230862180372a43134350d5f.png)

������ Lox ��֧�ּ̳еĵ�һ������������ʱָ������ķ������﷨�кܶ��֡�C++ �� C#���������ƺ���� һ��`:`��Ȼ���ǳ������ơ�Java ʹ��`extends`����ð�š�Python ���������������������С�Simula ����������Ʒ��ڹؼ���`class`֮ǰ��

����Ϸ�ĺ��ڣ�����Ը����ʷ�����������µı����ֻ��ǡ�����û��`extends`or even`:`���������ǽ���ѭ Ruby ��ʹ��С�ں� (`<`)��

```js
class Doughnut {
  // General doughnut stuff...
}

class BostonCream < Doughnut {
  // Boston Cream-specific stuff...
}
```

Ϊ�˽��������﷨�����������е�`classDecl`�����������һ���µĿ�ѡ�Ӿ䡣

```
classDecl      �� "class" IDENTIFIER ( "<" IDENTIFIER )?
                 "{" function* "}" ;
```

������֮����һ��`<`����ų�������ơ������Ӿ��ǿ�ѡ�ģ���Ϊ������*ӵ��*���ࡣ�� Java ������һЩ�����������Բ�ͬ��Lox û���κζ������̳��Եĸ���Object���࣬��˵���ʡ�Գ����Ӿ�ʱ������*û��*���࣬����û����ʽ���ࡣ

���������� AST �ڵ��в���������﷨��

```java
      "Block      : List<Stmt> statements",
      "Class      : Token name, Expr.Variable superclass," +
                  " List<Stmt.Function> methods",
      "Expression : Expr expression",
// tool/GenerateAst.java, in main(), replace 1 line
```

�����ܻ�����ǽ��������ƴ洢Ϊ Expr.Variable ������token�е����ȡ��﷨�������Ӿ�����Ϊ������ʶ������������ʱ���ñ�ʶ��������Ϊ�������ʡ������ڽ����������ư�װ�� Expr.Variable ��Ϊ�����ṩ��һ�����󣬽��������Խ�������Ϣ����

�µĽ���������ֱ����ѭ�﷨��

```java
    Token name = consume(IDENTIFIER, "Expect class name.");

    Expr.Variable superclass = null;
    if (match(LESS)) {
      consume(IDENTIFIER, "Expect superclass name.");
      superclass = new Expr.Variable(previous());
    }

    consume(LEFT_BRACE, "Expect '{' before class body.");
// lox/Parser.java, in classDeclaration()
```

һ�������ܣ�������һ�������������ͽ����洢�� AST �С�

```java
    consume(RIGHT_BRACE, "Expect '}' after class body.");

    return new Stmt.Class(name, superclass, methods);
  }
// lox/Parser.java, in classDeclaration(), replace 1 line
```

��������������Ӿ䣬������ʽ��Ϊ`null`.����ȷ��������*Resolver*������֤(later passes check)��һ�㡣���е�һ���ǽ�����

```java
    define(stmt.name);

    if (stmt.superclass != null) {
      resolve(stmt.superclass);
    }

    beginScope();
// lox/Resolver.java, in visitClassStmt()
```

������ AST �ڵ���һ���µ��ӱ��ʽ����˱�������������������ͨ���ڶ����������������ƺܿ�����һ��ȫ�ֱ�����������ͨ���������κ����õ����顣���ǣ�Lox ���������ڿ��ڽ�������������˳������ƿ������þֲ�����������������£�������Ҫȷ�����ѵõ�������

��Ϊ��ʹ�Ǻ��ĵĳ���Ա��ʱҲ��д����ֵĴ��룬���Ե�����������ʱ����Ҫ����һ���޴��ı�Ե��������������

```js
class Oops < Oops {}
```

�ⲻ�������κ����õ����飬��������� ����ʱ ����ִ�������������ƽ������Լ̳�����û��ѭ�����������ȫ�������Ǿ�̬���������������䱨��Ϊ����

```java
    define(stmt.name);

    if (stmt.superclass != null &&
        stmt.name.lexeme.equals(stmt.superclass.name.lexeme)) {
      Lox.error(stmt.superclass.name,
          "A class can't inherit from itself.");
    }

    if (stmt.superclass != null) {
// lox/Resolver.java, in visitClassStmt()
```

��������������AST �����͵���������

```java
  public Void visitClassStmt(Stmt.Class stmt) {
    Object superclass = null;
    if (stmt.superclass != null) {
      superclass = evaluate(stmt.superclass);
      if (!(superclass instanceof LoxClass)) {
        throw new RuntimeError(stmt.superclass.name,
            "Superclass must be a class.");
      }
    }

    environment.define(stmt.name.lexeme, null);
// lox/Interpreter.java, in visitClassStmt()
```

���������һ��������ʽ��������������Ϊ����ܻ�����Ϊĳ���������͵Ķ������Ա���������ʱ�����Ҫ��Ϊ����Ķ���ʵ������һ���ࡣ����������������Ĵ��룬�ͻᷢ�����õ����飺

```js
var NotAClass = "I am totally not a class";

class Subclass < NotAClass {} // ?!
```

������ͨ�������Ǽ�����ִ���������Ὣ����﷨��ʾ����AST �ڵ㣩ת��Ϊ������ʱ��ʾ���� LoxClass ��������Ҳ��Ҫ���ʳ��ࡣ���ǽ����ഫ�ݸ����캯����

```java
      methods.put(method.name.lexeme, function);
    }

    LoxClass klass = new LoxClass(stmt.name.lexeme,
        (LoxClass)superclass, methods);

    environment.assign(stmt.name, klass);
// lox/Interpreter.java, in visitClassStmt(), replace 1 line
```

���캯������洢��һ���ֶ��С�

```java
 LoxClass(String name, LoxClass superclass,
           Map<String, LoxFunction> methods) {
    this.superclass = superclass;
    this.name = name;
// lox/LoxClass.java, constructor LoxClass(), replace 1 line
```

����������������

```java
  final String name;
  final LoxClass superclass;
  private final Map<String, LoxFunction> methods;
// lox/LoxClass.java, in class LoxClass 
```

���������ǾͿ��Զ�����Ϊ�������������ࡣ���ڣ�ӵ��һ������ʵ����*����ʲô��*

## 13.2 �̳з���

����һ����̳���ζ�ų����һ�ж�Ӧ�û����ٵش������ࡣ�ھ�̬���������У����кܶຬ�塣��*��*Ҳ��������*����*��**�����ڴ沼�����ܿص�**��������Ϳ��Խ������ʵ�����ݸ���Ҫ����ĺ���������Ȼ������ȷ���ʼ̳е��ֶΡ�

> ������׶���ָ�ϵ�һ����Ư����������[*Liskov �滻ԭ��*](https://en.wikipedia.org/wiki/Liskov_substitution_principle)��Barbara Liskov ������������γ�ʱ�ڵ�һ�������ݽ��н���������

Lox ��һ�ֶ�̬���͵����ԣ�������ǵ�Ҫ��Ҫ�򵥵öࡣ�����ϣ�����ζ������������ڳ����ʵ���ϵ���ĳЩ���������ڸ��������ʵ��ʱҲӦ���ܹ����ø÷��������仰˵�������Ǵӳ���̳еġ�

����ϼ̳е�Ŀ��֮һ����Ϊ�û��ṩһ�ֿ������ô���ķ����������ǵĽ�������ʵ�����ǳ����ס�

```java
     return methods.get(name);
    }

    if (superclass != null) {
      return superclass.findMethod(name);
    }

    return null;
// lox/LoxClass.java, in findMethod()
```

���������ȫ�����ݡ���������һ��ʵ���ϲ���һ������ʱ���������û���ڸ�ʵ���������ҵ��������Ǿͻ�ݹ��������������������ҡ���һ�ԣ�

```js
class Doughnut {
  cook() {
    print "Fry until golden brown.";
  }
}

class BostonCream < Doughnut {}

BostonCream().cook();
```

���ˣ�һ��ļ̳�����ֻ�������� Java ���������ˡ�

## 13.3  ���ó��෽��

��`findMethod()`���ų�����������*֮ǰ*�������ڵ�ǰ����Ѱ��һ���������������ͳ����ж�����ͬ�����������������Ȼ�**����**���෽�����е����ڲ��������еı������Ӱ���ⲿ������

���������Ҫ��ȫ*�滻*ĳЩ������Ϊ���Ǿ�̫���ˡ����ǣ���ʵ���У�����ͨ��ϣ��*�Ľ�*�������Ϊ����������һЩ�ض�������Ĺ�������Ҳִ��ԭʼ�������Ϊ��

���ǣ����������Ѿ������˸÷���������޷�����ԭʼ������������෽����ͼͨ�����Ƶ���������ֻ��ݹ���������Լ��ĸ��ǡ�������Ҫһ�ַ�ʽ�������ô˷�������ֱ�����ҵĳ����ϲ������������ҵĸ��ǡ���Java��`super`�����ǽ��� Lox ��ʹ����ͬ���﷨������һ�����ӣ�

```js
class Doughnut {
  cook() {
    print "Fry until golden brown.";
  }
}

class BostonCream < Doughnut {
  cook() {
    super.cook();
    print "Pipe full of custard and coat with chocolate.";
  }
}

BostonCream().cook();
```

���������������Ӧ�ô�ӡ��

```
Fry until golden brown.
Pipe full of custard and coat with chocolate.
```

������һ���µı����ʽ��`super`�ؼ��ֺ��һ�����һ����ʶ�������ڲ��Ҿ��и����Ƶķ�������`this`���ò�ͬ�������ӳ��࿪ʼ��

### 13.3.1 �﷨

����`this`���ؼ��ֵ������е���һ������ı����������ʽ�����Ǹ�������token�����Ƕ���`super`��������`.`������������`super`���ʽ���ɷָ�Ĳ��֡��㲻�ܵ���ӵ��һ����`super`token��

```
print super; // Syntax error.
```

`primary`��ˣ��������﷨����ӵ������е����Ӿ�Ҳ�������Է��ʡ�

```
primary        �� "true" | "false" | "nil" | "this"
               | NUMBER | STRING | IDENTIFIER | "(" expression ")"
               | "super" "." IDENTIFIER ;
```

ͨ����`super`���ʽ���ڷ������ã����볣�淽��һ���������б�*����*���ʽ��һ���֡��෴��super ������һ��super�������һ���������á���������������һ���������Ի�ó��෽���ľ����������������

```js
var method = super.cook;
method();
```

����`super`���ʽ����ֻ����`super`�ؼ��ֵ�token�����ڲ��ҵķ��������ơ���˶�Ӧ���﷨���ڵ��ǣ�

```java
      "Set      : Expr object, Token name, Expr value",
      "Super    : Token keyword, Token method",
      "This     : Token keyword",
// tool/GenerateAst.java, in main()
```

> Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#super-expression)�С�

�����﷨���µĽ�����������������е�`primary()`������

```java
      return new Expr.Literal(previous().literal);
    }

    if (match(SUPER)) {
      Token keyword = previous();
      consume(DOT, "Expect '.' after 'super'.");
      Token method = consume(IDENTIFIER,
          "Expect superclass method name.");
      return new Expr.Super(keyword, method);
    }

    if (match(THIS)) return new Expr.This(previous());
// lox/Parser.java, in primary()
```

ǰ��`super`�ؼ��ָ������������Ѿ�������һ��`super`���ʽ��֮������ʹ��Ԥ�ڵ�`.`��һ���������ơ�

### 13.3.2 ����

��Щʱ����˵��`super`���ʽ�ӡ����ࡱ��ʼ�������ң�����*�ĸ�*�����أ�����Ĵ��� `this`�ĳ���?��������Χ�����Ķ����ںܶ�����£����ɺϵز�������ȷ����Ϊ������ʵ���ϲ�����ȷ����ϸ��һ�£�

```js
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

���˳������ Java��C# �� C++��������ӡ��A method������Ҳ������ϣ�� Lox ִ�еĲ������������������ʱ���� `test()`�������ڣ�`this`�� C ��һ��ʵ����C �ĳ����� B������*����*����Ӧ�ÿ�ʼ�ĵط�����������������ǻ�� B method()`��

�෴������Ӧ�ô�*����`super`expression*����ĳ��࿪ʼ������������£���Ϊ`test()`���� B �ڲ������ ������`super`���ڲ��ı��ʽӦ�ÿ�ʼ����*B*�ĳ��ࡪ��A��

![](./assets/ec99e757fac978baf6dba9bc74278455fedb3ece.png)

> ִ�����̿�������������
> 
> 1. ���ǵ���`test()`C ��һ��ʵ����
> 
> 2. ������`test()`�̳���B�ķ�����������`super.method()`��
> 
> 3. B �ĳ����� A����˵���A ��`method()`�������ӡ��A method����

��ˣ�Ϊ������`super`���ʽ��������Ҫ����Χ�Ƶ��õ��ඨ��ĳ��ࡣ���ǣ���ִ��`super`���ʽ�Ľ������У�����û����ô���׻�á�

����*����*�� LoxFunction ���һ���ֶ����洢ӵ�и÷����� LoxClass �����á��������������Ե�ǰ����ִ�е� LoxFunction �����ã��Ա������Ժ����������`super`���ʽʱ����������������ǽ���÷����� LoxClass��Ȼ�������ĳ��ࡣ

���кö����̡���[��һ��](http://craftinginterpreters.com/classes.html)�У���������Ҫ��Ӷ�`this`��֧��ʱ���������������Ƶ����⡣����������£�����ʹ�����еĻ����ͱհ��������洢�Ե�ǰ��������á����ǿ��������Ƶ��������洢�����𣿺ðɣ�������Ƿ񶨵ģ��ҿ��ܲ���̸����������... �ǵġ�

> ��û����ϲ�����ʣ�

һ����Ҫ��������������*����*����ʱ��`this`�������ڲ�ͬ��ʵ���ϵ�����ͬ�ķ�����ÿ��ʵ������Ҫ�Լ���`this`.����`super`���ʽ��������*����������*�Ĺ̶����ԡ�ÿ��������`super`���ʽʱ������������ͬ�ġ�

����ζ�����ǿ�����ִ���ඨ��ʱΪ���ഴ��һ��һ���ԵĻ������ڶ��巽��֮ǰ��������һ���»���������ĳ���󶨵� `super`��

![](./assets/ce6c9da2a783de5fe6d405e5178bfc1460319059.png)

������Ϊÿ���������� LoxFunction ����ʱ��ʾʱ����������ǽ��ڱհ��в���Ļ������Ժ󣬵����ò�`this`�󶨷���ʱ�����໷����Ϊ���������ĸ��࣬������ʾ��

![](./assets/322ca0988f1b2742bed98c8bac93fce1b8162c8a.png)

�ö���ƣ������ǻ�һ��һ������������ڿ�ʼ������ʱ��������֮ǰ����Ҫ�ڽ������д�����Ӧ������������

```java
      resolve(stmt.superclass);
    }

    if (stmt.superclass != null) {
      beginScope();
      scopes.peek().put("super", true);
    }

    beginScope();
// lox/Resolver.java, in visitClassStmt()
```

�����������һ�����࣬��ô���Ǵ���һ��Χ�������з��������������ڸ÷�Χ�ڣ����Ƕ������ơ�super����һ������������෽���Ľ������Ͷ������Ǹ�������

```java
    endScope();

    if (stmt.superclass != null) endScope();

    currentClass = enclosingClass;
// lox/Resolver.java, in visitClassStmt()
```

����һ��С���Ż����������ȷʵ*��*һ�����࣬����ֻ�ᴴ�����໷������û�г���ʱ��������û������ģ���Ϊ������ζ�û�г�����Դ洢�����С�

ͨ�������������ж��塰super���������ܹ�����`super`���ʽ����

```java
  @Override
  public Void visitSuperExpr(Expr.Super expr) {
    resolveLocal(expr, expr.keyword);
    return null;
  }
// lox/Resolver.java, add after visitSetExpr()
```

������ȫ��`super` token����Ϊ�����������洢�˽�����Ϊ�ҵ��洢����Ļ�������Ҫ�ߵĻ������е�Ծ������

�˴����ڽ��������о��񡣵������������ඨ��ʱ�����Ǵ�����һ���»�����

```java
        throw new RuntimeError(stmt.superclass.name,
            "Superclass must be a class.");
      }
    }

    environment.define(stmt.name.lexeme, null);

    if (stmt.superclass != null) {
      environment = new Environment(environment);
      environment.define("super", superclass);
    }

    Map<String, LoxFunction> methods = new HashMap<>();
// lox/Interpreter.java, in visitClassStmt()
```

�ڸû����У����Ǵ洢�˶Գ�������á�����������������ʱ��ӵ�еĳ����ʵ�� LoxClass ����Ȼ������Ϊÿ���������� LoxFunction����Щ������ǰ�����������Ǹոհ󶨡�super���Ļ���������Ϊ���ǵıհ����������ǵ���Ҫ�������ࡣ��ɺ����ǵ�������

```java
   LoxClass klass = new LoxClass(stmt.name.lexeme,
        (LoxClass)superclass, methods);

    if (superclass != null) {
      environment = environment.enclosing;
    }

    environment.assign(stmt.name, klass);
// lox/Interpreter.java, in visitClassStmt()
```

�����Ѿ�׼���ý���`super`���ʽ������һЩ�������������ǽ��ֶι����˷�����

```java
  @Override
  public Object visitSuperExpr(Expr.Super expr) {
    int distance = locals.get(expr);
    LoxClass superclass = (LoxClass)environment.getAt(
        distance, "super");
  }
// lox/Interpreter.java, add after visitSetExpr() 
```

���ȣ�����һֱ�ڹ����Ľ����С�����ͨ�����ʵ��Ļ����в��ҡ�super����������Χ��ĳ��ࡣ

�����Ƿ���һ������ʱ�����ǻ���Ҫ��`this`�����ʸ÷����Ķ�������`doughnut.cook`�����ı��ʽ�У����������Ǵ���ֵ�еõ����κζ���`doughnut`������`super.cook` ��`super`���ʽ�У���ǰ������ʽ������������ʹ�õĵ�ǰ������ͬ�����仰˵��`this`����ʹ�������ڲ��ҳ����ϵķ�����ʵ����Ȼ��`this`.

���ҵ��ǣ���`super`���ʽ�ڲ�������û�з���Ľڵ��ý���������������`this`���档���˵��ǣ�����ȷʵ�����˻������Ĳ��֡���this���󶨵Ļ������������Ǵ洢��super���Ļ�����

```java
    LoxClass superclass = (LoxClass)environment.getAt(
        distance, "super");

    LoxInstance object = (LoxInstance)environment.getAt(
        distance - 1, "this");
  }
// lox/Interpreter.java, in visitSuperExpr()
```

�������ȥ1 ���Ǹ��ڲ������в��ҡ�this�����ҳ����ⲻ�������ŵĴ��룬����ȷʵ��Ч��

> дһ������������ÿһ�д�����ζ���Ҳ���ͨ����������Ϊ�����ߵ���ϰ����������Щ���ɡ�

��������׼�����Ҳ��󶨷������ӳ��࿪ʼ��

```java
    LoxInstance object = (LoxInstance)environment.getAt(
        distance - 1, "this");

    LoxFunction method = superclass.findMethod(expr.method.lexeme);
    return method.bind(object);
  }
// lox/Interpreter.java, in visitSuperExpr()
```

�⼸������� get ���ʽ�����Ĵ�����ȫһ����ֻ�����ǵ���`findMethod()`��������ǵ�ǰ������ࡣ

�����Ͼ�����������Ȼ���������ǿ���*�Ҳ���*��������������Ҳ���һ�¡�

```java
    LoxFunction method = superclass.findMethod(expr.method.lexeme);

    if (method == null) {
      throw new RuntimeError(expr.method,
          "Undefined property '" + expr.method.lexeme + "'.");
    }

    return method.bind(object);
  }
// lox/Interpreter.java, in visitSuperExpr()
```

�㶨����֮ǰ�� BostonCream ʾ��Ϊ������һ�ԡ��������Ҷ������ˣ�Ӧ�����ȳ����������͡�

### 13.3.3 super �ķǷ�ʹ��

����ǰ�����Թ���һ�������û���д��ȷ�Ĵ���ʱ�����ǵ�ʵ�ֻ�����ȷ�����飬������û���ý��������ܴ��������ֺ����ر�Ҫ���ǣ�

```js
class Eclair {
  cook() {
    super.cook();
    print "Pipe full of cr��me p?tissi��re.";
  }
}
```

�������һ��`super`���ʽ����û�г��ࡣ������ʱ����������`super`���ʽ�Ĵ���ٶ���super���ѳɹ����������ڻ������ҵ����⽫������ʧ�ܣ���Ϊû�г��࣬���û�г������Χ������JVM ���׳��쳣��ʹ���ǵĽ�����������

ѽ�����и��򵥵� super �����÷���

```js
super.notEvenInAClass();
```

���ǿ���������ʱͨ����顰super���Ĳ����Ƿ�ɹ������������󡣵������ǿ��Ծ�̬�ظ����㡪������ͨ���鿴Դ���롪��Eclairû�г��࣬���û��`super`���ʽ���������й�����ͬ�����ڵڶ��������У�����֪��`super`���ʽ�������ڷ������ڡ�

���� Lox �Ƕ�̬���͵ģ����Ⲣ����ζ������Ҫ��*��������*�Ƴٵ�����ʱ������û����˴�������ϣ���������Ǿ����ҵ������������ǽ��ڽ������о�̬������Щ����

���ȣ�������ö�������һ���°������Ը��ٵ�ǰ���ڷ��ʵĴ�����Χ�������ࡣ

```java
    NONE,
    CLASS,
    SUBCLASS
  }
// lox/Resolver.java, in enum ClassType, add ��,�� to previous line
```

���ǽ�ʹ���������ֺ�ʱ���ھ��г���Ͳ����г�������С������ǽ���һ��������ʱ���������Ƿ���һ�����ࡣ

```java
    if (stmt.superclass != null) {
      currentClass = ClassType.SUBCLASS;
      resolve(stmt.superclass);
// lox/Resolver.java, in visitClassStmt()
```

Ȼ�󣬵����ǽ���һ��`super`���ʽʱ�����ǻ������ǵ�ǰ�Ƿ�������ķ�Χ�ڡ�

```java
  public Void visitSuperExpr(Expr.Super expr) {
    if (currentClass == ClassType.NONE) {
      Lox.error(expr.keyword,
          "Can't use 'super' outside of a class.");
    } else if (currentClass != ClassType.SUBCLASS) {
      Lox.error(expr.keyword,
          "Can't use 'super' in a class with no superclass.");
    }

    resolveLocal(expr, expr.keyword);
// lox/Resolver.java, in visitSuperExpr()
```

������ǡ���o no�����û�����һ������

## 13.4 �ܽ�

���������ˣ����һ������������ Lox �� Java ʵ����������һ����롣����һ�������ĳɾͣ���Ӧ��Ϊ�˸е��Ժ����ڹ�ȥ��ʮ���£���ǧ�д����У�����ѧϰ��ʵ����...

- [token�ʹʷ�����](http://craftinginterpreters.com/scanning.html)��
- [�����﷨��](http://craftinginterpreters.com/representing-code.html)��
- [�ݹ��½�����](http://craftinginterpreters.com/parsing-expressions.html)��
- ǰ׺����׺���ʽ��
- ���������ʱ��ʾ��
- [ʹ�÷�����ģʽ���ʹ���](http://craftinginterpreters.com/evaluating-expressions.html)��
- [�ʷ�������](http://craftinginterpreters.com/statements-and-state.html)��
- ���ڴ洢�����Ļ�������
- [��������](http://craftinginterpreters.com/control-flow.html)��
- [�������ĺ���](http://craftinginterpreters.com/functions.html)��
- �հ���
- [��̬���������ʹ�����](http://craftinginterpreters.com/resolving-and-binding.html)��
- [��](http://craftinginterpreters.com/classes.html)��
- ���캯����
- �ֶΣ�
- ���������
- �̳С�

![](./assets/5f1cfb97369c9cb576e2eaa9dd5bf5ffcfc285a4.png)

���Ǵ�ͷ��ʼ���������Щ������û���ⲿ����������湤�ߡ�ֻ������ң����Ǹ��Ե��ı��༭����Java ��׼���еļ��������࣬�Լ� JVM ����ʱ��

���־�ŵڶ����ֵĽ����������Ǳ���Ľ�������Ϣһ�¡�Ҳ��дһЩ��Ȥ�� Lox ��������Ľ��������������ǡ�����������Ϊ��ȡ�û�����Ȳ�����Ӹ��౾��������������������ˬ��׼������ʱ�����ǽ���ʼ[��һ��ð��](http://craftinginterpreters.com/a-bytecode-virtual-machine.html)��

---

## [��ս](http://craftinginterpreters.com/inheritance.html#challenges)

1. Lox ��֧��*���̳�*����һ���������һ�����࣬���ǿ������÷�����Ψһ���������������Ѿ�̽���˶��ַ�ʽ�������ɵؿ������ú͹���������mixins��traits�����ؼ̳С�����̳С���չ�����ȡ�
   
   �����Ҫ������Щ˼·�� Lox ���һЩ���ܣ����ѡ���ĸ���Ϊʲô��������е�����������ʱ��Ӧ������������������������

2. �� Lox �У����ڴ���������������������һ�����ڲ��ҷ���ʱ�����Ǵ����νṹ�ĵײ���ʼ��Ȼ�������ϡ�������ķ������ڳ���ķ�����Ϊ�˴Ӹ��Ƿ����л�ȡ���෽����������ʹ��`super`.
   
   ����[BETA](https://beta.cs.au.dk/)����[�෴�ķ���](http://journal.stuffwithstuff.com/2012/12/19/the-impoliteness-of-overriding-methods/)���������һ������ʱ���������νṹ��*������ʼ������*���������෽��ʤ�����෽����Ϊ�˵������෽�������෽�����Ե���`inner`�����е��� ��������`super`�������ӵ���νṹ�е���һ��������
   
   ���෽�����ƺ�ʱ�ε���������Ľ�����Ϊ��������෽������������`inner`����ô�����û�а취���ǻ��޸ĳ������Ϊ��
   
   ȥ�� Lox ��ǰ�ĸ��Ǻ�`super`��Ϊ������ BETA �������滻���������֮��
   
   - �����ϵ��÷���ʱ������ѡ����̳�����*��ߵķ�����*
   
   - �ڷ������ڣ��� `inner`�ĵ������Ű��� �����this����`inner`֮��ļ̳���������������в��Ҿ�����ͬ���Ƶķ��������û��ƥ��ķ����������`inner`��ִ���κβ�����`
   
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

3. ���ҽ��� Lox ����һ���У���Ҫ��[��](http://craftinginterpreters.com/the-lox-language.html#challenges)���һЩ����Ϊ������ȱʧ�����ԡ�������֪����ι�����������ʵ������һ�����ܡ�