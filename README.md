# aboutJavaLearningRecord
记录学习java的小笔记<br>

关于类<br>
1. 类中有成员变量和局部变量<br>
    1.1 局部变量：在方法中定义的变量<br>
    1.2 成员变量：分为实例变量和类变量<br>
        1.2.1 实例变量：没有static这顶帽子的变量<br>
        1.2.2 类变量： 通过static定义的变量<br>
        1.2.3 类的一生中，只会有初始化类变量一次，因为一个javaClass对应一个类，而类创建了多少实例，就会有多少实例变量<br>
        1.2.4 在类初始化时，类变量先于实例变量<br>
        1.2.5 变量初始化时，不管右边有没有值，先回将变量初始化为0/null/false，然后再赋值<br>
        1.2.6 一个有趣的例子：<br>
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
                System.out.println("FieldAndMethodTest -b" + b.count);
                b.display();

                DetrivedTest d = new DetrivedTest();
                System.out.println("FieldAndMethodTest -d" + d.count);
                d.display();

                BaseTest moly = new DetrivedTest();
                System.out.println("FieldAndMethodTest -moly" + moly.count);
                moly.display();

                BaseTest molyCopy = moly;
                System.out.println("FieldAndMethodTest -molyCopy" + molyCopy.count);
                molyCopy.display();
            }
        }
        //我是他们的输出
        FieldAndMethodTest -b2
        in BaseTEST2
        FieldAndMethodTest -d220
        in Detrieved220
        FieldAndMethodTest -moly2
        in Detrieved220
        FieldAndMethodTest -molyCopy2
        in Detrieved220
        ```
        看出来了么~~<br>
        第三段： 创建了类为爹的实例，但是初始化走的又是娃，典型的看不懂学不透的多态撒~~<br>
        这时候，moly调用的变量跟了爹，但是moly调用的方法却随了娃，神一般的java啊！<br>
        //我是2017/12/23的分割线，今天被微信小程序的request搞死了，即使在java里面也要吐槽他好么，他喵的竟然没有xmlHttpRequest对象，哼<br>
        //今天写到这儿哈，未完待续<br>



        
