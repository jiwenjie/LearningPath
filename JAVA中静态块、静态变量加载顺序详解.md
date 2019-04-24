```
public class test {                         //1.第一步，准备加载类

    public static void main(String[] args) {
        new test();                         //4.第四步，new一个类，但在new之前要处理匿名代码块        
    }

    static int num = 4;                    //2.第二步，静态变量和静态代码块的加载顺序由编写先后决定 

    {
        num += 3;
        System.out.println("b");           //5.第五步，按照顺序加载匿名代码块，代码块中有打印
    }

    int a = 5;                             //6.第六步，按照顺序加载变量

    { // 成员变量第三个
        System.out.println("c");           //7.第七步，按照顺序打印c
    }

    test() { // 类的构造函数，第四个加载
        System.out.println("d");           //8.第八步，最后加载构造函数，完成对象的建立
    }

    static {                              // 3.第三步，静态块，然后执行静态代码块，因为有输出，故打印a
        System.out.println("a");
    }

    static void run()                    // 静态方法，调用的时候才加载// 注意看，e没有加载
    {
        System.out.println("e");
    }
}
```
**一般顺序：静态块（静态变量）——>成员变量——>构造方法——>静态方法 
1、静态代码块（只加载一次） 2、构造方法（创建一个实例就加载一次）3、静态方法需要调用才会执行，所以最后结果没有e**

```
public class Print {
     public Print(String s){
         System.out.print(s + " ");
     }
 }
 
 public class Parent{

     public static Print obj1 = new Print("1");

     public Print obj2 = new Print("2");

     public static Print obj3 = new Print("3");

     static{
         new Print("4");
     }

     public static Print obj4 = new Print("5");

     public Print obj5 = new Print("6");

     public Parent(){
         new Print("7");
     }
 }
 
 public class Child extends Parent{

     static{
         new Print("a");
     }

     public static Print obj1 = new Print("b");

     public Print obj2 = new Print("c");

     public Child (){
         new Print("d");
     }

     public static Print obj3 = new Print("e");

     public Print obj4 = new Print("f");

     public static void main(String [] args){
         Parent obj1 = new Child ();
         Parent obj2 = new Child ();
     }
 }
```

**运行结果：执行main方法，程序输出顺序为： 1 3 4 5 a b e 2 6 7 c f d 2 6 7 c f d **

输出结果表明，程序的执行顺序为： 
如果类还没有被加载： 
1、先执行父类的静态代码块和静态变量初始化，并且静态代码块和静态变量的执行顺序只跟代码中出现的顺序有关。 
2、执行子类的静态代码块和静态变量初始化。 
3、执行父类的实例变量初始化 
4、执行父类的构造函数 
5、执行子类的实例变量初始化 
6、执行子类的构造函数 

如果类已经被加载： 
则静态代码块和静态变量就不用重复执行，再创建类对象时，只执行与实例相关的变量初始化和构造方法。