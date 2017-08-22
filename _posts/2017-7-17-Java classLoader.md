http://www.cnblogs.com/mailingfeng/archive/2012/07/02/2573419.html
bootStrap ClassLoader:
一般显示null
bootstrap classloader －引导（也称为原始）类加载器，它负责加载Java的核心类。
在Sun的JVM中，
在执行java的命令中使用-Xbootclasspath选项或使用 - D选项指定sun.boot.class.path系统属性值可以指定附加的类。这个加载器的是非常特殊的，
它实际上不是 java.lang.ClassLoader的子类，而是由JVM自身实现的。大家可以通过执行以下代码来获得bootstrap classloader加载了那些核心类库：
URL[] urls=sun.misc.Launcher.getBootstrapClassPath().getURLs();
    for (int i = 0; i < urls.length; i++) {
      System.out.println(urls[i].toExternalForm());
    }
extension classloader －扩展类加载器，它负责加载JRE的扩展目录（JAVA_HOME/jre/lib/ext或者由java.ext.dirs系统属性指定的）中JAR的类包。
这为引入除Java核心类以外的新功能提供了一个标准机制。因为默认的扩展目录对所有从同一个JRE中启动的JVM都是通用的，
所以放入这个目录的 JAR类包对所有的JVM和system classloader都是可见的。在这个实例上调用方法getParent()总是返回空值null，
因为引导加载器bootstrap classloader不是一个真正的ClassLoader实例。所以当大家执行以下代码时：
```java
System.out.println(System.getProperty("java.ext.dirs"));
    ClassLoader extensionClassloader=ClassLoader.getSystemClassLoader().getParent();
    System.out.println("the parent of extension classloader : "+extensionClassloader.getParent());
```
 结果为：
 C:\j2sdk1.4.1_01\jre\lib\ext
 the parent of extension classloader : null
 extension classloader是system classloader的parent，而bootstrap classloader是extension classloader的parent，但它不是一个实际的classloader，
所以为null。

 system classloader －系统（也称为应用）类加载器，它负责在JVM被启动时，加载来自在命令java中的-classpath或者java.class.path系统属性
或者 CLASSPATH*作系统属性所指定的JAR类包和类路径。总能通过静态方法ClassLoader.getSystemClassLoader()找到该类加载器。
sun.misc.Launcher$AppClassLoader@456d3d51
如果没有特别指定，则用户自定义的任何类加载器都将该类加载器作为它的父加载器。执行以下代码即可获得：
    System.out.println(System.getProperty("java.class.path"));
 输出结果则为用户在系统属性里面设置的CLASSPATH。
 classloader 加载类用的是全盘负责委托机制。所谓全盘负责，即是当一个classloader加载一个Class的时候，
这个Class所依赖的和引用的所有 Class也由这个classloader负责载入，除非是显式的使用另外一个classloader载入；
委托机制则是先让parent（父）类加载器 (而不是super，它与parent classloader类不是继承关系)寻找，只有在parent找不到的时候才从自己的类路径中去寻找。
此外类加载还采用了cache机制，也就是如果 cache中保存了这个Class就直接返回它，如果没有才从文件中读取和转换成Class，并存入cache，
这就是为什么我们修改了Class但是必须重新启动JVM才能生效的原因。



 类加载器的顺序是：
 先是bootstrap classloader，然后是extension classloader，最后才是system classloader。大家会发现加载的Class越是重要的越在靠前面。
这样做的原因是出于安全性的考虑，试想如果system classloader“亲自”加载了一个具有破坏性的“java.lang.System”类的后果吧。
这种委托机制保证了用户即使具有一个这样的类，也把它加入到了类路径中，但是它永远不会被载入，因为这个类总是由bootstrap classloader来加载的。
大家可以执行一下以下的代码：
    System.out.println(System.class.getClassLoader());
 将会看到结果是null，这就表明java.lang.System是由bootstrap classloader加载的，因为bootstrap classloader不是一个真正的ClassLoader实例，
而是由JVM实现的，正如前面已经说过的。
 
 下面就让我们来看看JVM是如何来为我们来建立类加载器的结构的：
 sun.misc.Launcher，顾名思义，当你执行java命令的时候，JVM会先使用bootstrap classloader载入并初始化一个Launcher，执行下来代码：
   System.out.println("the Launcher's classloader is "+sun.misc.Launcher.getLauncher().getClass().getClassLoader());
 结果为：
   the Launcher's classloader is null (因为是用bootstrap classloader加载,所以class loader为null)
 Launcher 会根据系统和命令设定初始化好class loader结构，JVM就用它来获得extension classloader和system classloader,并载入所有的需要载入的Class，
最后执行java命令指定的带有静态的main方法的Class。extension classloader实际上是sun.misc.Launcher$ExtClassLoader类的一个实例，
system classloader实际上是sun.misc.Launcher$AppClassLoader类的一个实例。并且都是 java.net.URLClassLoader的子类。
 
