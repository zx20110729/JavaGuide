# 工厂模式

# 1.简单工厂
```
Sample:接口
SampleA:Sample接口实现类A
SampleB:Sample接口实现类B
Factory:Sample的工厂类
```
Factory类代码
```java
public class Factory {
    public static Sample creator(int which) {
        switch (which) {
            case 1:
                return new SampleA();
            case 2:
                return new SampleB();
            default:
                return null;
        }
    }
}
```

# 2.抽象工厂
```
Sample:接口
SampleA:Sample接口实现类A
SampleB:Sample接口实现类B

Sample2:接口
Sample2A:Sample2接口实现类A
Sample2B:Sample2接口实现类B

Factory:Sample、Sample2的抽象工厂类
SimpleFactory:Factory的实现类1
BombFactory:Factory的实现类2
```

Factory类代码
```java
public abstract class Factory {
    public abstract Sample creator();
    public abstract Sample2 creator(String name);
}
```

SimpleFactory类代码
```java
public class SimpleFactory extends Factory {
    @Override
    public Sample creator() {
        return new SampleA();
    }

    @Override
    public Sample2 creator(String name) {
        return new Sample2A();
    }
}
```
BombFactory类代码
```java
public class BombFactory extends Factory {
    @Override
    public Sample creator() {
        return new SampleB();
    }

    @Override
    public Sample2 creator(String name) {
        return new Sample2B();
    }
}

```
抽象工厂还有另外一个关键要点，是因为 SimpleFactory内，生产Sample和生产Sample2的方法之间有一定联系，所以才要将这两个方法捆绑在一个类中，这个工厂类有其本身特征，也许制造过程是统一的，比如：制造工艺比较简单，所以名称叫SimpleFactory。