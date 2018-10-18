# 创建模式 建造者模式（Builder）

Builder模式定义:
将一个复杂对象的构建与它的表示分离,使得同样的构建过程可以创建不同的表示.

Builder模式是一步一步创建一个复杂的对象,它允许用户可以只通过指定复杂对象的类型和内容就可以构建它们.用户不知道内部的具体构建细节.Builder模式是非常类似抽象工厂模式,细微的区别大概只有在反复使用中才能体会到.

为何使用?
是为了将构建复杂对象的过程和它的部件解耦.注意: 是解耦过程和部件.

因为一个复杂的对象,不但有很多大量组成部分,如汽车,有很多部件:车轮 方向盘 发动机还有各种小零件等等,部件很多,但远不止这些,如何将这些部件装配成一辆汽车,这个装配过程也很复杂(需要很好的组装技术),Builder模式就是为了将部件和组装过程分开.

```
Builder:创建者接口
ConcreteBuilder:创建者接口的具体实现类
Director:构建最终的对象，Builder只是分布创建部件
Part:复杂对象的部件接口
Product:复杂对象
```

Builder接口代码
```java
public interface Builder {
    /**
     * 创建部件A
     */
    void buildPartA();

    /**
     * 创建部件B
     */
    void buildPartB();

    /**
     * 创建部件C
     */
    void buildPartC();

    /**
     * 获取完整部件
     *
     * @return
     */
    Product getResult();
}
```

ConcreteBuilder类代码
```java
public class ConCreteBuilder implements Builder {
    Part partA, partB, partC;

    @Override
    public void buildPartA() {
        //部件A具体实现代码
    }

    @Override
    public void buildPartB() {
        //部件B具体实现代码
    }

    @Override
    public void buildPartC() {
        //部件C具体实现代码
    }

    @Override
    public Product getResult() {
        //最终组装结果 Product
        return null;
    }
}

```
Director类代码
```java
public class Director {
    private Builder builder;

    public Director(Builder builder) {
        this.builder = builder;
    }

    /**
     * 构建部件A、B、C
     */
    public void construct() {
        builder.buildPartA();
        builder.buildPartB();
        builder.buildPartC();
    }
}
```
Part、Product接口代码
```java
public interface Part {
}

public interface Product {
}
```
Builder模式的调用
```java
        Builder builder = new ConCreteBuilder();
        Director director = new Director(builder);
        director.construct();
        Product product = builder.getResult();
```

## Builder模式的应用

在Java实际使用中,我们经常用到"池"(Pool)的概念,当资源提供者无法提供足够的资源,并且这些资源需要被很多用户反复共享时,就需要使用池.

"池"实际是一段内存,当池中有一些复杂的资源的"断肢"(比如数据库的连接池,也许有时一个连接会中断),如果循环再利用这些"断肢",将提高内存使用效率,提高池的性能.修改Builder模式中Director类使之能诊断"断肢"断在哪个部件上,再修复这个部件.



