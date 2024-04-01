# interview-java-Java17新特性-05_Java17新特性_instanceof模式匹配

通常我们使用instanceof时，一般发生在需要对一个变量的类型进行判断，如果符合指定的类型，则强制类型转换为一个新变量。

```Java
private static void oldStyle(Object o) {
    if (o instanceof Furit) {
        Furit furit = (GrapeClass) o;
        System.out.println("This furit is :" + furit.getName);
    }
}
```

在使用instanceof的模式匹配后，上面的代码可进行简写。可以将类型转换和变量声明都在if中处理。同时，可以直接在if中使用这个变量。

```Java
private static void oldStyle(Object o) {
    if (o instanceof Furit furit) {
        System.out.println("This furit is :" + furit.getName);
    }
}
```

因为只有当instanceof的结果为true时，才会定义变量furit，所以这里可以使用&&，但是改为||就会编译报错。

```Java
private static void oldStyle(Object o) {
    if (o instanceof Furit furit && furit.getColor()==Color.RED) {
        System.out.println("This furit is :" + furit.getName);
    }
}
```
