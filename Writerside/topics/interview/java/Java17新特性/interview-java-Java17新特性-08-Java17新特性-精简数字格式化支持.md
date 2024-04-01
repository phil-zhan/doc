# interview-java-Java17新特性-08_Java17新特性_精简数字格式化支持

在NumberFormat中添加了一个工厂方法，可以根据Unicode标准以紧凑的、人类可读的形式格式化数字。

SHORT格式如下所示:

```Java
NumberFormat fmt = NumberFormat.getCompactNumberInstance(Locale.ENGLISH, NumberFormat.Style.SHORT);
System.out.println(fmt.format(1000));
System.out.println(fmt.format(100000));
System.out.println(fmt.format(1000000));
```

输出格式为：

```Java
1K
100K
1M
```

LONG格式如下所示：

```Java
NumberFormat fmt = NumberFormat.getCompactNumberInstance(Locale.ENGLISH, NumberFormat.Style.LONG);
System.out.println(fmt.format(1000));
System.out.println(fmt.format(100000));
System.out.println(fmt.format(1000000));
```

输出结果为：

```Java
1 thousand
100 thousand
1 million
```