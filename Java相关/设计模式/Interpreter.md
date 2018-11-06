# 行为模式-解释器模式（Interpreter）
解释器模式提供了评估语言的语法或表达式的方式，这种模式实现了一个表达式接口，该接口解释一个特定的上下文。这种模式被用在 SQL 解析、符号处理引擎等。
#### 实现
我们将创建一个接口 Expression 和实现了 Expression 接口的实体类。定义作为上下文中主要解释器的 TerminalExpression 类。其他的类 OrExpression、AndExpression 用于创建组合式表达式。
![](../../resources/interpreter.jpg)

#### 代码
Expression、TerminalExpression类  
```java
public interface Expression {
    boolean interpret(String context);
}
public class TerminalExpression implements Expression {
    private String data;

    public TerminalExpression(String data) {
        this.data = data;
    }

    @Override
    public boolean interpret(String context) {
        if (context.contains(data)) {
            return true;
        }
        return false;
    }
}
```
OrExpression、AndExpression类
```java
public class OrExpression implements Expression {
    private Expression expre1;
    private Expression expre2;

    public OrExpression(Expression expre1, Expression expre2) {
        this.expre1 = expre1;
        this.expre2 = expre2;
    }

    @Override
    public boolean interpret(String context) {
        return expre1.interpret(context) || expre2.interpret(context);
    }
}

public class AndExpression implements Expression {
    private Expression expre1;
    private Expression expre2;

    public AndExpression(Expression expre1, Expression expre2) {
        this.expre1 = expre1;
        this.expre2 = expre2;
    }

    @Override
    public boolean interpret(String context) {
        return expre1.interpret(context) && expre2.interpret(context);
    }
}

```
使用   
```java
public class Demo {
    // 是否包含Robert或者John的表达式
    private static Expression getOrExpression() {
        Expression robert = new TerminalExpression("Robert");
        Expression john = new TerminalExpression("John");
        return new OrExpression(robert, john);
    }

    // 是否既包含Jane又包含female的表达式
    private static Expression getAndExpression() {
        Expression jane = new TerminalExpression("Jane");
        Expression female = new TerminalExpression("Female");
        return new AndExpression(jane, female);
    }

    public static void main(String[] args) {
        Expression orExpression = getOrExpression();
        Expression andExpression = getAndExpression();

        System.out.printf("OR-是否包含John字符串：【%b】。\n", orExpression.interpret("John"));
        System.out.printf("AND-是否包含Female Jane字符串：【%b】。\n", andExpression.interpret("Female Jane"));
    }
}
```
结果 
```
OR-是否包含John字符串：【true】。
AND-是否包含Female Jane字符串：【true】。
```