# interview-java-Java17新特性-06_Java17新特性_Helpful_NullPointerExceptions

Helpful NullPointerExceptions可以在我们遇到NPE时节省一些分析时间。如下的代码会导致一个NPE。

```Java
public static void main(String[] args) {
    Person p = new Person();
	String cityName = p.getAddress().getCity().getName();
}

```

在Java 11中，输出将显示NullPointerException发生的行号，但不知道哪个方法调用时产生的null，必须通过调试的方式找到。

```Java
Exception in thread "main" java.lang.NullPointerException
        at com.heiz.java17.HelpfulNullPointerExceptionsDemo.main(HelpfulNullPointerExceptionsDemo.java:13)
```

在Java 17中，则会准确显示发生NPE的精确位置。

```Java
Exception in thread "main" java.lang.NullPointerException: Cannot invoke "com.heiz.java17.Address.getCity()" because the return value of "com.heiz.java17.Person.getAddress()" is null
		at com.heiz.java17.HelpfulNullPointerExceptionsDemo.main(HelpfulNullPointerExceptionsDemo.java:13)
```