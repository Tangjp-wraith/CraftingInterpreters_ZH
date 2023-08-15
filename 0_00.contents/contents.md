![](./assets/6f7f36299a48f19840b95efbe9ecd1ccaf67956d.png)

# �������ߵ�ַ http://craftinginterpreters.com/

# ��л����



�����ý� 4 ���ʱ����������͸Ľ����飬������ Web �汾���������ϡ��Ȿֽ�����ڽ��� 7 �³��棬������ Hacker News ������ý���������ע�����ۡ�

�����������ȶ�����һ����̬���͵����� Lox��Lox ������һ�����򵥡��� toy ���ԣ���֧���� function��closure��class��inheritance��gabage collection �ȸ߼����ԡ�Ȼ�����������ֲ�ͬ��ʵ�ַ�ʽ��Tree-walk Interpreter �� Bytecode VM��������չʾ����� Parsing �Լ�ʵ����Щ�߼����ԡ�

1. Tree-walk Interpreter. ������Ϊ����ֱ���� C ����ʵ�� Bytecode VM ���ڶ�����˵�е㲻�Ѻã��������� Java ʵ����һ�� Tree-walk Interpreter���ⲿ����Ҫ focus �ڸ����ϣ�ʵ����һ��������⣬��������ô��Ч�� Interpreter��

2. Bytecode VM. �ڱ���ĵڶ����֣������� C ����ʵ����һ������ Bytecode VM �Ľ�������������Pratt Parser������ Lox ���뵽 VM �ֽ��롢VM��gabage collection��optimization �ȡ�

��������߰�һ�����ӵ� topic �ֽ��һ��һ�����Զ������е�ģ������������鹲�� 30 �£�����ÿһ�½�βʱ����һ���������еĽ�������ֻ�Ǻ�����½ڻ���ǰ����½��ϼ�һЩ feature�����˵�����򵥣�������η��� dependency���ֽ� feature���ع��Ȼ��ǱȽ���ս�ġ�

������е�ÿ����ͼ�����������ֻ���ֽ�ϣ�Ȼ��ɨ��ĵ����ϵġ�



# Ŀ¼

## 0.ǰ��

### ���� Dedication

### [��л Acknowledgements](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/0_01.acknowledgements/acknowledgements.md)

## һ.��ӭ

### 1.[���� Introduction](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/1_01.Introduction/Introduction.md)

 ���˵���������а���ʲô�� Design Note: What��s in a Name?

### 2.[���� A Map of the Territory](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/1_02.A%20Map%20of%20the%20Territory/A%20Map%20of%20the%20Territory.md)

### 3.[Lox ���� The Lox Language](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/1_03.The%20Lox%20Language/The%20Lox%20Language.md)

 ���˵�������ʽ����� Design Note: Expressions and Statements

## ��.[�����������Ľ����� A TREE-WALK INTERPRETER](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_00.A%20TREE-WALK%20INTERPRETER/A%20TREE-WALK%20INTERPRETER.md)

### 4.[ɨ�� Scanning](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_04.Scanning/Scanning.md)

 ���˵������ʽ�ֺ� Design Note: Implicit Semicolons

### 5.[�����ʾ Representing Code](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_05.Representing%20Code/Representing%20Code.md)

### 6.[�������ʽ Parsing Expressions](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_06.Parsing%20Expressions/Parsing%20Expressions.md)

 ���˵�����߼�����ʷ  Design Note: Logic Versus History

### 7.[���ʽ��ֵ Evaluating Expressions](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_07.Evaluating%20Expressions/Evaluating%20Expressions.md)

 ���˵������̬�Ͷ�̬���� Design Note: Static and Dynamic Typing

### 8.[����״̬ Statements and State](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_08.Statements%20and%20State/Statements%20and%20State.md)

 ���˵������ʽ�������� Design Note: Implicit Variable Declaration

### 9.[������ Control Flow](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_09.Control%20Flow/Control%20Flow.md)

 ���˵����һ���﷨��  Design Note: Spoonfuls of Syntactic Sugar

### 10.[���� Functions](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_10.Functions/Functions.md)

### 11.[�����Ͱ� Resolving and Binding](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_11.Resolving%20and%20Binding/Resolving%20and%20Binding.md)

### 12.[�� Classes](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_12.Classes/Classes.md)

 ���˵����ԭ�ͺ͵�Դ  Design Note: Prototypes and Power

### 13.[�̳� Inheritance](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/2_13.Inheritance/Inheritance.md)

## ��.[�ֽ�������� A BYTECODE VIRTUAL MACHINE](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_00.A%20BYTECODE%20VIRTUAL%20MACHINE/A%20BYTECODE%20VIRTUAL%20MACHINE.md)

### 14.[�ֽ���� Chunks of Bytecode](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_14.Chunks%20of%20Bytecode/Chunks%20of%20Bytecode.md)

 ��Ʊʼǣ������������   Design Note: Test Your Language

### 15.[����� A Virtual Machine](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_15.A%20Virtual%20Machine/A%20Virtual%20Machine.md)

 ���˵�������ڼĴ������ֽ���  Design Note: Register-Based Bytecode

### 16.[����ɨ�� Scanning on Demand](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_16.Scanning%20on%20Demand/Scanning%20on%20Demand.md)

### 17.[������ʽ Compiling Expressions](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_17.Compiling%20Expressions/Compiling%20Expressions.md)

 ���˵����ֻ�ǽ���    Design Note: It��s Just Parsing

### 18.[ֵ������ Types of Values](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_18.Types%20of%20Values/Types%20of%20Values.md)

### 19.[�ַ��� Strings](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_19.Strings/Strings.md)

 ���˵�����ַ�������  Design Note: String Encoding

### 20.[��ϣ�� Hash Tables](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_20.Hash%20Tables/Hash%20Tables.md)

### 21.[ȫ�ֱ��� Global Variables](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_21.Global%20Variables/Global%20Variables.md)

### 22.[�ֲ����� Local Variables](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_22.Local%20Variables/Local%20Variables.md)

### 23.[��ת���� Jumping Back and Forth](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_23.Jumping%20Back%20and%20Forth/Jumping%20Back%20and%20Forth.md)

 ���˵�������ǵ��к��� Goto   Design Note: Considering Goto Harmful

### 24.[��������   Calls and Functions](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_24.Calls%20and%20Functions/Calls%20and%20Functions.md)

### 25.[�հ� Closures](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_25.Closures/Closures.md)

 ���˵�����ر�ѭ������  Design Note: Closing Over the Loop Variable

### 26.[�����ռ� Garbage Collection](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_26.Garbage%20Collection/Garbage%20Collection.md)

 ���˵�����ִ��������� Design Note: Generational Collectors

### 27.[���ʵ��  Classes and Instances ](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_27.Classes%20and%20Instances/Classes%20and%20Instances.md)

### 28.[�����ͳ�ʼ����  Methods and Initializers ](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_28.Methods%20and%20Initializers/Methods%20and%20Initializers.md)

 ���˵��������Ԥ��  Design Note: Novelty Budget

### 29. [����  Superclasses ](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_29.Superclasses/Superclasses.md)

### 30. [�Ż�  Optimization ](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/3_30.Optimization/Optimization.md)

## ��.���

### [A1.��¼1��Lox �﷨    Lox Grammar  ](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/4_01.A1.Appendix%20I%20Lox%20Grammar/A1.Appendix%20I%20Lox%20Grammar.md)

### [A2.��¼2�����ɵ��﷨����   Generated Syntax Tree Classes ](https://gitee.com/xuejianxinokok/mysqldoc/blob/master/craftinginterpreters/4_02.A2.Appendix%20II%20Generated%20Syntax%20Tree%20Classes/A2.Appendix%20II%20Generated%20Syntax%20Tree%20Classes.md)