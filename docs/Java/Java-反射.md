# Java 反射

## 获取Class类对象

```java
    @Test
    public void classTest() throws Exception {
        // 获取Class对象的三种方式
        logger.info("根据类名:  \t" + User.class);
        logger.info("根据对象:  \t" + new User().getClass());
        logger.info("根据全限定类名:\t" + Class.forName("com.test.User"));
        // 常用的方法
        logger.info("获取全限定类名:\t" + userClass.getName());
        logger.info("获取类名:\t" + userClass.getSimpleName());
        logger.info("实例化:\t" + userClass.newInstance());
    }
```

## 获取Construct类对象

| 方法返回值                 | 方法名称                                           | 方法说明                                                  |
|----------------------------|----------------------------------------------------|-----------------------------------------------------------|
| static Class<?>            | forName(String className)                          | 返回与带有给定字符串名的类或接口相关联的 Class 对象       |
| Constructor                | getConstructor(Class<?>... parameterTypes)         | 返回指定参数类型、具有public访问权限的构造函数对象        |
| Constructor<?>[]           | getConstructors()                                  | 返回所有具有public访问权限的构造函数的Constructor对象数组 |
| Constructor                | getDeclaredConstructor(Class<?>... parameterTypes) | 返回指定参数类型、所有声明的（包括private）构造函数对象   |
| Constructor<?>[]           | getDeclaredConstructors()                          | 返回所有声明的（包括private）构造函数对象                 |
| T                          | newInstance()                                      | 调用无参构造器创建此 Class 对象所表示的类的一个新实例     |

```java
Class<?> clazz = null;

//获取Class对象的引用
clazz = Class.forName("com.example.javabase.User");

//第一种方法，实例化默认构造方法，User必须无参构造函数,否则将抛异常
User user = (User) clazz.newInstance();
user.setAge(20);
user.setName("Jack");
System.out.println(user);
```

## 获取Field对象

| 方法返回值 | 方法名称                      | 方法说明                                                                     |
|------------|-------------------------------|------------------------------------------------------------------------------|
| Field      | getDeclaredField(String name) | 获取指定name名称的(包含private修饰的)字段，不包括继承的字段                  |
| Field[]    | getDeclaredFields()           | 获取Class对象所表示的类或接口的所有(包含private修饰的)字段，不包括继承的字段 |
| Field      | getField(String name)         | 获取指定name名称、具有public修饰的字段，包含继承字段                         |
| Field[]    | getFields()                   | 获取修饰符为public的字段，包含继承字段                                       |

```java
Class<?> clazz = Class.forName("reflect.Student");
//获取指定字段名称的Field类,注意字段修饰符必须为public而且存在该字段,
// 否则抛NoSuchFieldException
Field field = clazz.getField("age");
System.out.println("field:"+field);
```

## 获取Method对象

| 方法返回值 | 方法名称                                                  |           方法说明                                                                                                                                                      |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Method     | getDeclaredMethod(String name Class<?>... parameterTypes) |   返回一个指定参数的Method对象，该对象反映此 Class 对象所表示的类或接口的指定已声明方法。                                                                               |
| Method[]   | getDeclaredMethods()                                      |   返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。                     |
| Method     | getMethod(String name Class<?>... parameterTypes)         |   返回一个 Method 对象，它反映此 Class 对象所表示的类或接口的指定公共成员方法。                                                                                         |
| Method[]   | getMethods()                                              |   返回一个包含某些 Method 对象的数组，这些对象反映此 Class 对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共 member 方法。 |

```java
Class clazz = Class.forName("reflect.Circle");

//根据参数获取public的Method,包含继承自父类的方法
Method method = clazz.getMethod("draw",int.class,String.class);

System.out.println("method:"+method);
```