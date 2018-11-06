# 行为模式-迭代器模式（Iterator）
迭代器模式用于顺序访问集合对象的元素，不需要知道集合对象的底层表示。

#### 实现
我们将创建一个叙述导航方法的 Iterator 接口和一个返回迭代器的 Container 接口。实现了 Container 接口的实体类将负责实现 Iterator 接口。
![](../../resources/iterator.jpg)

#### 代码
Iterator接口
```java
public interface Iterator {
    boolean hasNext();
    Object next();
}
```
Container接口
```java
public interface Container {
    Iterator getIterator();
}
```
NameRepository类
```java
public class NameRepository implements Container {
    private String[] names = {"Robert", "John", "Julie", "Lora"};

    @Override
    public Iterator getIterator() {
        return new NameIterator();
    }

    private class NameIterator implements Iterator {
        int index;

        @Override
        public boolean hasNext() {
            if (index < names.length) {
                return true;
            }
            return false;
        }

        @Override
        public Object next() {
            if (this.hasNext()) {
                return names[index++];
            }
            return null;
        }
    }
}
```
使用
```java
public class Demo {
    public static void main(String[] args) {
        Container container = new NameRepository();
        Iterator iterator = container.getIterator();
        while (iterator.hasNext()) {
            String next = (String) iterator.next();
            System.out.printf("名称：【%s】。\n", next);
        }
    }
}
```
结果
```
名称：【Robert】。
名称：【John】。
名称：【Julie】。
名称：【Lora】。
```


