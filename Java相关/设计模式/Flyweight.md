# 结构模式-享元模式（Flyweight）
享元模式主要用来减少创建对象的数量，以减少内存占用和提高性能。
在有大量对象时，有可能会造成内存溢出，我们把其中共同的部分抽象出来，如果有相同的业务请求，直接返回在内存中已有的对象，避免重新创建。
#### 实现
```
Shape、Circle：圆
ShapeFactory：享元模式类，缓存
```
![](../../resources/flyweight.jpg)
Shape、Circle代码
```java
public interface Shape {
    void draw();
}

public class Circle implements Shape {
    private String color;

    public Circle(String color) {
        this.color = color;
    }

    @Override
    public void draw() {
        System.out.printf("绘制了一个%s的圆。\n", color);
    }
}
```

ShapeFactory代码
```java
public class ShapeFactory {
    private static final Map<String, Circle> CIRCLE_MAP = new HashMap<>(16);

    public static Shape getCircle(String color) {
        Circle circle = CIRCLE_MAP.get(color);
        if (circle == null) {
            circle = new Circle(color);
            CIRCLE_MAP.put(color, circle);
            System.out.printf("创建一个%s的圆。\n", color);
        }
        return circle;
    }
}
```

使用
```java
public class Demo {
    private static final String[] colors = {"红色", "橙色", "黄色", "绿色", "青色", "蓝色", "紫色"};

    public static void main(String[] args) {
        for (int i = 0; i < colors.length; i++) {
            Shape circle = ShapeFactory.getCircle(colors[i]);
            circle.draw();
        }

        for (String color : colors) {
            Shape circle = ShapeFactory.getCircle(color);
            circle.draw();
        }
    }
}
```