# 语法基础

### 1. Scala实体类 VS Java实体类

| 不同                 | 描述                                                                                                                                                                       |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 语法                 | 在 Scala 中，一般使用 case class 关键字来定义实体类，它可以自动生成一些常用的方法，如构造函数、toString、equals 和 hashCode。而在 Java 中，则使用普通的类来定义实体类，需要手动添加这些方法。                                                  |
| 数据封装               | Scala 中的实体类中的字段默认是不可变的，如果要让某个字段可变，需要显式地声明为var，而 Java 中的实体类字段可以使用 final 关键字来表示它们是不可变的。这种不同的数据封装方式反映了 Scala 和 Java 在函数式编程范式上的不同                                            |
| Getter 和 Setter 方法 | 在 Scala 中，实体类的字段默认会自动生成 getter 和 setter 方法，你可以直接访问属性来获得或设置实例变量的值。而在Java中，你需要显式地编写 getter 和 setter 方法。                                                                    |
| 元组类                | 在 Scala 中，你可以使用元组类来一次性地描述多个相关的属性，类似于 Java 中的 Pair 类。元组类是不可变的，通过元组类可以在不创建类的情况下快速地组织数据。例如，Scala 中的 (name: String, age: Int) 就是一个元组类。元组类只是一个特殊的类，Scala也能像Java那样，定义自己的属性和方法等 |
| 模式匹配               | Scala 中的实体类可以与模式匹配结合使用。模式匹配是 Scala 中强大的特性，它允许你根据实例的特定属性或模式来执行不同的操作。这使得处理实体类的不同情况变得更加灵活和易于阅读。【如下面这个例子】                                                                    |
| 特质                 | Scala 中的实体类可以实现特质（Traits），特质类似于 Java 中的接口，但可以包含实现代码。通过特质的组合，你可以将多个功能组合成一个类，这对于代码重用和模块化非常有用。                                                                              |
| 隐式转换               | Scala 中的实体类可以通过隐式转换来提供额外的功能。隐式转换可以让你在需要某个类的功能时自动将其转换为另一个类。这可以减少冗余的代码，并且让代码更加简洁和易读。                                                                                       |
| 编程风格               | Scala 通常倡导函数式编程风格，而 Java 更偏向于面向对象编程。因此，在 Scala 中，你可能更倾向于使用不可变的实体类和纯函数，而在 Java 中，你可能更多地使用可变的实体类和命令式编程。                                                                    |

##### 模式匹配

```Scala
    // 定义一个元组类
    case class T1(id:Int,name:String,age:Int){
   
    }
   
    // 在 match 模式匹配的时候，case 后面是一个对象，需要对象的所有元组属性都匹配上，才能匹配成功，否则匹配失败。
    // 另外，这个对象类还需要时 cass 修饰的，才能这么写
    val t1 = Student(1,"zs",20)
    t1 match {
        case Student(1,"zs",20) => println("匹配成功")
        case Student(2,"zs",20) => println("匹配失败")
        case  _ => println("匹配失败")
    }
  
    // 下面的代码，最终将会是第一个case匹配成功。这是按照类型匹配
    val t2:Any = "hello"
    x match {
        case s: String if s.nonEmpty => println("Got a non-empty string: " + s)
        case s: String => println("Got an empty string")
        case i: Int => println("Got an integer: " + i)
        case _ => println("Got something else")
    }
  
    // 下面的代码，是第一个匹配成功，且不会再往下匹配了。元组匹配，可以指定某些值来匹配，这里的最后一个case，相当于Java的Switch的else。保底的
    val p: (String, Int) = ("Alice", 30)
    p match {
      case ("Alice", age) => println(s"Alice's age is $age")
      case (name, age) => println(s"$name is $age years old")
    }

```

##### 特质traits

类似于Java中的接口，不同点如下

| 区别     | 描述                                                                                                                                                                                                                                                                             |
|--------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 定义方式不同 | Java接口定义用interface，Scala特质定义用 traits                                                                                                                                                                                                                                           |
| 使用方式不同 | Java接口的使用，一般靠接口的继承，或者类的实现，实现的时候需要实现接口的所有非default修饰的默认方法。Scala的特质，可以被类直接继承，但是也需要重写抽象的方法 。当然，如果抽象的方法不使用的话，重写的时候，可以直接用三个问号代替`override def aa(): Unit = ???` 。在Scala中，特质还能用 with 关键字混入。混入的前提是这个类继承了其他特质，也就是 with只能用在 有extends的地方。混入的特质不需要重写抽象方法，且也能使用用其默认方法 ，当然，要使用其抽象方法也需要重写后才能使用 |
| 冲突解决   | 当一个类实现多个接口，并且这些接口具有相同的默认方法时，在 Java 中需要通过显式重写方法来解决冲突。在 Scala 特质中，当多个特质有相同的方法签名时，编译器会自动解决冲突。如果一个类混入了这些特质并使用了该方法，则编译器会根据特定的规则选择其中一个方法的实现。【重写的方法优先，靠右的特质方法优先、父特质优先（同时继承了父子特质）】                                                                                                     |

```Scala

// 类似于Java的重写默认方法
trait Greeter {
  def greet(name: String): Unit = {
    println(s"Hello, $name!")
  }
}

class Person(val name: String) extends Greeter {
  def sayHello(): Unit = {
    greet(name)
  }
}

val person = new Person("Alice")
person.sayHello()  // 输出：Hello, Alice!

class MyClass extends SomeClass with MyTrait

```