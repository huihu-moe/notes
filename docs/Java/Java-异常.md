# 异常问题

1. try{} 里有一个 return 语句，那么紧跟在这个 try 后的 finally{} 里的 code 会不会被执行，什么时候被执行，在 return 前还是后?

答案：会执行，在方法返回调用者前执行。

注意：在finally中改变返回值的做法是不好的，因为如果存在finally代码块，try中的return语句不会立马返回调用者，而是记录下返回值待finally代码块执行完毕之后再向调用者返回其值，然后如果在finally中修改了返回值，就会返回修改后的值。显然，在finally中返回或者修改返回值会对程序造成很大的困扰，C#中直接用编译错误的方式来阻止程序员干这种龌龊的事情，Java中也可以通过提升编译器的语法检查级别来产生警告或错误，Eclipse中可以在如图所示的地方进行设置，强烈建议将此项设置为编译错误。

> 仅当return变量是引用数据类型且在finally语句块中修改return变量的成员字段时，才会返回被修改的成员字段。

2. Java语言如何进行异常处理，关键字：throws、throw、try、catch、finally分别如何使用？

答：Java通过面向对象的方法进行异常处理，把各种不同的异常进行分类，并提供了良好的接口。在Java中，每个异常都是一个对象，它是Throwable类或其子类的实例。当一个方法出现异常后便抛出一个异常对象，该对象中包含有异常信息，调用这个对象的方法可以捕获到这个异常并可以对其进行处理。Java的异常处理是通过5个关键词来实现的：try、catch、throw、throws和finally。一般情况下是用try来执行一段程序，如果系统会抛出（throw）一个异常对象，可以通过它的类型来捕获（catch）它，或通过总是执行代码块（finally）来处理；try用来指定一块预防所有异常的程序；catch子句紧跟在try块后面，用来指定你想要捕获的异常的类型；throw语句用来明确地抛出一个异常；throws用来声明一个方法可能抛出的各种异常（当然声明异常时允许无病呻吟）；finally为确保一段代码不管发生什么异常状况都要被执行；try语句可以嵌套，每当遇到一个try语句，异常的结构就会被放入异常栈中，直到所有的try语句都完成。如果下一级的try语句没有对某种异常进行处理，异常栈就会执行出栈操作，直到遇到有处理这种异常的try语句或者最终将异常抛给JVM。

3. 运行时异常与受检异常有何异同？ 

答：异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见运行错误，只要程序设计得没有问题通常就不会发生。受检异常跟程序运行的上下文环境有关，即使程序设计无误，仍然可能因使用的问题而引发。Java编译器要求方法必须声明抛出可能发生的受检异常，但是并不要求必须声明抛出未被捕获的运行时异常。异常和继承一样，是面向对象程序设计中经常被滥用的东西，在Effective Java中对异常的使用给出了以下指导原则： 
- 不要将异常处理用于正常的控制流（设计良好的API不应该强迫它的调用者为了正常的控制流而使用异常） 
- 对可以恢复的情况使用受检异常，对编程错误使用运行时异常 
- 避免不必要的使用受检异常（可以通过一些状态检测手段来避免异常的发生） 
- 优先使用标准的异常 
- 每个方法抛出的异常都要有文档 
- 保持异常的原子性 
- 不要在catch中忽略掉捕获到的异常

4. 列出一些你常见的运行时异常？ 

答： 
- ArithmeticException（算术异常） 
- ClassCastException （类转换异常） 
- IllegalArgumentException （非法参数异常） 
- IndexOutOfBoundsException （下标越界异常） 
- NullPointerException （空指针异常） 
- SecurityException （安全异常）

自定义异常

```java
class MyException extends Exception {
    private int detail;

    MyException(int a) {
        detail = a;
    }

    public String toString() {
        return "MyException ["+ detail + "]";
    }
}

public class TestMyException {

    static void compute(int a) throws MyException {
        System.out.println("Called compute(" + a + ")");
        if(a > 10) {
            throw new MyException(a);
        }
        System.out.println("Normal exit!");
    }

    public static void main(String [] args){
        try {
            compute(1);
            compute(20);
        } catch(MyException me) {
            System.out.println("Caught " + me);
        }
    }
}
```