# 结构模式-装饰者模式（Decorator）
装饰者模式允许向一个现有对象添加新功能，同时又不修改其结构。动态的给一个类添加一些额外的功能，就增加功能而言，装饰者模式相比生成子类更灵活。
#### 实现
```
Shape：接口
Circle、Rectangle：类
ShapeDecorator：装饰者抽象类
ShapeDecorator：具体装饰者类
```

![](../../resources/decorator.jpg)

Shape、Circle、Rectangle代码：

```java
public interface Shape {
    void draw();
}    

public class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("圆");
    }
}
public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("矩形");
    }
}

```

ShapeDecorator代码

```java
public abstract class ShapeDecorator implements Shape {
    protected Shape decoratedShape;

    public ShapeDecorator(Shape decoratedShape) {
        this.decoratedShape = decoratedShape;
    }

    @Override
    public void draw() {
        decoratedShape.draw();
    }
}
```

RedShapeDecorator代码

```java
public class RedShapeDecorator extends ShapeDecorator {
    public RedShapeDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }

    @Override
    public void draw() {
        super.draw();
        setRedBorder();
    }

    private void setRedBorder() {
        System.out.println("边框：红色");
    }

}
```
