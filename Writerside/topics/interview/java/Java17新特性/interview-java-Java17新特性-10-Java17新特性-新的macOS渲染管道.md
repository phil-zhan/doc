# interview-java-Java17新特性-10_Java17新特性_新的macOS渲染管道

新的macOS渲染管道使用Apple Metal加速渲染API来替代被Apple弃用的OpenGL API。新的macOS渲染管道由JEP 382并在JDK 17中作为正式功能提供。

要使用新的macOS渲染管道，需要在运行Java程序时设置系统属性：

```Java
-Dsun.java2d.metal=true
```

这样，Java 2D API和Swing API用于渲染的Java 2D API就可以使用Metal API来加速渲染。这对于Java程序是透明的，因为这是内部实现的区别，对Java
API没有影响。Metal管道需要macOS 10.14.x或更高版本。在早期版本上设置它的尝试将被忽略。