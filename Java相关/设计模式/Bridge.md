#结构模式 桥接模式（Bridge）
桥接模式:将抽象部分与其实例部分分开，通过提供抽象化和实例化之间的桥接模式将抽象化和实例化解耦

意图：将抽象部分与实现部分分离，使它们都可以独立的变化。

主要解决：在有多种可能会变化的情况下，用继承会造成类爆炸问题，扩展起来不灵活。

>需求：三种形状，原型、矩形、三角形；三种颜色：红色、黄色 蓝色，需要实现各种颜色和形状的组合方案
>
>方案一：为每种形状都提供各种颜色的版本。如果组合少较方便
>
方案二：根据实际需要对颜色和形状进行组合。

此处仅介绍第二种情况

```
Shape:形状抽象类
Circle:圆
Rectangle:矩形
triangle:三角形
Color:颜色接口
Red:红色
Blue:蓝色
Yellow:黄色
```
Shape接口及实现类代码

```java
public abstract class Shape {
    protected Color mColor;
    protected String mShapeName;

    public Shape(Color color) {
        this.mColor = color;
    }

    public abstract void draw();
}

public class Actangle extends Shape {
    public Actangle(Color color) {
        super(color);
        this.mShapeName = "三角形";
    }

    @Override
    public void draw() {
        mColor.bepaint(this.mShapeName);
    }
}

public class Circle extends Shape {
    public Circle(Color color) {
        super(color);
        this.mShapeName = "圆形";
    }

    @Override
    public void draw() {
        mColor.bepaint(this.mShapeName);
    }
}

public class Rectangle extends Shape {
    public Rectangle(Color color) {
        super(color);
        this.mShapeName = "矩形";
    }

    @Override
    public void draw() {
        mColor.bepaint(this.mShapeName);
    }
}
```

Color接口及其实现类代码

```java
public interface Color {
    void bepaint(String shape);
}

public class Blue implements Color {
    private String colorName = "蓝色的";

    @Override
    public void bepaint(String shape) {
        System.out.println(colorName + shape);
    }
}

public class Red implements Color {
    private String colorName = "红色的";

    @Override
    public void bepaint(String shape) {
        System.out.println(colorName + shape);
    }
}

public class Yellow implements Color {
    private String colorName = "黄色的";

    @Override
    public void bepaint(String shape) {
        System.out.println(colorName + shape);
    }
}
```

使用

```java
public class Demo {
    public static void main(String[] args) {
        Color red = new Red();
        Shape circle = new Circle(red);
        circle.draw();
    }
}
```

### 桥接模式优缺点
####优点
1. 分离抽象接口及其实现部分。提高了比继承更好的解决方案。
2. 桥接模式提高了系统的可扩充性，在两个变化维度中任意扩展一个维度，都不需要修改原有系统。
3. 实现细节对客户透明，可以对用户隐藏实现细节。

####缺点
1. 桥接模式的引入会增加系统的理解与设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象进行设计与编程。 
2. 桥接模式要求正确识别出系统中两个独立变化的维度，因此其使用范围具有一定的局限性。


