# 结构模式-适配器模式（adapter）
>适配器模式是作为两个不兼容的接口之间的桥梁，它结合了两个独立接口的功能。
>
>目的：将一个类的接口转换成客户希望的另外一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
>
>例如，读卡器是作为内存卡和笔记本之间的适配器。

适配器模式可以简单的分为三类：类适配器模式、对象的适配器模式、接口的适配器模式

场景：手机充电需要将220V的交流电转化为手机锂电池需要的5V直流电，我们的demo就是写一个电源适配器，将 AC220v —> DC5V
### 01-类适配器模式

```
AC220:220V交流
DC5:目标，直流5V
PowerAdapter:适配器类
```

AC220类

```java
public class AC220 {
    public int output220V() {
        int output = 220;
        return output;
    }
}
```
DC5接口

```java
public interface DC5 {
    int output5V();
}
```
PowerAdapter类

```java
public class PowerAdapter extends AC220 implements DC5 {
    @Override
    public int output5V() {
        int output = output220V();
        return (output / 44);
    }
}
```
类适配器PowerAdapter使用

```java
DC5 dc5 = new PowerAdapter();
dc5.output5V();
```

### 02-对象适配器模式

```
AC220:220V交流，同上
DC5:目标，直流5V，同上
PowerAdapter:适配器类
```
PowerAdapter类

```java
public class PowerAdapter implements DC5 {
    private AC220 ac220;

    public PowerAdapter(AC220 ac220) {
        this.ac220 = ac220;
    }

    @Override
    public int output5V() {
        int output = ac220.output220V();
        return (output / 44);
    }
}
```
对象适配器PowerAdapter使用

```java
DC5 dc5 = new PowerAdapter(new AC220());
dc5.output5V();
```

### 03-接口适配器模式

```
AC220:220V交流，同上
DCOutput:目标输出，直流电5V、9V、12V、24V
PowerAdapter:适配器抽象类
Power5VAdapter:5V输出适配器类
```

DCOutput接口

``` java
public interface DCOutput {
    int output5V();
    int output9V();
    int output12V();
    int output24V();
}
```

PowerAdapter类

```java
public abstract class PowerAdapter implements DCOutput {
    protected AC220 mAc220;

    public PowerAdapter(AC220 ac220) {
        this.mAc220 = ac220;
    }


    @Override
    public int output5V() {
        return mAc220.output220V();
    }

    @Override
    public int output9V() {
        return mAc220.output220V();
    }

    @Override
    public int output12V() {
        return mAc220.output220V();
    }

    @Override
    public int output24V() {
        return mAc220.output220V();
    }
}
```
Power5VAdapter类

```java
public class Power5VAdapter extends PowerAdapter {
    public Power5VAdapter(AC220 ac220) {
        super(ac220);
    }

    @Override
    public int output5V() {
        int output = 0;
        if (mAc220 != null) {
            output = mAc220.output220V() / 44;
        }
        return output;
    }
}
```

接口适配器使用

```java
            //已经实现了子类
        Power5VAdapter adapter = new Power5VAdapter(new AC220());
        adapter.output5V();

        //直接实现子类
        PowerAdapter powerAdapter = new PowerAdapter(new AC220()) {
            @Override
            public int output5V() {
                return (mAc220.output220V() / 44);
            }
        };
        powerAdapter.output5V();
```

##总结
可以说Source的存在形式决定了适配器的名字，类适配器就是继承Source类，对象适配器就是持有Source类，接口适配器就是实现Source接口。
