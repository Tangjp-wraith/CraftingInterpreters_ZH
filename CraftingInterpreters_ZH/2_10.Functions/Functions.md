# ����

> ��Ҳ������˼ά�Ĺ�����ʽ����ͨ������˼�븴�ϳ��½ṹ���½ṹ��Ϊ��˼�룬��Щ��˼�뱾��������ڻ������У���������ֹ��ѭ����Խ��ԽԶ��ÿ���˵Ļ��������������Ե�������
> 
> -- Douglas R. Hofstadter,*I Am a Strange Loop*

���±�־�Ŵ������ڹ����ĸ߳���ǰ����½ڱ������������õĹ��ܣ���ÿһ��ֻ���ṩ��һ��ƴͼ�����ǽ�����Щ���֡������ʽ����䡢�������������ʹʷ������򡪡���Ӹ��࣬��������ȫ����װ�ɶ��������û����庯���ͺ������õ�֧�֡�

![](./assets/2b6ff3fcb26bb833f1b454621a78bce2d2a2826f.png)

## 10.1 ��������

���϶���Ϥ C ���ĺ��������﷨�����﷨����������ʶ����Ҫ΢��öࡣ����ͨ���Ƕ����������ĵ��ã����磺

```c
average(1, 2);
```

�������ú���������ʵ���ϲ����ǵ����﷨��һ���֡������õĶ�������**��������**�����������κ���ֵΪ�����ı��ʽ�����ðɣ���ȷʵ������һ���ǳ�*�����ȼ�*�ı��ʽ���������Żᴦ����һ�㡣�����磺

> ������*��*Pascal �е����﷨��һ���֡���ֻ�ܵ�������������ֱ�Ӵ洢�ڱ����еĺ�����

```c
getCallback()();
```

�������������ñ��ʽ����һ������`getCallback`�����ı������ߡ����ǵڶ������ý�����`getCallback()`���ʽ��Ϊ�䱻�����ߡ����ʽ��������ű�ʾ�������á������Խ�������Ϊ�е����� `(` ��ͷ�ĺ�׺�������

�����������������ȼ������κ������������������һԪ���������������ͨ����`unary`����ð�ݵ�һ���¹���`call`�����������﷨�С�

```
unary          �� ( "!" | "-" ) unary | call ;
call           �� primary ( "(" arguments? ")" )* ;
```

�˹���ƥ��һ��primary���ʽ�������������������á����û�����ţ��⽫��������һ��primary���ʽ������ÿ�����ö���һ������ʶ�����а�����ѡ�Ĳ����б������б��﷨�ǣ�

> �ù���ʹ��`*`����ƥ��һϵ�е��ã�����`fn(1)(2)(3)`.�������Ĵ����� C ���������в����������������ڴ� ML ���������Լ��塣���������һ������������ĺ�����������������Ϊһϵ��Ƕ�׺�����ÿ����������һ������������һ���º������ú���ʹ����һ��������������һ���������������ơ����գ�һ�����в����������ĵ������һ������������˲�����
> 
> ���ַ�񣬳�Ϊ**currying**���� Haskell Curry��ͬһ���ˣ���������Ϊ*����*�����ĺ���ʽ����������ʣ�֮�󣬱�ֱ�����������﷨�У�����������������������ô��֡�

```
arguments      �� expression ( "," expression )* ;
```

�˹�����Ҫ����һ���������ʽ�������������������ʽ��ÿ�����ʽǰ�涼��һ�����š�Ϊ�˴�����������ã�`call`����������`arguments`����ʽ��Ϊ��ѡ��

�ҳ��ϣ����ڷǳ������ġ�����������ŷָ������ģʽ�������﷨���ƺ�����Ԥ�ڵĸ����Ρ���һЩ���ӵ�Ԫ�﷨���Ը��õش���������⣬�������ǵ� BNF ���Ҽ�����������Թ淶�У���������ô�鷳��

�����ǵ��﷨���������У����������һ���½ڵ㡣

```java
      "Binary   : Expr left, Token operator, Expr right",
      "Call     : Expr callee, Token paren, List<Expr> arguments",
      "Grouping : Expr expression",
// tool/GenerateAst.java, in main()
```

> Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#call-expression)�С�

���洢�������߱��ʽ�Ͳ����ı��ʽ�б������洢�����ŵı�ǡ������Ǳ����ɺ����������������ʱ����ʱ�����ǽ�ʹ�øñ�ǵ�λ�á�

�޸Ľ�������`unary()`��ǰֱ����ת��`primary()`���ĳ�call���ţ�`call()`��

```java
      return new Expr.Unary(operator, right);
    }

    return call();
  }
// lox/Parser.java, in unary(), replace 1 line
```

���Ķ����ǣ�

```java
  private Expr call() {
    Expr expr = primary();

    while (true) { 
      if (match(LEFT_PAREN)) {
        expr = finishCall(expr);
      } else {
        break;
      }
    }

    return expr;
  }
// lox/Parser.java, add after unary() 
```

> ʹ��`while (match(LEFT_PAREN))`��δ����������޴���`while (true)`��`break`����򵥡����ģ��������Ժ���չ��������������������ʱ������������塣

����Ĵ��벻̫�����﷨�������ƶ���һЩ������ʹ���������������������ӵ����д���������ݳ�Ʒ֮һ�����������ǽ�����׺������ķ�ʽ�������ơ����ȣ����ǽ���һ��primary���ʽ�������õġ������������Ȼ��ÿ�����ǿ��� `(`ʱ�����ǵ���`finishCall()`��ʹ����ǰ�����ı��ʽ��Ϊ�����������������ñ��ʽ�����صı��ʽ����µ�`expr`������ѭ���鿴����Ƿ񱻵��á�

���������б�Ĵ�����������������У�

```java
  private Expr finishCall(Expr callee) {
    List<Expr> arguments = new ArrayList<>();
    if (!check(RIGHT_PAREN)) {
      do {
        arguments.add(expression());
      } while (match(COMMA));
    }

    Token paren = consume(RIGHT_PAREN,
                          "Expect ')' after arguments.");

    return new Expr.Call(callee, paren, arguments);
  }
// lox/Parser.java, add after unary()
```

���������`arguments`����ɴ�����﷨���򣬳��˻���������������������ͨ���鿴��һ��token�Ƿ�Ϊ`)` ������������������ǣ��򲻻᳢�Խ����κβ�����

���򣬻����һ�����ʽ��Ȼ��Ѱ��һ�����ţ���ʾ����֮������һ��������ֻҪ��ÿ�����ʽ���ҵ����ţ��ͻ���������������Ҳ�������ʱ��������ɲ����б���ʹ��Ԥ�ڵ������š���󣬽��������ߺ���Щ������װ������ AST �ڵ��С�

### 10.1.1 ����������

���ڣ����ǽ���������ѭ��û�н��ޡ�����������һ����������������һ��������������������������⡣Ҫ��������

���������в�ͬ�ķ�����C ��׼��ʾ���ϱ�׼��ʵ�ֱ���֧��*����*127 ��������������û��˵�����κ����ޡ�Java �淶�涨һ�����������Խ���255 ��*����*��

> ����÷�����ʵ��������������Ϊ 254 ��������������Ϊ`this`���������Ľ����ߡ�������һ����ʽ���ݸ������Ĳ���һ��������������������һ���ۡ�

���ǵ� Lox Java ������ʵ���ϲ���Ҫ���ƣ��������������������������[�� III ����](http://craftinginterpreters.com/a-bytecode-virtual-machine.html)�е��ֽ��������������ϣ�����ǵ������������˴˼��ݣ���ʹ����������ֵļ��������Ҳ����ˣ���˽��� jlox �����ͬ�����ơ�

```java
      do {
        if (arguments.size() >= 255) {
          error(peek(), "Can't have more than 255 arguments.");
        }
        arguments.add(expression());
// lox/Parser.java, in finishCall()
```

ע�⣬�˴��Ĵ�����������������ʱ��*����*��������*�׳�*�ô����׳������ǽ���ֻ�ģʽ�ķ�ʽ��������������ڻ���״̬���Ҳ���֪�������﷨�е�λ�ã������������Ҫ�ġ����������������Ȼ������ȫ��Ч��״̬������ֻ�Ƿ�����̫��Ĳ�����������������󲢼������֡�

### 10.1.2 ���ͺ�������

����û���κο��Ե��õĺ������������ȿ�ʼʵ�ֵ����ƺ�����֣����ǵ����ǵ�������ʱ���ǻᵣ�ĵġ����ȣ���������Ҫһ������һЩ�µİ���

```java
import java.util.ArrayList;
import java.util.List;
// lox/Interpreter.java
```

������һ�������ʹ��µĵ��ñ��ʽ�ڵ���·��ʷ�����ʼ��

```java
  @Override
  public Object visitCallExpr(Expr.Call expr) {
    Object callee = evaluate(expr.callee);

    List<Object> arguments = new ArrayList<>();
    for (Expr argument : expr.arguments) { 
      arguments.add(evaluate(argument));
    }

    LoxCallable function = (LoxCallable)callee;
    return function.call(this, arguments);
  }
// lox/Interpreter.java, add after visitBinaryExpr() 
```

���ȣ������������ߵı��ʽ��ͨ����������ʽֻ��һ����ʶ������ͨ�����Ʋ��Һ����������������κζ�����Ȼ��˳������ÿ���������ʽ�������ֵ�洢���б��С�

> ������һ��΢�������ѡ�����ڲ������ʽ�����и����ã�������ǵ���ֵ˳��������û��ɼ��ġ�������ˣ�ĳЩ���ԣ��� Scheme �� C����δָ��˳����ʹ�������������ɵض������������������Ч�ʣ�������ζ���������δ������������˳����ֵ���û����ܻ�е�����졣

һ��׼���ñ������ߺͲ�����ʣ�µľ���ִ�е��á�ͨ������������ת��ΪLoxCallableȻ�������ϵ���`call()`������������һ�㡣��������һ�����õ��κ� Lox ����� Java ��ʾ����ʵ�ִ˽ӿڡ�����Ȼ�����û�����ĺ�������Ҳ�����������Ϊ�౻�����á��Թ�����ʵ�������Ǻܿ컹�Ὣ��������һ��Ŀ�ġ�

> ��������ǰ���ˡ�Lox������������ Java ��׼���Դ��� Callable �ӿڡ�las�����кõļ����ƶ��ѱ�ʹ�á�

����½ӿ�û��̫�����ݡ�

```java
package com.craftinginterpreters.lox;

import java.util.List;

interface LoxCallable {
  Object call(Interpreter interpreter, List<Object> arguments);
}
// lox/LoxCallable.java, create new file
```

���Ǵ���������Է���ʵ��`call()`��Ҫ�������ǻ�������������ֵ���б�ʵ����Ĺ������Ƿ��ص��ñ��ʽ������ֵ��

### 10.1.3 �������ʹ���

�ڿ�ʼʵ�� LoxCallable ֮ǰ��������Ҫʹ���ʷ�������׳һЩ����Ŀǰ������һЩ���ǲ��ܼ�װ���ᷢ���Ĺ���ģʽ�����ȣ������������ʵ���ϲ��ǿ��Ե��õĶ��󣬻ᷢ��ʲô����������������������������

```java
"totally not a function"();
```

Lox �в��ɵ����ַ�����Lox �ַ���������ʱ��ʾ�� Java �ַ�������˽���ת��Ϊ LoxCallable ʱ��JVM ���׳� ClassCastException�����ǲ�ϣ���������³�һЩ����� Java ��ջ����Ȼ���������෴��������Ҫ���Լ�������͡�

```java
    }

    if (!(callee instanceof LoxCallable)) {
      throw new RuntimeError(expr.paren,
          "Can only call functions and classes.");
    }

    LoxCallable function = (LoxCallable)callee;
// lox/Interpreter.java, in visitCallExpr()
```

��Ȼ�׳�һ���쳣�������������׳��Լ����쳣���ͣ�һ�ֽ�����֪���������ŵز���ͱ�����쳣���͡�

### 10.1.4 ��鴫���������

��һ�������뺯����**arity**�йء�Arity �Ǻ�������������Ĳ����������������һԪ�������Ԫ��Ϊ 1����Ԫ�������Ԫ��Ϊ 2���ȵȡ����ں�����Ԫ�����������Ĳ�������������

```java
fun add(a, b, c) {
  print a + b + c;
}
```

�����������������������`a`,`b`, ��`c`����������Ԫ��������������Ҫ������������ô�����������������������

```java
add(1, 2, 3, 4); // Too many.
add(1, 2);       // Too few.
```

��ͬ�����Զ���������ȡ��ͬ�ķ�������Ȼ���������̬�������Ի��ڱ���ʱ�����һ�㣬������������뺯����Ԫ����ƥ�䣬��ܾ�������롣JavaScript �ᶪ�������ݵ��κζ������������㴫�ݵĲ����࣬����������� sort-of-like-null-but-not-really value ���ȱʧ�Ĳ���`undefined`��Python ���ϸ���������б�̫�̻�̫����������������ʱ����

����Ϊ�����Ǹ��õķ��������ݴ��������Ĳ�����������һ��������������ʵ���з��Ĵ��󡣼��ڴˣ�ʵʩԽ�������ҵ�ע��Խ�á����Զ��� Lox�����ǽ����� Python �ķ������ڵ��ÿɵ��ö���֮ǰ�����Ǽ������б�ĳ����Ƿ���ɵ��ö����Ԫ��ƥ�䡣

```java
    LoxCallable function = (LoxCallable)callee;
    if (arguments.size() != function.arity()) {
      throw new RuntimeError(expr.paren, "Expected " +
          function.arity() + " arguments but got " +
          arguments.size() + ".");
    }

    return function.call(this, arguments);
// lox/Interpreter.java, in visitCallExpr()
```

����Ҫ LoxCallable �ӿ��ϵ�һ���·�����ѯ������Ԫ����

```java
interface LoxCallable {
  int arity();
  Object call(Interpreter interpreter, List<Object> arguments);
// lox/LoxCallable.java, in interface LoxCallable
```

����*����*�� arity �������`call()`.���ǣ��������ǽ��ж����ʵ�� LoxCallable��������ջ��ڼ������н���������֤���������������ʷ����п�����һ���ط��������

## 10.2 ԭ������

���������ǿ��Ե��ú��������ǻ�û�к������Ե��á��ڽ����û�����ĺ���֮ǰ�������ǽ�������ʵ�ֵ�һ����Ҫ�����������ӵķ���ĺ�ʱ������**ԭ������**����Щ�ǽ��������û����빫���ĺ����������������������ԣ������ǵ��������� Java�������Ǳ�ʵ�ֵ����ԣ�Lox��ʵ�ֵġ�

��ʱ��Щ����Ϊ**ԭ��**��**�ⲿ����**��**�ⲿ����**�����ڿ������û���������ʱ������Щ������������ǹ�����ʵ������ʱ��һ���֡�����������鼮���ڸ�����Щ����Ϊ�����ڸ����ϲ�����Ȥ�����Ǵ���Ƿ��صĹ�����

> ��ֵ��ǣ���Щ�������������ơ�����native���͡�foreign�������Ƿ���ʡ�Ҳ����ȡ����ѡ���������˵Ĺ۵㡣�������Ϊ�Լ������������ʱ��ʵ�֣������ǵ��������� Java���У���ô������д�ĺ������ǡ�ԭ���ġ���������������������*�û�*����̬����ô����ʱ��������һЩ�����������ʵ�ֵġ�
> 
> ���߿����ǡ�native��ָ���ǵײ�Ӳ���Ļ����������ԡ��� Java �У���native���������� C �� C++ ʵ�ֲ�����Ϊ���ػ�������ķ�����

![](./assets/0615145ec6b523d736b132142c89e064c6f240b3.png)

���ǣ�Ҫʹ�������������ó������õ����飬����ʵ���ṩ��native�����ǹؼ��������ṩ�����г����������ݵĻ�������ķ��ʡ���������ṩ�����ļ�ϵͳ��native�������û������ѱ�д��ȡ����ʾ�ļ��ĳ���

> ����ÿһ�����Զ��ṩ�ľ��䱾�������ǽ��ı���ӡ����׼������� Lox �У�����`print`��һ�������������Ա����ǿ����ڱ���֮ǰ���½��н�������ʾ����Ļ�ϡ�
> 
> һ���������˺��������ǾͿ���ͨ��˺���ɵĴ�ӡ�﷨�������滻Ϊ���������������ԡ�������ζ�ű���ǰ������Ӳ����ں����½ڵĽ����������У���֮��Ȼ�����ԣ������Ȿ�飬�Ҳ���ȥ������
> 
> ���ǣ����������Ϊ*�Լ���*���Թ�������������������Ҫ����һ�¡�

������Ի������û��ṩ�Լ���ԭ�����ܡ��������Ļ��Ƴ�Ϊ**�ⲿ�����ӿ�**(**FFI**)��**������չ**��**�����ӿ�**�����ƵĶ�������Щ�ܺã���Ϊ����ʹ����ʵ���������ṩ�Եײ�ƽ̨֧�ֵ�ÿһ�����ܵķ��ʡ����ǲ���Ϊ jlox ���� FFI�������ǻ����һ��ԭ�������������˽��������ʲô���ӡ�

### 10.2.1 ���ʱ��

������[�� III ����](http://craftinginterpreters.com/a-bytecode-virtual-machine.html)����ʼ���ָ���Ч��ʵ�� Lox ʱ�����ǽ��ǳ��������ܡ����ܹ�����Ҫ��������������ζ��**��׼**����Щ��������ִ�н�����ĳ�����������ʱ�䡣

���ǿ��Բ������������������л�׼���Ժ��˳������ʱ�䣬��������Ӻܶ࿪������JVM����ʱ�䡢����ϵͳ������ȡ���Ȼ����Щ����ȷʵ����Ҫ���������ֻ����ͼ��֤��ĳЩ���������Ż�������ϣ�����ֿ����ڸ����Ľ����

���õĽ���������û�׼�ű������������������֮�侭����ʱ�䡣Ϊ�ˣ�Lox ������Ҫ�ܹ��ж�ʱ�䡣����û�а취������һ�㡪����������ʼ�����ϵĵײ�ʱ�ӣ������޷�����ͷ��ʼ��ʵ�����õ�ʱ�ӡ�

�������ǽ����`clock()`һ��ԭ����������������ĳ���̶�ʱ�������������������������������֮��Ĳ�����������ε���֮�侭���˶���ʱ�䡣�����������ȫ�ַ�Χ�ڶ���ģ�����������ȷ�����������Է�������

```java
class Interpreter implements Expr.Visitor<Object>,
                             Stmt.Visitor<Void> {
  final Environment globals = new Environment();
  private Environment environment = globals;

  void interpret(List<Stmt> statements) {
// lox/Interpreter.java, in class Interpreter, replace 1 line
```

�����ǽ�����˳����ط�Χʱ���������е�`environment`�ֶλᷢ���仯��������*��ǰ*�����������`globals`����ӵ�ж������ȫ�ֻ����Ĺ̶����á�

��ʵ����һ��������ʱ����ԭ��������䵽��ȫ�ַ�Χ�ڡ�

```java
  private Environment environment = globals;

  Interpreter() {
    globals.define("clock", new LoxCallable() {
      @Override
      public int arity() { return 0; }

      @Override
      public Object call(Interpreter interpreter,
                         List<Object> arguments) {
        return (double)System.currentTimeMillis() / 1000.0;
      }

      @Override
      public String toString() { return "<native fn>"; }
    });
  }

  void interpret(List<Stmt> statements) {
// lox/Interpreter.java, in class Interpreter 
```

�ⶨ����һ����Ϊ��clock���ı���������ֵ��һ��ʵ�� LoxCallable �� Java �����ࡣ��`clock()`�����������κβ������������Ԫ��Ϊ�㡣`call()`��ʵ�ֵ�����Ӧ�� Java ���������������Ϊ��λת��Ϊ double ֵ��

> �� Lox �У������ͱ���ռ����ͬ�������ռ䡣�� Common Lisp �У������������Լ��������������ͬ���Ƶĺ����ͱ������ᷢ����ͻ������������ƣ�������Һ������������������������ұ���������ȷʵ�뽫������Ϊһ��ֵ����ʱ����ȷʵ��Ҫ����һЩ���衣
> 
> Richard P. Gabriel �� Kent Pitman ���������Lisp-1����ָ���� Scheme �����������ͱ�������ͬһ�������ռ��е����ԣ�����Lisp-2����ָ�� Common Lisp ���������Ƿֿ������ԡ�������ȫ��͸��������Щ����һֱ��������Lox �� Lisp-1��

����������������ԭ�����ܡ�����ȡ�û����롢�����ļ��ȡ������ǿ��Խ����Ƿֱ����Ϊʵ�� LoxCallable ���Լ��������ࡣ���Ƕ����Ȿ�飬�Ȿ���������������Ҫ�ġ�

�����ǰ��ѹ��ܶ���ҵ�������ǵ��û��ӹ�...

## 10.3 ��������

�������ڿ�����`declaration`��ӱ���ʱ����Ĺ��������һ���µĲ���ʽ�����������������һ������һ�������ơ�����ζ������ֻ�������������ĵط�ʹ�á�

> �����������������������ĵ�һԭʼ����������������ͬ������﷨�ǣ�(1) ����һ���µĺ��������Լ� (2) ��һ���±����󶨵�������� Lox �������������﷨�����ǾͲ���Ҫ������������ˡ��������������
> 
> ```c
> var add = fun (a, b) {
>   print a + b;
> };
> ```

> Ȼ�����������������ǳ�����������Ҽ���Ϊ�����ṩ Lox Ư�����﷨��

```
declaration    �� funDecl
               | varDecl
               | statement ;
```

���º��`declaration`���������������¹���

```
funDecl        �� "fun" function ;
function       �� IDENTIFIER "(" parameters? ")" block ;
```

��`funDecl`����ʹ�õ����ĸ�������`function`������*�������*��`fun`�ؼ��֣����ʵ�ʵĺ������ݡ������ǽ�����ʱ�������ø�`function`������������������Щ�����������ں�����������ǰ��û��`fun`.

> ����̫�����ˣ�û����Ȥ��

����������һ�����ƣ���������ŵĲ����б�����塣����ʼ���ǻ����ſ飬ʹ����������ͬ���﷨���򡣲����б�ʹ�ô˹���

```
parameters     �� IDENTIFIER ( "," IDENTIFIER )* ;
```

��������ǰ���`arguments`����ֻ��ÿ����������һ����ʶ����������һ�����ʽ�����ǽ�������Ҫ��ϸ�о���������﷨�������ɵ� AST�ڵ㻹����

```java
      "Expression : Expr expression",
      "Function   : Token name, List<Token> params," +
                  " List<Stmt> body",
      "If         : Expr condition, Stmt thenBranch," +
// tool/GenerateAst.java, in main()
```

> Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#function-statement)�С�

�����ڵ���һ�����ơ�һ�������б����ǵ����ƣ���Ȼ���Ǻ����塣���ǽ�������洢Ϊ�������ڰ���������б�

�ڽ������У�����ע�����µ�������

```java
    try {
      if (match(FUN)) return function("function");
      if (match(VAR)) return varDeclaration();
//lox/Parser.java, in declaration()
```

���������һ����������ǰ���ؼ���ʶ�𡣵���������ʱ`fun`������`function`�����Ӧ��`function`�﷨������Ϊ�Ѿ�ƥ�䲢ʹ����`fun`�ؼ��֡����ǽ�һ�ι���һ���������������ʼ��

```java
 private Stmt.Function function(String kind) {
    Token name = consume(IDENTIFIER, "Expect " + kind + " name.");
  }
// lox/Parser.java, add after expressionStatement()  
```

���ڣ���ֻʹ�ú������Ƶı�ʶ��token����������֪���Ǹ�`kind`������ʲô�á��������������﷨����һ�����Ժ����ø�`function()`�������������ڲ��ķ�����������������ʱ�����ǽ����롰method����`kind`�Ա������Ϣ�ض������ڽ������������͡�

�����������������б�Ͱ���������Χ��һ�����š�

```java
    Token name = consume(IDENTIFIER, "Expect " + kind + " name.");
    consume(LEFT_PAREN, "Expect '(' after " + kind + " name.");
    List<Token> parameters = new ArrayList<>();
    if (!check(RIGHT_PAREN)) {
      do {
        if (parameters.size() >= 255) {
          error(peek(), "Can't have more than 255 parameters.");
        }

        parameters.add(
            consume(IDENTIFIER, "Expect parameter name."));
      } while (match(COMMA));
    }
    consume(RIGHT_PAREN, "Expect ')' after parameters.");
  }
// lox/Parser.java, in function()
```

���������ڵ����д�������Ĵ��룬ֻ��û�в�ֳɸ������������`if`��䴦�������������ڲ�`while`ѭ����������ֻҪ�����ҵ����ŷָ����ǡ������ÿ���������Ƶ�token�б�

�����������õĲ���һ�����ڽ���ʱ��֤��û�г������������������������

������ǽ������岢����ȫ����װ��һ�������ڵ��С�

```java
    consume(RIGHT_PAREN, "Expect ')' after parameters.");

    consume(LEFT_BRACE, "Expect '{' before " + kind + " body.");
    List<Stmt> body = block();
    return new Stmt.Function(name, parameters, body);
  }
// lox/Parser.java, in function()
```

��ע�⣬�����ڵ���`block()`֮ǰ����������Ŀ�ͷ`{`��������Ϊ`block()`��������ű���Ѿ�ƥ�䡣���û���ҵ�`{`��������ʹ�������������Ǳ����׼ȷ�Ĵ�����Ϣ����Ϊ����֪�����ں����������������С�

## 10.4 ��������

�����Ѿ�������һЩ�﷨�������Ѿ�׼���ý��н��ͣ���������Ҫ��������� Java �б�ʾ Lox ������������Ҫ���ٲ������Ա�����ڵ��ú���ʱ�����ǰ󶨵�����ֵ����Ȼ����Ҫ����������Ĵ��룬�Ա����ִ������

������Ͼ��� Stmt.Function ������ݡ����ǿ������Ǹ��𣿲�࣬������ȫ�ǡ����ǻ���Ҫһ��ʵ�� LoxCallable ���࣬�Ա����ǿ��Ե����������ǲ�ϣ��������������ʱ�׶���͸��ǰ�˵��﷨���У�������ǲ�ϣ�� Stmt.Function ����ʵ�������෴�����ǽ����װ��һ�������С�

```java
package com.craftinginterpreters.lox;

import java.util.List;

class LoxFunction implements LoxCallable {
  private final Stmt.Function declaration;
  LoxFunction(Stmt.Function declaration) {
    this.declaration = declaration;
  }
}
// lox/LoxFunction.java, create new file
```

����������ʵ�� LoxCallable ��`call()`��

```java
  @Override
  public Object call(Interpreter interpreter,
                     List<Object> arguments) {
    Environment environment = new Environment(interpreter.globals);
    for (int i = 0; i < declaration.params.size(); i++) {
      environment.define(declaration.params.get(i).lexeme,
          arguments.get(i));
    }

    interpreter.executeBlock(declaration.body, environment);
    return null;
  }
// lox/LoxFunction.java, add after LoxFunction()  
```

�⼸�д��������ǽ����������������ǿ��Ĳ���֮һ����������[������״̬](http://craftinginterpreters.com/statements-and-state.html)һ���п����ģ��������ƻ���������ʵ�ֵĺ��Ĳ��֡��������������ء�

> [���ǽ�����һ��](http://craftinginterpreters.com/resolving-and-binding.html)�������̽�ֻ�����

�����Ǻ����ĺ��ģ������Ǻ���*��װ*�����Ĳ�����������֮����������뿴�������ǡ�����ζ��ÿ�����������Լ��Ļ������洢��Щ������

���⣬���붯̬�����˻�����ÿ������*����*�����Լ��Ļ��������򣬵ݹ���жϡ����ͬʱ�ж����ͬһ�������ĵ��ã�ÿ�����ö���Ҫ*�Լ���*��������ʹ���Ƕ��Ƕ�ͬһ�������ĵ��á�

���磬������һ�����ӵ��������ķ�����

```java
fun count(n) {
  if (n > 1) count(n - 1);
  print n;
}

count(3);
```

����һ�£��ڽ��������������ڲ�Ƕ�׵����д�ӡ 1 ʱ��ͣ���������� print 2 �� 3 ���ⲿ���û�û�д�ӡ���ǵ�ֵ������ڴ���һ���л�����Ȼ�洢`n`��һ���������а󶨵� 3������һ���������а󶨵� 2����������󶨵� 1 ����ʵ�����磺

![](./assets/3803da5525a13c9692cb5cd4586ecac1c5418448.png)

�����Ϊʲô��ÿ��*����*ʱ����һ���»������������ں���*����*��������֮ǰ������`call()`���������������ġ��ڵ��ÿ�ʼʱ�����ᴴ��һ���»�����Ȼ������ͬ���ķ�ʽ���������Ͳ����б�����ÿһ�ԣ���ʹ�ò��������ƴ���һ���±���������󶨵�������ֵ��

���ԣ����������ĳ���

```java
fun add(a, b, c) {
  print a + b + c;
}

add(1, 2, 3);
```

�ڵ��� ʱ`add()`�������������������ݣ�

![](./assets/d0436ae41828ffd11ab05b8d63c04a8ad346deee.png)

Ȼ��`call()`���߽�����������µĺ����ֲ�������ִ�к����塣��ĿǰΪֹ����ǰ�����ǵ��ú����Ļ��������ڣ����Ǵ����ﴫ�͵�����Ϊ�����������²����ռ��ڡ�

����ǽ����ݴ��ݸ����������ȫ�����ݡ�ͨ����ִ������ʱʹ�ò�ͬ�Ļ�����ʹ����ͬ�Ĵ��������ͬ�ĺ������ܻ������ͬ�Ľ����

һ��������ִ����ϣ���`executeBlock()`�����ú����ֲ��������ָ�֮ǰ�ڵ��õ㴦�ڻ״̬�Ļ��������`call()`returns `null`��������`nil`�������ߡ��������Ժ����ӷ���ֵ����

�ӻ�е�Ͻ�������ǳ��򵥡��߼����嵥����һЩ�±���������һ��������������Ǻ��������Ľᾧ*����*��ɻ�������*����*�ĵط�������������������ϲ����Ƭ��֮һ�������Ը�⣬�뻨��ʱ��ڤ��һ�¡�

�����𣿺õġ���ע�⣬���󶨲���ʱ�����Ǽ�������Ͳ����б������ͬ�ĳ��ȡ����ǰ�ȫ�ģ���Ϊ`visitCallExpr()`�ڵ���`call()`֮ǰ��� arity���������ڱ����������Ĺ�����������һ�㡣

```java
  @Override
  public int arity() {
    return declaration.params.size();
  }
// lox/LoxFunction.java, add after LoxFunction()  
```

��������ǵĴ󲿷ֶ����ʾ��������������ʱ��Ҳ����ʵ��`toString()`.

```java
@Override
  public String toString() {
    return "<fn " + declaration.name.lexeme + ">";
  }
// lox/LoxFunction.java, add after LoxFunction()  
```

����û�������ӡ����ֵ������ṩ���õ������

```java
fun add(a, b) {
  print a + b;
}

print add; // "<fn add>".
```

### 10.4.1 ���ͺ�������

���Ǻܿ�ͻ�������� LoxFunction���������Կ�ʼ�ˡ��������ǿ��Է���һ������������

```java
  @Override
  public Void visitFunctionStmt(Stmt.Function stmt) {
    LoxFunction function = new LoxFunction(stmt);
    environment.define(stmt.name.lexeme, function);
    return null;
  }
// lox/Interpreter.java, add after visitExpressionStmt()  
```

�����������ǽ�����������ֵ���ʽ�ķ�ʽ�����ú���*�﷨�ڵ�*���������ı���ʱ��ʾ����������ת��Ϊ������ʱ��ʾ�����������һ����װ�﷨�ڵ�� LoxFunction��

����������ͬ����������ֵ�ڵ㣬��Ϊ����*����*�������󶨵��±�������ˣ��ڴ��� LoxFunction ֮�������ڵ�ǰ�����д���һ���°󶨲������д洢���������á�

���������ǾͿ����� Lox �ж���͵��������Լ��ĺ�������һ�ԣ�

```java
fun sayHi(first, last) {
  print "Hi, " + first + " " + last + "!";
}

sayHi("Dear", "Reader");
```

�Ҳ��˽��㣬��������˵���⿴��������һ�������ı�����ԡ�

## 10.5 Return���

����ͨ�����ݲ��������ݴ��뺯������û�а취�����*ȡ��*����� Lox ���� Ruby �� Scheme ������������ʽ�����ԣ���ô�����彫��һ�����ʽ����ֵ��ʽΪ�����Ľ�������� Lox �У���������һ��������ֵ������б����������Ҫר�ŵ��﷨��������������仰˵��`return`��䡣�������Ѿ��µ����﷨��

> �����ùݵ����ݡ�

```java
statement      �� exprStmt
               | forStmt
               | ifStmt
               | printStmt
               | returnStmt
               | whileStmt
               | block ;

returnStmt     �� "return" expression? ";" ;
```

���ǻ���һ������ʵ���������һ���������տɾ���`statement`����������`return`�����`return`�ؼ��ֺ��һ����ѡ���ʽ���ԷֺŽ�����

����ֵ�ǿ�ѡ�ģ���֧�ִӲ���������ֵ�ĺ�������ǰ�˳����ھ�̬���������У���void������������ֵ���� void ��������ֵ������ Lox �Ƕ�̬���͵ģ����û�������� void �������������޷���ֹ����ȡ���ò�����`return`���ĺ����Ľ��ֵ��

```java
fun procedure() {
  print "don't return anything";
}

var result = procedure();
print result; // ?
```

����ζ��ÿ�� Lox ���������뷵��*something*����ʹ�������������κ�`return`��䡣Ϊ������ʹ��`nil`������� LoxFunction ��ʵ�������`call()`���ص�ԭ��`null`��ͬ�����������`return`�����ʡ����ֵ��ֻ�ǽ�����Ϊ��ͬ�ڣ�

```java
return nil;
```

��AST �������У������һ���½ڵ㡣

```java
      "Print      : Expr expression",
      "Return     : Token keyword, Expr value",
      "Var        : Token name, Expr initializer",
// tool/GenerateAst.java, in main()
```

> Ϊ�½ڵ����ɵĴ�����[��¼ II](http://craftinginterpreters.com/appendix-ii.html#return-statement)�С�

������`return`�ؼ���token����˿���ʹ������λ������������Լ����ص�ֵ������У������������һ��������������ͨ��ʶ���ʼ�ؼ���

```java
    if (match(PRINT)) return printStatement();
    if (match(RETURN)) return returnStatement();
    if (match(WHILE)) return whileStatement();
// lox/Parser.java, in statement()
```

���֧����

```java
  private Stmt returnStatement() {
    Token keyword = previous();
    Expr value = null;
    if (!check(SEMICOLON)) {
      value = expression();
    }

    consume(SEMICOLON, "Expect ';' after return value.");
    return new Stmt.Return(keyword, value);
  }
// lox/Parser.java, add after printStatement() 
```

���ҵ�֮ǰʹ�õ�`return`�ؼ��ֺ�����Ѱ��һ��ֵ���ʽ��������಻ͬ��token���ܻ�����һ�����ʽ����˺����ж��Ƿ���ڷ���*ֵ*���෴�����Ǽ�����Ƿ�*������*�����ڷֺŲ��ܿ�ʼһ�����ʽ�������һ��token���Ǹ�������֪��һ��û��ֵ��

### 10.5.1 �ӵ����з���

����һ��`return`����Ǽ��ֵġ������ԴӺ������ڵ��κεط����أ�������������Ƕ������������С�ִ�з���ʱ����������Ҫһ·��������ǰ���ڵ��κ������Ĳ����º���������ɣ�����ĳ�ֱ�����Ŀ���������һ����

���磬��������������������������ǽ�Ҫִ��`return`��䣺

```java
fun count(n) {
  while (n < 100) {
    if (n == 3) return n; // <--
    print n;
    n = n + 1;
  }
}

count(1);
```

Java ���ö�ջĿǰ����������ʾ��

```java
Interpreter.visitReturnStmt()
Interpreter.visitIfStmt()
Interpreter.executeBlock()
Interpreter.visitBlockStmt()
Interpreter.visitWhileStmt()
Interpreter.executeBlock()
LoxFunction.call()
Interpreter.visitCallExpr()
```

������Ҫ��ջ��һֱ���ص�`call()`���Ҳ�֪������ô��Ϊ����������˵�������������쳣��������ִ��һ��`return`���ʱ�����ǽ�ʹ��һ���쳣���ý�����ͨ�����а������ķ��ʷ����ص���ʼִ������Ĵ��롣

�����µ� AST �ڵ�ķ��ʷ���������ʾ��

```java
  @Override
  public Void visitReturnStmt(Stmt.Return stmt) {
    Object value = null;
    if (stmt.value != null) value = evaluate(stmt.value);

    throw new Return(value);
  }
// lox/Interpreter.java, add after visitPrintStmt() 
```

����з���ֵ������������������ʹ��`nil`.Ȼ���ȡ��ֵ�������װ���Զ����쳣���в��׳�����

```java
package com.craftinginterpreters.lox;

class Return extends RuntimeException {
  final Object value;

  Return(Object value) {
    super(null, null, false, false);
    this.value = value;
  }
}
// lox/Return.java, create new file
```

����ʹ�� Java ����ʱ�쳣�������װ����װ����ֵ��������Щ����`null`��`false`��������ֵĳ������캯�����ý�����һЩ���ǲ���Ҫ�� JVM ���ơ����ڽ��쳣�����ڿ�����������ʵ�ʵĴ�����������ǲ���Ҫ���ջ���������Ŀ�����

> ��Ϊ��¼����ͨ����ϲ��ʹ���쳣���������̡�����һ���߶ȵݹ�������߽������У�����Ҫ�ߵ�·�����������Լ����﷨�������� Java ���ö�ջ������أ�������ǲ��ò�ż������һЩ�������ĵ��ö�ջ���������쳣��һ������Ĺ��ߡ�

����ϣ����һֱչ�����������ÿ�ʼ�ĵط�����LoxFunction �е�`call()`������

```java
         arguments.get(i));
    }

    try {
      interpreter.executeBlock(declaration.body, environment);
    } catch (Return returnValue) {
      return returnValue.value;
    }
    return null;
// lox/LoxFunction.java, in call(), replace 1 line
```

���ǽ����ð�װ`executeBlock()`�� try-catch ���С��������񵽷����쳣ʱ��������ȡֵ��������Ϊ`call()`����ֵ���������δ����`return`��Щ�쳣֮һ������ζ�Ÿú�����û��������������µ������������ĩβ������������£�����ʽ����`nil`��

���������Կ����������㹻��������֧�������������ӡ���һ������쳲��������еĵݹ麯����

```c
fun fib(n) {
  if (n <= 1) return n;
  return fib(n - 2) + fib(n - 1);
}

for (var i = 0; i < 20; i = i + 1) {
  print fib(i);
}
```

���С���򼸺���ϰ�������ڹ�ȥ������ʵ�ֵ������������ԡ������ʽ����������֧��ѭ�����������������������á������󶨺ͷ��ء�

> �����ܻ�ע�⵽��ǳ�������Ȼ���ݹ鲻�Ǽ��� Fibonacci ��������Ч����������Ϊ΢��׼�������Ժܺõض����ǵĽ�����ʵ�ֺ������õ��ٶȽ���ѹ�����ԡ�
> 
> �������������ǡ����Ǻܿ족��û��ϵ�����ǵ� C ����������졣

## 10.6 �ֲ������ͱհ�

���ǵĺ������ܷǳ���ȫ������һ��©����Ҫ�޲�����ʵ�ϣ�����һ���㹻��Ĳ�࣬���ǽ���[��һ��](http://craftinginterpreters.com/resolving-and-binding.html)�л��Ѵ󲿷�ʱ����������������ǿ��Դ����￪ʼ��

LoxFunction ��ʵ��`call()`������һ���󶨺����������»�������������չʾ�ô���ʱ��������һ���ص㣺�û�����*��*����ʲô��

���ڣ���ʼ���Ƕ���ȫ�ֻ���`globals`�����������һ����ʶ��û���ں������ڶ��壬������������ȫ�ַ�Χ�ڵĺ����ⲿѰ������`fib`�� Fibonacci ʾ���У�����ǽ������ܹ��ں��������ڲ����ҵݹ���� �ķ�ʽ����`fib`��һ��ȫ�ֱ�����

������һ�£��� Lox �У�������������*���κ�*���԰����Ƶĵط�ʹ�á������ Lox �ű��Ķ��㣬Ҳ������������������ڲ���Lox ֧������һ�������ڶ����Ƕ���ڿ��ڵ�**�ֲ�������**

�����������ʾ����

```c
fun makeCounter() {
  var i = 0;
  fun count() {
    i = i + 1;
    print i;
  }

  return count;
}

var counter = makeCounter();
counter(); // "1".
counter(); // "2".
```

�����`count()`�е�`i`�������ⲿ��`makeCounter()`������������`makeCounter()`���ض�`count()`���������ã�Ȼ�����Լ��ĺ������Ѿ�ִ�����ˡ�

ͬʱ�����������÷��ص�`count()`���������ִ��`count()`�ĺ����壬����ֵ����ȡ`i`����ʹ����ĺ���`i`�Ѿ��˳���

�������ǰ��δ����������Ƕ�׺��������ԣ�����ܿ������ܷ�񣬵��û�ȷʵϣ�����ܹ��������ǣ����������������������ڵ���`counter()`ʱ  ��õ�һ��δ����ı�������`i`��������Ϊ������ʵ�����������ģ�

![](./assets/a7796628aaa67b37602a8e2f81fa1e62c00e5b00.png)

�����ǵ���ʱ`count()`��ͨ���洢��`counter`�еĶ��������ã�������Ϊ�����崴����һ���µĿջ��������ĸ�����ȫ�ֻ���������ʧȥ��`makeCounter()`��`i`�Ļ�����

�����ؿ�һ�㡣������`makeCounter()`�ĺ�����������`count()`ʱ�����������ӣ�

![](./assets/db5a0218a97a124535d91e4219a2ed7b005880d0.png)

���������������ĵط������ǿ��Կ���`i`�����ǵ�����`makeCounter()`���˳����ĺ�����ʱ���������ᶪ���Ǹ����������ڽ��������ᱣ��`count()`��Χ�Ļ�����Ȼ���������������������

�����ݽṹ��Ϊ**�հ�**����Ϊ�����رա�������������������Χ�������հ������ڵ� Lisp ʱ�����Ѿ����ڣ��������Ժڿ��Ѿ�����˸��ָ����ķ�����ʵ�����ǡ����� jlox�����ǽ��������Ч�����顣�� LoxFunction �У��������һ���ֶ����洢������

> ���հ����� Peter J. Landin �������һ������Ҽ�����������֮ǰ���������ѧ�ҽ�ʹ��ԭʼ�Ĺ������ͱȻ������໥������

```java
  private final Stmt.Function declaration;
  private final Environment closure;

  LoxFunction(Stmt.Function declaration) {
// lox/LoxFunction.java, in class LoxFunction  
```

�����ڹ��캯���г�ʼ������

```java
LoxFunction(Stmt.Function declaration, Environment closure) {
    this.closure = closure;
    this.declaration = declaration;
// lox/LoxFunction.java, constructor LoxFunction(), replace 1 line
```

�����Ǵ��� LoxFunction ʱ���Ჶ��ǰ����

```java
public Void visitFunctionStmt(Stmt.Function stmt) {
    LoxFunction function = new LoxFunction(stmt, environment);
    environment.define(stmt.name.lexeme, function);
// lox/Interpreter.java, in visitFunctionStmt(), replace 1 line
```

������*����*����ʱ����Ļ�������������*����*����ʱ����Ļ�����������������Ҫ�ġ�����ʾ����������Χ�Ĵʷ���Χ����󣬵����øú���ʱ�����û����������õĸ�����������ֱ��ת��`globals`.

```java
                     List<Object> arguments) {
    Environment environment = new Environment(closure);
    for (int i = 0; i < declaration.params.size(); i++) {
// lox/LoxFunction.java, in call(), replace 1 line
```

�ⴴ����һ�����������Ӻ����嵽���������Ļ�����һֱ��ȫ�ַ�Χ������ʱ������ƥ��������Ҫ��Դ������ı�Ƕ�ס����ǵ��øú��������ս��������ʾ��

![](./assets/47b096bb40ac998afc1ff60f688100e7aa7b1a41.png)

���ڣ�������������������Ȼ��������Ҫʱ�ҵ�`i`����Ϊ��λ�ڻ��������м䡣���ڳ������и�`makeCounter()`ʾ���������ˣ�

���������ǳ������ú���ϴ��롣Lox ����ǰ�Ļ�������������ǿ��öࡣ���ǣ������Ǽ��������հ��Ĺ����У��Ѿ���һ��㶯̬������й©���������С���[��һ��](http://craftinginterpreters.com/resolving-and-binding.html)�У����������̽���ʷ���Χ������©����

---

## [��ս](http://craftinginterpreters.com/functions.html#challenges)

1. ���ǵĽ�������ϸ��鴫�ݸ������Ĳ��������Ƿ����������Ĳ���������ƥ�䡣���ڴ˼������ÿ�ε���ʱ������ʱ��ɵģ���˻�������ܳɱ���Smalltalk ʵ��û��������⡣Ϊʲô����

2. Lox �ĺ��������﷨ִ�����������Ĳ�����������һ������������󶨵�һ�����ơ����������ȷʵϣ���������뺯��������ĳ�������Ŀ����ԡ������ں���ʽ�����У���ͨ��ϣ������һ���������������䴫�ݸ����������򷵻���������������£�������Ҫ���ơ�
   
   ��������ʽ��������ͨ��֧��**��������**��**lambda**����һ�ִ���������������󶨵����Ƶı��ʽ�﷨�������������﷨��ӵ� Lox �Ա��������ã�
   
   ```c
   fun thrice(fn) {
     for (var i = 1; i <= 3; i = i + 1) {
       fn(i);
     }
   }
   
   thrice(fun (a) {
     print a;
   });
   // "1".
   // "2".
   // "3".
   ```
   
   ����δ�����ʽ����г��ֵ������������ʽ�ļ��������
   
   ```c
   fun () {};
   ```

3. ���������Ч��
   
   ```c
   fun scope(a) {
     var a = "local";
   }
   ```
   
   ���仰˵�������Ĳ�����������ֲ�����*��ͬ*�ķ�Χ�ڣ��������ⲿ��Χ�ڣ����˹����ʲô�ģ�����Ϥ�����������أ�����Ϊ����*Ӧ*����ʲô��