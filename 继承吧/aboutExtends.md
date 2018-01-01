# 继承
## super关键字
1. 子类在集成父类后，会重写父类的方法（Override）
2. 子类在集成父类后，如果还是想调用父类的那个方法，可以用super关键字
```java
public abstract class Report {
    void runReport () {}
    void printReport () {}
}
public class GovernmentReport extends Report {
    @Override 
    void runReport() {
        super.runReport();
        readReport();
        printReport();
    }
    void readReport() {}
}
```