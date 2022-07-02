# Bean 循环依赖

[《Spring Bean IOC、AOP 循环依赖解读》](https://bugstack.cn/md/java/interview/2021-05-05-%E9%9D%A2%E7%BB%8F%E6%89%8B%E5%86%8C%20%C2%B7%20%E7%AC%AC31%E7%AF%87%E3%80%8ASpring%20Bean%20IOC%E3%80%81AOP%20%E5%BE%AA%E7%8E%AF%E4%BE%9D%E8%B5%96%E8%A7%A3%E8%AF%BB%E3%80%8B.html)

setter 注入的循环依赖问题可以通过将类的生成和属性的赋值分离解决。

```java
public class CircleTest {

    private final static Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);

    public static void main(String[] args) throws Exception {
        System.out.println(getBean(B.class).getA());
        System.out.println(getBean(A.class).getB());
    }

    private static <T> T getBean(Class<T> beanClass) throws Exception {
        String beanName = beanClass.getSimpleName().toLowerCase();
        if (singletonObjects.containsKey(beanName)) {
            return (T) singletonObjects.get(beanName);
        }
        // 实例化对象入缓存
        Object obj = beanClass.newInstance();
        singletonObjects.put(beanName, obj);
        // 属性填充补全对象
        Field[] fields = obj.getClass().getDeclaredFields();
        for (Field field : fields) {
            field.setAccessible(true);
            Class<?> fieldClass = field.getType();
            String fieldBeanName = fieldClass.getSimpleName().toLowerCase();
            field.set(obj, singletonObjects.containsKey(fieldBeanName) ? singletonObjects.get(fieldBeanName) : getBean(fieldClass));
            field.setAccessible(false);
        }
        return (T) obj;
    }

}

class A {

    private B b;

    // ...get/set
}

class B {
    private A a;

		// ...get/set
}
```