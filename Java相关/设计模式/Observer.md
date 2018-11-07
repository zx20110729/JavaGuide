# 行为模式-观察者模式（Observer）
当对象间存在一对多的关系时，则使用观察者模式。当一个对象被修改时，则会自动的通知给它的依赖对象。
解决了一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。
#### 实现
观察者模式使用三个类 Subject、Observer 、Client。Subject 对象带有绑定观察者到 Client 对象和从 Client 对象解绑观察者的方法。我们创建 Subject 类、Observer 抽象类和扩展了抽象类 Observer 的实体类。

![](../../resources/observer.jpg)

#### 代码
Observer类
```java
public abstract class Observer {
    protected Subject subject;

    public abstract void update();
}
```
BinaryObserver类
```java
public class BinaryObserver extends Observer {
    public BinaryObserver(Subject subject) {
        this.subject = subject;
        subject.attach(this);
    }

    @Override
    public void update() {
        System.out.printf("二进制：【%s】。\n", Integer.toBinaryString(subject.getState()));
    }
}
```
OctalObserver类
```java
public class OctalObserver extends Observer {
    public OctalObserver(Subject subject) {
        this.subject = subject;
        subject.attach(this);
    }

    @Override
    public void update() {
        System.out.printf("八进制：【%s】。\n", Integer.toOctalString(subject.getState()));
    }
}

```
HexObserver类
```java
public class HexObserver extends Observer {
    public HexObserver(Subject subject) {
        this.subject = subject;
        subject.attach(this);
    }

    @Override
    public void update() {
        System.out.printf("十六进制：【%s】。\n", Integer.toHexString(subject.getState()));
    }
}
```
Subject类
```java
public class Subject {
    private List<Observer> observerList = new ArrayList<>();

    private int state;

    public void setState(int state) {
        this.state = state;
        notifyAllObserver();
    }

    public int getState() {
        return state;
    }

    public void attach(Observer observer) {
        observerList.add(observer);
    }

    public void notifyAllObserver() {
        for (Observer observer : observerList) {
            observer.update();
        }
    }
}

```
实现
```java
public class Demo {
    public static void main(String[] args) {
        Subject subject = new Subject();
        new BinaryObserver(subject);
        new OctalObserver(subject);
        new HexObserver(subject);

        subject.setState(10);
        subject.setState(15);
    }
}
```
结果
```
二进制：【1010】。
八进制：【12】。
十六进制：【a】。
二进制：【1111】。
八进制：【17】。
十六进制：【f】。
```
