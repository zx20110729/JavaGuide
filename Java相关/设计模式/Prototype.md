# 创建模式 原型模式（prototype）

Prototype模式允许一个对象再创建另外一个可定制的对象，根本无需知道任何如何创建的细节,工作原理是:通过将一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象拷贝它们自己来实施创建。
如何使用?

因为Java中的提供clone()方法来实现对象的克隆,所以Prototype模式实现一下子变得很简单.
```
AbstractSpoon:勺子抽象类
SoupSpoon:AbstractSpoon的实现类，汤勺
```
AbstractSpoon类代码
```java
@Data
public abstract class AbstractSpoon implements Cloneable{
    String spoonName;

    @Override
    protected Object clone() {
        try {
            return super.clone();
        } catch (CloneNotSupportedException e) {
            System.err.println("AbstractSpoon is not cloneable.");
        }
        return null;
    }
}
```
SoupSpoon代码
```java
public class SoupSpoon extends AbstractSpoon {
    public SoupSpoon(){
        setSpoonName("Soup Spoon");
    }
}
```
调用Prototype模式很简单:
```java
AbstractSpoon spoon = new SoupSpoon();
AbstractSpoon spoonClone = spoon.clone();
```


