# 行为模式-中介者模式（Mediator）
中介者模式用来降低多个对象和类之间的通信的复杂性。这种模式提供了一个中介类，该类通常处理不同类之间的通信，并支持松耦合，是代码易于维护。
#### 实现
我们通过聊天室实例来演示中介者模式。实例中，多个用户可以向聊天室发送消息，聊天室向所有的用户显示消息。我们将创建两个类 ChatRoom 和 User。User 对象使用 ChatRoom 方法来分享他们的消息。
![](../../resources/mediator.jpg)

#### 代码
ChatRoom类
```java
public class ChatRoom {
    public static void showMessage(User user, String message) {
        System.out.printf("%s【%s】：%s\n", new Date().toString(), user.getName(), message);
    }
}

```
User类
```java
@Data
public class User {
    private String name;

    public User(String name) {
        this.name = name;
    }

    public void sendMessage(String message) {
        ChatRoom.showMessage(this, message);
    }
}
```
使用
```java
public class Demo {
    public static void main(String[] args) {
        User robert = new User("Robert");
        User jane = new User("Jane");

        robert.sendMessage("你好，Jane");
        jane.sendMessage("你好，Robert");
    }
}
```
结果
```
Tue Nov 06 15:10:25 CST 2018【Robert】：你好，Jane
Tue Nov 06 15:10:25 CST 2018【Jane】：你好，Robert
```
