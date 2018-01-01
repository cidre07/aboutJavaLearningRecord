# 接口
多重继承会有一个致命的问题：如果有一个dog类，他同时继承了犬科类和食肉类，这两个父类中同时存在bark方法，此时，新建一个dog实例，调用bark方法，此时，究竟是调用犬科类的bark方法还是食肉类的bark方法呢？这是一个致命方块问题。
解决多重继承的方法就是接口。

## 关于接口
- 接口中的方法都是抽象的，看个例子：
```java
public interface Pet {
    public abstract void beFriendly (); //注意不需要花括号，且必须定义为abstract
    public abstract void play();
}
public class MeatEater {}
public class Canine {}
public class Dog extends Canine implements Pet {
    public void beFriendly () {} //必须在此处出现Pet的具体方法，必须！！！
    public void play() {};
}
```
interface中必须将方法定义为abstract，同时不需要花括号，而实现接口的类中必须要出现这些方法的具体定义！！！
