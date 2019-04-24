#继承 Thread 类本身

```
public class Test {
    public static void main(String[] args) {
        MyThread mt = new MyThread();
        mt.start();
    }
}

class MyThread extends Thread {
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```
#实现Runnable接口
*用法需要在外层包裹一层 Thread *

```
public class Test {
    public static void main(String[] args) {
        MyRunnable mr = new MyRunnable();
        new Thread(mr).start();
    }
}

class MyRunnable implements Runnable {
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}
```

#实现 Callable 接口
比较少见，不常用
需要实现的是 call() 方法

代码拷过来的，确实没用过

```
public class Test {
    public static void main(String[] args) throws Exception {
        ExecutorService es = Executors.newSingleThreadExecutor();

        // 自动在一个新的线程上启动 MyCallable，执行 call 方法
        Future<Integer> f = es.submit(new MyCallable());

        // 当前 main 线程阻塞，直至 future 得到值
        System.out.println(f.get());

        es.shutdown();
    }
}

class MyCallable implements Callable<Integer> {
    public Integer call() {
        System.out.println(Thread.currentThread().getName());

        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        return 123;
    }
}
```






