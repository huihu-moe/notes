# Java 拷贝

clone()方法并不是Cloneable接口的方法，而是Object的一个protected方法。Cloneable接口是一个规定，如果一个类没有实现Cloneable接口又调用了clone()方法，就会抛出CloneNotSupportedException。

### 浅拷贝

```java
public class ShallowCloneExample implements Cloneable {
    private int[] arr;

    public ShallowCloneExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }

    @Override
    protected ShallowCloneExample clone() throws CloneNotSupportedException {
        return (ShallowCloneExample) super.clone(); // 获得拷贝对象，与原始对象中的引用类型引用同一个对象
    }

    public static void main(String[] args) {
        ShallowCloneExample e1 = new ShallowCloneExample();
        ShallowCloneExample e2 = null;
        try {
            e2 = e1.clone(); // 浅拷贝，拷贝对象和原始对象中的引用类型（例如数组arr）引用同一个对象
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        e1.set(2, 222);
        System.out.println(e2.get(2)); // 222
    }
}
```

### 深拷贝

```java
public class DeepCloneExample implements Cloneable {
    private int[] arr;

    public DeepCloneExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }

    @Override
    protected DeepCloneExample clone() throws CloneNotSupportedException {
        DeepCloneExample result = (DeepCloneExample) super.clone(); // 获得拷贝对象，与原始对象中的引用类型引用同一个对象
        result.arr = new int[arr.length]; // 修改拷贝对象中的引用类型，使其引用不同对象
        for (int i = 0; i < arr.length; i++) {
            result.arr[i] = arr[i];
        }
        return result;
    }

    public static void main(String[] args) {
        DeepCloneExample e1 = new DeepCloneExample();
        DeepCloneExample e2 = null;
        try {
            e2 = e1.clone(); // 深拷贝，拷贝对象和原始对象中的引用类型（例如数组arr）引用不同对象
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        e1.set(2, 222);
        System.out.println(e2.get(2)); // 2
    }
}
```

### 使用构造函数实现对象拷贝

```java
public class CloneConstructorExample {
    private int[] arr;

    public CloneConstructorExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public CloneConstructorExample(CloneConstructorExample original) {
        arr = new int[original.arr.length];
        for (int i = 0; i < original.arr.length; i++) {
            arr[i] = original.arr[i];
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }

    public static void main(String[] args) {
        CloneConstructorExample e1 = new CloneConstructorExample();
        CloneConstructorExample e2 = new CloneConstructorExample(e1);
        e1.set(2, 222);
        System.out.println(e2.get(2)); // 2
    }
}
```
