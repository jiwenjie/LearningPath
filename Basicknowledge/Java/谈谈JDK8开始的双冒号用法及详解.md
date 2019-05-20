# 谈谈 JDK8 开始的双冒号 :: 用法及详解

新建一个类，里面声明四个代表各种情况的方法：

    public class DoubleColon {
        public static void printStr(String str) {
            System.out.println("printStr : " + str);
    	}
    
        public void toUpper(){
            System.out.println("toUpper : " + this.toString());
        }
    
        public void toLower(String str){
            System.out.println("toLower : " + str);
        }
    
        public int toInt(String str){
            System.out.println("toInt : " + str);
            return 1;
        }
    }

把它们用::提取为函数，再使用：

```
Consumer<String> printStrConsumer = DoubleColon::printStr;
printStrConsumer.accept("printStrConsumer");

Consumer<DoubleColon> toUpperConsumer = DoubleColon::toUpper;
toUpperConsumer.accept(new DoubleColon());

BiConsumer<DoubleColon,String> toLowerConsumer = DoubleColon::toLower;
toLowerConsumer.accept(new DoubleColon(),"toLowerConsumer");

BiFunction<DoubleColon,String,Integer> toIntFunction = DoubleColon::toInt;
int i = toIntFunction.apply(new DoubleColon(),"toInt");
```

非静态方法的第一个参数为被调用的对象，后面是入参。静态方法因为jvm已有对象，直接接收入参。

再写一个方法使用提取出来的函数：

```
public class TestBiConsumer {
    public void test(BiConsumer<DoubleColon,String> consumer){
        System.out.println("do something ...");
    }
}
下面这俩种传入的函数是一样的：

TestBiConsumer obj = new TestBiConsumer();
obj.test((x,y) -> System.out.println("do something ..."));
obj.test(DoubleColon::toLower);

```

**总结
用::提取的函数，最主要的区别在于静态与非静态方法，非静态方法比静态方法多一个参数，就是被调用的实例。**

