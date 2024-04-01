# interview-java-Java17新特性-04_Java17新特性_密封类_sealed_class

密封类可以让我们更好的控制哪些类可以对我们定义的类进行拓展，密封类可能对于框架或者中间件的开发者更有用。在这之前，一个类要么是可以被extends的，要么就是被final修饰（不能被继承）。只有这两个选项

密封类可以控制有哪些类可以对超类进行继承，在Java17之前，如果我们需要控制哪些类可以被继承，可以通过改变类的访问级别，比如将类的修饰符public去掉，访问级别变成默认的
protected ，例如下面com.heiz.java11包中定义的三个类

```Java
package com.heiz.java11;

public abstract class Furit {
}

public class Apple extends Furit {
}

public class Pear extends Furit {
}
```

那么我们可以在另一个包com.heiz123.java11中写如下的代码：

```Java

private static void test() {
    Apple apple = new Apple();
    Pear pear = new Pear();
    Fruit fruit = apple;
    class Avocado extends Fruit {};
}

```

既可以定义Apple，Pear，也可以将apple实例赋值给Fruit，并且可以对Fruit进行继承。

如果我们不想让Fruit在com.heiz.java11包以外被扩展，在Java11版本中只能改变访问权限，去掉class的public修饰符。这样虽然可以控制被被继承，但是也会导致Fruit
fruit = apple;也编译失败；在Java 17中通过密封类可以解决这个问题。

```Java
package com.heiz.java17;

public abstract sealed class Furit permits Apple,Pear {
}
public non-sealed class Apple extends Furit {
}
public final class Pear extends Furit {

}

```

在定义Furit时通过关键字sealed声明为密封类，通过permits可以指定Apple,Pear类可以进行继承扩展。

子类需要指明它是final,non-sealed或sealed的。父类不能控制子类是否可以被继承。

```Java
private static void test() {
    Apple apple = new Apple();
    Pear pear = new Pear();
	// 可以将apple赋值给Fruit
    Fruit fruit = apple;
    // 只能继承Apple，不能继承Furit
    class Avocado extends Apple {};
}
```