# �������ʽ

> �����ҵĴ����ߣ�������������ˣ����أ�
> 
> --Mary Shelley,*Frankenstein*

�������Ϊ��һ���趨�ʵ��Ļ�������������һ�����꣬һ��ϲ���ڹ��¸߳�ʱ��������Ҷ�������з籩��Ҳ���Ӽ������硣�ڱ����У����ǵĽ�����������һ�����������۾�����ִ��һЩ���롣

![](./assets/f693555e20dd988f18e05ce74a34023f37b8e376.png)

> �ƾɵ�ά������ʽ��լ�ǿ�ѡ�ģ��������˷�Χ��

����ʵ�ֿ���ͨ�����ַ�ʽʹ�����ִ���û�Դ�����е�������ǿ��Խ������Ϊ�������룬���䷭�����һ�ָ߼����ԣ������Ϊĳ���ֽ����ʽ�Թ���������С��������������ǵĵ�һ�������������ǽ�������򵥡���̵�·����ִ���﷨������

���ڣ����ǵĽ�����ֻ֧�ֱ��ʽ����ˣ�Ϊ�ˡ�ִ�С����룬���ǽ�����һ�����ʽ������һ��ֵ���������ǿ��Խ�����ÿ�ֱ��ʽ�﷨������������������ȡ���������Ҫһ����Ӧ�Ĵ������֪���������������������������������������⣺

1. ���ǲ���ʲô����ֵ��

2. ���������֯��Щ����飿

�±����ν������...

## 7.1 ֵ�ı�ʾ

�� Lox �У�ֵ���������������ɱ��ʽ���㣬���洢�ڱ����С��û�����Щ��Ϊ*Lox*���󣬵������������ǵĽ�������д�ĵײ�����ʵ�ֵġ�����ζ���� Lox �Ķ�̬���ͺ� Java �ľ�̬����֮�����������Lox�е�һ���������Դ洢���⣨Lox�����͵�ֵ���������Դ洢��ͬʱ���Ĳ�ͬ���͵�ֵ�����ǿ���ʹ��ʲô Java ��������ʾ����

> ������Ҽ������Ի���ʹ�á�ֵ���͡����󡱡�
> 
> �Ժ��� C �������У����ǽ������ǽ���ϸ΢���֣�������Ҫ�����ʵ�ֵ�������ͬ����ʹ�ö��ص�������͵�������ѷ������ݡ����û��ĽǶ���������Щ������ͬ��ʡ�

����һ�����иþ�̬���͵� Java ���������ǻ������ܹ�ȷ����������ʱ�����������͵�ֵ����������ִ��һ��`+`�����ʱ������Ҫ�ж�����������������ֻ������������ַ������Ƿ���һ�� Java ���Ϳ����������֡��ַ���������ֵ�ȣ���û��һ�����Ը���������������ʱ������ʲô���У����� java.lang.Object��

�ڽ���������Ҫ�洢 Lox ֵ�ĵط������ǿ���ʹ�� Object ��Ϊ���͡�Java ����ԭʼ���͵İ�װ���ͣ����Ƕ��� Object �����࣬������ǿ��Խ��������� Lox ���������ͣ�

| Lox type      | Java representation |
| ------------- | ------------------- |
| Any Lox value | Object              |
| `nil`         | `null`              |
| Boolean       | Boolean             |
| number        | Double              |
| string        | String              |

����һ����̬���� Object ��ֵ�����ǿ���ʹ�� Java ������`instanceof`�����ȷ������ʱֵ�����ֻ����ַ����������κ����ݡ����仰˵��JVM�Լ��Ķ����ʾ�����Ϊ�����ṩ��ʵ�� Lox �������������һ�С��Ժ�������� Lox �ĺ��������ʵ���ĸ���ʱ�����ǽ����ò�������Ĺ��������� Object ��װ���ԭʼ��������������������Ҫ�����͡�

> ������Ҫ��ֵ������һ�����ǹ������ǵ��ڴ棬Java Ҳ������������Ķ����ʾ�ͷǳ��õ������ռ����������� Java ��д��һ������������Ҫԭ��

## 7.2 �������ʽ

��������������Ҫһ�Ѵ�����Ϊ������ÿ�ֱ��ʽʵ����ֵ�߼������Խ��ô���������`interpret()`�����ķ�ʽ��䵽�﷨�����С�ʵ���ϣ����ǿ��Ը���ÿ���﷨���ڵ㣬���������Լ�������������˰��[���������ģʽ](https://en.wikipedia.org/wiki/Interpreter_pattern)������һ������ģʽ����������֮ǰ�ᵽ�ģ�������ǽ������߼����������У������û��ҡ�

�෴�����ǽ��������е�[Visitor ģʽ](http://craftinginterpreters.com/representing-code.html#the-visitor-pattern)������һ���У����Ǵ�����һ�� AstPrinter �ࡣ�������﷨�����ݹ������������һ�����շ��ص��ַ������⼸�����������Ľ����������ģ����������������ַ��������Ǽ���ֵ��

���Ǵ�һ����class��ʼ��

```java
package com.craftinginterpreters.lox;

class Interpreter implements Expr.Visitor<Object> {
}
// lox/Interpreter.java, create new file
```

������������һ�������ߡ����ʷ����ķ������ͽ��� Object���������������� Java ���������� Lox ֵ�ĸ��ࡣΪ��ʵ�� Visitor �ӿڣ�������ҪΪ���������ɵ��ĸ����ʽ�����е�ÿһ��������ʷ�����������򵥵Ŀ�ʼ...

### 7.2.1 ��������ֵ

���ʽ����Ҷ�ӡ��������������ʽ��ɵ��﷨��ԭ�ӡ���������ֵ������ֵ�����Ѿ���ֵ�����������Ҫ������ֵ�ǲ���ֵ��һЩ�﷨������ֵ���ǳ������û�Դ�����ĳ�������ֵ���ɼ�������ģ����������ڴ��뱾����κεط�����Щ��������ֵ������ֵ���Խ���������ֵ��һ�����������������ʱ�����һ���֡�

> ��[��һ��](http://craftinginterpreters.com/statements-and-state.html)�У�������ʵ�ֱ���ʱ������ӱ�ʶ�����ʽ����Ҳ��Ҷ�ڵ㡣

��ˣ����������ڽ������н�����ֵ*token*ת��Ϊ����ֵ*�﷨���ڵ�*һ�� ���������ǽ�����ֵ���ڵ�ת��Ϊ����ʱֵ����ʵ֤����ܼ򵥡�

```java
  @Override
  public Object visitLiteralExpr(Expr.Literal expr) {
    return expr.value;
  }
// lox/Interpreter.java, in class Interpreter  
```

������ɨ������м��еز���������ʱֵ��������䵽token�С���������ȡ��ֵ�������������ֵ���ڵ��У����Ҫ��������ֵ������ֻ�轫��ֱ�ӷ��ؼ��ɡ�

### 7.2.2 ��������

��һ��Ҫ��������򵥵Ľڵ��Ƿ��顪�������ڱ��ʽ��ʹ����ʽ���Ŷ��õ��Ľڵ㡣

```java
  @Override
  public Object visitGroupingExpr(Expr.Grouping expr) {
    return evaluate(expr.expression);
  }
// lox/Interpreter.java, in class Interpreter  
```

����ڵ���ж԰����������ڵı��ʽ���ڲ��ڵ�����á�Ϊ������������ʽ�������ǵݹ���������ӱ��ʽ�����䷵�ء�

������������������������򵥵ؽ����ʽ���ͻؽ������ķ�����ʵ�֣�

һЩ��������Ϊ���Ŷ������ڵ㡣�෴���ڽ��������ŵı��ʽʱ������ֻ�Ƿ����ڲ����ʽ�Ľڵ㡣����ȷʵ�� Lox ��Ϊ���Ŵ�����һ���ڵ㣬��Ϊ�Ժ�������Ҫ������ȷ����ֵ���ʽ����ࡣ

```java
  private Object evaluate(Expr expr) {
    return expr.accept(this);
  }
// lox/Interpreter.java, in class Interpreter 
```

### 7.2.3 ����һԪ���ʽ

�����һ����һԪ���ʽ��һ�����Ǳ���������ֵ���ӱ��ʽ����֮ͬ�����ڣ�һԪ���ʽ�������֮����һЩ������

```java
  @Override
  public Object visitUnaryExpr(Expr.Unary expr) {
    Object right = evaluate(expr.right);

    switch (expr.operator.type) {
      case MINUS:
        return -(double)right;
    }

    // Unreachable.
    return null;
  }
// lox/Interpreter.java, add after visitLiteralExpr()  
```

���ȣ������������������ʽ��Ȼ�����ǽ�һԪ���������Ӧ���������������ֲ�ͬ��һԪ���ʽ���������token�����ͱ�ʶ��

������ʾ����`-`�������ӱ��ʽ�Ľ�����ӱ��ʽ���������֡������� Java ������*��̬*�ز�֪����һ�㣬������ִ�в���֮ǰ�������ת������������ת������������ʱ`-`���� ʱ������ʹ���Զ�̬����ĺ��ġ�

> ��������֪�����ת��ʧ�ܻᷢ��ʲô�����ģ����Ǻܿ�ͻ�̸����һ�㡣

�����Կ�ʼ�˽���ֵ��εݹ�ر�������������������������ӱ��ʽ֮ǰ�������޷�����һԪ�������������ζ�����ǵĽ��������ڽ���**�������**����ÿ���ڵ���ִ���Լ��Ĺ���֮ǰ�������ӽڵ㡣

��һ��һԪ��������߼��ǡ�

```java
   switch (expr.operator.type) {
      case BANG:
        return !isTruthy(right);
      case MINUS:
// lox/Interpreter.java, in visitUnaryExpr()
```

ʵ�ֺܼ򵥣����������truthy���Ķ�����ʲô��������Ҫ��΢˳��̸̸������ѧ��һ���ش����⣺ʲô����ֵ?

### 7.2.4  ��ֵ���ֵ

�ðɣ�Ҳ�����ǲ������������ձ����⣬�������� Lox �������������Ҫ�����᷵��ʲô��������ʹ�ó�`true`��`false`�߼�����֮��Ķ��������߼�����`!`���κ������ڴ��ǲ���ֵ�ĵط���

����*����*˵����һ��������Ϊ���ǲ�ʹ����ʽת�������������̬�������Բ�������ô����ɮ���෴�����ǽ��������͵�ֵ�������Ϊ���飬����һ�鶨��Ϊ���桱����ʵ��������ϲ���ģ�����ֵ��������Ϊ���١� ������١������ֻ�����Щ���⣬������һЩ�����б�ú���֡�

> �� JavaScript �У��ַ�������ʵ�ģ������ַ������ǡ���������ʵ�ģ�������������ʵ��?��.?.?Ҳ����ʵ������`0`����ٵģ���*�ַ���*?`"0"`����ʵ�ġ�
> 
> �� Python �У����ַ����� JS һ���Ǽٵģ�������������Ҳ�Ǽٵġ�
> 
> �� PHP �У�����`0`���ַ���`"0"`���Ǽٵġ�����������ǿ��ַ���������ʵ�ġ�
> 
> ��������

Lox ��ѭ Ruby �ļ򵥹���`false`����`nil`�Ǽٵģ�����һ�ж�����ġ���������ʵ�֣�

```java
 private boolean isTruthy(Object object) {
    if (object == null) return false;
    if (object instanceof Boolean) return (boolean)object;
    return true;
  }
// lox/Interpreter.java, add after visitUnaryExpr()
```

### 7.2.5 ������Ԫ�����

�������һ�����ʽ���࣬��Ԫ�������������һЩ�����ǽ���������ʼ��

```java
@Override
  public Object visitBinaryExpr(Expr.Binary expr) {
    Object left = evaluate(expr.left);
    Object right = evaluate(expr.right); 

    switch (expr.operator.type) {
      case MINUS:
        return (double)left - (double)right;
      case SLASH:
        return (double)left / (double)right;
      case STAR:
        return (double)left * (double)right;
    }

    // Unreachable.
    return null;
  }
// lox/Interpreter.java, add after evaluate()  
```

> ���Ƿ�ע�⵽����������̶������������һ��΢����䣿�ڶ����Ʊ��ʽ�У����ǰ������ҵ�˳�����������������Щ�������и����ã����ѡ�����û��ɼ��ģ�����ⲻ������һ��ʵ��ϸ�ڡ�
> 
> �������ϣ�����ǵ���������������һ�£���ʾ����������������������Ҫȷ�� clox ��ͬ�������顣

���������Ū������﷢����ʲô����һԪ�����������Ҫ������������������Ҫ����Ĳ�������

��ʡ����һ���������������Ϊ���е����⡣

```java
    switch (expr.operator.type) {
      case MINUS:
        return (double)left - (double)right;
      case PLUS:
        if (left instanceof Double && right instanceof Double) {
          return (double)left + (double)right;
        } 

        if (left instanceof String && right instanceof String) {
          return (String)left + (String)right;
        }

        break;
      case SLASH:
// lox/Interpreter.java, in visitBinaryExpr()
```

��`+`�����Ҳ���������������ַ�����Ϊ�˴���������⣬���ǲ�ֻ�Ǽ����������ĳ�����Ͳ�*ǿ��ת��*���ǣ����Ƕ�̬��*���*���Ͳ�ѡ���ʵ��Ĳ����������Ϊʲô������Ҫ���ǵĶ����ʾ��֧��`instanceof`��

> ���ǿ���ר��Ϊ�ַ������Ӷ���һ�������������� Perl (`.`)��Lua (`..`)��Smalltalk (`,`)��Haskell (`++`) ���������������ġ�
> 
> ����Ϊʹ���� Java��JavaScript��Python ������������ͬ���﷨��ʹ Lox ������ʹ�á�����ζ��`+`�������**����**��֧��������ֺ������ַ�������ʹ�ڲ���`+`���ַ����������У�������Ȼ��������������������͸�������

�������ǱȽ��������

```java
    switch (expr.operator.type) {
      case GREATER:
        return (double)left > (double)right;
      case GREATER_EQUAL:
        return (double)left >= (double)right;
      case LESS:
        return (double)left < (double)right;
      case LESS_EQUAL:
        return (double)left <= (double)right;
      case MINUS:
// lox/Interpreter.java, in visitBinaryExpr()
```

����������������ͬ��Ψһ�����������������������ֵ������������ֻ��ַ�������������ͬ�����Ƚ���������ǲ�������ֵ��

���һ�����������ȵġ�

```java
      case BANG_EQUAL: return !isEqual(left, right);
      case EQUAL_EQUAL: return isEqual(left, right);
// lox/Interpreter.java, in visitBinaryExpr()
```

����Ҫ���ֵıȽ��������ͬ����������֧���κ����͵Ĳ������������ǻ�ϲ��������㲻���� Lox 3 �Ƿ�*С��*��`"three"`?,������������Ƿ�*����*����

> ��͸���棺���ǡ�

����ֵ����һ��������߼���������һ�������ķ����С�

```java
private boolean isEqual(Object a, Object b) {
    if (a == null && b == null) return true;
    if (a == null) return false;

    return a.equals(b);
  }
// lox/Interpreter.java, add after isTruthy() 
```

������������� Java ��ʾ Lox �����ϸ�ڵĽ���֮һ��������Ҫ��ȷʵ��*Lox ��*����Ը��������� Java ��ͬ��

���˵��ǣ����߷ǳ����ơ�Lox ����������Ե���ʽת������ Java Ҳ����������ȷʵ�����ر���`nil`/`null`���Ա������ǳ�����`null`�ϵ���`equals()`ʱ�����׳� NullPointerException�����ã�������Ǻ��Ѻá�Java ��`equals()`Boolean��Double �� String ��������������Ҫ�� Lox ��Ϊ��

> ��ϣ��������ʲô��
> 
> ```java
> ( 0 / 0 ) == ( 0 / 0 )
> ```

> ����ָ��˫��������Ϊ��[IEEE 754](https://en.wikipedia.org/wiki/IEEE_754)����������õ������**NaN**���������֡���ֵ����ֵ��ǣ�NaN*��*��������

> �� Java �У�`==`ԭʼ˫���ϵ�����������˸���Ϊ����?`equals()`Double ���ϵķ���û�С�Lox ʹ�ú��ߣ���˲���ѭ IEEE����Щ΢��Ĳ�������ռ��������ʵ�������������˾�ɥ��һС���֡�

����Щ�������������ȷ������Ч Lox ���ʽ��������д��롣����*��Ч*���أ��ر��ǣ����ӱ��ʽ�ļ�����Ϊִ�в����Ĵ������͵Ķ���ʱ�ᷢ��ʲô��

## 7.3 ����ʱ����

ÿ��һ���ӱ��ʽ����һ�������������Ҫ������һ�����ֻ�һ���ַ���ʱ���Ҿͺ�����ظ���ǿ��ת������Щ�������ܻ�ʧ�ܡ���Ȼ�û��Ĵ����Ǵ���ģ����������Ҫ�������õ����ԣ�����Ҳ���������ŵش���ô���

> ���Ǹ����޷����򱨸����ʹ����������ָ��ת��Ϊĳ����ʵ��ָ������ݲ�ƥ������ͣ������ C �����ġ�C ͨ���������������������Ժ��ٶȣ���Ҳ�ǳ�������Σ�ա�һ����������ڴ��е�λ�����еĶ�ע������ա�
> 
> �������ִ����Խ��������Ĳ���ȫ�������෴�����������**�ڴ氲ȫ**�ģ���ͨ����̬������ʱ�������ȷ��������Զ�������ؽ��ʹ洢��һ���ڴ��е�ֵ��

��������������**����ʱ����**��ʱ���ˡ���ǰ����½��У�����̸�۴�����ʱ���˺ܶ�ī������Щ����*�﷨*��*��̬*����*��ִ���κ�*����֮ǰ��Ⲣ������Щ������ʱ��������������Ҫ�������ڳ�������ʱ���ͱ���Ĺ��ϣ���˵�������

���ڣ������������������ִ�еĲ��������Ǵ�������ͣ��� Java ת����ʧ�ܲ��� JVM ���׳� ClassCastException���⽫չ��������ջ���˳�Ӧ�ó������û��³� Java ��ջ���١�����ܲ���������Ҫ�ġ�Lox ���� Java ʵ�ֵ���һ��ʵӦ���Ƕ��û����ص�ϸ�ڡ��෴������ϣ�������˽ⷢ����*Lox*����ʱ���󣬲��������ṩ�����ǵ����Ժ����ǵĳ�����صĴ�����Ϣ��

������Java ��Ϊȷʵ�����ŵ㡣��������ʱ��������ȷ��ִֹͣ���κδ��롣�����û�������һЩ���ʽ�����磺

```java
2 * (3 / -"muffin")
```

������ ��muffin�����������Ҫ�ڸ��ڲ�`-`���ʽ�б�������ʱ�����ⷴ������ζ�������޷�����`/`���ʽ����Ϊ��û����������Ҳ�����������`*`Ҳһ��.���Ե�ĳ�����ʽ���������ʱ����ʱ��������Ҫһ·���ݳ�ȥ��

> �Ҳ�֪������ƣ���*��*�ܾ��ɱ���

![](./assets/d98df7197f0bd09d25e9bc8b8832048fb0d06b48.png)

���ǿ��Դ�ӡ����ʱ����Ȼ����ֹ���̲���ȫ�˳�Ӧ�ó��������һ������ھ��츳����������˷��½��ı�����Խ�������

����������ˣ������ǻ���Ӧ����һЩ����ô�����Ե����顣��Ȼ����ʱ������Ҫֹͣ����*���ʽ*��������Ӧ��ɱ��*������*������û��������� REPL ������һ�д�������ƴд����������ȻӦ���ܹ��������лỰ���ڴ�֮�����������롣

### 7.3.1 �������ʱ����

���ǵ�������������ʹ�õݹ鷽����������Ƕ�ױ��ʽ��������Ҫ������ЩǶ�׷������� Java ���׳��쳣��ʵ����һ��ĺ÷��������ǣ����ǽ�����һ���ض��� Lox �Ĵ��󣬶�����ʹ�� Java �Լ���ת�������Ա����ǿ��԰����Լ�����Ը��������

�����ǽ���ת��֮ǰ�������Լ�����������͡���ˣ�����һԪ`-`��������ӣ�

```java
      case MINUS:
        checkNumberOperand(expr.operator, right);
        return -(double)right;
// lox/Interpreter.java, in visitUnaryExpr()
```

���������Ĵ����ǣ�

```java
private void checkNumberOperand(Token operator, Object operand) {
    if (operand instanceof Double) return;
    throw new RuntimeError(operator, "Operand must be a number.");
  }
// lox/Interpreter.java, add after visitUnaryExpr()
```

�����ʧ��ʱ�������׳�����֮һ��

```java
package com.craftinginterpreters.lox;

class RuntimeError extends RuntimeException {
  final Token token;

  RuntimeError(Token token, String message) {
    super(message);
    this.token = token;
  }
}
// lox/RuntimeError.java, create new file
```

�� Java ת���쳣��ͬ�����ǵ������token����token��ʶ�û�����������ʱ�������Դ���뾲̬����һ�������������û�֪���������޸����ǵĴ��롣

> �ҳ��ϡ�RuntimeError�������������������Ϊ Java ������һ�� RuntimeException �ࡣ������������һ�����˵�������������־�����ʵ�������Ѿ����õ����ֳ�ͻ���ȵ�����֧�� Lox �ࡣ

������Ҫ�Զ�Ԫ������������Ƶļ�顣������������֤��ʵ�ֽ����������ÿһ�д��룬�����ҽ���һ������ǡ�

���ڣ�

```java
      case GREATER:
        checkNumberOperands(expr.operator, left, right);
        return (double)left > (double)right;
// lox/Interpreter.java, in visitBinaryExpr()
```

���ڻ���ڣ�

```java
      case GREATER_EQUAL:
        checkNumberOperands(expr.operator, left, right);
        return (double)left >= (double)right;
// lox/Interpreter.java, in visitBinaryExpr()
```

С�ڣ�

```java
      case LESS:
        checkNumberOperands(expr.operator, left, right);
        return (double)left < (double)right;
// lox/Interpreter.java, in visitBinaryExpr()
```

С�ڻ���ڣ�

```java
      case LESS_EQUAL:
        checkNumberOperands(expr.operator, left, right);
        return (double)left <= (double)right;
// lox/Interpreter.java, in visitBinaryExpr()
```

������

```java
      case MINUS:
        checkNumberOperands(expr.operator, left, right);
        return (double)left - (double)right;
// lox/Interpreter.java, in visitBinaryExpr()
```

������

```java
      case SLASH:
        checkNumberOperands(expr.operator, left, right);
        return (double)left / (double)right;
// lox/Interpreter.java, in visitBinaryExpr()
```

�˷���

```java
      case STAR:
        checkNumberOperands(expr.operator, left, right);
        return (double)left * (double)right;
// lox/Interpreter.java, in visitBinaryExpr()
```

������Щ�������������֤������ʵ������һԪ��֤����ͬ��

```java
  private void checkNumberOperands(Token operator,
                                   Object left, Object right) {
    if (left instanceof Double && right instanceof Double) return;

    throw new RuntimeError(operator, "Operands must be numbers.");
  }
// lox/Interpreter.java, add after checkNumberOperand()  
```

> ��һ��΢�������ѡ�������ڼ������һ��������֮ǰ����*����*����*��*������һ�£�������һ��������ӡ���Ĳ���Ȼ�󷵻�����ʹ����������д��`say()`

```java
say("left") - say("right");
```

> ���ǵĽ������ڱ�������ʱ����֮ǰ��ӡ��left���͡�right�������Ǳ�����ָ���������Ҳ�����֮ǰ�ȼ�����������

ʣ�µ����һ���������Ҳ���������������Ǽӷ�������`+`�����ֺ��ַ������������أ�������Ѿ����м�����͵Ĵ��롣��������ɹ���������ƥ�䣬������Ҫ���ľ���ʧ�ܡ�

```java
          return (String)left + (String)right;
        }

        throw new RuntimeError(expr.operator,
            "Operands must be two numbers or two strings.");
      case SLASH:
// lox/Interpreter.java, in visitBinaryExpr(), replace 1 line
```

���������ܹ���⵽�������ڲ��������ʱ���󡣴������ڱ��׳�����һ���Ǳ�д�������ǵĴ��롣Ϊ�ˣ�������Ҫ�� Interpreter �����ӵ����������� Lox ���С�

## 7.4 ��ͨ������

visit ������ Interpreter ��ĺ��ģ������Ĺ������������������Ҫ��������Χ����һ��Ƥ�������������ಿ�ֽ��н�����Interpreter �Ĺ��� API ֻ��һ�ַ�����

```java
  void interpret(Expr expression) { 
    try {
      Object value = evaluate(expression);
      System.out.println(stringify(value));
    } catch (RuntimeError error) {
      Lox.runtimeError(error);
    }
  }
// lox/Interpreter.java, in class Interpreter  
```

����ܱ��ʽ���﷨����������ֵ������ɹ���`evaluate()`�򷵻ؽ��ֵ�Ķ���`interpret()`����ת��Ϊ�ַ�����������ʾ���û���Ҫ�� Lox ֵת��Ϊ�ַ��������������ڣ�

```java
  private String stringify(Object object) {
    if (object == null) return "nil";

    if (object instanceof Double) {
      String text = object.toString();
      if (text.endsWith(".0")) {
        text = text.substring(0, text.length() - 2);
      }
      return text;
    }

    return object.toString();
  }
// lox/Interpreter.java, add after isEqual()  
```

������һ������`isTruthy()`�Ĵ��룬����Խ Lox ������û���ͼ�������� Java �е��ڲ���ʾ֮��ĸ��ҡ�

��ܼ򵥡����� Lox �����Ϊ��Ϥ Java ���ˣ���˲���ֵ֮��Ķ��������������п�������һ���ġ����ֱ�Ե�����`nil`������ʹ�� Java ��ʾ`null`�������֡�

Lox ����������ֵҲʹ��˫������������������£�����Ӧ�ò���С�����ӡ������ Java ͬʱ���и��������������ͣ���ϣ����֪����ʹ�õ�����һ�֡���ͨ������ʽ���`.0`������ֵ˫���������������ǲ�����������������ǰ��������ˡ�

> ��һ�Σ��������������������ֱ�Ե�������ȷ�� jlox �� clox ������ͬ���������������Ե���ֽ�������㷢�裬�����ǹ�������Ҫ��ɲ��֡�
> 
> �û�����������������Щϸ�ڣ����ʵ�ֲ�һ�£����ǵĳ����ڲ�ͬ�Ľ�����������ʱ�ͻ������

### 7.4.1 ��������ʱ����

������������ʽʱ�׳�����ʱ������`interpret()`���������������ǿ������û��������Ȼ�����ŵؼ������������е����д��󱨸���붼λ�� Lox ���У��������Ҳ���˷����������

```java
  static void runtimeError(RuntimeError error) {
    System.err.println(error.getMessage() +
        "\n[line " + error.token.line + "]");
    hadRuntimeError = true;
  }
// lox/Lox.java, add after error() 
```

����ʹ���� RuntimeError �����ı���������û���������ʱ����ִ����һ�д��롣���õİ취��Ϊ�û��ṩ�����ĵ��ö�ջ������ʾ�������*ִ��*�ô��롣�������ǻ�û�к������ã������������ǲ��ص��ġ�

��ʾ�����`runtimeError()`���ô��ֶΣ�

```java
  static boolean hadError = false;
  static boolean hadRuntimeError = false;

  public static void main(String[] args) throws IOException {
// lox/Lox.java, in class Lox  
```

���ֶκ�С�����ź���Ҫ�����á�

```java
    run(new String(bytes, Charset.defaultCharset()));

    // Indicate an error in the exit code.
    if (hadError) System.exit(65);
    if (hadRuntimeError) System.exit(70);
  }
// lox/Lox.java, in runFile()
```

����û����ڴ��ļ����� Lox �ű����ҷ�������ʱ�������ǻ��ڽ����˳�ʱ�����˳������Ը�֪���ý��̡�����ÿ���˶�����shell���򣬵����ǹ��ġ�

> ����û��������� REPL�����ǲ����ĸ�������ʱ���������Ǳ���������Ǽ򵥵�ѭ���������������´��벢�������С�

### 7.4.2 ���н�����

�����������˽�������Lox ��Ϳ��Կ�ʼʹ�����ˡ�

```java
public class Lox {
  private static final Interpreter interpreter = new Interpreter();
  static boolean hadError = false;
// lox/Lox.java, in class Lox
```

���ǽ��ֶ���Ϊ��̬���Ա��`run()`REPL �Ự�ڲ���������������ͬһ����������������û��ʲô���𣬵����Ժ󵱽������洢ȫ�ֱ���ʱ����������ͬ����Щ����Ӧ�������� REPL �Ự�г������ڡ�

�������ɾ����[��һ��](http://craftinginterpreters.com/parsing-expressions.html)�����ڴ�ӡ�﷨������ʱ�����У��������滻Ϊ��

```java
    // Stop if there was a syntax error.
    if (hadError) return;

    interpreter.interpret(expression);
  }
// lox/Lox.java, in run(), replace 1 line
```

����������һ�����������Թܵ���ɨ�衢������ִ�С���ϲ��������ӵ�����Լ���������������

�����������������ǳ��򵥡��������ǽ������õ� Interpreter ��� Visitor ģʽ�����˹Ǽܣ�������½ڽ�������Ȥ�����ݡ��������������ȡ����ڣ���������û����̫�����飬��������������

![](./assets/f11df8c9bff3af55c57c8476211a1024cc4f93ad.png)

---

## [��ս](http://craftinginterpreters.com/evaluating-expressions.html#challenges)

1. �����������������ͽ��бȽϿ��ܺ����á���������ܶ��ַ����к���Ľ��͡���ʹ�ǻ������֮��ıȽϣ�`3 < "pancake"`Ҳ���Ժܷ���������칹���͵����򼯺�֮��Ķ���������������ֻ�ᵼ�´���ͻ��ҡ�
   
   �����չ Lox ��֧�ֱȽ������������������������������Щ���Ͷ��Լ���ζ������ǵ�˳��֤�����ѡ���Ǻ���ģ������������������Խ��бȽϡ�

2. ������Զ���������`+`�����*����һ��*���������ַ���������һ��������ת��Ϊ�ַ�����Ȼ�󽫽���������������磬`"scone" + 4`�����`scone4`.��չ����ʹ`visitBinaryExpr()`֧������

3. ����㽫һ���������㣬���ڻᷢ��ʲô������ΪӦ�÷���ʲô��֤�����ѡ����֪��������������δ���������Լ�����Ϊʲô������������ѡ��
   
   �����е�ʵ��`visitBinaryExpr()`�Լ�Ⲣ����˰���������ʱ����

## [���˵������̬�Ͷ�̬����](http://craftinginterpreters.com/evaluating-expressions.html#design-note)

ĳЩ���ԣ��� Java���Ǿ�̬���͵ģ�����ζ���������κδ���֮ǰ�ı���ʱ���Ⲣ�������ʹ��������ģ��� Lox���Ƕ�̬���͵ģ����ҽ����ʹ������Ƴٵ�����ʱ���Բ���֮ǰ��������������Ϊ����һ���Ǻڼ��׵�ѡ�񣬵�ʵ��������֮���������ͳһ�塣

��ʵ֤������ʹ�Ǵ������̬��������Ҳ��������ʱ����*һЩ*���ͼ�顣����ϵͳ��̬�ؼ���������͹��򣬵������ɵĴ�����Ϊ����������������ʱ��顣

���磬�� Java �У�*��̬*����ϵͳ�ٶ�ǿ��ת�����ʽ���ǰ�ȫ�سɹ�����ת��һЩֵ�������Խ��侲̬����ΪĿ�����ͣ�����������κα�����󡣵������ԣ���ɥ���ܻ�ʧ�ܡ���̬��������Լٶ�ת�����ǳɹ�����Υ�����ԵĽ�ȫ�Ա�֤��Ψһԭ������Ϊת��*������ʱ*����鲢��ʧ��ʱ�׳��쳣��

һ����΢���������Java �� C# �е�[Э����](https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)#Covariant_arrays_in_Java_and_C.23)�顣����ľ�̬�����͹�����������Ĳ��������ǣ�

```java
Object[] stuff = new Integer[1];
stuff[0] = "not an int!";
```

��δ������û���κδ��󡣵�һ������ת�� Integer ���鲢����洢�� Object �������͵ı����С��ڶ���������һ����Ԫ���д洢һ���ַ����������������;�̬���������ַ���*��*���󡪡���������ʱ���õ�ʵ����������`stuff`��Ӧ�ð����ַ�����Ϊ�˱����������ѣ�������ֵ�洢��������ʱ��JVM ��ִ��*����ʱ*�����ȷ��������������͡����������׳� ArrayStoreException��

Java ����ͨ���ڵ�һ�н�ֹת��������������ʱ�������������ʹ����*����*��ʹ����������*����*�������顣���ھ�̬���Ǻ���ģ�������ֹ���������ж�ȡ�ĳ����Ұ�ȫ�Ĵ���ģʽ��������Ӳ�*д��*���飬��Э�����ǰ�ȫ�ġ��� Java 1.0 ֧�ַ���֮ǰ����Щģʽ���ڿ������ر���Ҫ��James Gosling ������ Java ���ʦ��һЩ��̬��ȫ�Ժ������Ͻ�����Ȩ�⡪����Щ����洢�����Ҫʱ�䡪���Ի�ȡһЩ����ԡ�

*�������ִ���̬�������Բ�����ĳ��*��������Ȩ��?������ Haskell Ҳ���������д��з��ƥ��Ĵ��롣����������Լ������һ�־�̬�������ԣ����ס������ʱ����ͨ����ĳЩ���ͼ���Ƴٵ�����ʱ��Ϊ�û��ṩ���������ԣ�����������*̫�ྲ̬��ȫ�ĺô���*

��һ���棬�û�ѡ��̬�������Ե�һ���ؼ�ԭ������Ϊ�����������������ţ������ǵĳ�������ʱ��ĳЩ���͵Ĵ���*��Զ���ᷢ����*��̫������ͼ���Ƴٵ�����ʱ����������������ġ�