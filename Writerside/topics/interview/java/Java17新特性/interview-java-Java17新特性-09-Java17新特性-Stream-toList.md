# interview-java-Java17新特性-09_Java17新特性_Stream.toList()

如果需要将Stream转换成List,需要通过调用collect方法使用Collectors.toList()，代码非常冗长。

```Java
private static void oldStyle() {
    Stream<String> stringStream = Stream.of("a", "b", "c");
    List<String> stringList =  stringStream.collect(Collectors.toList());
    for(String s : stringList) {
        System.out.println(s);
    }
}

```

在Java 17中将会变得简单，可以直接调用toList()。

```Java
private static void streamToList() {
    Stream<String> stringStream = Stream.of("a", "b", "c");
    List<String> stringList =  stringStream.toList();
    for(String s : stringList) {
        System.out.println(s);
    }
}

```
