# interview-java-Java17新特性-07_Java17新特性_日期周期格式化

在Java 17中添加了一个新的模式B,用于格式化DateTime,它根据Unicode标准指示一天时间段。

使用默认的英语语言环境，打印一天的几个时刻：

```Java
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("B");
System.out.println(dtf.format(LocalTime.of(8, 0)));
System.out.println(dtf.format(LocalTime.of(13, 0)));
System.out.println(dtf.format(LocalTime.of(20, 0)));
System.out.println(dtf.format(LocalTime.of(23, 0)));
System.out.println(dtf.format(LocalTime.of(0, 0)));
```

输出结果：

```Java
in the morning
in the afternoon
in the evening
at night
midnight
```

如果是中文语言环境，则输出结果为：

```Java
上午
下午
晚上
晚上
午夜
```

可见咱们的晚上是包括英美国家的evening和night的。