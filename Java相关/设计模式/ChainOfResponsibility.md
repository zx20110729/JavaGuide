# 行为模式-责任链模式（Chain Of Responsibility）
责任链模式为请求者创建了一个接收者对象的链，这种模式对请求的类型、请求的发送者和请求的接收者进行了解耦。
在这种模式中，每个接收者都包含了对另一个接收者的引用。如果一个对象不能处理当前情况，会把当前的请求传给下一个接收者，依此类推。
责任链上的处理者处理请求，客户只需要请求发送到责任链即可，无需关心请求的细节和请求的传递，so责任链将请求发送者和请求处理者解耦。
#### 实现
我们创建抽象类 AbstractLogger，带有详细的日志记录级别。然后我们创建三种类型的记录器，都扩展了 AbstractLogger。每个记录器消息的级别是否属于自己的级别，如果是则相应地打印出来，否则将不打印并把消息传给下一个记录器。

```
AbstractLogger：日志抽象类
ConsoleLogger：控制台日志
FileLogger：文件日志
ErrorLogger：错误级别日志
```
![](../../resources/chain.jpg)
AbstractLogger：日志抽象类
```java
public abstract class AbstractLogger {
    public static final int INFO = 1;
    public static final int WARN = 2;
    public static final int ERROR = 3;

    protected int level;

    private AbstractLogger nextLogger;

    public void setNextLogger(AbstractLogger nextLogger) {
        this.nextLogger = nextLogger;
    }

    public void logMessage(int level, String message) {
        if (this.level <= level) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.logMessage(level, message);
        }
    }

    abstract public void write(String message);
}
```
ConsoleLogger类
```java
public class ConsoleLogger extends AbstractLogger {
    public ConsoleLogger(int level) {
        this.level = level;
    }

    @Override
    public void write(String message) {
        System.out.printf("控制台日志：级别【%s】，日志：【%s】。\n", level, message);
    }
}

```
FileLogger类
```java
public class FileLogger extends AbstractLogger {
    public FileLogger(int level) {
        this.level = level;
    }

    @Override
    public void write(String message) {
        System.out.printf("文件日志：级别【%s】，日志：【%s】。\n", level, message);
    }
}
```
ErrorLogger类
```java
public class ErrorLogger extends AbstractLogger {
    public ErrorLogger(int level) {
        this.level = level;
    }

    @Override
    public void write(String message) {
        System.out.printf("错误日志：级别【%s】，日志：【%s】。\n", level, message);
    }
}

```

使用
```java
public class Demo {
    public static void main(String[] args) {
        AbstractLogger consoleLogger = new ConsoleLogger(INFO);
        AbstractLogger fileLogger = new FileLogger(WARN);
        AbstractLogger errorLogger = new ErrorLogger(ERROR);

        consoleLogger.setNextLogger(fileLogger);
        fileLogger.setNextLogger(errorLogger);

        consoleLogger.logMessage(INFO, "information.");
        consoleLogger.logMessage(WARN, "warn info.");
        consoleLogger.logMessage(ERROR, "error info.");
    }
}

```

结果
```
控制台日志：级别【1】，日志：【information.】。
控制台日志：级别【1】，日志：【warn info.】。
文件日志：级别【2】，日志：【warn info.】。
控制台日志：级别【1】，日志：【error info.】。
文件日志：级别【2】，日志：【error info.】。
错误日志：级别【3】，日志：【error info.】。
```