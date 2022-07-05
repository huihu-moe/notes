# String 小结

String 被 final 关键字修饰，不可被继承。

java8 源码：

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char[] value[];
}
```

java9 源码：

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```

- Java8 String 使用 char 数组；
- Java9 String 使用 byte 数组，并使用 coder 标识编码，如 latin1，utf16；
- value 数组被声明为 final，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 String 不可变。