# —————————:fast_forward:lambda表达式与内部类:rewind:————————— #

<p id="t"></p>

## :book:lambda表达式与内部类 ##

:arrow_double_down:<a href="#a1">lambda简介与语法</a>

:arrow_double_down:<a href="#a2">访问权限</a>

:arrow_double_down:<a href="#a3">方法引用</a>

:arrow_double_down:<a href="#a4">常用的接口</a>

<p id="a1"><p>
  
### :trophy:lambda简介与语法 ###

:arrow_double_up:<a href="#t">返回目录</a>

#### :pushpin:函数式接口 ####

学习lambda表达式就要先知道是什么？

>函数式接口（Functional Interfaces）：如果一个接口定义个唯一一个抽象方法，那么这个接口就成为函数式接口。同时，引入了一个新的注解：@FunctionalInterface。可以把他它放在一个接口前，表示这个接口是一个函数式接口。这个注解是非必须的，只要接口只包含一个方法的接口，虚拟机会自动判断，不过最好在接口上使用注解 @FunctionalInterface 进行声明。在接口中添加了 @FunctionalInterface 的接口，只允许有一个抽象方法，否则编译器也会报错。

Lambda表达式：可以让你的代码更加的简洁。lambda无法单独出现，需要一个函数式接口来盛放，可以说lambda表达式方法体是函数式接口的实现，lambda实例化函数式接口，可以将函数作为方法参数，或者将代码作为数据对待。

主要优点：

* 1.代码变得更加简洁紧凑 
* 2.可读性强， 
* 3.并行操作大集合变得很方便，可以充分发挥多核cpu的优势，更加便于多核处理器编写代码等，


#### :pushpin:语法形式 ####

通俗点来说，lambda表达式就是一个代码块，以及必须传入代码的变量规范。所以在java中有一种lambda表达式格式为：`参数 -> 以及一个表达式`，以下为常用形式：

() -> 变量/表达式 ： 返回该变量值或者表达式的值，如下

```java
()-> 2;            //返回2
()-> "Hello";      //返回字符串"Hello"
()-> x+1;          //返回变量x+1（x必须在上下文存在）
()-> Student;      //返回Student（对象必须存在）
```

(参数)-> 变量/表达式 :返回该变量值或者表达式的值,如下：

```java
(x)->x*2;              //返回x*2的值
(x,y)->x+y;            //返回x+y的值，类型不限
(String x,String y)-> System.out.println(x+y)    //没有返回值，打印x+y，且必须为字符串
name -> name + ":";    //返回字符串“name:”
```

需要知道的是圆括号其实是声明参数的，这个参数你可以不声明类型，他会自动判断类型，或者根据你的要使用参数类型来判断。如果你要固定参数类型，也可以加上类型。值得注意的是lambda不一定需要返回值，如上你可以打印一条信息。如果代码要完成的计算无法放在一个表达式里，不像上面只需要使用一行代码就可以完成，可以像写方法一样，把这些代码写进{}中，并包含显式的return语句，如：

```java
(x,y)->{
if(x>y) retrun x;
else return y;
}
```

需要注意的是一旦是多行代码并且有返回值就必须要写return，没有返回值可以不用写，单行代码有返回值就不能写return。

lambda简单的使用：

首先是接口的声明：

```java
@FunctionalInterface
public interface Say {
    void Say(String s);
}
```

然后是使用这个接口：

```java
public class Start  implements Say {   //继承接口

    public  static void main(String arg[]){
        Say StudentSay = (s)->{     //lambda引用方法
            System.out.println(s);
        };
        StudentSay.Say("Hello World");
    }
    @Override                  //重写接口
    public  void Say(String s){

    }
}
```

<p id="a2"><p>
  
### :trophy:lambda访问权限 ###

:arrow_double_up:<a href="#t">返回目录</a>

在Lambda表达式使用中，Lambda表达式外面的局部变量会被JVM隐式的编译成final类型，Lambda表达式内部只能访问，不能修改。 Lambda表达式内部对静态变量和成员变量是可读可写的 。Lambda不能访问函数接口的默认方法，在函数接口中可以添加default关键字定义默认的方法。

局部变量示例：

```java
 public static void main(String[] args) {
        int num = 6;//局部变量
        Sum sum = value -> {
             // num = 8; 这里会编译出错
            return num + value;
        };
        sum.add(8);
    }

    /**
     * 函数式接口
     */
    @FunctionalInterface
    interface Sum{
        int add(int value);
    }
```

静态变量和成员变量示例：

```java
public int num1 = 6;
    public static int num2 = 8;
    private int getSum(){
        Sum sum = value -> {
            num1 = 10;
            num2 = 10;
            return  num1 + num2;
        };
        return sum.add(1);
    }

    /**
     * 函数式接口
     */
    @FunctionalInterface
    interface Sum{
        int add(int value);
    }
```

<p id="a3"><p>
  
### :trophy:lambda方法引用 ###

:arrow_double_up:<a href="#t">返回目录</a>

可以看出在前面的lambda表达式实际上就是声明方法主体。方法引用可以简化写法。引用的方法就是Lambda表达式的方法体的实现，语法结构：`类名或者实例名::所要引用的方法名`.详细参见下面内容：

#### :pushpin:静态方法引用示例 ####


```java
public class Start{

    public  static void main(String arg[]){
        Say StudentSay = Start::SayHello;   //引用SayHello方法
        StudentSay.Say("Lumnca");
    }
    static  void SayHello(String name){
        System.out.println("Hello! "+name);
    }
}

//接口
@FunctionalInterface
public interface Say {
    void Say(String s);
}
```

需要注意的是引用的方法要与接口类型一致，如上接口` void Say(String s)`所以引用的方法需要为void型，且含有一个参数。这里就不需要继承接口，直接声明接口类型方法就行。

#### :pushpin:实例方法引用示例 ####

```java
public class Start   {

    public  static void main(String arg[]){
        Start s1 = new Start();         //实例化对象
        Say StudentSay = s1::SayHello;  //引用
        StudentSay.Say("Lumnca");
    }
    void SayHello(String name){
        System.out.println("Hello! "+name);
    }
}
```

#### :pushpin:构造方法引用示例： ####

泛型接口：

```java
@FunctionalInterface
public interface Say<T> {
    T Say(String s);
}
```

含有构造方法的Students类：

```java
public class Students extends  Person  {
      private  String Name;
      private  int ID;
      public int i = 0;
      public Students(int h,int w,String n,int id){
          super.height = h;
          super.weight = w;
          Name = n;
          ID = id;
      }
      public  Students(String name){
          Name = name;
      }
      public  void Show(){
          System.out.println("Name:"+Name);
      }
}
```

主程序运行：

```java
public class Start   {
    public  static void main(String arg[]){
        Say<Students> StudentSay = Students::new;   //引用Students的构造函数
        Students s =  StudentSay.Say("Lumnca");
        s.Show();
    }
}
```

注意每个引用的方法一定要和接口方法类型参数一致。

<p id="a4"><p>
  
### :trophy:lambda常用的接口 ###

:arrow_double_up:<a href="#t">返回目录</a>

有些接口是Java自带的，可以让我们不用自己去定义接口，直接调用，方便操作与简化工作常用的接口如下表：

|函数式接口|参数类型|返回值|抽象方法名|           描述           |其他方法|
|:-------:|:-----:|:-----:|:------:|:----------------------: |:-----:|
|Runnable | 无    | void  | run    |作为无参数或返回值的动作运行|   无  |
|Supplier<T> | 无 |   T   | get    |提供一个T类型的值          |  无   |
|Consumer<T> | T  | void  | accept | 处理一个T类型的值         | addThen|
|BiConsumer<T,U>|T,U|void | accept | 处理T和U类型的值          | addThen|
|Function<T,R>|T   | R    |  apply | 处理一个有T类型参数的函数  | compose，andThen，identity|
|BiFunction<T,U,R>|T,U|R| | apply  |有T和U类型参数的函数       | addThen |
|UnaryOperator<T> |T  |T  | apply  |类型T上的一元操作符        |compose，andThen，identity|
|BinaryOperator<T>|T,T|T  |apply   |类型T上的二元操作符        | compose，andThen，identity|
|Predicate<T>|T|boolean   |test    |布尔值函数                |and，or，negate，isEqual |
|BiPredicate<T，U>|T，U|boolean  |test|有两个参数的布尔值函数   |and，or，negate|
 


