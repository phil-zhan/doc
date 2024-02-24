# interview-java-Java17新特性-03_Java17新特性_record关键字

record用于创建不可变的数据类（属性都是final修饰的），适用于那些主要目的是持有数据并提供简单访问的类。然而，对于需要更复杂逻辑、继承、或其他高级功能的类，传统的
class 仍然是更好的选择。

在Java17之前，如果需要创建一个存放数据的类，需要先创建一个class，然后生成构造方法、getter、setter、hashCode、equals和toString等这些方法，或者使用Lombok来简化这些操作。

比如定义一个Person类：

```Java
// 这里使用lombok减少代码
@Data
@AllArgsConstructor
public class Person {
    private String name;

    private int age;

    private String address;
}
```

我们来通过Person类做一些测试，比如创建两个对象，对他们进行比较，打印这些操作。

```Java
public static void testPerson() {
    Person p1 = new Person("小黑说Java", 18, "北京市西城区");
    Person p2 = new Person("小白说Java", 28, "北京市东城区");
    System.out.println(p1);
    System.out.println(p2);
    System.out.println(p1.equals(p2));
}
```

假设有一些场景我们只需要对Person的name和age属性进行打印，在有record之后将会变得非常容易。

```Java
public static void testPerson() {
    Person p1 = new Person("小黑说Java", 18, "北京市西城区");
    Person p2 = new Person("小白说Java", 28, "北京市东城区");
    // 使用record定义
    record PersonRecord(String name,int age){}
    
    PersonRecord p1Record = new PersonRecord(p1.getName(), p1.getAge());
    PersonRecord p2Record = new PersonRecord(p2.getName(), p2.getAge());
    System.out.println(p1Record);
    System.out.println(p2Record);
}
```

record也可以单独定义作为一个文件定义，但是因为Record的使用非常紧凑，所以可以直接在需要使用的地方直接定义。

```Java

// 类似Scala中的元组类的写法
public record PersonRecord(String name,int age){

}

```

record同样也有构造方法，可以在构造方法中对数据进行一些验证操作。

```Java
public static void testPerson() {
    Person p1 = new Person("小黑说Java", 18, "北京市西城区");
    Person p2 = new Person(null, 28, "北京市东城区");
    record PersonRecord(String name, int age) {
        // 构造方法
        PersonRecord {
            System.out.println("name " + name + " age " + age);
            if (name == null) {
                throw new IllegalArgumentException("姓名不能为空");
            }
        }
    }
    PersonRecord p1Record = new PersonRecord(p1.getName(), p1.getAge());
    PersonRecord p2Record = new PersonRecord(p2.getName(), p2.getAge());
}
```

