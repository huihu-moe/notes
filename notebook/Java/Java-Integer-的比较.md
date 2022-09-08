# Java Integer 的比较

```java
public class Main {
    public static void main(String[] args) {
        Integer a = 1;
        Integer b = 2;
        Integer c = 3;
        Integer d = 3;
        Integer e = 321;
        Integer f = 321;
        Long g = 3L;
        Long h = 2L;
        System.out.println(c == d); // true，比较地址值
        System.out.println(e == f); // false，比较地址值
        System.out.println(c == (a + b)); // true，存在表达式，比较具体值
        System.out.println(c.equals(a + b)); // true，Integer 的 equals 比较具体值
        System.out.println(g == (a + b)); // true，存在表达式，比较具体值
        System.out.println(g.equals(a + b)); // false，Long 的 equals 方法首先判断是否是 Long 类型
        System.out.println(g.equals(a + h)); // true，Long 的 equals 方法首先判断是否是 Long 类型
    }
}

```