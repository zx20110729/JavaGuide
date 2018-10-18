# 创建模式 单例模式（singleton）
Singleton模式主要作用是保证在Java应用程序中，一个类Class只有一个实例存在。
* 饿汉式,JVM在加载这个类时就马上创建此唯一的单例实例
* 懒汉式,单例实例在第一次被使用时构建，而不是在JVM在加载这个类时就马上创建此唯一的单例实例

1. 饿汉式代码实例
```java
public class Singleton {
    private static Singleton INSTANCE = new Singleton();

    private Singleton() {
    }

    public static Singleton getINSTANCE() {
        return INSTANCE;
    }
}
```
2. 懒汉式代码
```java
public class Singleton {
    private static Singleton INSTANCE;

    private Singleton() {
    }

    public static synchronized Singleton getINSTANCE() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
```
3. 懒汉式代码2（双重加锁版本）
```java
public class Singleton {
    /**
     * volatile保证，当uniqueInstance变量被初始化成Singleton实例时，
     * 多个线程可以正确处理uniqueInstance变量
     */
    private static volatile Singleton INSTANCE;

    private Singleton() {
    }

    public static Singleton getINSTANCE() {
        //检查实例，如果不存在，就进入同步代码块
        if (INSTANCE == null) {
            //只有第一次才彻底执行这里的代码
            synchronized (Singleton.class) {
                //进入同步代码块后，再检查一次，如果仍是null，才创建实例
                if (INSTANCE == null) {
                    INSTANCE = new Singleton();
                }
            }
        }
        return INSTANCE;
    }
}
```
4. 懒汉式代码3（静态内部类版本）
> 静态内部实现的单例是懒加载的且线程安全。
只有通过显式调用 getInstance 方法时，才会显式装载 SingletonHolder 类，从而实例化 instance（只有第一次使用这个单例的实例的时候才加载，同时不会有线程安全问题）。
```java
public class Singleton {
    //静态内部类
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    private Singleton() {
    }

    public static Singleton getINSTANCE() {
        return SingletonHolder.INSTANCE;
    }
}
```

5. 饿汉式代码2（枚举模式）
> 使用枚举实现单例更简洁，自动支持序列化机制，绝对防止多次实例化 （如果单例类实现了Serializable接口，默认情况下每次反序列化总会创建一个新的实例对象
```java
public enum  Singleton {
    INSTANCE;

    public void doSomething(){
        //doSomething
    }
}
```
>枚举单例的使用
```java
Singleton instance = Singleton.INSTANCE;
instance.doSomething();
```
