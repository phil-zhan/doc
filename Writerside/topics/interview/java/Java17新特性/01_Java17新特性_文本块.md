# interview-java-Java17新特性-01_Java17新特性_文本块

在Java17之前的版本里，如果我们需要定义一个字符串，比如一个JSON数据，基本都是如下方式定义：

```java
public void lowVersion() {
    String text = "{\n" +
        "  \"name\": \"小黑说Java\",\n" +
        "  \"age\": 18,\n" +
        "  \"address\": \"北京市西城区\"\n" +
        "}";
    System.out.println(text);
}
```

这种方式定义具有几个问题：

- 双引号需要进行转义;
- 为了字符串的可读性，需要通过+号来连接;
- 如果需要将JSON复制到代码中需要做大量的格式调整（当然这一点也可以通过其他工具来解决）;

通过Java17中的文本块语法，类似的字符串处理则会方便很多; 通过三个双引号可以定义一个文本块，并且结束的三个双引号不能和开始的在同一行;

上面例子中的JSON可以更方便、可读性更好的通过文本块定义，代码如下:

```Java
private void highVersion() {
    String text = """
            {
              "name": "小黑说Java",
              "age": 18,
              "address": "北京市西城区"
            }
            """;
    System.out.println(text);
}
```

这段代码的输出结果:

```json
{
  "name": "小黑说Java",
  "age": 18,
  "address": "北京市西城区"
}
```