# ɨ��

> ��ҧһ�ڡ��κ�ֵ���������鶼ֵ�ù���Ŭ����
> 
> -- Robert A. Heinlein,*Time Enough for Love*

�κα�������������ĵ�һ������ɨ�衣ɨ������ԭʼԴ������Ϊһϵ���ַ������������Ϊһϵ�г�Ϊ**tokens**�Ŀ顣��Щ�ǹ��������﷨��������ġ����ʡ��͡������š���

> ���������������񱻳�Ϊ��ɨ��(scanning)���͡��ִ�(lexing)�������ʷ�����������д�����ص���ȥ����������� Winnebagos һ�����ڴ���ֱ���ʱ����Щ��ʹ�á�ɨ����(scanner)��ֻ��ָ����Ӵ��̶�ȡԭʼԴ�����ַ������仺�����ڴ��еĴ���Ρ�Ȼ��lexing���Ƕ��ַ������õ��������һ���׶Ρ�
> 
> ��񣬽�Դ�ļ������ڴ���΢������ģ������(scanner)���ٳ�Ϊ�������е�һ�������׶Ρ���ˣ���������������Ͽ��Ի�����

ɨ���������˵Ҳ��һ���ܺõ���㣬��Ϊ���벻�Ǻ��ѣ������ṹ��һ���Ӵ��`switch`��䡣�������Ժ���һЩ����Ȥ�Ĳ���֮ǰ����������������������½���ʱ�����ǽ�ӵ��һ��������ȫ�����ٵ�ɨ����(scanner)������ȡ���� Lox Դ�����ַ��������ɽ�����һ���������������tokens��

## 4.1 ���������

�������ǵ�һ���������½ڣ��ڿ�ʼʵ��ɨ��һЩ����֮ǰ��������Ҫ���ճ����ǵĽ����� jlox �Ļ�����ܡ�һ�ж��� Java �е��࿪ʼ��

```java
package com.craftinginterpreters.lox;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.charset.Charset;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

public class Lox {
  public static void main(String[] args) throws IOException {
    if (args.length > 1) {
      System.out.println("Usage: jlox [script]");
      System.exit(64); 
    } else if (args.length == 1) {
      runFile(args[0]);
    } else {
      runPrompt();
    }
  }
}

// lox/Lox.java, create new file
```

> �����˳����룬��ʹ���� UNIX [��sysexits.h��](https://www.freebsd.org/cgi/man.cgi?query=sysexits&apropos=0&sektion=0&manpath=FreeBSD+4.3-RELEASE&format=html)ͷ�ļ��ж����Լ�������������ҵ�����ӽ���׼�Ķ�����

����ճ�����ı��ļ��У�Ȼ�����ú����� IDE �� Makefile �ļ�������׼����ʱ���һ���������𣿺õģ�

Lox ��һ�ֽű����ԣ�����ζ����ֱ�Ӵ�Դ����ִ�С����ǵĽ�����֧���������д���ķ�ʽ������������������ jlox ������һ���ļ�·���������ȡ�ļ���ִ������

```java
private static void runFile(String path) throws IOException {
   byte[] bytes = Files.readAllBytes(Paths.get(path));
   run(new String(bytes, Charset.defaultCharset()));
}

// lox/Lox.java, add after main()
```

���������������и����ܵĽ����Ի����������Խ�����ʽ����������û���κβ�������������� jlox����������һ��������ʾ���У�����������һ������һ�д��벢ִ�С�

> ����ʽ��ʾҲ��Ϊ��REPL����������rebel�������С�p����������������� Lisp������ʵ��һ������Χ��һЩ���ú�����װһ��ѭ��һ���򵥣�
> 
> (print (eval (read)))
> 
> �����ڴ�Ƕ�׵ĵ������⹤����**R**eadһ�����룬**E**valuate����**P**rint�����Ȼ��**L**oop����һ�顣

```java
  private static void runPrompt() throws IOException {
    InputStreamReader input = new InputStreamReader(System.in);
    BufferedReader reader = new BufferedReader(input);

    for (;;) { 
      System.out.print("> ");
      String line = reader.readLine();
      if (line == null) break;
      run(line);
    }
  }
// lox/Lox.java, add after runFile()
```

`readLine()`����˼�壬�ú����������ж�ȡ�û���һ��������Ϊ������ء�Ҫ��ֹ����ʽ������Ӧ�ó���ͨ������ Control-D��������������򷢳���end-of-file���źš��������������ʱ`readLine()`����`null`��������������Ƿ��˳�ѭ����

��ʾ�����ļ�����������������Ĺ��ܵļ򵥷�װ��

```java
private static void run(String source) {
    Scanner scanner = new Scanner(source);
    List<Token> tokens = scanner.scanTokens();

    // For now, just print the tokens.
    for (Token token : tokens) {
      System.out.println(token);
    }
  }
// lox/Lox.java, add after runPrompt() 
```

�����Ƿǳ����ã���Ϊ��û�б�д��������������֪�������ڣ������ӡ����������д��ɨ����(scanner)������tokens���Ա���Բ鿴�����Ƿ�ȡ���˽�չ��

### 4.1.1 ������

�����ǽ�������ʱ��������ʩ����һ���ؼ�������*������*���̿�����ʱ��������һ�㣬��Ϊ��������һ��ʵ�����⣬��������ʽ�ļ������ѧ���⡣���ǣ���������Ĵ���һ��ʵ��*����*�����ԣ���ô���ŵش��������������Ҫ�ġ�

�����ṩ�����ڴ������Ĺ��߹��������û�����ĺܴ�һ���֡����û��Ĵ�����������ʱ�����Ǹ������ῼ�����ǵ����ԣ����ǵ�������ȫ�ǹ���*���ǵĳ���*��ͨ��ֻ�е���������ʱ�����ǲŻ�ע�⵽���ǵĴ�����

�������������ʱ���������������û��ṩ������Ҫ��������Ϣ�����˽����ʲô���⣬���º͵��������ǻص�������ͼȥ�ĵط��������������ζ�Ŵ����ڿ�ʼ���ڽ�����������ʵ�ֹ����п��Ǵ�����

> ˵����ô�࣬����*���*�����������ǽ��������Ƿǳ��򵥵Ŀ�ܡ��Һ���̸�۽���ʽ����������̬��������������Ȥ�Ķ��������Ǳ���ֻ����ô��īˮ��

```java
  static void error(int line, String message) {
    report(line, "", message);
  }

  private static void report(int line, String where,
                             String message) {
    System.err.println(
        "[line " + line + "] Error" + where + ": " + message);
    hadError = true;
  }
// lox/Lox.java, add after run()
```

���`error()`������`report()`�������������û���ȷ�������Ϸ�����һЩ�﷨������ȷʵ���ܹ�����������*��*���󱨸������޶ȡ�����һ�£�����㲻С����ĳ������������������һ�����յĶ��ţ����ҽ�������ӡ������

```java
Error: Unexpected "," somewhere in your code. Good luck finding it!
```

�ǲ��Ǻ��а���������������Ҫ������ָ����ȷ���кš����õ��ǿ�ʼ�ͽ����У������û���֪��*��*�е�λ�á�*����*���õ������û�*��ʾ*Υ���У����磺

```java
Error: Unexpected "," in argument list.

    15 | function(first, second,);
                               ^-- Here.
```

�Һ����ڱ�����ʵ�����ƵĶ���������ʵ˵���кܶ����ŵ��ַ����������롣���û��ǳ����ã������������Ǻ���Ȥ��������Ҳ���Ǻ���Ȥ����������ֻʹ���кš������Լ��Ľ������У�������˵��������������������������

�������� Lox ���б����˴��󱨸湦�ܵ���Ҫԭ������Ϊ��`hadError`�ֶ������ﶨ�壺

```java
public class Lox {
  static boolean hadError = false;
// lox/Lox.java, in class Lox
```

���ǽ�ʹ������ȷ�����ǲ��᳢��ִ�о�����֪����Ĵ��롣���⣬�������������Է�������˳�������һ���õ������й���Ӧ������������

```java
 run(new String(bytes, Charset.defaultCharset()));

    // Indicate an error in the exit code.
    if (hadError) System.exit(65);
  }
// lox/Lox.java, in runFile()
```

������Ҫ�ڽ���ʽѭ�������ô˱�־������û����˴�������Ӧ����ֹ���ǵ������Ự��

```java
   run(line);
      hadError = false;
    }
// lox/Lox.java, in runPrompt()
```

���ڴ˴���ȡ���󱨸�����ǽ�������ɨ�������������ܷ�������Ľ׶ε���һ��ԭ��������������*����*����Ĵ�����*����*����Ĵ���ֿ���һ�ֺܺõĹ���ʵ����

ǰ�˵ĸ����׶λ�����󣬵�֪����ν�������ָ��û����������ǵĹ������ڹ�����ȫ������ʵ���У������ܻ�ͨ�����ַ�ʽ��ʾ������ stderr �ϡ��� IDE �Ĵ��󴰿��С���¼���ļ��еȡ�����ϣ��������ɨ�����ͽ������е������ǡ�

��������£����ǻ���һ��ʵ�ʵĳ���ĳ�ִ��ݸ�ɨ�����ͽ������ġ�ErrorReporter���ӿڣ��������ǾͿ����滻��ͬ�ı�����ԡ���������ļ򵥽���������û�����������������ٽ����󱨸�����Ƶ�����һ�����С�

> ���ҵ�һ��ʵ��jlox ʱ���Ҿ��������������հ���ȥ���ˣ���Ϊ���о����ڱ����е���С��������˵�ǹ�����Ƶġ�

����һЩ�����Ĵ��������ǵ�Ӧ�ó�����Ǿ�׼�����ˡ�һ������һ������`scanTokens()`������ Scanner �࣬�Ϳ��Կ�ʼ�������ˡ��ڿ�ʼ֮ǰ���ø���һ�����˽�tokens��ʲô��

## 4.2  Lexemes ��Tokens

����һ�� Lox ���룺

```c
var language = "lox";
```

�����`var`�����������Ĺؼ��֡������ַ����С�v-a-r����ȷ�������塣����������Ǵ�`language` ���м���������ĸ�����硰gua�������Ǳ����û���κ����塣

����Ǵʷ��������������ڡ����ǵĹ�����ɨ���ַ��б���������ϳ���Ȼ����ĳЩ��������С���С���Щ�ַ����е�ÿһ������Ϊ**����(lexeme)**���ڸ�ʾ���������У�����(lexeme)�ǣ�

![](./assets/1bf290fc6c584d858c62344809f64ae0934a5a1b.png)

����(lexeme)ֻ��**Դ�����ԭʼ�Ӵ�**��Ȼ�����ڽ��ַ����з���Ϊ����(lexeme)�Ĺ����У�����ҲżȻ������һЩ�������õ���Ϣ������ȡ����(lexeme)����������������������һ��ʱ���������һ�����(token)�����������õĶ��������磺

### 4.2.1 token����

�ؼ����������﷨�ṹ��һ���֣����Խ�����ͨ���������Ĵ��룬�������һ��token��`while`��ô��...�� , ����ζ�Ž�����������֪������ĳ����ʶ���Ĵ��أ�������֪������һ��*����*�֣��Լ�����*�ĸ�*�ؼ��֡�

����������ͨ���Ƚ��ַ�����ԭʼ�����еı�ǽ��з��࣬������������е��ѿ����෴��������ʶ���һ������ʱ������Ҳ���ס������������ִ���*��*����ÿ���ؼ��֡�������������ź��������ͣ����Ƕ��в�ͬ�����͡�

> �Ͼ����ַ����Ƚ����ջ�鿴�����ַ����ⲻ��ɨ�����Ĺ�����

```java
package com.craftinginterpreters.lox;

enum TokenType {
  // Single-character tokens.
  LEFT_PAREN, RIGHT_PAREN, LEFT_BRACE, RIGHT_BRACE,
  COMMA, DOT, MINUS, PLUS, SEMICOLON, SLASH, STAR,

  // One or two character tokens.
  BANG, BANG_EQUAL,
  EQUAL, EQUAL_EQUAL,
  GREATER, GREATER_EQUAL,
  LESS, LESS_EQUAL,

  // Literals.
  IDENTIFIER, STRING, NUMBER,

  // Keywords.
  AND, CLASS, ELSE, FALSE, FUN, FOR, IF, NIL, OR,
  PRINT, RETURN, SUPER, THIS, TRUE, VAR, WHILE,

  EOF
}
// lox/TokenType.java, create new file
```

### 4.2.2 ����ֵ

����ֵ�д��ء������ֺ��ַ����ȡ�����ɨ����������������е�ÿ���ַ�������ȷʶ�������������Խ�ֵ���ı���ʾת��Ϊ�Ժ��ɽ�����ʹ�õ�ʵʱ����ʱ����

### 4.2.3 ��λ��Ϣ

��֮ǰ�������������ĺô�ʱ�����ǿ�����Ҫ�����û�*����*�����˴��󡣲������￪ʼ���١��ڼ򵥵Ľ������У�����ֻע��token��������һ�У��������ӵ�ʵ��Ҳ�����кͳ��ȡ�

> һЩtokenʵ�ֽ�λ�ô洢Ϊ�������֣���Դ�ļ���ͷ�����ؿ�ͷ��ƫ�������Լ����صĳ��ȡ�ɨ����������ζ���Ҫ֪����Щ������û�ж���Ŀ������������ǡ�
> 
> ͨ����ͷ�鿴Դ�ļ�������ǰ��Ļ��з��������Ժ�ƫ����ת��Ϊ�к���λ�á�������������������ȷʵ��ˡ�*���ǣ�ֻ������Ҫʵ�����û���ʾһ�к�һ��ʱ��*��Ҫ������������������Զ��������ڴ�����Ϣ�С�������Щ����ǰ����λ����Ϣ��ʱ��Խ��Խ�á�

���ǻ�ȡ������Щ���ݲ������װ��һ�����С�

```java
package com.craftinginterpreters.lox;

class Token {
  final TokenType type;
  final String lexeme;
  final Object literal;
  final int line; 

  Token(TokenType type, String lexeme, Object literal, int line) {
    this.type = type;
    this.lexeme = lexeme;
    this.literal = literal;
    this.line = line;
  }

  public String toString() {
    return type + " " + lexeme + " " + literal;
  }
}
// lox/Token.java, create new file
```

����������һ�������㹻�ṹ�Ķ��󣬿����ڽ����������к����׶Ρ�

## 4.3 ����ͱ��ʽ

��Ȼ����֪��Ҫ����ʲô����ô��������������ɨ�����ĺ�����һ��ѭ������Դ����ĵ�һ���ַ���ʼ��ɨ�����ҳ����ַ������ĸ�����(lexeme)����ʹ���������ڸô���(lexeme)���κκ����ַ�����������ô���(lexeme)��ĩβʱ���������һ��token��

Ȼ����ѭ�����ٴ�ִ�У���Դ�����е���һ���ַ���ʼ����һֱ�����������ַ���ż��������������ǣ�ֱ�������������ĩβ��

![](./assets/59656726271a04ab7f4a6bf03c59838860062ce6.png)

> �ʷ���������

��ѭ���У�����ͨ���鿴һЩ�ַ���ȷ������ƥ�䡱�����ִ���(lexeme)���ⲿ������������Ϥ�������֪��������ʽ�����ܻῼ��Ϊÿ�ִ���(lexeme)����һ��������ʽ��ʹ��������ƥ���ַ������磬���ڱ�ʶ�����������ȣ���Lox ������ C ��ͬ�Ĺ������±����������ʽƥ�䣺

```
[a-zA-Z_][a-zA-Z_0-9]*
```

�����ȷʵ�뵽��������ʽ����ô����ֱ���Ǻ���̵ġ�ȷ���ض�������ν��ַ�����Ϊ����(lexeme)�Ĺ����Ϊ��**�ʷ�**���� Lox �У����ڴ�������������һ�����ʷ��Ĺ���ǳ��򵥣����Խ����Թ���Ϊ**[regular language](https://en.wikipedia.org/wiki/Regular_language)**������������ʽ�еġ�������ͬ��

> �����������������Һ�ʹ�࣬�����ǵ���������Ϊ��[��ķ˹����νṹ](https://en.wikipedia.org/wiki/Chomsky_hierarchy)��[����״̬��](https://en.wikipedia.org/wiki/Finite-state_machine)һ����Ȥʱ ������ʵ˵�������鼮�����ܸ��õؽ�����һ�㡣[*Compilers: Principles, Techniques, and Tools*](https://en.wikipedia.org/wiki/Compilers:_Principles,_Techniques,_and_Tools)���׳ơ����顱���ǹ淶�ο���

���Ը�⣬����ʹ��������ʽ�ǳ���ȷ��*ʶ��*Lox �����в�ͬ����(lexeme)��������һ����Ȥ������֧��Ϊʲô�������Լ�����ζ��ʲô����[Lex](http://dinosaur.compilertools.net/lex/)��[Flex](https://github.com/westes/flex)�����Ĺ��߾���ר��Ϊ��������������Ƶġ�����������һЩ������ʽ�����Ǿͻ����һ��������ɨ������

> Lex �� Mike Lesk �� Eric Schmidt �������ǵģ����ǹȸ�ִ����ϯ�����ʩ���ء��Ҳ�����˵���������ͨ���Ƹ��������ıؾ�֮·������������*�������һλ���������̡�*

�������ǵ�Ŀ�����˽�ɨ�������ִ���������������ǲ���ί�и����������ǹ����ֹ���Ʒ�ġ�

## 4.4 Scanner��

�²��˳٣��������Լ����һ��ɨ������

```java
package com.craftinginterpreters.lox;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import static com.craftinginterpreters.lox.TokenType.*; 

class Scanner {
  private final String source;
  private final List<Token> tokens = new ArrayList<>();

  Scanner(String source) {
    this.source = source;
  }
}
// lox/Scanner.java, create new file
```

> ��֪����Щ����Ϊ��̬���������ķ�񣬵�����ʹ�Ҳ�����ɨ�����ͽ������ϵ�������`TokenType.`��ԭ���ң������е�ÿ���ַ�������Ҫ��

���ǽ�ԭʼԴ����洢Ϊһ���򵥵��ַ���������׼����һ���б�����佫Ҫ���ɵ�token��ǰ���ᵽ��ѭ����������������

```java
  List<Token> scanTokens() {
    while (!isAtEnd()) {
      // We are at the beginning of the next lexeme.
      start = current;
      scanToken();
    }

    tokens.add(new Token(EOF, "", null, line));
    return tokens;
  }
// lox/Scanner.java, add after Scanner() 
```

ɨ����ͨ������Դ���룬���token��ֱ�����������ַ���Ȼ��������һ�����ġ��ļ���������ǡ��ⲻ���ϸ���Ҫ�ģ�����ʹ���������ɾ�һЩ��

���ѭ�������ڼ����ֶ�������ɨ������Դ�����е�λ�á�

```java
  private final List<Token> tokens = new ArrayList<>();
  private int start = 0;
  private int current = 0;
  private int line = 1;

  Scanner(String source) {
// lox/Scanner.java, in class Scanner  
```

`start`��`current`�ֶ����������ַ����е�ƫ��������`start`�ֶ�ָ������ɨ��Ĵ���(lexeme)�еĵ�һ���ַ�����`current`ָ��ǰ���ڿ��ǵ��ַ���`line`�ֶθ���`current`���ڵ�Դ���к�λ�ã���˿�������֪����λ�õı�ǡ�

Ȼ��������һ��С���������������������Ƿ��Ѿ������������ַ���

```java
 private boolean isAtEnd() {
    return current >= source.length();
  }
// lox/Scanner.java, add after scanTokens() 
```

## 4.5 ʶ��lexemes

��ÿһ��ѭ���У�ɨ��һ��token������ɨ�������������ġ����ǽ��Ӽ򵥵Ŀ�ʼ������һ�£����ÿ������(lexeme)ֻ��һ���ַ���������Ҫ���ľ���ʹ����һ���ַ���Ϊ��ѡ��һ��token���͡�ĳЩ����(lexeme)�� Lox*��*ֻ��һ���ַ������������Ǵ���Щ��ʼ��

```java
  private void scanToken() {
    char c = advance();
    switch (c) {
      case '(': addToken(LEFT_PAREN); break;
      case ')': addToken(RIGHT_PAREN); break;
      case '{': addToken(LEFT_BRACE); break;
      case '}': addToken(RIGHT_BRACE); break;
      case ',': addToken(COMMA); break;
      case '.': addToken(DOT); break;
      case '-': addToken(MINUS); break;
      case '+': addToken(PLUS); break;
      case ';': addToken(SEMICOLON); break;
      case '*': addToken(STAR); break; 
    }
  }
// lox/Scanner.java, add after scanTokens() 
```

> ��֪��Ϊʲô`/`����������ģ����ǻ����ġ�

ͬ����������ҪһЩ����������

```java
  private char advance() {
    return source.charAt(current++);
  }

  private void addToken(TokenType type) {
    addToken(type, null);
  }

  private void addToken(TokenType type, Object literal) {
    String text = source.substring(start, current);
    tokens.add(new Token(type, text, literal, line));
  }
// lox/Scanner.java, add after isAtEnd() 
```

��`advance()`����ʹ��Դ�ļ��е���һ���ַ������䷵�ء�����`advance()`�����룬������`addToken()`���������ȡ��ǰ���ص��ı���Ϊ�䴴��һ���µı�ǡ����ǽ��ܿ�ʹ�����������������������ֵ�ı�ǡ�

### 4.5.1  �ʷ�����

������֮ǰ�������ǻ���ʱ�俼��һ�´ʷ�����Ĵ�������û���������׳�һ������ Lox ��ʹ�õ�ĳЩ�ַ���Դ�ļ������� `@#^`, �ᷢ��ʲô��Ŀǰ����Щ�ַ���ĬĬ�ض����ˡ�Lox ���Բ�ʹ�����ǣ����Ⲣ����ζ�Ž��������Լ�װ���ǲ����ڡ��෴�����Ǳ���һ������

```java
      case '*': addToken(STAR); break; 

      default:
        Lox.error(line, "Unexpected character.");
        break;
    }
// lox/Scanner.java, in scanToken()
```

��ע�⣬�����ַ��Իᱻ֮ǰ��`advance()` �ĵ�����*����*�������Ҫ���������ǾͲ�����������ѭ����

��ע�⣬���ǽ�����*����ɨ��*�����������ܻ������������������һ�μ�⵽�����ܶ����Щ��������û��������õ����顣�������ǿ���һ��С�����޸������ֻ������һ�������������ơ��﷨���� Whac-A-Mole һ�㶼�����档

�����ġ���Ϊ`hadError`�����ˣ�������Զ���᳢��*ִ��*�κδ��룬��ʹ���Ǽ���ɨ���������ಿ�֡���

> �ô���ֱ𱨸�ÿ����Ч�ַ����������û���С��ճ����һ�����ֵ��ı��������û������������󡣽�һϵ����Ч�ַ��ϲ�Ϊһ��������ṩ���õ��û����顣

### 4.5.2 ������ operators

���ڵ��ַ�����(lexeme)�Ѿ��������Ⲣû�к��� Lox �������������`!`��ô����`!`�ǵ����ַ����԰ɣ���ʱ���������һ���ַ��ǵȺţ���ô����Ӧ�ô���һ��`!=`���ء���ע�⣬`!`��`=` ����������������������������� Lox �б�д`! =`��ʹ����ֵ��񲻵�ʽ������������Ϊʲô������Ҫ����ɨ��Ϊ��������(lexeme)��ͬ����`<,>`��`=`���涼���Ը���`=`���ڴ���������ȺͱȽ������������`>= ,<=, == `��

����������Щ��������Ҫ���ڶ����ַ���

```java
      case '*': addToken(STAR); break; 
      case '!':
        addToken(match('=') ? BANG_EQUAL : BANG);
        break;
      case '=':
        addToken(match('=') ? EQUAL_EQUAL : EQUAL);
        break;
      case '<':
        addToken(match('=') ? LESS_EQUAL : LESS);
        break;
      case '>':
        addToken(match('=') ? GREATER_EQUAL : GREATER);
        break;

      default:
// lox/Scanner.java, in scanToken()
```

��Щcase ʹ������·�����

```java
 private boolean match(char expected) {
    if (isAtEnd()) return false;
    if (source.charAt(current) != expected) return false;

    current++;
    return true;
  }
// lox/Scanner.java, add after scanToken()
```

�����һ������`advance()`.���������������Ѱ�ҵģ�ֻ�����ĵ�ǰ�ַ���

ʹ��`match()`���������׶�ʶ����Щ���ء����磬�����ǵ���ʱ`!`��������ת������ switch case������ζ������֪����λ*��*?`!`Ȼ��鿴��һ���ַ���ȷ������  `!=`�ϻ��ǽ����� `!`�ϡ�

## 4.6 ����lexemes

������Ȼȱ��һ���������`/`���ڳ��������ַ���ҪһЩ���⴦����Ϊע��Ҳ��б�ܿ�ͷ��

```java
        break;
      case '/':
        if (match('/')) {
          // A comment goes until the end of the line.
          while (peek() != '\n' && !isAtEnd()) advance();
        } else {
          addToken(SLASH);
        }
        break;

      default:
// lox/Scanner.java, in scanToken()
```

������������˫�ַ��������ֻ�ǵ��ҵ��ڶ���ʱ`/`����û�н���token���෴����һֱ�����ַ���ֱ��������β��

�������Ǵ���ϳ����ص�һ����ԡ��ڼ�⵽һ����ʼ֮�󣬷�����һЩ�ض��ڴ��صĴ��룬��Щ����һֱ�������ַ�ֱ��������������

���ǻ���Ҫ��һ������������

```java
 private char peek() {
    if (isAtEnd()) return '\0';
    return source.charAt(current);
  }
// lox/Scanner.java, add after match() 
```

�е���`advance()`�����������ַ������Ϊ**ǰհlookahead**��������ֻ�鿴��ǰδʹ�õ��ַ������������*һ�� lookahead �ַ�*��ͨ�����������ԽС��ɨ�������е�Խ�졣�ʷ������������Ҫ����ǰհ�ԡ����˵��ǣ�������㷺ʹ�õ�����ֻ�ܿ���ǰ���һ�����ַ���

> �Ӽ����Ͻ���`match()`Ҳ����ǰհ��`advance()`����`peek()`�ǻ������������`match()`���������һ��

ע���Ǵ���(lexeme)��������û�����壬���������봦�����ǡ����Ե����ǵ���ע�͵�ĩβʱ������*����*����`addToken()`.������ѭ����ȥ��ʼ��һ������ʱ��`start`�������ò���ע�͵Ĵ�����ʧ��һ�������С�

����������ʱ�����������������������ַ��ĺ�ʱ�������з��Ϳո�

```java
         break;

      case ' ':
      case '\r':
      case '\t':
        // Ignore whitespace.
        break;

      case '\n':
        line++;
        break;

      default:
        Lox.error(line, "Unexpected character.");
// lox/Scanner.java, in scanToken()
```

�������ո�ʱ���򵥵ػص�ɨ��ѭ���Ŀ�ͷ���ڿհ��ַ�*֮��*��ʼһ���µĴ��ء����ڻ��У���ͬ�������飬������Ҳ�����м��������������Ϊʲô���ǹ�ȥ����`peek()`�ҵ�ע�ͽ�β�Ļ��з�������`match()`������ϣ�����з������Ǵ�������Ա����ǿ��Ը���`line`����

���ǵ�ɨ�������Խ��Խ���ܡ������Դ����൱���ɸ�ʽ�Ĵ��룬���磺

```
// this is a comment
(( )){} // grouping stuff
!*+-/=<> <= == // operators
```

### 4.6.1  �ַ���������

���������Ѿ�ϰ���˸����Ĵ��أ�����׼���ô��������ˡ������ȴ����ַ�������Ϊ�����������ض��ַ���ͷ`"`��

```java
        break;

      case '"': string(); break;

      default:
// lox/Scanner.java, in scanToken()
```

�����ã�

```java
   private void string() {
    while (peek() != '"' && !isAtEnd()) {
      if (peek() == '\n') line++;
      advance();
    }

    if (isAtEnd()) {
      Lox.error(line, "Unterminated string.");
      return;
    }

    // The closing ".
    advance();

    // Trim the surrounding quotes.
    String value = source.substring(start + 1, current - 1);
    addToken(STRING, value);
  }
// lox/Scanner.java, add after scanToken()  
```

��ע��һ���������ַ���ֱ����������`"`�����ַ��������ǻ����ŵش��������ַ����ر�֮ǰ�������벢�������������

û���ر��ԭ��Lox ֧�ֶ����ַ�����������Ҳ�бף�����ֹ���Ǳ��������Ǹ���һ�㣬�����ұ��������ǡ���ȷʵ��ζ�����ǻ���Ҫ`line`���ַ������������з�ʱ���и��¡�

���һ����Ȥ���ǣ������Ǵ���tokenʱ����������ʵ�ʵ��ַ���*ֵ*���Ժ��ɽ�����ʹ�á��������ת��ֻ��Ҫ `substring()`ȥ����Χ�����š���� Lox ֧��ת�����У���`\n`�����ǽ��ڴ˴�ȡ��ת�塣

### 4.6.2 ����������

Lox �е���������������ʱ���Ǹ���������֧��������С����������������һϵ�����֣���ѡ�غ��һ��`.`��һ������β�����֡�

```c
1234 
12.34
```

> ��������ֻѰ��һ����������ʼһ�����֣�����ζ��`-123`�����������������෴��`-123`, ��һ��Ӧ����������������*���ʽ*����ʵ���У��������ͬ�ģ������������Ҫ��Ӷ����ֵķ������ã�����һ����Ȥ�ı�Ե��������ǣ�`-``123`
> 
> ```
> print -123.abs();
> ```

> ֮���Դ�ӡ`-123`������Ϊ�񶨵����ȼ����ڷ������á����ǿ���ͨ��ʹ`-`���ֵ�һ���ֳ�Ϊ���������������⡣���뿼�ǣ�
> 
> ```
> var n = 123;
> print -n.abs();
> ```

> ����Ȼ�����`-123`���������������ƺ���һ�¡���������ʲô����Щ�������ջ��ú���֡�

���ǲ�����ǰ����β��С���㣬�����Щ������Ч�ģ�

```c
.1234
1234.
```

���Ժ����׵�֧��ǰ�ߣ���Ϊ�˼�������ҽ���ʡ���ˡ����������Ҫ������ . �����������ϵķ��������߻��ú����`123.sqrt()`��

Ϊ��ʶ�����ִ��صĿ�ͷ������Ѱ���κ����֡�Ϊÿ��ʮ����������Ӵ�Сд�е㷦ζ��������ǽ������ΪĬ�ϴ�Сд��

```java
  default:
        if (isDigit(c)) {
          number();
        } else {
          Lox.error(line, "Unexpected character.");
        }
        break;
// lox/Scanner.java, in scanToken(), replace 1 line
```

���������������������

```java
  private boolean isDigit(char c) {
    return c >= '0' && c <= '9';
  } 
// lox/Scanner.java, add after peek()
```

> Java ��׼���ṩ��[`Character.isDigit()`](http://docs.oracle.com/javase/7/docs/api/java/lang/Character.html#isDigit(char))���⿴�����ܺ��ʡ�Ȼ�����÷����������� Devanagari ���֡�ȫ�����ֺ��������ǲ���Ҫ����Ȥ���ݡ�

һ��֪����һ�������У��ͻ��֧��һ�������ķ�����ʹ����������֣��������Ǵ����ַ���һ����

```java
  private void number() {
    while (isDigit(peek())) advance();

    // Look for a fractional part.
    if (peek() == '.' && isDigit(peekNext())) {
      // Consume the "."
      advance();

      while (isDigit(peek())) advance();
    }

    addToken(NUMBER,
        Double.parseDouble(source.substring(start, current)));
  }
// lox/Scanner.java, add after scanToken()
```

�������ֵ��������֣������˾����ܶ�����֡�Ȼ������Ѱ��С�����֣���С���� (`.`) �������һλ���֡��������ȷʵ��С�����֣�ͬ���������ľ����ܶ�����֡�

Խ��С������Ҫ�ڶ����ַ� lookahead����Ϊ���ǲ�������`.`ֱ������ȷ����*����*��һ�����֡�����������ӣ�

```java
  private char peekNext() {
    if (current + 1 >= source.length()) return '\0';
    return source.charAt(current + 1);
  } 
// lox/Scanner.java, add after peek()
```

> �ұ�����`peek()`ΪҪ�鿴���ַ�������Ϊһ�������������Ƕ����������������⽫����*����*Զ��ǰհ���ṩ���������������ô���Ķ��߸�������ǵ�ɨ���������ǰ�������ַ���

��󣬽�����ת����������ֵ�����ǵĽ�����ʹ�� Java ��`Double`��������ʾ���֣�����������ɸ����͵�ֵ����������ʹ�� Java �Լ��Ľ�������������ת��Ϊ������ Java ˫�����������ǿ����Լ�ʵ������������ʵ˵����������Ϊ���������ı�����Բ�ϰ������ֵ���㻨ʱ�䡣

����������ǲ���ֵ��`nil`�������ǽ�������Ϊ�ؼ����������±��ֵ���...

## 4.7 �����ؼ��ֺͱ�ʶ��

ɨ�����������ˡ�Ψһʣ�µ�Ҫʵ�ֵĴʷ������Ǳ�ʶ�������ǵĽ��ף������֡���������Ϊ���� ƥ��ؼ��� ����`or`����������ַ������������`<=`һ��.

```java
 case 'o':
  if (match('r')) {
    addToken(OR);
  }
  break;
```

����һ������û�����һ�������ᷢ��ʲô`orchid`��ɨ�����ῴ��ǰ������ĸ`or`������������`or`�ؼ���token���������ǵõ���һ����Ҫ��ԭ�򣬳�Ϊ**Maximal munch ���׽�**���������ʷ����򶼿���ƥ��ɨ�������ڲ鿴��һ�δ���ʱ��*ƥ������ַ����Ǹ�����ʤ*��

�ù���涨��������ǿ���`orchid`��Ϊ��ʶ����`or`�ؼ��ֽ���ƥ�䣬��ǰ�߻�ʤ����Ҳ������֮ǰĬ�ϵ�ԭ�򣬼�`<=`Ӧ����Ϊ����`<=`��ǽ���ɨ�裬������`<`���`=`.

> ���������������� C ���룺
> 
> ---a;
> 
> ��Ч����ȡ����ɨ������β�ִ�λ�����ɨ���ǿ���������������ô�죺
> 
> ```
> - --a;
> ```

> Ȼ��Ϳ��Խ����ˡ�������Ҫɨ�����˽���Χ������﷨�ṹ�����������Ҫ�ĸ����ӡ��෴�����׽�����˵��*����*������ɨ�裺
> 
> ```
> -- -a;
> ```

> �������ַ�ʽɨ��������ʹ�������ᵼ���Ժ��ڽ������г����﷨����

Maximal munch ��ζ�������޷����׼�⵽�����֣�ֱ�����ǵ�������Ǳ�ʶ���Ľ�β���Ͼ�����������һ�ֱ�ʶ������ֻ��һ�ֱ���������Ϊ�Լ�ʹ�õı�ʶ�������������**������**����Դ��

��ˣ��������ȼ����κ�����ĸ���»��߿�ͷ�Ĵ��ض��Ǳ�ʶ��

```java
      default:
        if (isDigit(c)) {
          number();
        } else if (isAlpha(c)) {
          identifier();
        } else {
          Lox.error(line, "Unexpected character.");
        }
// lox/Scanner.java, in scanToken()
```

������붼�����

```java
  private void identifier() {
    while (isAlphaNumeric(peek())) advance();

    addToken(IDENTIFIER);
  }
// lox/Scanner.java, add after scanToken()
```

���Ǹ�����Щ�������������壺

```java
  private boolean isAlpha(char c) {
    return (c >= 'a' && c <= 'z') ||
           (c >= 'A' && c <= 'Z') ||
            c == '_';
  }

  private boolean isAlphaNumeric(char c) {
    return isAlpha(c) || isDigit(c);
  }
// lox/Scanner.java, add after peekNext()
```

��ʹ��ʶ�����Թ�����Ϊ�˴���ؼ��֣����ǲ鿴��ʶ���Ĵ����Ƿ��Ǳ�����֮һ������ǣ���ʹ���ض��ڸùؼ��ֵ�token���͡���ӳ���ж�����һ�鱣���֡�

```java
  private static final Map<String, TokenType> keywords;

  static {
    keywords = new HashMap<>();
    keywords.put("and",    AND);
    keywords.put("class",  CLASS);
    keywords.put("else",   ELSE);
    keywords.put("false",  FALSE);
    keywords.put("for",    FOR);
    keywords.put("fun",    FUN);
    keywords.put("if",     IF);
    keywords.put("nil",    NIL);
    keywords.put("or",     OR);
    keywords.put("print",  PRINT);
    keywords.put("return", RETURN);
    keywords.put("super",  SUPER);
    keywords.put("this",   THIS);
    keywords.put("true",   TRUE);
    keywords.put("var",    VAR);
    keywords.put("while",  WHILE);
  }
// lox/Scanner.java, in class Scanner
```

Ȼ����ɨ��һ����ʶ��֮�󣬼�����Ƿ���Map�е��κζ�����ƥ�䡣

```java
while (isAlphaNumeric(peek())) advance();

    String text = source.substring(start, current);
    TokenType type = keywords.get(text);
    if (type == null) type = IDENTIFIER;
    addToken(type);
  }
// lox/Scanner.java, in identifier(), replace 1 line
```

�������������ʹ�øùؼ��ֵ�token���͡���������һ��������û������ʶ����

�������������ھ�����һ�������� Lox �ʷ�ɨ���������� REPL ������һЩ��Ч����Ч�Ĵ��롣���������������token�𣿳������һЩ��Ȥ�ı�Ե������������Ƿ��ܰ�Ӧ�еķ�ʽ�������ǡ�

---

## [��ս](http://craftinginterpreters.com/scanning.html#challenges)

1. Python �� Haskell �Ĵʷ��﷨��*������*�ġ�����ʲô��˼��Ϊʲô�����أ�

2. ���˷ָ�token����������`print foo`��`printfoo`֮�⣬�ո��ڴ�����������ô�����Ȼ������һЩ�ڰ��Ľ��䣬�ո�*ȷʵ*��Ӱ������� CoffeeScript��Ruby �� C Ԥ�������еĽ�����ʽ������ÿ�������е�λ�ú�Ӱ����ʲô��

3. ������ɨ����һ�������������ɨ�����ᶪ��ע�ͺͿո���Ϊ����������Ҫ���ǡ���ΪʲôҪдһ��*��*������Щ��ɨ����������ʲô�ã�

4. Ϊ Lox ��ɨ������Ӷ� C ��ʽ`/* ... */`��ע�͵�֧�֡�ȷ���������еĻ��з�������������Ƕ�ס���Ӷ�Ƕ�׵�֧���Ƿ����Ԥ�ڵ�Ҫ�ࣿΪʲô��

## [���˵������ʽ�ֺ�](http://craftinginterpreters.com/scanning.html#design-note)

����ĳ���Ա�����Ե�ѡ��軵�ˣ����Ҷ��﷨������ޡ�����ϣ�����Կ������ɾ����ִ�������ÿһ�������ԣ��Լ�һЩ���ϵ����ԣ��� BASIC ��δ�й��ģ������ε���һ��䷨������`;`��Ϊ��ʽ�����ֹ����

�෴�����ǽ����з���Ϊ�����ֹ������������������ġ���������ĵط��������Ǿ�����ս�ԵĲ��֡���Ȼ*�����*��䶼���Լ������У�����ʱ����Ҫ����������ɢ�ڼ����С���Щ��ϵĻ��з���Ӧ����Ϊ��ֹ����

�����Ӧ�ú��Ի��з���������������׼�⵽����Ҳ��һЩ��������������

- ��һ�еķ���ֵ��
  
  ```c
  if (condition) return
  "value"
  ```
  
  ��value���Ǳ����ص�ֵ������������һ��`return`û��ֵ�������һ�������ַ������ֵı��ʽ��䣿

- ��һ�д����ŵı��ʽ��
  
  ```c
  func
  (parenthesized)
  ```
  
  ���Ƕ� �ĵ���`func(parenthesized)`�������������ʽ��䣬һ������`func`�����ڵı��ʽ��һ�����������ڵı��ʽ��

- ����һ����һ��`-`��
  
  ```c
  first
  -second
  ```
  
  ����һ��`first - second`��׺�����������������ʽ��䣬һ�����ڣ�һ������`first`��`second`��

��������Щ�У������Ƿ񽫻��з���Ϊ�ָ������������Ч���룬�����ܲ����û���Ҫ�Ĵ��롣�ڸ��������У����ھ�����Щ���з��Ƿָ����Ĺ������෱�࣬���˲�����������һ�ԣ�

- [Lua](https://www.lua.org/pil/1.1.html)��ȫ���Ի��з�������ϸ�������﷨���Ա��ڴ��������¸�������Ҫ���֮��ķָ�����������ȫ�Ϸ��ģ�
  
  ```
  a = 1 b = 2
  ```
  
  Lua`return`ͨ��Ҫ��`return`����ǿ��е����һ�����������������⡣`return`����ؼ���֮ǰ��ֵ`end`������*����*��`return`�����������������������������ʽ`;`�������û�ʹ������ʵ���ϣ��⼸����Զ���ᷢ������Ϊ�����ŵĻ�һԪ�񶨱��ʽ���û�����塣

- [Go](https://golang.org/ref/spec#Semicolons)��ɨ�����д����з���������з���������֪���ܽ������������������֮һ֮�����з�������Ϊ�ֺš����򽫱����ԡ�Go �Ŷ��ṩ��һ���淶�Ĵ����ʽ������[gofmt](https://golang.org/cmd/gofmt/)��������̬ϵͳ������ʹ�÷ǳ����ԣ���ȷ���˹��÷��Ĵ�����Ժܺõ���Ӧ����򵥵Ĺ���

- [Python](https://docs.python.org/3.5/reference/lexical_analysis.html#implicit-line-joining)�����л��з�����Ϊ��Ҫ�ģ�������һ�е�ĩβʹ����ʽ��б�ܽ����������һ�С����ǣ�����һ�Է����ţ� �� �� ��`()`��`[]`�κ�λ�õĻ��з���`{}`���÷��ǿ�������ں��ߡ�
  
  �������������� Python����Ϊ����һ�ָ߶������������ԡ��ر��ǣ�Python ���﷨ȷ�������Զ��������ڱ��ʽ�С�C ��ͬ�������飬������������С�lambda�����������﷨������ȴû�С�
  
  JavaScript �е�һ��ʾ����
  
  ```js
  console.log(function() {
    statement();
  });
  ```
  
  ���`console.log()`*���ʽ*����һ���������������������������ְ���*���*`statement();`��
  
  ��������Է���*��*���з�Ӧ�ñ�������嵫��Ƕ���������ڵ���䣬Python ����Ҫһ�鲻ͬ�Ĺ�������ʽ�����С�

������֪��Ϊʲô Python`lambda`ֻ����һ�����ʽ�����ˡ�

- JavaScript �ġ�[�Զ��ֺŲ���](https://www.ecma-international.org/ecma-262/5.1/#sec-7.9)�����������������⡣�������Լ����������з�*��*������ģ����ڶ��������ֻ������Ӧ�ñ����ԣ��� JS �����෴��*����*�����������󣬷����������л��з���Ϊ������Ŀո���������������᷵�ز����Խ�֮ǰ�Ļ��з�ת��Ϊ�ֺ��Ի���﷨����Ч�����ݡ�
  
  *�������ϸ����������ι���*�ģ���ô������˵���������Ʒ̰���������˵ JavaScript �ġ������������һ������������и��ַ�ʽ��һ���㡣JavaScript ������֪����Ψһһ�����ԣ������ָ��Ҫ����ÿ��������ȷ�ֺţ���ʹ������������������ʡ�����ǡ�

������������һ�������ԣ��������϶�*Ӧ��*����ʹ����ʽ�����ֹ��������Ա��������һ������׷��ʱ�е�����ֺź�ȫ��д�ؼ���һ���Ѿ���ʱ�ˡ�ֻ��ȷ����ѡ����һ����������Ե��ض��﷨��ϰ��������Ĺ��򡣲��Ҳ�Ҫ�� JavaScript ���������顣