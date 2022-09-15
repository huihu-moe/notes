# 反射使用例子

[深入解析Java反射（1） - 基础 ](https://www.sczyh30.com/posts/Java/java-reflection-1/)

1. 获得 Class 对象

方法有三种：

(1) 使用 Class 类的 forName 静态方法:

```java
public static Class<?> forName(String className)

// 比如在 JDBC 开发中常用此方法加载数据库驱动:
Class.forName(driver);
 ```
 
(2) 直接获取某一个对象的 class，比如:

```java
Class<?> klass = int.class;
Class<?> classInt = Integer.TYPE;
```

(3) 调用某个对象的 getClass() 方法，比如:

```java
StringBuilder str = new StringBuilder("123");
Class<?> klass = str.getClass();
```

2. 判断是否为某个类的实例

```java
public native boolean isInstance(Object obj);
```

3. 创建实例

- 使用Class对象的newInstance()方法来创建Class对象对应类的实例。

```java
Class<?> c = String.class;
Object str = c.newInstance();
```


- 先通过Class对象获取指定的Constructor对象，再调用Constructor对象的newInstance()方法来创建实例。这种方法可以用指定的构造器构造类的实例。

```java
//获取String所对应的Class对象
Class<?> c = String.class;
//获取String类带一个String参数的构造器
Constructor constructor = c.getConstructor(String.class);
//根据构造器创建实例
Object obj = constructor.newInstance("23333");
System.out.println(obj);
```

4. 获取方法

- getDeclaredMethods 方法返回类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。

```java
public Method[] getDeclaredMethods() throws SecurityException
```

- getMethods 方法返回某个类的所有公用（public）方法，包括其继承类的公用方法。

```java
public Method[] getMethods() throws SecurityException
```

- getMethod 方法返回一个特定的方法，其中第一个参数为方法名称，后面的参数为方法的参数对应Class的对象。

```java
public Method getMethod(String name, Class<?>... parameterTypes)
```

```java
package org.ScZyhSoft.common;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;
public class test1 {
	public static void test() throws IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
	        Class<?> c = methodClass.class;
	        Object object = c.newInstance();
	        Method[] methods = c.getMethods();
	        Method[] declaredMethods = c.getDeclaredMethods();
	        //获取methodClass类的add方法
	        Method method = c.getMethod("add", int.class, int.class);
	        //getMethods()方法获取的所有方法
	        System.out.println("getMethods获取的方法：");
	        for(Method m:methods)
	            System.out.println(m);
	        //getDeclaredMethods()方法获取的所有方法
	        System.out.println("getDeclaredMethods获取的方法：");
	        for(Method m:declaredMethods)
	            System.out.println(m);
	    }
    }
class methodClass {
    public final int fuck = 3;
    public int add(int a,int b) {
        return a+b;
    }
    public int sub(int a,int b) {
        return a+b;
    }
}
```

5. 获取构造器信息

获取类构造器的用法与上述获取方法的用法类似。主要是通过Class类的getConstructor方法得到Constructor类的一个实例，而Constructor类有一个newInstance方法可以创建一个对象实例:

```java
public T newInstance(Object ... initargs)
```

6. 获取类的成员变量（字段）信息

- getFiled：访问公有的成员变量
- getDeclaredField：所有已声明的成员变量，但不能得到其父类的成员变量
- getFileds 和 getDeclaredFields 方法用法同上（参照 Method）

7. 调用方法

当我们从类中获取了一个方法后，我们就可以用 invoke() 方法来调用这个方法。invoke 方法的原型为:

```java
public Object invoke(Object obj, Object... args)
        throws IllegalAccessException, IllegalArgumentException,
           InvocationTargetException
```

invoke 使用例子

```java
public class test1 {
    public static void main(String[] args) throws IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException {
        Class<?> klass = methodClass.class;
        //创建methodClass的实例
        Object obj = klass.newInstance();
        //获取methodClass类的add方法
        Method method = klass.getMethod("add",int.class,int.class);
        //调用method对应的方法 => add(1,4)
        Object result = method.invoke(obj,1,4);
        System.out.println(result);
    }
}
class methodClass {
    public final int fuck = 3;
    public int add(int a,int b) {
        return a+b;
    }
    public int sub(int a,int b) {
        return a+b;
    }
}
```

8. 通过反射创建数组

```java
public static void testArray() throws ClassNotFoundException {
    Class<?> cls = Class.forName("java.lang.String");
    Object array = Array.newInstance(cls,25);
    //往数组里添加内容
    Array.set(array,0,"hello");
    Array.set(array,1,"Java");
    Array.set(array,2,"fuck");
    Array.set(array,3,"Scala");
    Array.set(array,4,"Clojure");
    //获取某一项的内容
    System.out.println(Array.get(array,3));
}
```

其中的Array类为java.lang.reflect.Array类。我们通过Array.newInstance()创建数组对象，它的原型是:

```java
public static Object newInstance(Class<?> componentType, int length)
        throws NegativeArraySizeException {
        return newArray(componentType, length);
    }
```
