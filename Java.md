# 面对对象编程
## 变量的分类  
+ 成员变量(堆内存)
  + 实例变量(不以static修饰)实例化成对象以后才能使用
  + 类变量(以static修饰)不需实例化，可直接类名.属性调用
+ 局部变量(栈内存)
  + 形参(方法签名中定义)
  + 方法局部变量(方法内定义)
  + 代码块局部变量(代码块定义)类中直接写个大括号写变量

## 匿名对象
可以不定义对象的句柄而直接调用方法，如new Person().speak();  
对一个对象只进行一次方法调用或作为实参传递给一个方法调用时使用

## 方法的重载(overload)
同一个类中，允许存在一个以上的同名方法，只要它们的参数列表不同(参数个数、类型、顺序)，和返回类型无关。调用时，根据方法列表不同来区分。

## 可变个数的形参
数组方法：`public static void test(int a, String[] books);`  
...方法：`pubic static void test(int a, String... books);`  
...表示0到多个参数，使用时与数组一致  
方法若有可变形参，可变形参放在最后

## 方法的参数传递
形参：方法声明时的参数  
实参：方法调用时传给形参的参数值  
Java的参数传递方式只有值传递：将实际参数值的副本传入方法内尔参数本身不受影响  
若方法的形参是基本数据类型，实参向形参传递值  
若方法的形参是对象，实参向形参传递对象在堆中的地址

## JVM内存模型
+ 栈 stack
  + 基础数据类型
  + 对象的引用(地址)
+ 堆 heap
  + 所用的对象
+ 方法区 method
  + 所有的class和static变量


## Package
作为Java源文件的第一条语句，指明该文件中的定义的类所在的包  
格式为：`package 顶层包名.子包名;`  
通常用小写单词，用"."指包名的层次

## 封装和隐藏
使用者对类内部定义的属性(对象的成员变量)直接操作会导致数据的错误、混乱或安全性问题。此时需要将属性封装和隐藏起来。  
具体方法：通过将数据声明为private，再提供public的方法实现对属性的操作以实现以下目的：  
1. 隐藏一个类中不需要对外提供的实现细节
2. 使用者只能通过事先制定好的方法来访问数据，可以方便地加入控制逻辑，限制对属性的不合理操作
3. 便于修改，增强代码的可维护性


## 四种访问权想修饰符修饰符
用来限定对象对该类成员的访问权限
| 修饰符    | 类内部 | 同一个包 | 子类 | 任何地方 |
| --------- | ------ | -------- | ---- | -------- |
| private   | yes    |
| default   | yes    | yes      |
| protected | yes    | yes      | yes  |
| public    | yes    | yes      | yes  | yes      |
public:可以在类的外部也可以在本类方法中使用  
private:只能在类的方法中使用，不能在类外部使用(不能通过对对象.属性调用)

## 类的成员之三：构造器(构造方法)
new 对象的根本原理？ new对象实际上就是调用类的构造方法。      

根据参数不同，构造器可分为如下两类：
+ 隐式无参构造器(系统默认提供)
+ 显式定义一个或多个构造器(无参、有参)

语法格式：
```
修饰符 类名(参数列表){
    初始化语句;
}
```

构造器的特征：
+ 具有与类相同的名称和修饰符
+ 不声明返回值类型(与声明为void不同)
+ 不能被static、final、synchronized、abstract、native修饰，不能有return语句返回值

注意：
+ Java语言中，每个类都至少有一个构造器
+ 默认构造器的修饰符与所属类的修饰符一致
+ 一旦显式定义了构造器，则系统不再提供默认构造器
+ 一个类可以创建多个重载的构造器
+ 父类的构造器不可被子类继承

## 构造器重载
既然构造器也是方法，它也可以进行重载。
构造器一般用来创建对象的同时初始化对象，如：
```
class Person{
    String name;
    int age;
    public Person(String n, int a){name=n,age=a;}
}
```
构造器重载使得对象的创建更加地灵活，方便创建各种不同的对象，提供了多个初始化new对象的模板。重载时，参数列表必须不同。
```
public class Person{
    public Person(String name,int age,Data d){...}
    public Person(Strin name, int age){...}
    public Person(String name, Data d){...}
}
```

## 关键字——this
使用情况：
1. 当形参与成员变量重名时，如果在方法内部需要使用成员变量，必须添加this来表明该变量时类成员
2. 在任意方法内，如果使用当前类的成员变量或成员方法可以在其前面添加this，增强程序的阅读性
3. this可以作为一个类中，构造器相互调用的特殊格式

表示意义：
1. this在方法内部使用，即这个方法所属对象的引用  
2. this在构造器内部使用，表示构造器正在初始化的对象  
3. this表示当前对象，可以调用类的属性、方法和构造器
4. 当在方法内部需要用到该方法的对象时，就用this  
示例：
```
void setName(String name){
    this.name = name;
}

public Person(){}
public Person(String name){}
public Person(String name, int age){
    this();  //调用无参构造器
    this("name"); //调用第二个构造器
}
```
注意：
1. 使用this()必须放在构造器的首行
2. 使用this调用本类中的其他的构造器，保证至少有一个构造器是不用this的(实际上就是构造器不可以自己调用自己)

## JavaBean
JavaBean是一种Java语言写成的可重用组件。  
所谓JavaBean，是指符合如下标准的Java类：
+ 类是公共的
+ 有一个无参的公共构造器
+ 有属性，属性一般是私有的，且有对应的get、set方法

用户可以使用JavaBean将功能、处理、值、数据库访问和其他任何可以用java代码创造的对象进行打包，并且其他的开发者可以通过内部的JSP页面、Servlet、其他JavaBean、applet程序或者应用来使用这些对象。用户可以认为JavaBean提供了一种随时随地的复制和粘贴的功能，而不用关心任何改变  
在写好私有的属性之后可以直接选中所有属性右键source自动生成getters和setters

# 高级类特性
## 继承
本质：把共性的东西抽取出来形成父类，实际需求的子类在继承父类的基础上写自己特有的代码即可。子类继承了父类，就继承了父类的方法和属性。在子类中，可以使用父类中定义的方法和属性，也可以创建新的数据和方法。   
  
为什么要有继承？  
多个类中存在相同属性和行为是，将这些内容抽取到单独一个类中，那么多个类无需再定义这些属性和行为，只要继承那个类即可。此处的多个类称为子类，单独的这个类称为父类(基类或超类)。可以理解为"子类 is a 父类"。  

类继承语法规则：`class Subclass extends Superclass{}`

作用：
+ 提高代码的复用性
+ 让类与类之间产生了关系，提供了多态的前提

注意：
+ 不要仅为了获取其他类中的某个功能而去继承，继承要有逻辑关系
+ 在Java中，继承的关键字是"extends"，即子类不是父类的子集，而是对父类的"拓展" 
+ 子类不能直接访问父类中私有的(private)成员变量和方法
+ Java只支持单继承，不允许多重继承，一个子类只能有一个父类

## 方法的重写
定义：在子类中可以根据需要对从父类中继承来的方法进行改造，也称方法的重置、覆盖。在程序执行时，子类的方法将覆盖父类的方法。  
要求：
+ 重写方法必须和被重写方法具有相同的方法名称、参数列表和返回值类型(只是重写方法体的代码)
+ 重写方法不能使用比被重写方法更严格的访问权限
+ 重写和被重写的方法必须同时为static或非static的
+ 子类方法抛出的异常不能大于父类被重写方法的异常

与访问权限修饰符相关的两条结论：
+ 如果子类和父类在同一个包下，那么对于父类的成员修饰符只要不是私有的private，那子类就都可以使用
+ 如果子类和父类不在同一个包下，子类只能使用父类中protected和public修饰的成员

## 关键字——super
多层继承时，使用super，子类可以调用子类之上所有父类层级。  
在Java类中使用super来调用父类中的指定操作：
+ super可用于访问父类中定义的属性
+ super可用于调用父类中定义的成员方法
+ super可用于在子类构造方法中调用父类的构造器

注意：
+ 尤其当子父类出现同名成员时，可以用super进行区分
+ super的追溯不仅限于直接父类
+ super和this的用类似，this代表本类对象的引用，super代表父类的内存空间的标识

调用父类的构造器：
+ 子类中所有的构造器默认都会访问父类中空参数的构造器
+ 当父类中没有空参数的构造器时，子类的构造器必须用过this(参数列表)或者sper(参数列表)语句指定调用本类或者父类中相应的构造器，且必须放在构造器的第一行
+ 如果子类构造器中既未显式调用父类或本类的构造器，且父类中又没有无参的构造器，则编译出错

this和super的区别：
No.|区别点|this|super
---|------|----|---
1|访问属性|访问本类中的属性，如果本类没有此属性则从父类中继续查找|访问父类中的属性
2|调用方法|访问本类中的方法|直接访问父级中的方法
3|调用构造器|调用本类构造器，必须放在构造器的首行|调用父类构造器，必须放在子类构造器的首行
4|特殊|表示当前对象|无此概念
 
## 简单类对象的实例化过程
1. 在方法区中加载Person.class
2. 在栈中申请空间，声明变量p
3. 在堆内存中开辟空间，分配地址，假设地址是BE2500
4. 在对象空间中，对对象属性进行默认初始化，再进行显示初始化。
5. 构造函数方法进栈进行初始化
6. 初始化完毕后，将堆内存中的地址值赋给引用变量，构造方法出栈

## 子类对象的实例化过程
1. 先加载Person.class，再Student.class
2. 在栈中申请空间，声明变量stu
3. 在堆内存中开辟空间，分配地址，假设地址是BE3500
4. 并在对象空间中，对对象中的属性(包括父类的属性)进行默认初始化
5. 子类构造器函数方法进栈
6. 显式初始化父类的属性
7. 父类构造方法进栈，执行完毕出栈
8. 显式初始化父类的属性
9. 初始化完毕后，将堆内存中的地址值赋给引用变量，子类构造方法出栈

## 面向对象特征之三：多态(Polymorphism)
多态在Java中有两种体现：
1. 方法的重载(overload)和重写(overwrite)
2. 对象的多态性——可以直接应用在抽象类和接口上

Java引用变量有两个类型：编译时类型和运行时类型。编译时类型由声明该变量时使用的类型决定，运行时类型由实际赋给该变量的对象决定。若编译时类型和运行时类型不一致，就出现多态。  
对象的多态：子类的对象可以替代父类的对象使用  
+ 一个变量只能有一种确定的数据类型
+ 一个引用类型变量可能指向(引用)多种不同类型的对象
+ 子类可看作是特殊的父类，所以父类类型的引用可以指向子类的对象：向上转型(upcasting)
+ 一个引用类型变量如果声明为父类的类型，但实际引用的是子类对象，那么变量就不能再访问子类中添加的属性和方法

虚拟方法调用(Virtual Method Invocation)
```
Person e = new Student();
e.getInfo();  //调用Student类的getInfo()方法
```
编译时e为Person类型，而方法的类型是在运行时确定的，所以调用的是Student类的getInfo()方法。  
成员方法的多态性，也就是动态绑定，必须要存在于方法的重写之上

## instanceof操作符
x instanceof A: 检验x是否为类A的对象，返回值为boolean型(就是检验某个对象是不是类A的子类)。  
要求x所属的类与类A必须是子类和父类的关系，否则编译错误。  
如果x属于类A的子类B，x instanceof B值也为true。

## Object类

Object类是所有Java类的根父类(基类)  
如果在类的声明中未使用extends关键字指明其父类，则默认父类为Object类  

Object类的主要方法：
No.|方法名称|类型|描述
---|--------|---|---
1|public Object()|构造|构造方法
2|public boolean equals(Object obj)|普通|引用对象比较
3|public int hashCode()|普通|取得Hash码
4|public String toString()|普通|对象打印时调用

## 对象类型转换

基本数据类型的Casting：
+ 自动类型转换：小的数据类型可以自动转换成大的数据类型
+ 强制类型转换：可以把大的数据类型强制转换成小的数据类型

对Java对象的强制类型转换称为造型：
+ 从子类到父类的类型转换可以自动进行
+ 从父类到子类的类型转换必须通过造型(强制类型转换)实现
+ 无继承关系的引用类型间的转换是非法的

## ==操作符与equals方法
==:
+ 基本类型比较值：只要两个变量的值相等，即为true
+ 引用类型比较引用：是否指向同一个对象，比较对象的内存地址，只有指向同一个对象时，才返回true
+ 用==进行比较时，符号两边的数据类型必须兼容(可自动转换的基本数据类型除外)，否则编译出错

equals():
+ 所用类都继承了Object，也就获得了equals()，还可以重写
+ 只能比较引用类型，其作用与"=="相同，比较是否指向同一对象，即对象的地址
+ 对于类File、String、Date及包装类(Wrapper Class)来说，是比较类型及内容而不考虑引用的是否是同一个对象，因为这些类中重写了Object类的equals方法

## String对象的创建
字面量创建String对象：
```
String s1 = "abc";
String s2 = "abc";
System.out.println(s1==s2);
```
常量池是堆内存中的一处区域
常量池中添加"abc"对象，返回引用地址给s1对象  
通过equals()方法判断常量池中已有值为abc的对象，返回相同的引用  

new创建String对象：
```
String s3 = new String("def");//
String s4 = new String("def");//
```
在常量池中创建值为'def'的对象s3，返回指向堆中s3的引用  
常量池中已有值为'def'的对象，不做处理，在堆中创建值为'del'的对象s4，返回指向堆中s4的引用  

字面创建对象的时候，只在常量池创建一个对象  
new创建对象的时候，常量池有对象，堆中也要有对象  
所以字面量方法比new省内存
```
String s5 = "x"+"y";
```
经过JVM优化，直接在常量池中添加"xy"对象
```
String s6 = new String("1") + new String("1") + new String("2");
```
通过StringBuilder实现，在常量池中添加"1"和"2"两个对象，在堆中创建值为"112"的对象，把引用地址给s6

## 包装类
针对八种基本定义相应的引用类型——包装类(封装类)  
基本数据类型|包装类
-----------|------
boolean|Boolean
byte|Byte
short|Short
int|Integer
long|Long
char|Character
float|Float
double|Double
装箱：基本数据类型包装成包装类的实例,有了类的特点，可以调用类中的方法
+ 通过包装类的构造器实现 `int i=500; Integer t =new Integer(i);`
+ 通过字符串参数构造包装类对象 `Float f = new Float("4.56");`
+ 用字符串应是数字，否则NumberFormatException`Float f = new Float("abc")`


拆箱：获得包装类对象中包箱的基本类型变量`int i0 = t.intValue();`

JDK1.5后，支持自动自动装箱、自动拆箱，但类型必须匹配
```
Integer i = 112;//自动装箱
int i0 = i;//自动拆箱
```

+ 字符串转换成基本数据类型
  + 通过包装类的构造器实现:`int i = new Integer("12")`
  + 通过包装类的parseXXX(String s)静态方法:`Float f = Float.parseFloat("12.3")`
+ 基本数据类型转换成字符串
  + 调用字符串重载的valueOf()方法:`String fstr = String.valueOf(2.34f)`
  + 更直接的方式:`String intStr = 5+""`

toString方法：
父类Object的toString方法就输出当前内存地址，如果想要输出类的其他信息，就要重写toString方法。  
直接输出引用`sysout(m)`等价于输出toString方法`sysout(m.toString())`

## 关键字static
当我们编写一个类是，其实就是在描述其对象的属性和行为，而并没有产生实质上的对象，只有通过new关键字才会产生出对象，这是系统才会分配内存空间给对象，其方法才可以供外部调用。我们有时候希望无论是否产生了对象或无论产生了多少对象的情况下，某些特定的数据在内存空间里只有一份，例如所有的中国人都有个国家名称，每个中国人都共享这个国家名称，不必再每一个中国人的实例对象中都单独分配一个用于代表国家名称的变量。  
前有static修饰的属性为类属性，修饰的方法为类方法。在设计类时，应分析哪些类属性不因对象的不同而改变，将这些属性设置为类属性，相应方法设置为类方法  
如果方法与调用者无关，则这样的方法通常被称为类方法，由于不需要创建对象就可以调用类方法从而简化了方法的调用。  
使用范围：
在Java类中，可用static修饰属性、方法、代码块、内部类  
被修饰的成员具有以下特点：
+ 随着类的加载而加载，类加载之后静态的方法就可通过类名.方法使用
+ 优先于对象存在，不用new就能用
+ 修饰的成员，被所有对象所共享
+ 访问权限允许是，可不创建对象，直接被类调用
+ 修饰的方法北部不能有this和super，因为二者指的是对象不是类

## 单例(SIngleton)设计模式
设计模式：设计模式是在大量的事件中总结和理论化之后优选的代码结构、编程风格、以及解决问题的思考方式。设计模式就像是经典的棋谱，不同的棋局，我们用不同的棋谱，免去我们的思考和摸索。  
所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类**只能存在一个对象实例**，并且该类只提供一个取得其对象实例的方法。如果我们要让类在一个虚拟机中只能产生一个对象，我们首先必须将类的构造方法的访问权限设置为private，这样，就不能那个用new操作符在类的外部产生类的对象了，但在类内部仍可以产生该类的对象。因为在类的外部开始还无法得到类的对象，只能调用该类的某个静态方法以返回类内部创建的对象，静态方法只能访问类中的静态成员变量，所以，指向类内部产生的该对象的变量也必须定义成静态的。

单例设计模式有两种——饿汉式和懒汉式
```
饿汉式：
class Single{
    //private私有化的构造器，不能再类的外部创建该类的对象
    private Single(){}
    private static Single onlyone = new Single();
    //getSingle()为static,不用创建对象即可访问
    public static Single getSingle(){
        return onlyone;
    }
}

懒汉式：
class Single{
    //1.将构造器私有化，保证在此类的外部，不能调用本类的构造器
    private Single(){}
    //声明类的引用，且用static修饰
    private static Single instance = null;
    //设置公共的方法来访问类的示例
    public static Single getInstance(){
        //如果类的实例未创建，那要先创建，类变量用static修饰
        if(instance == null){
            instance = new Single();
        }
        //若有了类的实例，则直接返回给调用者
        return instance;
    }

}

public class TestSingle{
    public static void main(String[] args){
        Single s = Single.getSingle();
    }
}
```
饿汉式和懒汉式的区别就是什么时候new这个对象  
饿汉式时在类加载之后，还没有人调用的时候就先new好了一个对象，以后不论谁来调用getInstance方法，都是直接返回之前第一次new好的对象  
懒汉式是在第一次调用getInstance方法时来new对象，以后再有人调用就返回第一次new好的对象
## 理解main方法的语法
`public static void main(String[] args){}`  
由于Java虚拟机需要调用类的main()方法，所以该方法的访问权限必须是public，又因为Java虚拟机在执行main()方法时不必创建对象，所以该方法必须是static的，该方法接受一个String类型的数组参数，该数组中保存执行Java命令时传递给所运行的类的参数。

## 类的成员之四：初始化块
初始化块作用：对Java对象进行初始化  
程序的执行顺序：  
1. 声明成员变量的默认值  
2. 显式初始化、多个初始化块依次被执行(同级别下按先后顺序执行)  
3. 构造器再对成员进行赋值操作

一个类中初始化块若有修饰符，则只能被static修饰，称为静态代码块(static block)，当类被载入时，类属性的声明和静态代码块先后顺序被执行，被执行一次，且先于非静态代码块，其中只能使用static修饰的属性和方法。  
```
class Person{
    private String name;
    static{
        name = "JOJO";
    }
}
```
在实际开发中，static静态代码块用的比较多，用来初始化类的静态属性。 

## 关键字：final
在Java中声明类、属性和方法时，可使用关键字final来修饰，表示"最终"。  
+ final修饰的类不能被继承，以提高安全性和程序的可读性。 String类、System类、StringBuffer类
+ final修饰的方法不能被子类重写。 Object类中的getClass()
+ final修饰的变量(成员变量或局部变量)即称为常量。名称大写，多个单词组成的名称用下杠连接，且只能被赋值一次。final标记的成员变量必须在声明的同时或在每个构造方法中或代码块中显式赋值，然后才能使用。

## 抽象类(abstract class)
随着继承层次中一个个新子类的定义，类变得越来越具体，而父类则更一般，更通用。类的设计应该保证父类和子类能够共享特征。有时将父类设计得非常抽象，以至于它没有具体的实例，这样的类叫做抽象类。  
用abstract关键字来修饰一个类时，这个类叫做抽象类。
用abstract来修饰一个方法时，该方法叫做抽象方法，只有方法的声明，没有方法的实现。  
含有抽象方法的类必须被声明为抽象类。  
抽象类不能被实例化，是用来被继承的，抽象类的子类必须重写父类的抽象方法，并提供方法体。若没有重写全部的抽象方法，仍为抽象类。  
不能用abstract修饰属性、私有方法、构造器、静态方法、final的方法。  

## 接口(Interface)
有时候必须从几个类中派生出一个子类，继承它们所有的属性和方法。但是，Java不支持多重继承。有了接口，就可以得到多重继承的效果。  
接口时抽象方法和常量值的定义的集合。从本质上讲，接口是一种特殊的抽象类，这种抽象类中只包含常量和方法的定义，而没有变量和方法的实现。  
一个类可以实现多个接口，接口也可以继承其他接口。  
实现接口类：`class SubClass implements InterfaceA{}`  
接口的特点：
+ 用interface来定义
+ 接口中的成员变量和方法都默认是由public static final修饰的
+ 接口没有构造器
+ 接口采用多层继承机制
接口定义举例：
```
public interface Runner{
    int ID = 1;
    void start();
    void run();
    void stop();
}
```
实现接口的类必须提供接口中所有方法的具体实现内容方可实例化。否则仍为抽象类。接口的主要用途就是被实现类实现。  
```
interface Runner{public void run();}
interface Swimmer{public void swim();}
class Person{public void eat(){...}}
class Man extends Person implements Runner,Swimmer{
    public void run(){...}
    public void swim(){...}
    public void eat(){...}
}
```
与继承关系类似，接口与实现类之间存在多态性。如果一个类既继承父类又实现接口，那么先继承再实现。  
```
public class Test{
    public static void main(String[] args){
        Test t = new Test();
        Man m = new Man();
        t.m1(m);
        t.m2(m);
        t.m3(m);
    }
    public void m1(Runner f){f.run();}
    public void m2(Swimmer s){s.swim();}
    public void m3(Person a){a.eat();}
}
```
接口的意义：  
子类继承抽象父类，当我们确实需要给父类增加一些方法时，具体实现的子类必须重写覆盖新增的抽象方法，否则只能变成抽象类。这样父类不可避免的变动对实现子孙类都有影响。解决方法是不直接在父类上下手，新建一个接口，在接口上拓展方法，其他需要的子类自行去实现接口。  
抽象类和接口的区别：  
+ 抽象类是对于一类事物的高度抽象，其中既有属性也有方法。当需要对一类事物抽象的时候，应该使用抽象类，好形成一个父类。
+ 接口是对方法的抽象，也就对一系列动作的抽象。当需要对一系列的动作抽象，就使用接口，需要使用这些动作的类去实现相应的接口即可。

## 类的成员之五：内部类
在Java中，允许一个类的定义位于另一个类的内部，前者称为内部类，后者成为外部类。  
Inner class一般用在定义它的类或语句块之内，在外部引用它时必须给出完整的名称，且名字不能与包含它的类名相同。  
Inner class可以使用外部类的私有数据，而外部类要访问内部类中的成员需要：内部类.成员或者内部类对象.成员。  
分类：
+ 成员内部类(static成员内部类和非static成员内部类)
+ 局部内部类(不谈修饰符)、匿名内部类

Inner class作为类的成员：
+ 可以声明为final的
+ 和外部类不同，inner class可声明为private或protected
+ inner class可以声明为static的，但此时就不能再使用外层类的非static的成员变量

Inner class作为类：
+ 可以声明为abstract类，因此可以被其他的内部类继承
+ 非static的内部类中的成员不能声明为static的，只有在外部类或static的内部类中在可声明为static成员

内部类的作用: 主要是解决Java不能多重继承的问题




# 异常处理
内容:
+ 异常概述
+ 异常处理机制
+ 使用try...catch...finally处理异常
+ 声明抛出异常
+ 人工抛出异常
+ 创建用户自定义异常类

## 异常概述
任何一种程序设计语言设计的程序在运行时都有可能出现错误，例如除数为0，数组下标越界，要读写的文件不存在等等。  
捕获错误最理想的是在编译期间，但有的错误只有在运行时才会发生。对于这些错误，一般有两种解决方法：
+ 遇到错误就终止程序的运行
+ 由程序员编写程序时，就考虑到错误的检测、错误消息的提示，以及错误的处理

Java异常：在Java语言中，将程序执行中发生的不正常情况称为"异常"。Java中的异常用于处理非预期的情况，如文件没找到，网络错误，非法的参数  
Java程序运行过程中所发生的异常事件可分为两类：
+ Error:JVM系统内部错误、资源耗尽等严重状况
+ Exception:其它因编程错误或偶然的外在因素导致的一般性问题，例如:
  + 空指针访问
  + 试图读取不存在的文件
  + 网络连接中断

Java异常类层次：
+ Throwable
  + Error
    + VirtualMachineError(JVM虚拟机错误)
      + StackOverFlowError(栈溢出)
      + OurOfMemoryError(内存溢出)
    + AWTError(其他错误)
  + Exception
    + IOException(磁盘文件读写异常)
      + EOFException(越过文件结尾继续读取数据)
      + FileNotFoundException
    + RuntimeException(运行时异常)
      + ArrithmeticException
      + MissingResouceException
      + ClassNotFoundException
      + NullPointerException
      + IllegalArgumentException
      + ArrayIndexOutOfBoundException
      + UnkownTypeException

## 异常处理机制
在编写程序时，经常要在可能出现错误的地方加上检测的代码，过多的分支会导致代码加长，可读性差。因此采用异常机制来防止程序中断。  
Java异常处理：Java采用异常处理机制，将异常处理的程序代码集中在一起，与正常的程序代码分开，使得程序简洁，并易于维护。  
Java提供的是异常处理的**抓抛模型**。  
出现异常时，自动生成一个异常类对象，该异常对象将被提交给Java运行时系统，这个过程称为抛出(throw)异常。  
如果一个方法内抛出异常，该异常会被抛到调用方法中。如果异常没有再调用方法中处理，它继续被抛给这个调用方法的调用者。这个过程将一直继续下去，知道异常被处理。这一过程称为捕获(catch)异常。  
如果一个异常回到main()方法，并且main()也不处理，则程序运行终止。程序员通常只能处理Exception，而对Error无能为力。
## 使用try-catch-finally处理异常  
异常处理机制是通过try-catch-finally语句实现的：
```
try{
    //可能产生异常的代码
}catch(Exception1 e){
    //当产生Exception1型异常时的处置措施
}catch(Exception2 e){
    //当产生Exception2型异常时的处置措施
}finally{
    //不管有没有异常无条件执行的语句
}
```
try catch是为了防止程序可能出现的异常。在捕获异常的代码块中，如果前面代码有异常，就不会执行后面的。

## 声明抛出异常
声明抛出异常是Java中处理异常的第二种方式。如果一个方法可能生成某种异常，但是并不能确定如何处理这种异常，则此方法应显示地声明抛出的异常，表明该方法将不对这些异常进行处理，而由该方法的调用者扶着处理。在方法声明中用throws子句可以声明抛出异常的列表，throws后面的异常类型可以实方法中产生的异常类型，也可以是它的父类。  
声明抛出异常举例：`public void readFile(String file) throws FileNotFoundException{}`  
子类重写方法不能抛出比重写方法范围更大的异常类型。

## 人工抛出异常
Java异常类对象除在程序执行过程中出翔异常时由系统自动生成并抛出，也可根据需要人工创建并抛出  
首先要生成异常类对象，然后通过throw语句实现抛出操作(提交给Java运行环境)
```
IOException = new IOException();
throw e;
```
可以抛出的一场必须是Throwable或其子类的实例。`throw new String("want to throw")`这句就不行

## 创建用户自定义异常类
用户自定义异常类MyException，用于描述数据取值范围错误信息。用户自己的异常类必须继承现有的异常类。Java提供的异常类一般是够用的，只有特殊情况需要自己定义异常类。
```
class MyException extends Exception{
    private int idNumber;
    public MyException(String message, int id){
        super(message);
        this.idNumber = id;
    }
    ...
}
```

# 集合
Java的集合类存放于java.util包中，是一个用来存放对象的容器
1. 集合只能存放对象。存入基本类型都会被转变成对象，每一种基本类型有其对应的引用类型
2. 集合存放的是多个对象的引用，对象本身还是放在堆内存中。
3. 集合可以存放不同的类型，不限数量的数据类型。

Java集合可分为Set、List和Map三种大体系：
+ Set:无序、不可重复的集合
+ List:有序、可重复的集合
+ Map:具有映射关系的集合

在JDK5之后，增加了泛型，Java集合可以记住容器中对象的数据类型

## HashSet
HashSet是Set接口的典型实现，大多数时候使用Set集合时都是用这个实现类。我们大多数时候说的set集合指的都是HashSet  
HashSet按Hash算法来存储集合中的元素，因此具有很好的存取和查找性能，具有以下特点：
+ 不能保证元素的排列顺序
+ 不可重复(指的是hashCode不相同)
+ HashSet不是线程安全的
+ 集合元素可以是null

当向HashSet集合中存入一个元素时，HashSet会调用该对象的hashCode()方法来得到该对象的hashCode值，然后根据hashCode值决定该对象在HashSet中的存储位置。  
如果两个元素的equals()方法返回true，但它们的hashCode()返回值不相等，hashSet将会把它们存储在不用的位置，但依然可以添加成功。

HashSet方法示例：
```
Set set = new HashSet();
set.add(obj);
set.remove(obj);
set.clear();

//使用迭代器遍历集合
Iterator<E> it = set.iterator();
while(it.hasNext()){
    System.out.println(it.next());
}

//使用for each迭代集合
for(Object obj:set){//把set的每一个值取出来赋值给obj，直到循环set的所有值
    System.out.println(obj);
}

System.out.println(set.size());//获取集合元素个数

//如果想要让集合只能存同样类型的对象，要使用泛型
Set<String> set = new HashSet<String>();//该集合只能存String类型的数据
```

## TreeSet
TreeSet是SortedSet接口的实现类，TreeSet可以确保集合元素处于排序状态。 
TreeSet支持两种排序方法：自然排序和定制排序。默认情况下，TreesSet采用自然排序，其他方法与HashSet相同。  
自然排序：TreeSet会调用集合元素的compareTo(Object obj)方法来比较元素之间的大小关系，然后将集合元素按升序排列。必须放入同样类的对象，否则则可能会发生类型转换异常，我们可以使用泛型来进行限制。  
定制排序：如果需要实现定制排序，则需要在创建TreeSet集合对象时，提供一个Comparator接口的实现类对象。由该Comparator对象负责集合元素的排序逻辑。
```
public class TesrDrive {
    public static void main(String[] args) {
        Person p1 = new Person(23, "张三");
        Person p2 = new Person(12, "Lucy");
        Person p3 = new Person(20, "Molly");

        Set<Person> set = new TreeSet<Person>(new Person());
        set.add(p1);
        set.add(p2);
        set.add(p3);

        for (Person p : set) {
            System.out.println(p.name + "  " + p.age);
        }
    }
}

class Person implements Comparator<Person> {
    int age;
    String name;

    public int compare(Person o1, Person o2) {
        if (o1.age > o2.age) {
            return 1;
        } else if (o1.age < o2.age) {
            return -1;
        } else {
            return 0;
        }
    }

    public Person() {}

    public Person(int age, String name) {
        this.age = age;
        this.name = name;
    }
}
```

## List与ArrayList
List代表一个元素有序、可重复的结合，集合中的每个元素都有其对应的顺序索引。  
List允许使用重复元素，可以通过索引来访问指定位置的集合元素。  
List默认按元素的添加顺序设置元素的索引。  
List集合里添加了一些根据元素索引来操作集合元素的方法。  
List示例：
```
public class Test {
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        list.add("a");
        list.add("b");
        list.add("c");
        list.add("a");//允许使用重复元素
        System.out.println(list.get(2));//通过索引来访问指定位置的集合元素

        list.add(1,"d");//在指定索引下标的位置插入数据

        List<String> list2 = new ArrayList<String>();
        list2.add("123");
        list2.add("456");
        list.addAll(2, list2);//在指定索引下标的位置插入集合

        System.out.println(list.indexOf("a"));//获取指定元素在集合中第一次出现的索引下标
        System.out.println(list.lastIndexOf("a"));//获取指定元素在集合中最后一次出现的索引下标

        list.remove(2);//根据指定的索引下标移除数据

        list.set(1, "f");

        List<String> sublist = list.subList(2, 4);//根据索引下标的起始位置截取一段元素形成新的集合

        System.out.println(list.size());//集合的长度


    }
}
```

ArrayList和Vector：是List接口的两个典型实现
+ Vector是一个古老的集合，通常建议使用ArrayList
+ ArrayList是线程不安全的，而Vector是线程安全的
+ 即使为保证List集合线程安全，也不推荐使用Vector

## Map
Map用于保存具有映射关系的数据，因此Map集合里保存着两组值，一组用于保存Map里的Key，另外一组用于保存Map里的Value  
Map中的Key和Value都可以是任何引用类型的数据  
Map中的Key不允许重复，即同一个Map对象的任何两个Key通过equals方法比较中返回false  
Key和Value之间存在单向一对一关系，即通过指定的Key总能找到唯一确定的Value  
Map接口与HashMap：
```
public class TesrDrive {
    public static void main(String[] args) {
        Map<String,Integer> map = new HashMap<String,Integer>();
        map.put("b",1);//添加数据
        map.put("c",2);
        map.put("e",2);

        System.out.println(map.get("b"));//根据key取值

        map.remove("c");//根据key移除键值对
        

        System.out.println(map.size());//map集合的长的

        System.out.println(map.containsKey("b"));//判断map集合是否包含指定的key

        System.out.println(map.containsValue(2));//判断map集合是否包含指定的value


        //遍历map集合
        map.keySet();//获取集合的key的集合
        map.values();//获取集合的所有value值
        Set<String> keys = map.keySet();

        for(String key : keys){
            System.out.println("key: "+key+", value: "+map.get(key));
        }

        //通过map.entrySet();遍历
        Set<Entry<String,Integer>> entrys = map.entrySet();
        for(Entry<String, Integer> en: entrys){
            System.out.println("key: "+en.getKey()+", value: "+en.getValue());
        }

        map.clear();//清空集合
    }
}
```

HashMap & Hashtable  
HashMap和Hashtable是Map接口的两个典型实现类：  
+ Hashtable是一个古老的Map实现类，不建议使用
+ Hashtable是一个线程安全的Map实现，但HashMap是线程不安全的
+ Hashtable不允许使用null作为key和value，而HashMap可以
+ 与HashSet集合一样，Hashtable和HashMap也不能保证其中key-value的顺序
+ Hashtable、HashMap判断两个Key相等的标准是：两个Key通过equals方法返回true且HashCode的值也相等
+ Hashtable相等的标准是：两个Value通过equalHashMap判断两个Values方法返回true

## 操作集合的工具类：Collections
Collections是一个操作Set、List和Map等集合的工具类
Collections中提供了大量方法堆集合元素机型排序、查询和修改等操作，还提供了对集合对象设置不可变、对集合对象实现同步控制等方法  

排序操作：
+ reverse(List)：反转List元素中的顺序
+ shuffle(List)：对List集合元素进行随机排序
+ sort(List)：根据元素的自然顺序对指定List集合元素按升序排序
+ sort(List，Comparator)：根据指定的Comparator产生的谁徐对List集合进行排序
+ swap(List，int，int)：将指定List集合中的i处元素和j处元素进行交换

查找、替换：
+ Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
+ Object max(Collection, Comparator)：根据Comparator指定的顺序，返回给定集合中的最大元素
+ min(Collection)和Object min(Collection, Comparator)：与上同
+ int frequency(Collection,Object)：返回指定集合中指定元素的出现次数
+ boolean replaceAll(List list, Object oldVal, Object newVal)：使用新值替换List对象的所有旧值

同步控制：  
Collections类中提供了多个synchronizedXxx()方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题。

# 泛型
+ 为什么使用泛型
+ 泛型怎么用

## 为什么要有泛型Generic？  
泛型，JDK1.5新加入的，解决数据类型的安全性问题，其主要原理是在类声明时通过一个表示表示类中某个属性的类型或者是某个方法的返回值及参数类型。这样在类声明或实例化时只要指定好需要的具体的类型即可。  
Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会生产ClassCastException异常。保证类型安全，只能使设置类型存入。同时，代码更加简洁、健壮。  
Java中的泛型，只在编译阶段有效。在编译过程中，正确检验泛型结果后，会将泛型的相关信息擦出，并且在对象进入和离开方法的边界处添加类型检查和类型转换的方法。也就是说，泛型信息不会进入到运行时阶段。

## 泛型的使用
1. 泛型类
2. 泛型方法
3. 泛型接口

泛型类：  
```
public class Test {
    public static void main(String[] args) {

        A<String> a1 = new A<String>();//在new A的对象指定泛型的类型String
        a1.setKey("xxx");//对象使用setKey(T key)方法中的key形参就是String
        System.out.println(a1.getKey());//T getKey(),返回值就有new对象确定返回值是String

        A<Integer> a2 = new A<Integer>();
        a2.setKey(1);
        System.out.println(a2.getKey());

        A a3 = new A<>();//不指定泛型，相当于指定了一个Object类型
        a3.setKey(new Object());
        Object obj = a3.getKey();

        //同样的类，但是在new对象时泛型指定不用的数据类型，这些对象不能互相复制
        a1 = a2;

    }
}

class A<T>{//此处的T可以任意取，一般使用大写T，意为type
    private T key;

    public void setKey(T key){
        this.key = key;
    }

    public T getKey(){
        return this.key;
    }
}

```

## 泛型接口
```
public class Test {
    public static void main(String[] args) {
        B1<Object> b1 = new B1<Object>();
        B1<String> b2 = new B1<String>(); 
        B2 b3 = new B2();
    }
}

interface IB<T>{
    T test(T t);
}

//未传入泛型实参时，与泛型类的定义相同，在声明类的时候，需将泛型类的声明也一起加到类中
class B1<T> implements IB<T>{

    @Override
    public T test(T t){
        return t;
    }

}

//如果实现接口时指定接口的泛型的具体数据类型，这个类实现接口所有方法的位置都要泛型替换实际的具体数据类型
class B2 implements IB<String>{
    @Override
    public String test(String t){
        return null;
    }
}
```

## 泛型方法
```
public static void main(String[] args) {
        Cc<Object> c = new Cc<Object>();
        c.test("s");
        // 泛型方法在调用之前没有固定的数据类型
        // 在调用时，传入的参数是什么类型，就会把泛型改成什么类型
        //也就是说，泛型方法会在调用时确定泛型的具体数据类型
        Integer i = c.test1(2);// 传入的参数是Integer，泛型就固定成Integer，返回值就是Integer
        Boolean b = c.test1(true);//传入的参数是Boolean，泛型就固定成Boolean，返回值就是Boolean
    }
}

class Cc<E> {
    private E e;

    // 在静态方法中，不能使用类定义泛型，如果要使用泛型，只能使用静态方法自己定义的泛型
    public static <T> void test3(T t) {
        System.out.println(t);
    }

    // 无返回值的泛型方法
    public <T> void test(T s) {
        // 在类上定义的泛型可以在普通的方法中使用
        System.out.println(this.e);
        T t = s;
    }

    // 有返回值的泛型方法
    public <T> T test1(T s) {
        return s;
    }

    // 可变参数的泛型方法
    public <T> void test2(T... strs) {
        for (T s : strs) {
            System.out.println(s);
        }
    }
}

```

## 通配符
有限制的通配符：  
+ <? extends Person>  只允许泛型为Person及Person子类的引用调用(无穷小，Person)
+ <? super Person>  只允许泛型为Person及Person父类的引用调用(无穷大,Person)
+ <? extends Comparable> 只允许泛型为实现Comparable接口的实现类的引用调用

# 枚举和注解
+ 枚举
  + 定义
  + 自实现枚举类
  + 使用enum定义枚举类
  + 实现接口的枚举类
  + 枚举类的方法
+ 注解

## 枚举
在某些情况下，一个类的对象是有限而且固定的。例如季节类只能有四个对象。  
手动实现枚举类：  
+ private修饰构造器
+ 属性使用private final修饰
+ 把该类的所有实例都使用public static final来修饰


枚举类和普通类的区别：
+ 使用enum定义的枚举类默认继承了java.lang.Enum类
+ 枚举类的构造类只能使用private访问控制符
+ 枚举类的所有实例必须在枚举类中显式列出(,分隔 ;结尾)列出的实例系统会自动添加public static final修饰
+ 所有的枚举类都提供了一个values方法，该方法可以很方便地遍历所有的枚举值

```
public class TesrDrive {
    public static void main(String[] args) {
        Season spring = Season.SPRING;//获取一个Season的对象
        spring.showInfo();
        
        Season spring1 = Season.SPRING;
        System.out.println(spring.equals(spring1));
        //true，每次执行Season.spring得到的是相同的对象，枚举类中的每个枚举都是单例模式的
        
        spring1.test();//实现接口的枚举类方法
    }
}

enum Season implements ITest {
    SPRING("春天", "春暖花开"),//此处相当于在调用有参的私有构造方法
    SUMMER("夏天", "炎炎夏日"), 
    AUTUMN("春天", "秋高气爽"), 
    WINTER("春天", "寒风凛冽");

    private final String name;
    private final String desc;

    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public void showInfo() {
        System.out.println(this.name + ":" + this.desc);
    }

    @Override
    public void test() {
        System.out.println("这是实现的ITest接口的test方法");

    }
}

interface ITest{
    void test();
}

```

## 注解(Annotation)
从JDK5.0开始，Java增加了对元数据(MetaData)的支持，也就是Annotation  
Annotation其实就是代码里的特殊标记，这些标记可以在编译，类加载，运行时被读取，并执行相应的处理。通过使用Annotation，程序员可以在不改变原有逻辑的情况下，在源文件中嵌入一些补充信息。  
Annotation可以像修饰符一样被使用，可用于修饰包，类，构造器，方法，成员变量，参数，局部变量的声明，这些信息被保存在Annotation的"name=value"对中。  
Annotation能被用来为程序元素(类，方法，成员变量等)设置元数据  
使用Annotation时要在其前面增加@符号，并把该Annotation当成一个修饰符使用，同于修饰它支持的程序元素。三个基本的Annotation：
+ @Override:限定重写父类方法，该注释只能用于方法
+ @Deprecated:用于表示某个程序元素(类，方法等)已过时
+ @SuppressWarnings:抑制编译器警告

自定义Annotation(了解即可，用不太上)：  
定义新的Annotation类型使用@interface关键字  
Annotation的成员变量在Annotation定义中以无参数方法的形式来声明。其方法名和返回值定义了该成员的名字和类型。  
可以在定义Annotation的成员变量时为其指定初始值，指定成员变量的初始值可使用default关键字  
没有成员定义的Annotation称为标记，包含成员变量的Annotation称为元数据Annotation

# IO流(Input&Output)
主要内容：
+ java.io.File类的使用(File:计算机操作系统中的文件和文件夹)
+ IO原理及流的分类
+ 文件流(基于文件的操作):FileInputStream/FileOutputStream/FileReader/FileWriter
+ 缓冲流(基于内存的操作):BufferedInputStream/BufferedOutputStream/BufferedReader/BufferedWriter
+ 转换流:InputStreamReader/OutputStreamReader
+ 打印流(了解):PrintStream/PrintWriter
+ 数据流(了解):DataInputStream/DataOutputStream
+ 对象流(涉及序列化、反序列化):ObjectInputStream/ObjectOutputStream
+ 随机存取文件流:RandomAccessFile(随机:一个多行文件可以在中间行插入读取数据)

通过程序把一个图片放到某一个文件夹，把图片转换为一个数据集(例如二进制)，把这些数据一点一点传到文件夹，这个传递过程类似水的流动，我们就可以称这个整体的数据集是一个数据流。  


## File类
java.io.File类：文件和目录路径名的抽象表示形式，与平台无关  
File能新建、删除、重命名文件和目录，但File不问访问文件内容本身。如果需要访问文件内容本身，则需要使用输入/输出流。  
File对象可以作为参数传递给流的构造函数  
File类的常见构造方法:
+ `public File(String pathname)` 以pathname为路径创建File对象，可以实绝对或者相对路径，如果pathname时相对路径，则默认的当前路径在系统属性user.dir中储存
+ `public File(String parent, String child)` 以parent为父路径，child为子路径创建File对象。FIle的静态属性String separator储存了当前路径系统的路径分隔符。UNIX中为'/', Window中为'\\'  

File类的方法:

```
File f = new File(filePath);//实例化一个File对象
f.getName();//返回文件或文件夹名称
f.getPath();//返回文件或文件夹new File对象时的路径
f.getAbsolutePath();//获取文件或文件夹的绝对路径
f.getAbsoluteFile();//返回一个用当前的文件的绝对路径构建的file对象
f.getParent(); //返回父级路径
f.renameTo(new File(filePath));//给文件或文件夹重命名

f.exists();//返回判断文件或文件夹是否存在的布尔值
f.canWrite();//返回判断文件或文件夹是否可写的布尔值
f.canRead();//返回判断文件或文件夹是否可读的布尔值
f.isFile();//返回判断当前File对象是不是文件的布尔值
f.isDirectory();//返回判断当前File对象是不是文件夹的布尔值

f.lastModified();//返回文件的最后修改时间，返回毫秒数
f.length();//返回文件的长度，单位是字节数

f.createNewFile();//创建新的文件
f.delete();//删除文件

f.mkdir();//创建单层目录
f.mkdirs();//创建多级目录
f.list();//返回当前文件夹子集的名称的字符串列表
f.listFiles();//返回当前文件夹子集的File对象列表
```

文件夹的遍历：  
无论层级有多深，遍历某文件夹下所有的目录与文件，需要使用递归的方式来实现。  
```
public class TesrDrive {
    public static void main(String[] args) {
        File f = new File("C:/Users/41361/Documents/Workspace/Java/Exercise/");
        new TesrDrive().test(f);
    }

    public void test(File file){
        if(file.isFile()){
            System.out.println(file.getAbsolutePath()+"是文件");
        }else{
            System.out.println(file.getAbsolutePath()+"是文件夹");
            //如果是文件夹，就可能有子文件夹或者文件
            File [] fs = file.listFiles();
            if(fs!=null&&fs.length>0)
            for(File ff :fs){
                test(ff);//递归
            }
        }
    }
}
```

## JavaIO原理
IO流用来处理设备之间的数据传输。Java程序中，对于数据的输入输出以"流(stream)"的方式进行。java.io包下提供了各种"流"类和接口，用以获取不同种类的数据，并通过标准的方法输入或输出数据。  
流的分类：  
按操作数据单位不同分为：字节流(8 bit),字符流(16 bit)  
按数据流的流向不同分为：输入流，输出流  
按流的角色的不同分为：节点流，处理流
抽象基类|字节流|字符流
---|---|---
输入流|InputStream|Reader
输入流|OutputStream|Writer
Java的IO流共涉及40多个类，实际上非常规则，都是从如上4个抽象基类派生的。由这四个类派生出来的子类名称都是以其都是以其父类名作为子类名后缀。 
### 字节流
注意：文件字节流非常通用，可以用来操作字符的文档，还可以操作任何的其他类型文件(图片、压缩包等等)，因为字节流直接使用二进制  
文件字节输入流：  
```
public void testFileInputStream(String inPath) {
    try {
        FileInputStream in = new FileInputStream(inPath);
        byte[] b = new byte[1024];// 设置一个byte数组接受读取的文件内容
        int len = 0;
        while ((len = in.read(b)) != -1) {
            // in.read(b);有一个返回值是读取的数据长度，如果读取到最后一个数据还会向后读一个，也就意味着当in.read的返回值是-1的时候整个文件就读取完毕了
            System.out.println(new String(b, 0, len));
            // new String(b,0,len),参数1是缓冲数据的数组，参数2是从数组的那个位置开始转化字符串，参数3是总共转化几个字节
        }
        in.close();// 注意，流在使用完毕后一定要关闭
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

文件字节输出流：
```
public void testFileOutputStream(String outPath) {
    try {
        FileOutputStream out = new FileOutputStream("Java/Exercise/file1.txt");
        String str = "If I fell.";
        out.write(str.getBytes());// 把数据写到内存
        out.flush();// 把内存中的数据写到硬盘
        out.close();// 关闭流
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

文件字节流复制文件：
```
public void copy(String inPath, String outPath) {
    try {
        FileInputStream fr = new FileInputStream(inPath);
        FileOutputStream fo = new FileOutputStream(outPath);
        byte[] b = new byte[1024];
        int len = 0;
        while ((len = fr.read(b)) != -1) {
            fo.write(b, 0, len);
        }
        fo.flush();
        fo.close();
        fr.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

### 字符流
文件字符输入流：
读取文件操作步骤：
1. 建立一个流对象，将已存在的一个文件加载进流。`FileReader fr = new FileReader("Test.txt");`
2. 创建一个临时存放数据的数组。`char[] ch = new char[1024];`
3. 调用流对象的读取方法将流中的数据读入到数组中。`fr.read(ch);`

文件字符输入流：
```
public void testFileReader(String inPath) {
  try {
      FileReader fr = new FileReader(inPath);
      char[] c = new char[100];
      int len = 0;
      while ((len = fr.read(c)) != -1) {
          System.out.println(new String(c, 0, len));
      }
      fr.close();
  } catch (Exception e) {
      e.printStackTrace();
  }
}
```

文件字符输出流：
```
public void testFileWriter(String text, String outPath) {
  try {
      FileWriter fw = new FileWriter(outPath);
      fw.write(text);
      fw.flush();
      fw.close();
  } catch (Exception e) {
      e.printStackTrace();
  }
}
```
文件字符流拷贝文档：
```
public void copy(String inPath, String outPath){
  try {
      FileReader fr = new FileReader(inPath);
      FileWriter fw = new FileWriter(outPath);
      char[] c = new char[100];
      int len = 0;
      while((len = fr.read(c))!=-1){
          fw.write(c,0,len);
      }
      fw.flush();
      //关闭流的时候本着最晚开的最早关，一次关的顺序
      fw.close();
      fr.close();
  } catch (Exception e) {
      e.printStackTrace();
  }
}
```

## 处理流之一：缓冲流
字节流和字符流都是计算机与硬盘之间发生的io操作，基于硬盘的读写相对较慢，这个操作的速度受到硬盘读写速度的制约。为了提高数据读写的速度，Java API提供了带缓冲功能的流类，在使用这些流类时，会创建一个内部缓冲区数组。根据数据操作单位可以把缓冲流分为：
+ BufferedInputStream
+ BufferedOutputStream
+ BufferedReader
+ BufferedWriter

缓冲流要"套接"在相应的节点流之上，对读写的数据提供了缓冲的功能，提高了读写的效率，同时增加了一些新的方法。对于输出的缓冲流，写出的数据会现在内存中缓存，使用flush()将会使内存中的数据立刻写出。  
缓冲流就是先把数据冲内存里，在内存中去做io操作，基于内存的io操作大概能比基于硬盘的io操作快75000多倍。

缓冲字节输入流：
```
public void testBufferedInputStream(String inPath) {
    try {
        // 文件字节输入流对象
        FileInputStream in = new FileInputStream(inPath);
        // 把文件字节输入流放到缓冲字节输入流对象
        BufferedInputStream br = new BufferedInputStream(in);
        byte[] b = new byte[500];
        int len = 0;
        while ((len = br.read(b)) != -1) {
            System.out.println(new String(b, 0, len));
        }
        br.close();
        in.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

缓冲字节输出流：
```
public void testBufferedOutputStream(String outPath) {
    try {
        FileOutputStream out = new FileOutputStream(outPath);
        BufferedOutputStream bo = new BufferedOutputStream(out);
        String s = "hello world";
        bo.write(s.getBytes());
        bo.flush();
        bo.close();
        out.close();
    } catch (Exception e) {
        e.printStackTrace();
    }  
}
```

缓冲字节流复制文件：
```
public void copy(String inPath, String outPath){
    try {
        BufferedInputStream br = new BufferedInputStream(new FileInputStream(inPath));
        BufferedOutputStream bo = new BufferedOutputStream(new FileOutputStream(outPath));
        byte[] b = new byte[1024];
        int len = 0;
        while((len = br.read(b))!=-1){
            bo.write(b,0,len);
            bo.flush();
            bo.close();
            br.close();
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

缓冲字节输入流：
```
public void testBufferedReader(String inPath) {
    try {
        FileReader fr = new FileReader(inPath);
        BufferedReader br = new BufferedReader(fr);
        char[] c = new char[100];
        int len = 0;
        while ((len = br.read(c)) != -1) {
            System.out.println(new String(c, 0, len));
        }
        br.close();
        fr.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

缓冲字节输出流：
```
public void testBufferedWriter(String text,String outPath) {
    try {
        FileWriter fw = new FileWriter(outPath);
        BufferedWriter bw = new BufferedWriter(fw);
        bw.write(text);
        bw.flush();
        bw.close();
        fw.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

缓冲字节流复制文件：
```
public void copy1(String inPath, String outPath) {
    try {
        BufferedReader br = new BufferedReader(new FileReader(inPath));
        BufferedWriter bw = new BufferedWriter(new FileWriter(outPath));
        char[] c= new char[1024];
        int len=0;
        while((len = br.read(c))!=-1){
            bw.write(c, 0, len);
        }
        bw.flush();
        bw.close();
        br.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## 处理流之二：转换流
转换流提供了在字节流和字符流之间的转换，Java API提供了两个转换流：InputStreamReader和OutputStreamWriter。  
字节流的数据都是字符时，转成字符流操作更高效。字符流的数据都是字节时，转成字节流操作更高效。  
InputStreamReader用于将字节流中读取到的字节按指定字符集解码成字符。需要和InputStream"套接"。  
OutputStreamWriter用于将字符流中读取到的字符按指定字符集解码成字节。需要和OutputStream"套接"。
转换字节输入流为字符输入流:  
```
public void testInputStreamReader(String inPath){
    try {
        FileInputStream fs = new FileInputStream(inPath);
        //把字节流转化为字符流，参数1是字节流，参数2是编码
        //在转换字符流时，设置的字符集编码要与读取文件的数据的编码一致，否则会出现乱码
        InputStreamReader in = new InputStreamReader(fs,"UTF-8");
        char[] c = new char[1024];
        int len=0;
        while((len=in.read(c))!=-1){
            System.out.println(new String(c,0,len));
        }
        in.close();
        fs.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
转换字节输出流为字符输出流:
```
public void testOutputStreamWriter(String text,String outPath){
    try {
        FileOutputStream fo = new FileOutputStream(outPath);
        OutputStreamWriter ow = new OutputStreamWriter(fo,"UTF-8");
        ow.write(text);
        ow.flush();
        ow.close();
        fo.close(); 
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## 处理流之三：标准输入输出流
System.in和System.out分别代表了系统标准的输入和输出设备。默认输入设备是键盘，输出设备是显示器。  
System.in的类型是InputStream。System.out的类型是PrintStream。  
终端输入数据：
```
public void testSystemIn(){
    try {
        //创建一个接受键盘输入数据的输入流
        InputStreamReader is = new InputStreamReader(System.in);
        //把输入流放到缓冲流里
        BufferedReader br = new BufferedReader(is);
        String str = "";//定义一个临时接受数据的字符串
        while((str=br.readLine())!=null){
            System.out.println(str);
        }
        br.close();
        is.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
把终端输入的数据保存在txt文件中：
```
public void write2TXT(String outPath) {
    try {
        InputStreamReader is = new InputStreamReader(System.in);
        BufferedReader br = new BufferedReader(is);
        BufferedWriter out = new BufferedWriter(new FileWriter(outPath));
        String line="";
        while((line=br.readLine())!=null){
            if(line.equals("over")){
                break;
            }
            out.write(line);
        }
        out.flush();
        out.close();
        br.close();
        is.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## 处理流之四：打印流(了解)
在整个IO包中，打印流是输出信息最方便的类。  
PrintStream(字节打印流)和PrintWriter(字符打印流)提供了一系列重载的print和printIn方法，用于多种数据类型的输出。  
PrintStream和PrintWriter的输出不会抛出异常，有自动flush功能。System.out返回的是PrintStream实例。

## 处理流之五：数据流(了解)
为了方便地操作Java语言的基本数据类型的数据，可以使用数据流。数据流有两个类：(用于读取和写出基本数据类型的数据)DataInputStream和DataOutputStream分别"套接"在InputStream和OutputStream节点流上。用数据输出流写到文件的中的基本数据类型的数据，是乱码的，不能直接辨认，需要数据输入流读取，且输入流类型与写入时输出流相同。    
DataInputStream中的方法：  
```
boolean readBoolean()   byte readByte()
char readChar()         float readFloat()
double readDouble()     short readShort()
long readLong()         int ReadInt()
String readUTF()        void readFully(byte[] b)
```
DataOutputStream中的方法：
将上述方法改成write即可

## 处理流之六：对象流
ObjectInputStream和ObjectOutputStream  
用于存储和读取对象的处理流。它的强大之处就是可以把Java中的对象写入到数据源中，也能把对象从数据源中还原回来。正是因为需要保存对象到硬盘(对象的持久化)和对象的网络传输，就产生了对象的输入与输出流。  
序列化(Serialize)：用ObjectOutStream类将一个Java对象写入IO流中  
反序列化(Deserialize)：用ObjectInputStream类从IO流中恢复该Java对象  
ObjectOutputStream和ObjectInputStream不能序列化static和transient修饰的成员变量。序列化与反序列化针对的是对象的各种属性，不包括类的属性。 
### 对象的序列化： 
对象序列化机制允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传输到另一个网络节点。当其他程序获取了这种二进制流，就可以恢复成原来的Java对象。  
序列化的好处在于可将任何实现类Serializable接口的对象转化为字节数据，使其在保存和传输时可被还原。  
序列化时RMI(Remote Method Invoke-远程方法调用)过程的参数和返回值都必须实现的机制，而RMI时JavaEE的基础。因此序列化机制是JavaEE平台的基础。  
如果需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下的两个接口之一：Serialzable,Externalizable
```
public class Test {
    public static void main(String[] args) {
        Person p = new Person();
        p.name = "Gao Yibo";
        p.age  = 20;
        Test.testSerialize(p,"object.txt");
        Object obj = Test.testDeserialize("object.txt");
        Person p1 = (Person) obj;
        System.out.println(p1.name+": "+p1.age);
    }

    public static void testSerialize(Object obj, String outPath){
        try {
            ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream(outPath));
            out.writeObject(obj);
            out.flush();
            out.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static Object testDeserialize(String inPath){
        //注意：对象的序列化与反序列化使用的类要严格一致，包名，类名，类结构等等都要一致，否则有类型转换异常
        try {
            ObjectInputStream in = new ObjectInputStream(new FileInputStream(inPath));
            Object obj = in.readObject();
            return obj;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}

class Person implements Serializable{
    //一个表示序列化版本表示的静态变量用来表示类的不同版本的兼容性
    private static final long serialVersionUID = 1L;
    String name;
    int age;
}
```

## RandomAccessFile类
RandomAccessFile类支持"随机访问"的方式，程序可以直接跳到文件的任意地方来读写文件。支持只访问文件的部分内容，可以向已存在的文件后追加内容。  
RandomAccessFile对象包含一个记录指针，用以表示当前读写处的位置。RandomAccessFile类对象可以自由移动记录指针：
+ `long getFilePointer()` 获取文件记录指针的当前位置
+ `void seek(long pos)` 将文件记录指针定位到pos位置

构造器：  
+ `public RandomAccessFile(File file,String mode)`
+ `public RandomAccessFile(String name,String mode)`

创建RandomAccessFile类实例需要指定一个mode参数，该参数指定RandomAccessFile的访问模式，最常用r和rw：
+ r:以只读方式打开
+ rw：读取和写入
+ rwd：读取和写入以及同步文件内容的更新
+ rws：读取和写入，同步文件内容和元数据的更新

随机读：
```
public static void testRandomAccessFileRead(String inPath) {
    try {
        RandomAccessFile ra = new RandomAccessFile(inPath, "r");
        ra.seek(0);// 设置读取文件的起始点
        byte[] b = new byte[1024];
        int len = 0;
        while ((len = ra.read(b)) != -1) {
            System.out.println(new String(b, 0, len));
        }
        ra.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
随机写：
```
public static void testRandomAccessFileWrite(String text,String outPath){
    try {
        RandomAccessFile ra = new RandomAccessFile(outPath, "rw");
        ra.seek(0);//0代表从开头写
        //ra.seek(ra.length());//ra.length代表从文件的最后结尾写，也就是文件的追加，如果在文件的中间写，新写的内容会覆盖掉原有的内容
        ra.write(text.getBytes());
        ra.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## 流的基本应用小结
流是用来处理数据的。处理数据时，一定要先明确数据源与数据目的地。数据源可以是文件，可以是硬盘。数据目的地可以实文件、显示器或者其他设备。而流只是在帮助数据进行传输，并对传输的数据进行处理，比如过滤处理，转换处理等。  
+ 字节流-缓冲流(重点)
  + 输入流:InputStream-FileInputStream-BufferedInputStream
  + 输出流:OutputStream-FileOutputStream-BufferedOutputStream
+ 字符流-缓冲流(重点)
  + 输入流:Reader-FileReader-BufferedReader
  + 输出流:Writer-FileWriter-BufferedWriter
+ 转换流
  + InputStreamReader
  + OutputStreamWriter
+ 对象流(难点)
  + 序列化:ObjectInputStream
  + 反序列化:ObjectOutputStream
+ 随机存取流RandomAccessFile(掌握读取、写入)


# 反射
主要内容:
1. 理解Class类并实例化Class类对象
2. 运行时创建类对象并获取类的完整结构
3. 通过反射调用类的指定方法、指定属性
4. 动态代理

反射机制：通过一个抽象的类名能够在自己加载类的内存中找到相匹配类的具体信息  
Java能够反射的前提：已经加载过这个类  
Java Reflection(反射)是被视为动态语言的关键，反射机制允许程序在执行期借助Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。  
Java反射机制提供的功能：  
+ 在运行时判断任意一个对象所属的类
+ 在运行时构造任意一个类的对象
+ 在运行时判断任意一个类所具有的成员变量和方法
+ 在运行时调用任意一个对象的成员变量和方法
+ 生成动态代理(关键应用)

一个类极限情况都有什么？  
+ 一个父类，多个接口
+ 多个属性
+ 多个构造方法，包括构造的参数及其类型
+ 多个普通方法，包括方法的参数及其类型，返回值类型

反射相关的主要API(要涵盖一个类中要包含的所有东西):
+ java.lang.Class 代表一个类
+ java.lang.reflect.Method 代表类的方法
+ java.lang.reflect.Field 代表类的成员变量
+ java.lang.reflect.Constructor 代表类的构造方法

## 一、Class类
在Object类中定义了以下的方法，此方法将被所有子类继承:`public final Class getClass`, 以上的方法返回值的类型是一个Class类，此类是Java反射的源头，实际上所谓反射从程序的运行结果来看也很好理解，即：可以通过对象反射求出类的名称。Class对随意的类进行高度的抽象，是可以描述所有类的类。  
反射可以得到的信息：某个类的属性、方法和构造器、某个类到底实现了哪些接口。对于每个类而言，JRE都为其保留一个不变的Class类型的对象。一个Class对象包含了特定某个类的有关信息。  
Class本身也是一个类  
Class对象只能由系统建立对象  
一个类在JVM中只会有一个Class实例  
一个Class对象对应的是一个加载到JVM中的一个.class文件  
每个类的实例都会记得自己是由那个Class实例所生成  
通过Class可以完整地得到一个类中的完整结构  



实例化Class类对象的三种方法：
```
Class c0 = Person.class;// 通过类名.class创建指定类的Class实例

Class c1 = p.getClass();// 通过一个类的实例对象.getClass(),获取对应实例对象类的实例

try {
    Class c2 = Class.forName("Person");
    //通过Class的静态方法forName()来获取一个类的Class实例
    //其中参数是要获取的Class示例的全路径(包名.类名)
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}

ClassLoader cl = this.getClass().getClassLoader();
Class c4 = cl.loadClass("Person")
```

## 二、通过反射调用类的完整结构
内容：Field、Method、Constructor、Superclass、Interface、Annotation  
通过反射得到类的父类和类的全部接口：
```
public static void main(String[] args) {
    try {
        Class clazz = Class.forName("Exercise.Student");
        //通过包名.雷鸣的字符串，调用Class.forName方法获取指定类的class实例
        
        Class superClazz = clazz.getSuperclass();//获取父类
        System.out.println(superClazz.getName());

        Class[] interfaces = clazz.getInterfaces();//获取当前类的所有接口
        for(Class c:interfaces){
            System.out.println(c.getName());
        }
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
}
```
构造器：
```
public static void main(String[] args) {
    try {
        Class clazz = Class.forName("Exercise.Student");
        Constructor[] cons = clazz.getConstructors();//获取类的公有构造方法
        Constructor[] cons1 = clazz.getDeclaredConstructors();//获取类的所有构造方法
        for(Constructor c: cons1){
            System.out.println(c.getName());//获取构造方法名称
            System.out.println(c.getModifiers());
            //获取构造方法修饰符，返回1代表public返回2代表private
            Class[] paramClazz = c.getParameterTypes();
            for(Class pc: paramClazz){
                System.out.println(pc.getName());//获取构造方法参数
            }
        }
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }    
}
```
通过反射创建一个对象：
```
public static void main(String[] args) {
    try {
        Class clazz = Class.forName("Exercise.Student");
        try {
            Object obj = clazz.newInstance();//相当于调用无参共有构造方法
            Student stu = (Student) obj;

            Constructor c = clazz.getConstructor(String.class);
            //指定获取有一个参数并且为String类型的共有的构造方法
            Student stu1 = (Student)c.newInstance("ZZFLS");
            //通过newInstance实例化对象，相当于调用构造方法
            System.out.println(stu1.school);

            //通过反射机制可以强制地调用私有的构造方法
            Constructor c1 = clazz.getDeclaredConstructor(String.class,int.class);
            c1.setAccessible(true);//解除私有的封装，下面就可以对这个私有方法直接调用
            Student stu2 = (Student)c1.newInstance("Molly",12);

        } catch (Exception e) {
            e.printStackTrace();
        }
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }    
}
```
获取方法：   
```
public static void main(String[] args) {
    try {
        Class clazz = Class.forName("Exercise.Student");
        Method[] mt = clazz.getDeclaredMethods();// 获取类所有方法
        Method[] ms = clazz.getMethods();// 获取类所有公有方法
        for (Method m : mt) {
            System.out.println("方法名：" + m.getName());
            System.out.println("返回值类型：" + m.getReturnType());
            System.out.println("修饰符：" + m.getModifiers());

            Class[] pcs = m.getParameterTypes();
            if (pcs != null && pcs.length > 0) {
                for (Class pc : pcs) {
                    System.out.println("参数类型：" + pc.getName());
                }
            }
            System.out.println("-------------------------------");
        }

    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
}
```

获取类的属性和类所在的包：
```
public static void main(String[] args) {
    try {
        Class clazz = Class.forName("Exercise.Student");

        Package p = clazz.getPackage();
        System.out.println(p.getName());

        Field[] fs = clazz.getFields();//获取类的公有属性,包含父类的公有属性
        Field[] fa = clazz.getDeclaredFields();//获取本类的(不包含父类的)所有属性
        for(Field f:fa){
            System.out.println("修饰符："+f.getName());
            System.out.println("属性的类型："+f.getType());
            System.out.println("名称："+f.getModifiers());
        }

    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
}
```

## 三、通过反射调用类中的指定方法、指定属性
### 调用指定方法：
1. 通过Class类的getMethod(String name,Class...parameterTypes)方法获取一个Method对象，并设置此方法操作时所需要的参数类型。
2. 之后只用Object invoke(Object obj, Object[] args)进行调用，并向方法中传递要设置的obj对象的参数信息。

```
public static void main(String[] args) {
  try {
      Class clazz = Class.forName("Exercise.Student");

      Constructor c = clazz.getConstructor();//获取无参构造
      Object obj = c.newInstance();//使用无参构造创建对象

      //调用公有方法
      Method m = clazz.getMethod("setInfo", String.class,String.class);//获取想要调用的方法
      m.invoke(obj, "Mike","ZZFLS");//参数1是需要实例化的对象，后面的是调用当前的方法的实参

      //调用重载方法
      Method m1 = clazz.getMethod("setInfo", int.class);
      m1.invoke(obj, 18);

      //调用私有方法
      Method m2 = clazz.getDeclaredMethod("test", String.class);
      m2.setAccessible(true);//解除私有封装，可以强制调用私有方法
      m2.invoke(obj, "李四");

      //调用有返回值但没有参数的方法
      Method m3 = clazz.getMethod("getSchool");
      String school = (String) m3.invoke(obj);

  } catch (Exception e) {
      e.printStackTrace();
  }
}
```

### 调用指定属性：
在反射机制中，可以直接通过Field类操作类中的属性，通过Field类提供的set()和set()方式就可以完成设置和去的属性内容的操作。
```
public static void main(String[] args) {
  try {
      Class clazz = Class.forName("Exercise.Student");
      Constructor con = clazz.getConstructor();
      Student stu = (Student) con.newInstance();

      Field f = clazz.getField("school");//获取名称为school的属性
      f.set(stu, "ZZFLS");
      String school = (String) f.get(stu);
      System.out.println(school);

      //如果是私有的属性
      Field f1 = clazz.getDeclaredField("privateField");
      f1.setAccessible(true);//接触私有封装，就可以强制调用属性
      f1.set(stu, "测试私有属性");

  } catch (Exception e) {
      e.printStackTrace();
  }
}
```

## 四、Java动态代理
一个Java项目，其中有100个Java类，每个Java类有10个方法，这总共100个方法。现在有这样一个需求，需要在每个java方法上加上2句话，在方法执行前和执行后输出某句话。如何实现？----动态代理  
Proxy：专门完成代理的操作类，是所有动态代理的父类。通过此类为一个或多个接口动态地生成实现类。  
动态代理步骤：
1. 创建一个实现接口InvocationHandler的类，它必须实现invoke方法，以完成代理的具体操作
2. 创建被代理的类以及接口
3. 通过Proxy静态方法newProxyInstance创建一个Subject接口代理
4. 通过Subject代理调用RealSubject实现类的方法
```
public static void main(String[] args) {
  ITestDemo test = new TestDemoimpl();
  /**
   * 注意：如果一个对象想要通过proxy被代理
   * 那么这个对象的类一定要有相应的接口
   * 就像本例中的ITestDemo接口和实现类ITestDemoImpl
   */
  test.test1();
  test.test2();
  //需求：在执行方法前和方法后打印方法名
  //打印方法名需要和当时调用的方法保持一致 
  InvocationHandler handler = new ProxyDemo(test);
  /**
   * Proxy.newProxyInstance(loader, interfaces, h)有三个参数。
   * 参数1是代理对象的类加载器
   * 参数2是被代理的对象的接口
   * 参数3是代理对象
   * 返回的值就是成功被代理后的对象,是Object类型，需要类型转换
   */
  
  ITestDemo t =(ITestDemo)Proxy.newProxyInstance(handler.getClass().getClassLoader(), test.getClass().getInterfaces(),handler);
  t.test1();
  t.test2();
}

interface ITestDemo{
    void test1();
    void test2();
}

class TestDemoimpl implements ITestDemo{
    @Override
    public void test1() {
        System.out.println("执行test1方法");
    }
    
    @Override
    public void test2() {
        System.out.println("执行test2方法");
    }
}

class ProxyDemo implements InvocationHandler{//动态代理类
    Object obj;//被代理的对象
    public ProxyDemo(Object obj){
        this.obj = obj;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable{
        System.out.println(method.getName()+"开始执行");
        Object result = method.invoke(this.obj, args);//执行的是指定代理对象的指定方法
        System.out.println(method.getName()+"开始完毕");
        return result;
    }
}
```

# 线程
主要内容：
+ 程序、进程、线程的概念
+ Java中多线程的创建和使用
  + 继承Thread类与实现Runnable接口
  + Thread类的主要方法
  + 相乘的调度与设置优先级
+ 线程的生命周期
+ 线程的同步
+ 线程的通信

## 程序-进程-线程
程序(program)是为完成特定任务、用某种语言编写的一组指令的合计。即指一段静态的代码，静态对象。  
进程(process)是程序的一次执行过程，或是正在运行的一个程序。动态过程：有它自身的产生、存在和消亡的过程。如：运行中的QQ、MP3播放器。程序时静态的，进程是动态的。  
线程(thread),进程可进一步细化为线程，是一个程序内部的一条执行路径。若一个程序可同一时间执行多个线程，就是支持多线程的。  
CPU的核数：几核的CPU就代表在同一个瞬时时间能处理的任务数。  
CPU的主频：CPU在不同执行进程之间切换的频率。  
何时需要多线程：  
1. 程序需要同时执行两个或多个任务
2. 需要实现一些等待的任务时，如用户输入、文件读写操作、网络操作、搜索等
3. 需要一些后台运行的程序时

## 多线程的创建和启动
Java语言的JVM允许程序运行多个线程，它通过java.lang.Thread类来实现。  
Thread类的特性：  
+ 每个线程都是通过某个特定的Thread对象的run()方法来完成操作的，经常把run()方法的主体称为线程体。想要在开启的多线程中运行的代码逻辑就写到run方法里。
+ 通过该Thread对象的start()方法来调用这个线程。启动线程，本质上就是开始运行run方法
构造方法：
+ Thread():创建新的Thread对象
+ Thread(String threadname):创建线程并指定线程实例名
+ Thread(Runnable target)：指定创建线程的目标对象，它实现了Runnable接口中的run方法
+ Thread(Runnable target,String name):创建新的Thread对象

### 创建线程的两种方式：
+ 继承Thread类
  1. 定义子类继承Thread类
  2. 子类中重写Thread类中的run方法
  3. 创建Thread子类对象，即创建了线程对象
  4. 调用线程对象start方法：启动线程，调用run方法

```
public class Test{
    public static void main(String[] args) {
        Thread t0 = new TestThread();
        t0.start();//启动线程
        Thread t1 = new TestThread();//第二条线程，可以开任意多个线程
        t1.start();//启动线程
        System.out.println("----------");
        System.out.println("----------");
        System.out.println("----------");
    }
}

class TestThread extends Thread{
    @Override
    public void run(){
        for(int i =0;i<5;i++)
        System.out.println("多线程运行的代码"+i);
    }
}

/**
 * 多次运行这个main方法之后
 * 我们发现main方法中打印的3行与开启线程运行run方法中的打印语句时混合起来的
 * 而且main方法中打印与run方法中打印语句的顺序是不固定的，为什么？
 * main执行t.start()方法开启多线程后，就相当于在main方法之外开启了一个支流
 * 这个时候t.start之后的main方法的其他代码就和run方法代码无关且并行运行
 * 就像两条河流一样各走各的，控制台输出的结果就是两条并行程序的运行结果总和
 * 如果结果拆成两部份看就发现各自保持自己输出的顺序
 * 这个就是多线程的异步性
 */
```

+ 实现Runnable接口
  1. 定义子类，实现Runnable接口
  2. 子类中重写Runnable接口中的run方法
  3. 通过Thread类含参构造器创建线程对象
  4. 将Runnable接口的子类对象作为实际参数传递给Thread类的构造方法中
  5. 调用Thread类的start方法：开启线程，调用Runnable子类接口的run方法

```
public static void main(String[] args) {

   Thread t3 = new Thread(new TestRunnable());
   t3.start();

   Thread t4 = new Thread(new TestRunnable(),"t-1");//第二个字符串可以被getName方法返回，用来观察线程的异步
   t4.start();
   System.out.println("----------");
   System.out.println("----------");
   System.out.println("----------");
}

class TestRunnable implements Runnable{//通过实现Runnable接口方式实现多线程
    @Override
    public void run(){
        System.out.println(Thread.currentThread().getName());
        for(int i =0;i<5;i++)
        System.out.println("多线程运行的Runnable方法"+i);
    }
}
```

继承方式和线程实现方式的联系和区别  
`public class Thread extends Object implements Runnable`  
区别：
+ 继承Thread：线程代码存放Thread子类run方法中，重写run方法
+ 实现Runnable：现成代码存在接口的子类的run方法中，实现run方法

实现接口方式的好处：
1. 避免了单继承的局限性，父类有一个是Thread，不能继承其他父类
2. 多个线程可以共享一个接口实现类的对象，非常适合多个相同线程来处理同一份资源
3. 一般使用实现接口的方式来实现多线程


多线程程序的优点：
1. 提高应用程序的响应。对图形化界面更有意义，可增强用户体验。
2. 提高计算机系统CPU的利用率。
3. 改善程序结构。将既长又复杂的进程分为多个线程，独立运行，利于理解和修改。


### Thread类的相关方法
+ void start()：启动线程，并执行对象的run()方法  
+ run()：线程在被调度时执行的操作  
+ String getName()：返回线程的名称
+ void setName()：设置该线程名称
+ static currentThread()：返回当前线程
+ static void yield()
  + 暂停当前正在执行的线程，把执行机会让给优先级相同或更高的线程
  + 若队列中没有同优先级的线程，忽略此方法
+ join()
  + 当某个程序执行流中调用其他线程的join()方法时，调用线程将被阻塞，直到join()方法加入的join线程执行完为止
  + 低优先级的线程也可以获得执行
+ static void sleep(long millis)：指定时间毫秒
  + 令当前活动线程在指定时间段内放弃对CPU控制，使其他线程有机会被执行，时间到后重排队
  + 抛出InterruptedException异常
+ stop()：强制线程生命期结束(过时了)
+ boolean isAlive()：返回boolean，判断线程是否还活着

### 线程的优先级
线程的优先级就是哪个线程有较大的概率被执行。优先级使用数字1-10表示的，数字越大优先级越高，如果没有设置默认优先级是5。  
+ getPriority()：返回线程优先级
+ setPriority(int newPriority)：改变线程的优先级

## 线程的生命周期
JDK中用Thread.State枚举表示类线程的几种状态  
想要实现多线程，必须在主线程中创建新的线程对象。Java语言使用Thread类及其子类的对象来表示线程，在它一个完整的生命周期中通常要经历如下的五种状态：  
+ 新建(线程实例的创建)：当一个Thread类或其子类的对象被声明并创建时，新生的线程对象处于新建状态
+ 就绪(执行start方法之后)：处于新建状态的线程被start()后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件
+ 运行(run方法的代码开始执行)：当就绪的线程被调度并获得处理器资源时，便进入运行状态，run()方法定义了线程的操作和功能
+ 阻塞(run方法代码暂停执行)：在某种特殊情况下，被认为挂起或执行输入输出操作时，让出CPU并临时种植自己的执行，计入阻塞状态
+ 死亡(执行完毕或执行stop方法)：线程完成了他的全部工作或线程提前强制性地中止

## 线程的同步
问题的提出：  
+ 多个线程执行的不确定性引起执行结果的不稳定
+ 多个线程对账本的共享，会造成操作的不完整性，会破坏数据  

比如：同一个账户，有3000元。两个手机支护宝和微信同时转账，各提2000元，账户上就变成-1000元，这种情况不能出现  
线程安全问题：当多条语句在操作同一个线程共享数据时，一个线程对多条语句只执行了一部分，还没有执行完，另一个线程参与进来执行。导致共享数据的错误。  
解决思路：对多条操作共享数据的语句，只能让一个线程都执行完，在执行过程中，其他线程不可以参与执行。  
通过synchronized同步锁来完成。可直接在方法上加上synchronized关键字。若方法是普通方法，锁的是整个对象，不同的对象就是不同的锁。若对象是普通方法，则对于所有对象都是同一个锁。  
synchronized也可以对代码块加同步锁。
```
synchronized(this){
    代码
}
```
用this锁代表当前的对象，如果在其他的方法的也有synchronized(this)，使用的是同一个同步锁。  
synchronized修饰代码块，想要根据不同的对象有不同的锁，需要：
```
public void method(int m, Object obj){
    synchronized(obj){
        代码
    }
}
```

总结：  
普通方法加同步锁，锁的是当前方法对应的对象，当前对象的所有加了同步锁的方法共有一个同步锁。  
静态方法加同步锁，所有的对象都是使用同一个同步锁。  
代码块加入synchronized(this)同步锁，所有当前对象的的同步的代码块使用同一个锁。  
代码块加入synchronized(obj)同步锁，传入的不同对象使用不同的锁。

线程的死锁问题：  
死锁：不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成了线程死锁。  

解决方法：
+ 专门的算法、原则，比如加锁顺序一致
+ 尽量减少同步资源的定义，尽量避免锁未释放的场景

## 线程的通信
wait()与notify()和notifyAll()
+ wait()：令当前线程挂起并放弃CPU、同步资源，使别的线程可访问并修改共享资源，而当前线程排队等候再次对资源的访问
+ notify()：唤醒正在排队等待同步资源的线程中优先级最高者结束等待
+ notifyAll()：唤醒正在排队等待资源的所有线程结束等待

Java.lang.Object提供的这三个方法只有在synchronized方法或synchronized代码块中才能使用，否则会报异常




