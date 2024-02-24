# interview-java-Java17新特性-02_Java17新特性_switch表达式

Java17版本中的switch表达式将允许switch有返回值，并可以直接作为结果赋值给一个变量，等等一系列的变化。下面有一个switch例子，依赖于给定的枚举值，执行case操作，故意忽略break。

```Java
private static void lowVesion(Fruit fruit) {
    switch (fruit) {
        case APPLE, PEAR:
            System.out.println("普通水果");
        case MANGO, AVOCADO:
            System.out.println("进口水果");
        default:
            System.out.println("未知水果");
    }
}
```

我们调用这个方法传入一个APPLE，会输出以下结果：

```Text
普通水果
进口水果
未知水果
```

显然这不是期望的结果，因为我们需要在每个case里添加break防止所有的case都没执行。这里注意，switch会在第一次匹配后，依次执行后面所有的case代码块，直到遇到break才会跳出switch，调整如下：

```Java
private static void lowVesion(Fruit fruit) {
    switch (fruit) {
        case APPLE, PEAR:
            System.out.println("普通水果");
            break;
        case MANGO, AVOCADO:
            System.out.println("进口水果");
			break;
        default:
            System.out.println("未知水果");
    }
}
```

升级到Java17，可以通过switch表达式来进行简化，将冒号（:）替换为箭头（->），并且switch表达式默认带了break，不需要再手动添加

```Java
private static void withSwitchExpression(Fruit fruit) {
    switch (fruit) {
        case APPLE, PEAR -> System.out.println("普通水果");
        case MANGO, AVOCADO -> System.out.println("进口水果");
        default -> System.out.println("未知水果");
    }
}
```

switch表达式也可以返回一个值，比如上面的例子中，我们可以让switch返回一个字符串来表示我们要打印的文本；需要注意，有返回值时，switch的最后需要加一个分号。

```Java
private static void withReturnValue(Fruit fruit) {
    String text = switch (fruit) {
        case APPLE, PEAR -> "普通水果";
        case MANGO, AVOCADO -> "进口水果";
        default -> "未知水果";
    };
    System.out.println(text);
}
```

也可以直接省略赋值动作，直接打印

```Java
private static void withReturnValue(Fruit fruit) {
    System.out.println(switch (fruit) {
        case APPLE, PEAR -> "普通水果";
        case MANGO, AVOCADO -> "进口水果";
        default -> "未知水果";
    });
}
```

如果你想在case里面做不止一件事，比如在返回之前先做一些打印或计算操作，可以通过大括号扩起来作为case块，最后的返回值使用关键字yield进行返回。

```Java
private static void withYield(Fruit fruit) {
    String text = switch (fruit) {
        case APPLE, PEAR -> {
            System.out.println("给的水果是: " + fruit);
            yield "普通水果";
        }
        case MANGO, AVOCADO -> "进口水果";
        default -> "未知水果";
    };
    System.out.println(text);
}
```

这个输出结果是：

```Text
给的水果是: APPLE
普通水果
```

当然也可以直接使用yield返回结果。

```Java
private static void oldStyleWithYield(Fruit fruit) {
    System.out.println(switch (fruit) {
        case APPLE, PEAR:
            yield "普通水果";
        case MANGO, AVOCADO:
            yield "进口水果";
        default:
            yield "未知水果";
    });
}
```
