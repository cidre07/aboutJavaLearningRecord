# Final关键字
## 加了final的变量就不能变了，加了final的方法就不能被覆盖了， 加了final的类就不能被继承了
来个例子助兴：
   ```
   public class Price {
       static Price INSTANCE = new Price(2.8);
       static double initPrice = 20;
       double currentPrice;
       public Price (double discount) {
          currentPrice = initPrice - discount;
       }
   }
   public class PriceTest {
      public static void main (String[] args) {
           System.out.println(Price.INSTANCE.currentPrice); //=> -2.8
          Price p = new Price(3);
          System.out.println(p.currentPrice);  //=> 17.0
      }
   }
   
   
    public class Price {
      final static Price INSTANCE = new Price(2.8);
      final static double initPrice = 20;
      double currentPrice;
      public Price (double discount) {
          currentPrice = initPrice - discount;
      }
    }
    public class PriceTest {
       public static void main (String[] args) {
           System.out.println(Price.INSTANCE.currentPrice); //=> 17.2
           Price p = new Price(3);
           System.out.println(p.currentPrice);  //=> 17.0
       }
    }

   ```
第一个例子中，虽然Price中传入了参数，但是类的构造器早于变量赋值，此时initPrice的初始值为0
第二个例子中，final关键字使变量在初始化时就被赋值。

## 存储宏变量
final的作用之一是存储宏变量，java会缓存之前用过的字符串直接量（e.g. String a = "read"）,如果再有一个变量声明了一个同样的值（e.g. String b = 'read'），则两个变量相等。
   ```
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
   
但是，java怎么可能这么轻松放过你呢，宏替换必须是个磨人的小妖精啊。
```
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
**只有直接为final赋了初始值才能被宏替换，在非静态代码块和构造器都不算数哦**
