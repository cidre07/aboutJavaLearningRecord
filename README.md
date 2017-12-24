# aboutJavaLearningRecord
记录学习java的小笔记<br>

关于类<br>
·1. 类中有成员变量和局部变量<br>
    ·1.1 局部变量：在方法中定义的变量
    ·1.2 成员变量：分为实例变量和类变量
       ··· 1.2.1 实例变量：没有static这顶帽子的变量
       ··· 1.2.2 类变量： 通过static定义的变量
        ···1.2.3 类的一生中，只会有初始化类变量一次，因为一个javaClass对应一个类，而类创建了多少实例，就会有多少实例变量
       ··· 1.2.4 在类初始化时，类变量先于实例变量
       ··· 1.2.5 变量初始化时，不管右边有没有值，先回将变量初始化为0/null/false，然后再赋值
       ··· 1.2.6 一个有趣的例子
       
        ```java
       //我是爹
        public class BaseTest {
            int count = 2;
            public void display() {
                System.out.println("in BaseTEST" + this.count);
            }
        }
        ```

       ```java
        //我是娃
        public class DetrivedTest extends BaseTest {
            int count = 220;
            @Override
            public void display () {
                System.out.println("in Detrieved" + this.count);
            }
        }
        ```
         ```java
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
        ```
         ```java
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
        
        
        看出来了么~~
        第三段： 创建了类为爹的实例，但是初始化走的又是娃，典型的看不懂学不透的多态撒~~
        这时候，moly调用的变量跟了爹，但是moly调用的方法却随了娃，神一般的java啊！<br>
        //我是2017/12/23的分割线，今天被微信小程序的request搞死了，即使在java里面也要吐槽他好么，他喵的竟然没有xmlHttpRequest对象，哼<br>
        //今天写到这儿哈，未完待续<br>

       ··· 1.2.7 紧接着一个有趣的例子 e.g. Parent moly = new Child(); 这个moly是没办法访问到子类的方法的！！

       ·· 1.3 final 加了final的变量就不能变了，加了final的方法就不能被覆盖了， 加了final的类就不能被继承了
       ···1.3.1 来两段代码对比下撒：
       ```java
       public class Price {
           static Price INSTANCE = new Price(2.8);
           static double initPrice = 20;
          double currentPrice;
          public Price (double discount) {
              currentPrice = initPrice - discount;
          }
      }
       ```
       ```java
       public class PriceTest {
           public static void main (String[] args) {
               System.out.println(Price.INSTANCE.currentPrice); //=> -2.8

               Price p = new Price(3);
               System.out.println(p.currentPrice);  //=> 17.0
           }
       }
       ```
       ```java
       public class Price {
           final static Price INSTANCE = new Price(2.8);
           final static double initPrice = 20;
           double currentPrice;
           public Price (double discount) {
               currentPrice = initPrice - discount;
           }
       }
       ```
       ```java
       public class PriceTest {
           public static void main (String[] args) {
               System.out.println(Price.INSTANCE.currentPrice); //=> 17.2

               Price p = new Price(3);
               System.out.println(p.currentPrice);  //=> 17.0
           }
       }
       ```
       第一个例子中，虽然Price中传入了参数，但是类的构造器早于变量赋值，此时initPrice的初始值为0；
       第二个例子中，final关键字使变量在初始化时就被赋值。
       
       ···1.3.2 final的作用之一是存储宏变量，java会缓存之前用过的字符串直接量（e.g. String a = "read"）,如果再有一个变量声明了一个同样的值（e.g. String b = 'read'），则两个变量相等。
       下面是有趣的例子：
       
       ```java
       public class EqualTest {
           public static void main (String[] args) {
               String s1 = "read book";
               String s2 = "read " + "book";
               System.out.println(s1 == s2 );  //=> true
               
               String st1 = "read ";
               String st2 = "book";
               String s3 = st1 + st2;
               System.out.println(s1 == s3 );  //=> false 因为st1和st2都属于普通变量，编译器不会执行宏替换
           
               final String stg1 = "read ";
               final String stg2 = "book";
               String s3 = stg1 + stg2;
               System.out.println(s1 == s3 );  //=> true
           }
       }
       
       ```
       
       ···1.3.3 宏替换也很是个磨人的小妖精：
       看下面的例子：
       ```java
       public class FinalInitTest {
          final String st1;
          final String st2;
          final String st3 = "book";

          {
              st1 = "book";
          }
          public FinalInitTest () {
              st2 = "book";
          }

          public void display () {
              System.out.println(st1 + st1 == "bookbook"); //false
              System.out.println(st2 + st2 == "bookbook"); //false
              System.out.println(st3 + st3 == "bookbook"); //true
          }

          public static void main (String[] args) {
              FinalInitTest fit = new FinalInitTest();
              fit.display();
          }
      }

       ```
       只有直接为final赋了初始值才能被宏替换，在非静态代码块和构造器都不算数哦 

 



        
