# 多态
## 基本知识
- 继承使得子类可以拥有父类所有的public方法。换个角度说，通过继承，子类和父类之间会有一些共同的协议，就是这些方法。
- 多态下，引用和定义的对象可以是不同的类型：
    ```
    Animal helloKitty = new Cat()
    ```
- 多态下，引用类型一般是实际对象类型的父类
- 更有趣的是，参数和返回类型也可以多态，还是来个例子吧：
    ```
    class Vet {
        //kill the animal when it makes noise
        public void giveShot(Animal a) {
            a.makeNoise()
        }
    }
    class PetOwner {
        public void start () {
            Vet v = new Vet();
            Dog d = new Dog();
            Bird h = new Bird();
            v.giveShot(d); //执行d的方法
            v.giveShot(h); //执行h的方法
        }
    }

    ```
##覆盖的规则
1. 参数必须一致，且返回类型必须是一样的类型或者该类型的子类
2. 不能降低方法的存取权限，比如，将一个公有的方法变为私有

##在多态的情况下，实例调用的成员变量是父类的值，而实例调用的方法，则是子类的值
            ```
            //我是爹
            public class BaseTest {
                int count = 2;
                public void display() {
                    System.out.println("in BaseTEST" + this.count);
                }
            }
            //我是娃
            public class DetrivedTest extends BaseTest {
                int count = 220;
                @Override
                public void display () {
                    System.out.println("in Detrieved" + this.count);
                }
            }
            //我是专门来测试的
            public class FieldAndMethodTest {
                public static void main (String[] args) {
                    BaseTest b = new BaseTest();
                    System.out.println("FieldAndMethodTest -b" + b.count); ->2
                    b.display(); ->2
    
                    DetrivedTest d = new DetrivedTest();
                    System.out.println("FieldAndMethodTest -d" + d.count); ->220
                    d.display(); ->220
    
                    BaseTest moly = new DetrivedTest();
                    System.out.println("FieldAndMethodTest -moly" + moly.count); ->2
                    moly.display(); ->220
    
                    BaseTest molyCopy = moly;
                    System.out.println("FieldAndMethodTest -molyCopy" + molyCopy.count);  ->2
                    molyCopy.display(); ->220
                }
            }
            ```


##在多态的情况下，实例调用的一个父类没有但是子类有的方法，也是会报错的