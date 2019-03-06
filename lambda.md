# —————————:fast_forward:lambda表达式与内部类:rewind:————————— #

<p id="t"></p>

## :book:lambda表达式与内部类 ##

:arrow_double_down:<a href="#a1">lambda简介与语法</a>

:arrow_double_down:<a href="#a2">访问权限</a>

:arrow_double_down:<a href="#a3">方法引用</a>

:arrow_double_down:<a href="#a4">常用的接口</a>

:arrow_double_down:<a href="#a5">内部类</a>

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
        StudentSay.Say("Hello World");    //接口方法调用
    }
    @Override                  //重写接口
    public  void Say(String s){

    }
}
```

函数接口的使用是声明一个接口类型如上 Say StudentSay ， StudentSay为接口实例，然后再用实例调用方法。

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
 
 
 对于上面的接口直接使用即可，不需要多的声明，可以直接声明类型使用，如下：
 
 使用Function接口声明一个可以返回Students对象的Name属性：
 
 Students类:
 
 ```java
 public class Students   {
      private  String Name;
      private  int ID;
      public int i = 0;
      public  Students(String name){
          Name = name;
      }
      public  void Show(){
          System.out.println("Name:"+Name);
      }
      public String GetName(){
          return Name;
      }
}
 ```
 
运行测试类：
 
```java
 public class Start   {
    public  static void main(String arg[]){
        Function<Students,String> method = (Stu)->{
            return Stu.GetName();
        };

        Students stu = new Students("Lumnca");
        String _name =  method.apply(stu);
        System.out.println(_name);
    }
}

```

可以看出Function接口需要T，R参数，Function<Students,String>即为一个Students的参数，返回值为String，抽象方法为apply()提供一个Students参数即可执行。
 
 再如：
 
 ```java
 public class Start   {
    public  static void main(String arg[]){
        Consumer<Students> method = (Stu)->{
            Stu.Show();
        };
        method.accept(new Students("Lumnca"));
    }
}
```

Consumer<T>只有一个T类型的参数，无返回值，我们可以在里面调用方法。
  
对于接口还有基本类型的接口，这里就不做展示，上面的接口以及够我们平常使用。

<p id="a5"><p>
  
### :trophy:内部类 ###

:arrow_double_up:<a href="#t">返回目录</a>
  
  
内部类是定义在一个类中的类，使用内部类主要是以下特点：

* 内部类方法可以访问该类定义所在的作用域中的数据，包括私有数据。

* 内部类可以对同一个包中的其他类隐藏起来。

* 当想要定义一个回调函数且不想编写大量代码时可以使用匿名内部类比较比便捷。
 
* 每个内部类都能独立的继承一个接口的实现，所以无论外部类是否已经继承了某个(接口的)实现，对于内部类都没有影响。内部类使得多继承的解决方案变得完整，

* 方便将存在一定逻辑关系的类组织在一起，又可以对外界隐藏。

* 方便编写事件驱动程序

* 方便编写线程代码

  
#### :taxi:成员内部类 ####

成员内部类可以无条件访问外部类的所有成员属性和成员方法（包括private成员和静态成员）。如下示例:

Students类:

```java
public class Students   {
      private  String Name;
      private  int ID;
      public static int i = 0;
      public  Students(String name){
          Name = name;
          i+=1;
      }
      public String GetName(){
          return Name;
      }
      public class StudentInfor{
          public void ShowInfor(){
              System.out.println("学生姓名:"+Name);
              System.out.println("实例化学生数:"+i);
          }
      }
}
```

运行测试类：

```java
public class Start   {
    public  static void main(String arg[]){

       Students stu1  = new Students("Lumnca");
       Students stu2  = new Students("Kainny");
       Students.StudentInfor stuInfor1 = stu1.new StudentInfor();  
       Students.StudentInfor stuInfor2 = stu2.new StudentInfor();
       stuInfor1.ShowInfor();
       stuInfor2.ShowInfor();
    }
}
```

可以看出在内部类中，可以访问任意成员与对象，包括私有与静态。

对于内部类需要先声明外部类，然后再由实例化的对象再实例化一个内部类。如上样式。不过要注意的是，当成员内部类拥有和外部类同名的成员变量或者方法时，会发生隐藏现象，即默认情况下访问的是成员内部类的成员。如果要访问外部类的同名成员，需要以下面的形式进行访问：

```java
外部类.this.成员变量
外部类.this.成员方法
```

虽然成员内部类可以无条件地访问外部类的成员，而外部类想访问成员内部类的成员却不是这么随心所欲了。在外部类中如果要访问成员内部类的成员，必须先创建一个成员内部类的对象，再通过指向这个对象的引用来访问：

Students类：

```java
public class Students   {
      private  String Name;
      private  int ID;
      public static int i = 0;
      public  Students(String name){
          Name = name;
          i+=1;
      }
      public String GetName(){
          return Name;
      }
      public void Show(Students Stu){
          System.out.println(Stu.new StudentInfor().CodeName());       //对象构建
      }
      public class StudentInfor{
          public String CodeName(){
              return Name +"001";
          }

          public void ShowInfor(){
              System.out.println("学生姓名:"+Name);
              System.out.println("实例化学生数:"+i);
          }
      }
}
```


运行测试类：

```java
public class Start   {
    public  static void main(String arg[]){

       Students stu1  = new Students("Lumnca");
       Students stu2  = new Students("Kainny");
       stu1.Show(stu1);
       stu2.Show(stu2);
    }
}
```


#### :taxi:匿名内部类 ####

匿名内部类应该是平时我们编写代码时用得最多的，在编写事件监听的代码时使用匿名内部类不但方便，而且使代码更加容易维护。下面这段代码是一段Android事件监听代码：

```java
scan_bt.setOnClickListener(new OnClickListener() {
             
            @Override
            public void onClick(View v) {
                // TODO Auto-generated method stub
                 
            }
        });
         
        history_bt.setOnClickListener(new OnClickListener() {
             
            @Override
            public void onClick(View v) {
                // TODO Auto-generated method stub
                 
            }
        });
```

这段代码为两个按钮设置监听器，这里面就使用了匿名内部类。这段代码中的：

```java
new OnClickListener() {
             
            @Override
            public void onClick(View v) {
                // TODO Auto-generated method stub
                 
            }
        }
```

就是匿名内部类的使用。代码中需要给按钮设置监听器对象，使用匿名内部类能够在实现父类或者接口中的方法情况下同时产生一个相应的对象，但是前提是这个父类或者接口必须先存在才能这样使用。当然像下面这种写法也是可以的，跟上面使用匿名内部类达到效果相同。

```java
private void setListener()
{
    scan_bt.setOnClickListener(new Listener1());       
    history_bt.setOnClickListener(new Listener2());
}
 
class Listener1 implements View.OnClickListener{
    @Override
    public void onClick(View v) {
    // TODO Auto-generated method stub
             
    }
}
 
class Listener2 implements View.OnClickListener{
    @Override
    public void onClick(View v) {
    // TODO Auto-generated method stub
             
    }
}
```

这种写法虽然能达到一样的效果，但是既冗长又难以维护，所以一般使用匿名内部类的方法来编写事件监听代码。同样的，匿名内部类也是不能有访问修饰符和static修饰符的。

匿名内部类是唯一一种没有构造器的类。正因为其没有构造器，所以匿名内部类的使用范围非常有限，大部分匿名内部类用于接口回调。匿名内部类在编译的时候由系统自动起名为Outter$1.class。一般来说，匿名内部类用于继承其他类或是实现接口，并不需要增加额外的方法，只是对继承方法的实现或是重写。


#### :taxi:静态内部类 ####

静态内部类也是定义在另一个类里面的类，只不过在类的前面多了一个关键字static。静态内部类是不需要依赖于外部类的，这点和类的静态成员属性有点类似，并且它不能使用外部类的非static成员变量或者方法，这点很好理解，因为在没有外部类的对象的情况下，可以创建静态内部类的对象，如果允许访问外部类的非static成员就会产生矛盾，因为外部类的非static成员必须依附于具体的对象。

```java
public class Students   {
      private  String Name;
      private  int ID;
      public static int i = 0;
      public  Students(String name,int id){
          Name = name;
          ID = id;
          i+=1;
      }
      public String  GetName(){
          return Name;
      }
      public  int GetID(){
          return  ID;
      }
      public static class Infor{
          public void ShowNumber(){
              System.out.println("学生人数为 : "+i);
          }
    }
}
```

测试类：

```java
public class Start   {
    public  static void main(String arg[]){
        Students s1 = new Students("lmc",20170329);
        Students s2 = new Students("ljk",20160422);

        Students.Infor infor = new Students.Infor();
        infor.ShowNumber();
    }
}
```









 
 
 
 
 
 
 
 
 
 
 
 


