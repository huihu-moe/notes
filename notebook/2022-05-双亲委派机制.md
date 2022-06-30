# 双亲委派机制

双亲委派机制：将请求委托给父加载器完成，防止内存中存在多份同样的字节码

1. JDK 中的本地方法类一般由根加载器（Bootstrp loader）装载
2. JDK 中内部实现的扩展类一般由扩展加载器（ExtClassLoader ）实现装载
3. 程序中的类文件则由系统加载器（AppClassLoader ）实现装载

## 打破双亲委派机制

打破双亲委派机制指的是加载字节码不遵循双亲委派机制顺序，例如自定义类加载器，重新loadClass方法。

### 打破双亲委派机制的例子

**Tomcat**
1. Bootstrap ClassLoader(Java核心类库)
2. Ext ClassLoader（扩展包类库）
3. APP ClassLoader（当前应用的classpath的所有类）
4. Common ClassLoader（Web应用程序与Tomcat的共享类）
    1. catalina ClassLoader（Tomcat自身的依赖）
    2. Share ClassLoader（web应用程序共享的类）
        1. WebApp ClassLoader（WebApp 独享的类）

