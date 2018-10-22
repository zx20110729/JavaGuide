# 结构模式 代理模式（Proxy）

>从出发点到目的地有一道中间层，即为代理
>
设计模式中定义: 为其他对象提供一种代理以控制对这个对象的访问.

我们将创建一个 Image 接口和实现了 Image 接口的实体类。ProxyImage 是一个代理类，减少 RealImage 对象加载的内存占用。
![](../../resources/proxy.jpg)

Image接口代码

```java
public interface Image {
    void display();
}
```
RealImage类代码

```java
public class RealImage implements Image {
    private String fileName;

    public RealImage(String fileName) {
        this.fileName = fileName;
        loadImage(fileName);
    }

    @Override
    public void display() {
        System.out.println("Displaying " + fileName);
    }

    private void loadImage(String fileName) {
        System.out.println("Loading " + fileName);
    }
}

```
ProxyImage类代码

```java
public class ProxyImage implements Image {
    private RealImage realImage;
    private String fileName;

    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public void display() {
        if (realImage == null) {
            realImage = new RealImage(fileName);
        }
        realImage.display();
    }
}

```
使用示例

```java
public class Demo {
    public static void main(String[] args) {
       Image image =  new ProxyImage("test.jpg");
       image.display();
    }
}
```
