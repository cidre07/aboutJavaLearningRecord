# 构造器
## 构造函数和其参数
### 如果没有创建构造函数，编译器会帮助我们创建构造函数，如果创建了带参的构造函数，构造器**一定**不会帮我们创建不带参的构造函数
#### 构造器一定要有不需要参数的构造函数
#### 构造器可以有带参数的构造函数
##### 都靠重载
来来来，一切靠例子来说明
```java
public class Cat {
    int age;
    public Cat () {
        age = 1;
    }
    public Cat (int catAge) {
        age = catAge;
    } 
}
public class CatTest {
    public static void main (String[] args) {
        Cat helloKitty = new Cat(); // -> age = 1
        Cat babeCatty = new Cat(6); // -> age = 6
    }
}
```

## 构造函数链
### 在创建新对象时，所有继承下来的构造函数都会执行
#### 如果没有显式的调用super()方法，编译器会自动添加
看例子：
```java
public class Animal {
    public Animal () {}
}
public class Cat {
    public Cat () {}
}
public class CatTest {
        public static void main (String[] args) {
            Cat helloKitty = new Cat();  //栈上的调用顺序必然是Object(),Animal(),Cat()
        }
}
```


