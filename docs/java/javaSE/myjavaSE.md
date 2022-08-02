---
typora-copy-images-to: images
---

# 上课记录



## 1、idea快捷键

* Alt+shift+上下键：将光标选中部分上下移动
* Alt+shift+M：将代码块封装成方法
* Alt+shift+t：快速创建方法
* ctrl+r：查找替换





## 2、设计程序时遵循事项

* if尽量使用短路语法，使程序快速回到正轨；
* 低耦合，高聚合
* 将重复的代码逻辑抽象成一个方法，封装起来





## 3、一些理论性知识

* class.变量 = value;与class.set变量(value);的区别，调用方法能够让程序认知到值的改变，而用 . 来调用的话，程序不能认知到值的改变
* 回答问题时

  * 是什么？
  * 优点，缺点
  * 如果有多个名词，说明他们之间的联系





## 4、数据类型

java中分为两种类型，基本数据类型（值类型）和引用类型

基本数据类型就不说了，我们来研究一下引用类型，一个非常典型的例子`对象` 它就是引用数据类型

当我们执行以下语句：

~~~java
public class Text{
    String str = "start";

    void change(Text text){
        text.str = "end";
    }

    String  getStr1(){
        return str;
    }
    
    void setStr1(String str){
        this.str = str;
    }

    public static void main(String[] agrs){
        Text text;
        text = new Text();
        
        //测试方法传参为地址还是值
        text.change(text);
        System.out.println(text.getStr1());

        
        
        //测试赋值过程中使用的是地址还是值
        Text text1;
        text1 = new Text();
        text = text1;
        text1.setStr1("这是text1");
        System.out.println(text.getStr1());
        
    }
}
~~~

程序运行结果：

![1657960650077](images/1657960650077.png)

从上面代码我们可以看出，引用类型持有的是实例的地址，当它进行传递时传递的是地址；当地址指向的值发生改变时，所有持有该地址引用的变量的值都会发生改变

>有一个特殊的引用类型：`String`，本来我是准备用它做例子说明引用类型在传递过程中是传的地址（虽然String传的也是地址，但底层逻辑有点不一样），而不是传的值；我会在下面专门开一个标题说一下我对`String`类型的初步了解

说到数据类型，我们就需要引入两个内存的概念，栈和堆，这两个东西可以帮助你来理解何为地址，何为值

* 值：对基本数据类型而言，值就是它存储的数据；对引用类型而言，值是它持有的地址所指向的空间中存储的数据
* 地址：地址是对引用类型而言，是引用类型变量实际上持有的数据；



**栈是程序执行的地方，也是存储基本数据类型的地方；**

**堆是用来存放对象和数组的实例的，我们平时用的`new`就是在堆中开辟空间;**



凭我的三言两语可能并不能让你们理解到栈和堆，这里我推荐你可以去看看《[java中的栈与堆](https://www.cnblogs.com/ibelieve618/p/6380328.html)》





### 4.1、String专题

首先我们知道String是一个引用类型

那么把它作为形参传入方法，然后在方法里改变它的值，它本身的值会改变吗?

答案：是不会

想必有些人已经开始跟我一样懵逼了吧，String是引用类型啊，传地址，改值不应该本身也会变吗

然后我在网上查阅了一些资料发现是jvm搞得鬼，因为String类型我们用的太多了，jvm为了提高性能和减少内存

的开销，在对字符串实例化时进行了一些优化，它在内存中划分了一个名为**字符串常量池**的空间，**每当我们创建字  符串常量时，JVM会首先检查字符串常量池，如果该字符串已经存在常量池中，那么就直接返回常量池中的实例引用。如果字符串不存在常量池中，就会实例化该字符串并且将其放到常量池中。**



> 比如我们令`String str = "a";`那么jvm会检索字符串常量池，如果发现有相同的对象（一个字符串便是一个变量），那么它会将已经存在的对象的地址返回给str持有，如果没有检索到相同对象那么它会实例化一个”a“，然后将它存入字符串常量池中去



我浅薄的理解应该无法满足你的求知欲，这里有一个写的比较好的博客，可以去看看

《[深入理解java中的String类](https://www.cnblogs.com/xiaoxi/p/6036701.html)》



## 5、关于对象的创建

* 对象创建的过程

   ​                    `A a =new A();`

   * 第一步：jvm先向内存（堆）中申请一个空间
   * 第二步：为对象的变量赋予初值，如果没有给初始值，那么jvm会为变量提供默认值，int默认值为0；boolean默认值为false，引用类型默认值为NULL
   * 第三步：jvm会调用构造器，执行构造器中的语句
   * 第四步：jvm将开辟出来的空间地址交予栈中某一变量持有

   ![1657960670533](images/1657960670533.png)

     

   > 如果你理解了对象的创建，那么你就能推断出一些有趣的结论
   >
   > 1、实例的成员变量会比构造器先执行，与代码所写前后位置无关，即：成员变量写在构造器后面也是成员变量先赋初始值，再经过构造器赋值；
   >
   > 2、栈中变量持有的是实例的地址，而不是实例本身；
   >
   > 3、值得注意的一点是：如果有static修饰符，对象实例话的情况又会有所不同，这一点我会在下面讲到static时提出

这里放一下类的装载过程

![1659060688635](images\1659060688635.png)




## 6、关于封装

封装可以对类封装也可以对方法封装，

* 对类封装，将类的两个要素包装到一个单元里
* 对方法封装，将具有相同功能或相同逻辑的代码块抽取出来，包装成一个方法
* 说到封装就离不开抽象，抽象就是把一个具体的东西简单化，只留下一个由主要特征构成的概念性事物。
* 抽象也体现在当我们使用一个类型的实例的时候，在调用其方法时，只需要关注有哪些入参，可以得到什么返回，而无需关注内部的具体实现方式或细节



>关于封装我的一些理解：
>
>1、封装，根据字面意思即可以知道，它是将一堆东西装在一起，封闭起来。而应用到编程中，就会产生几个令人思考的点，一堆什么东西，装在哪里，如何才是封闭起来？
>
>2、一堆东西大概是指拥有相同特征或行为的事物，或者一种特定的行为模式
>
>3、装在哪里：在现实生活中我们会将东西打包起来装在盒子中，而java程序中有与之代替的东西，那就是类/方法，我们将拥有相同特征或行为的事物装在类中，而特定的行为模式装在方法中
>
>4、如何才是封闭起来：我理解为让别人看到你想让他看到的东西，就像人慢慢的长大后，会从小时候的童言无忌到现在的察颜悦色，这就是我们将带刺的自己封装后的结果
>
>
>
>值得一提的是，提到封装他就离不开抽象，当你去提炼 **"一堆东西"** 的时候你往往会将与这堆东西并不是那么相关的杂志去除掉，留下来的便是这堆东西的精髓，这也就是我们常说的“关注主要的，忽略次要的”





## 7、关于继承

继承就是儿子可以拿到父亲愿意留给你的东西，至于拿到东西怎么去用还是得看儿子

* 子类继承父类中除private修饰的成员

* 子类可以重写父类方法

* 子类在new的时候是先创建父类对象，再创建子类对象，在创建父类和子类时遵循对象创建的过程

  即：先赋值，再调用构造器，类中的static成员会在jvm加载时就完成创建；所以static优先，父类第二，子类最后

* java只能单继承，但可以通过多级继承来实现间接的多继承

> 关于继承：
>
> 通过反编译子类我们可以得知它的运行过程（Javap -v xxx.class）

~~~class
D:\qian-feng-study\target\classes\project\com\Test>javap -v Childs.class

Classfile /D:/qian-feng-study/target/classes/project/com/Test/Childs.class
  Last modified 2022-7-15; size 609 bytes
  MD5 checksum 0673524492c304d227e3b0ec4536b50e
  Compiled from "Super.java"
class project.com.Test.Childs extends project.com.Test.Super
  minor version: 0
  major version: 52
  flags: ACC_SUPER
Constant pool:
1 = Methodref          #7.#19         // project/com/Test/Super."<init>":()V
2 = Fieldref           #20.#21        // java/lang/System.out:Ljava/io/PrintStream;
3 = String             #22            // 这是子类静态方法！
4 = Methodref          #23.#24        // java/io/PrintStream.println:(Ljava/lang/String;)V
5 = String             #25            // 这是子类普通方法！
6 = Class              #26            // project/com/Test/Childs
7 = Class              #27            // project/com/Test/Super
8 = Utf8               <init>
9 = Utf8               ()V
10 = Utf8               Code
11 = Utf8               LineNumberTable
12 = Utf8               LocalVariableTable
13 = Utf8               this
14 = Utf8               Lproject/com/Test/Childs;
15 = Utf8               m1
16 = Utf8               m2
17 = Utf8               SourceFile
18 = Utf8               Super.java
19 = NameAndType        #8:#9          // "<init>":()V
20 = Class              #28            // java/lang/System
21 = NameAndType        #29:#30        // out:Ljava/io/PrintStream;
22 = Utf8               这是子类静态方法！
23 = Class              #31            // java/io/PrintStream
24 = NameAndType        #32:#33        // println:(Ljava/lang/String;)V
25 = Utf8               这是子类普通方法！
26 = Utf8               project/com/Test/Childs
27 = Utf8               project/com/Test/Super
28 = Utf8               java/lang/System
29 = Utf8               out
30 = Utf8               Ljava/io/PrintStream;
31 = Utf8               java/io/PrintStream
32 = Utf8               println
33 = Utf8               (Ljava/lang/String;)V
{

  project.com.Test.Childs();
  
    descriptor: ()V
    flags:
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1             // Method project/com/Test/Super."<init>":()V
         4: return
      LineNumberTable:
        line 13: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lproject/com/Test/Childs;
            
  static void m1();
    descriptor: ()V
    flags: ACC_STATIC
    Code:
      stack=2, locals=0, args_size=0
      0: getstatic     #2   // Field java/lang/System.out:Ljava/io/PrintStream;
      3: ldc           #3   // String 这是子类静态方法！
      5: invokevirtual #4   // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      8: return
   LineNumberTable:
      line 15: 0
      line 16: 8
      
  void m2();
    descriptor: ()V
    flags:
    Code:
      stack=2, locals=1, args_size=1
      0: getstatic     #2  // Field java/lang/System.out:Ljava/io/PrintStream;
      3: ldc           #5  // String 这是子类普通方法！
      5: invokevirtual #4  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
      8: return
  LineNumberTable:
      line 19: 0
      line 20: 8
  LocalVariableTable:
      Start  Length  Slot  Name   Signature
          0       9     0  this   Lproject/com/Test/Childs;
}

SourceFile: "Super.java"
~~~

> 从上面反编译得到的代码我们可以看出，子类实例化时会先实例化一个父类，而且从反编译的代码中我们能推断出子类实例化时是持有一个父类实例地址（这个非常隐蔽，只能用super来调用）
>
> 那么结合上面所提到的对象的创建，我们是不是可以得出一个执行顺序：
>
> 1、父类成员变量执行
>
> 2、父类构造器执行
>
> 3、子类成员变量执行
>
> 4、子类构造器执行



## 8、向上转型&&向下转型

* 向上转型：子类转型为父类，自动转型
* 向下转型：父类转型为子类，强制转型

无论是向上转型还是向下转型，对象地址指向的空间是由new 后面的构造器决定的

前面的引用类型用来规定对象可以用什么



> 总结：能做什么看左边，有什么看右边



## 9、final修饰词

* 被final修饰的变量，具有 “ 不可改变 ” 的特性
* 修饰基本类型：其值不可改变
* 修饰引用类型：储存的地址不可改变，但地址指向的值可以改变
* 修饰类：表示这个类为最终版，不可被继承
* 修饰方法：表示这个方法为最终版，不可被重写
* final修饰的变量可以在构造器中进行第一次赋值初始化，如果已经初始化赋值过了，那么就不能在构造器中赋值





## 10、static修饰词

* 由static修饰的成员，不依赖类的实例而存在，一般直接通过类名调用，他是这个类的实例所共有的

* 被static修饰的成员为静态成员，有且只有一份并在jvm加载过程中就已经生成
* 当方法被static修饰时，该方法不可以使用非静态成员
* 当子类重写父类static方法后，**具体实现方法根据实例的类型**来确定，如下：

~~~java
public class Super{
    static void m1(){
        System.out.println("这是父类静态方法！");
    }
    
    void m2(){
        System.out.println("这是父类普通方法！");
    }
}

class Child{
    static void m1(){
        System.out.println("这是子类静态方法！");
    }
    
    void m2(){
        System.out.println("这是子类普通方法！");
    }
}

class test{
    public static void main(String[] args){
        //子类转为父类，向上转型
        Super sup = new Child();
        
        sup.m1();//实例类型为父类，调用父类静态方法
        sup.m2();//new的为子类空间，故调用子类重写后的方法
        
        //父类转为子类，向下转型
        Child child = (Child)sup;
        
        child.m1();//实例类型为子类，调用子类静态方法
        child.m2();//由于地址没发生改变，故实例指向的空间也没变，所以还是调用子类方法
       
    }
}
~~~

程序运行结果为：

![](images/Snipaste_2022-07-15_20-20-56.png)







## 11、对象的比较

这个没啥好解释的就是重写Object提供的equals方法由比较地址改为比较值，然后加入一些防出错判断，不过引入了一个新的关键字`A instanceof B`用于查看A对象是否为B类型代码如下

~~~java
public boolean equals(Object obj) {

        // 用短路思路来编写：让程序尽早返回结果 / 让不合格的元素尽早退出该段逻辑

        if (obj == null) return false;
        Point point;
        if (!(obj instanceof Point)) return false;
        point = (Point) obj;
        return (x == point.x && y == point.y);


    }
~~~





## 12、关于多态

**多态，多种形态或形式，即允许一个对象有多种体现形式。**

向上转型很好的体现了多态，一个父类型的形参能够接受许许多多子类型的是实参，从而让程序在不改变代码结构的情况下运行出来不同的结果



多态的必要条件：

1. 继承
2. 父类引用指向子类对象
3. 子类重写父类中的方法



多态的几种方式：

1. 传参
2. 多态的定义（父类引用指向子类对象）



## 13、abstract 抽象类

**所谓的抽象类，就是我们不想具体化一个类型所封装出来的类型**

​	1、不能实例化的

​	2、抽象类存在的意义，他就是用来被继承或扩展的

​	3、抽象类中不一定有抽象方法，但是有抽象方法一定它就是抽象类

​	4、抽象类中也可以有具体的方法

​	5、抽象类的子类不一定非要实现父类型中的抽象方法，它可以继续声明自己也是抽象的

​	6、没有具体实现（方法体）的方法称之为抽象方法



## 14、Interface接口

**接口**：可以理解为一种抽象到极致的东西。有如下特性

```java
接口：主要是用来封装一揽子抽象方法的，它侧重在于对行为的一种抽象（动词）
- 必须使用implements关键字
- 实现接口的类，必须要实现其中所有的抽象方法，只要有一个没有实现，这个类本身必须声明为abstract class
- 接口也可以定义属性，但是它默认是public final static
- 接口中不能有具体的方法实现，它只是一种行为的规约（规范和约定）
- 接口中其实可以定义具体方法（默认方法，可以定义多个），前面必须加default修饰
- 接口可以说是抽象类抽象到极致的一种特殊情况

- 接口和其他类型之间的关系：
-- 接口和实现类之间的是implements
-- 接口和接口之间是extends
```



## 15、常用类





## 16、泛型***





## 17、集合

​	目前集合有分为两大类Collection和Map，Collection下又分为List和Set,List和set下还可细分，Map下分为HashMap，TreeMap。说起来可能有点乱，看下面的图吧

![](images\20200321185804869.png)

​	下面说说每个集合吧

### 17.1、ArrayList





```java
package project.com.gather;
import java.util.*;

@SuppressWarnings("all")
public class MyArrayList<T> implements List<T> {

    public static void main(String[] args) {
        List<String> list = new MyArrayList<>();
        list.add("1");
        list.add("2");
        list.add("3");
        list.add("4");
//        list.add(null);
        list.add("6");
        list.add("7");
        System.out.println(list.remove("3"));
        System.out.println(list);
//        System.out.println(list.contains(null));
        List<String> list1 = new ArrayList<>();
        list1.add("8");
        list1.add("9");
        list1.add(null);
        list1.add("11");
        System.out.println(list1.indexOf(null));
        System.out.println(list1);
        list.addAll(2,list1);
        System.out.println(list);
        list1 = list.subList(2,4);
        System.out.println(list1);
        list.clear();
        System.out.println(list);
        list.add("1");
        list.add("2");
        list.add("3");
        System.out.println(list);
        for (String s : list) {
            System.out.println(s);
        }
    }

    // elementData
    private Object[] data;
    private int size; // 用来记录实际存放的元素个数


    /**
     * 重写toString()方法
     * @return 以字符串形式返回list中元素
     */
    @Override
    public String toString() {
        if (size==0){
            return "["+"]";
        }
        StringBuilder str = new StringBuilder();
        str.append("[");
        for (int i = 0; i < size-1; i++) {
            str.append(data[i]);
            str.append(",\t");
        }
        str.append(data[size-1]);
        str.append("]");
        return str.toString();
    }

    /**
     * 默认长度：5
     */
    public MyArrayList() {
        this(5);
    }

    /**
     * 构造一个长度为capacity长度的list
     * @param capacity 用户自定义长度
     */
    public MyArrayList(int capacity) {
        data = new Object[capacity];
    }

    /**
     * @return 返回list实际存有数据个数
     */
    @Override
    public int size() {
        return size;
    }

    /**
     * 判断list是否为空
     * @return 为空返回true，不为空返回false
     */
    @Override
    public boolean isEmpty() {
        return size == 0;
    }

    /**
     * 如果此列表包含指定元素，则返回true 。
     * 更正式地说，当且仅当此列表包含至少一个元素e
     * 满足(o==null ? e==null : o.equals(e))时，才返回true 。
     * @param o 要寻找的对象
     * @return  找到返回true
     */
    @Override
    public boolean contains(Object o) {
        return indexOf(o)>=0;
    }

    /**
     * 确保此集合包含指定的元素（可选操作）。如果此集合因调用而更改，则返回true 。
     * （如果此集合不允许重复且已包含指定元素，则返回false。）
     * @param o element whose presence in this collection is to be ensured
     */
    @Override
    public boolean add(T o) {
        // 判断容量是否足够：Y-继续 N-扩容
        extracted(data,1);
        data[size++]=o;
        return true;
    }



    /**
     * 判断数组是否需要扩容，如需要扩容将以(size+len)2倍被扩容形式扩容
     */
    private void extracted(Object[] data,int len) {
        if(size+len>=data.length) {
            Object[] data1 = new Object[len+size<<1];
            System.arraycopy(data,0,data1,0,size);
            this.data = data1;
        }
    }

    /**
     * 如果指定元素存在，则从该列表中删除第一次出现的指定元素（可选操作）。
     * 如果此列表不包含该元素，则它不变。
     * 更正式地说，删除具有最低索引i的元素，
     * 使得(o==null ? get(i)==null : o.equals(get(i)))如果存在这样的元素）。
     * 如果此列表包含指定的元素（或等效地，如果此列表因调用而更改），则返回true 。
     * @param o element to be removed from this list, if present
     */
    @Override
    public boolean remove(Object o) {
        int index = indexOf(o);
        return index != -1 && (remove(index) != null);
    }



    /**
     * 将指定集合中的所有元素插入到此列表中的指定位置（可选操作）。
     * 将当前位于该位置的元素（如果有）和任何后续元素向右移动（增加它们的索引）。
     * 新元素将按照指定集合的迭代器返回的顺序出现在此列表中。
     * 如果在操作正在进行时修改了指定的集合，则此操作的行为是未定义的。
     * （请注意，如果指定的集合是这个列表，并且它是非空的，则会发生这种情况。）
     * @param index index at which to insert the first element from the
     *              specified collection
     * @param c collection containing elements to be added to this list
     */
    @Override
    public boolean addAll(int index, Collection c) {
        if(index>size)return false;
//         extracted(data,c.size());
//        System.out.println(data.length+":"+size);
        for (Object o : c) {
            add(index++, o);
        }
        return true;
    }



    /**
     * 从此列表中删除所有元素（可选操作）。
     * 此调用返回后，列表将为空。
     */
    @Override
    public void clear() {
        int num = size;
        for (int i = 0; i < num; i++) {
            remove(0);
        }
    }



    /**
     *返回此列表中指定位置的元素
     * @param index index of the element to return
     */
    @Override
    public T get(int index) {
        return (T) data[index];
    }



    /**
     *将此列表中指定位置的元素替换为指定元素（可选操作）。
     * @param index index of the element to replace
     * @param element element to be stored at the specified position
     */
    @Override
    public T set(int index, Object element) {
        data[index] = element;
        return null;
    }

    /**
     * 在此列表中的指定位置插入指定元素（可选操作）。
     * 将当前位于该位置的元素（如果有）和任何后续元素向右移动（将其索引加一）
     * @param index index at which the specified element is to be inserted
     * @param element element to be inserted
     */
    @Override
    public void add(int index, Object element) {
        extracted(data,1);
        System.arraycopy(data,index,data,index+1,(size++)-index);
        set(index,element);
    }



    /**
     * 移除此列表中指定位置的元素（可选操作）。
     * 将任何后续元素向左移动（从它们的索引中减去 1）。
     * 返回从列表中删除的元素
     * @param index the index of the element to be removed
     */
    @Override
    public T remove(int index) {
        if (index>=size)return null;
        T value = (T) data[index];
        System.arraycopy(data,index+1,data,index,(size--)-index);
        return value;
    }



    /**
     * 返回此列表中指定元素第一次出现的索引，如果此列表不包含该元素，则返回 -1。
     * 更正式地说，返回满足(o==null ? get(i)==null : o.equals(get(i)))的最低索引i ，
     * 如果没有这样的索引，则返回 -1。
     * @param o element to search for
     */
    @Override
    public int indexOf(Object o) {
        if (o==null){
            for (int i = 0; i < size; i++)
                if (data[i]==null)
                    return i;
        }else {
            for (int i = 0; i < size; i++)
                if (o.equals(data[i]))
                    return i;
        }
        return -1;
    }



    /**
     * 返回此列表中指定元素最后一次出现的索引，如果此列表不包含该元素，则返回 -1。
     * 更正式地说，返回最高索引i使得(o==null ? get(i)==null : o.equals(get(i)))
     * 如果没有这样的索引，则返回 -1。
     * @param o element to search for
     */
    @Override

    public int lastIndexOf(Object o) {
        if (o==null){
            for (int i = size-1; i >= 0; i++)
                if (data[i]==null)
                    return i;
        }else {
            for (int i = size-1; i >= 0; i++)
                if (o.equals(data[i]))
                    return i;
        }
        return -1;
    }



    /**
     * 返回此列表在指定fromIndex （包括）和toIndex （不包括）之间部分的视图。
     * （如果fromIndex和toIndex相等，则返回列表为空。）
     * 返回列表由此列表支持，因此返回列表中的非结构性更改会反映在此列表中
     * 反之亦然。返回的列表支持此列表支持的所有可选列表操作。
     *
     * @param fromIndex low endpoint (inclusive) of the subList
     * @param toIndex high endpoint (exclusive) of the subList
     */

    @Override
    public List subList(int fromIndex, int toIndex) {
        if(fromIndex == toIndex) return null;
        MyArrayList list = new MyArrayList();
        for (int i = fromIndex; i < toIndex; i++) {
            list.add(data[i]);
        }
        return list;
    }



    /**
     * 以正确的顺序（从第一个元素到最后一个元素）返回一个包含此列表中所有元素的数组；
     * 返回数组的运行时类型是指定数组的运行时类型。
     * 如果列表适合指定的数组，则在其中返回。
     * 否则，将使用指定数组的运行时类型和此列表的大小分配一个新数组。
     * 如果列表适合指定的数组并有剩余空间（即，数组的元素比列表多），则数组中紧随列表末尾的元素设置为null 。
     * （仅当调用者知道列表不包含任何空元素时，这对确定列表的长度很有用。）
     * 与toArray()方法一样，此方法充当基于数组的 API 和基于集合的 API 之间的桥梁。
     * 此外，此方法允许对输出数组的运行时类型进行精确控制，并且在某些情况下可用于节省分配成本。
     * 假设x是一个已知仅包含字符串的列表。以下代码可用于将列表转储到新分配的String数组中：
     *  String[] y = x.toArray(new String[0]);
     * 请注意， toArray(new Object[0])在功能上与toArray()相同。
     *
     * @param a the array into which the elements of this list are to
     *          be stored, if it is big enough; otherwise, a new array of the
     *          same runtime type is allocated for this purpose.
     */
    @Override
    public Object[] toArray(Object[] a) {
        if (a.length<size) return toArray();
        System.arraycopy(data,0,a,0,size);
        return a;
    }

    /**
     * 以正确的顺序（从第一个元素到最后一个元素）返回包含此列表中所有元素的数组。
     * 返回的数组将是“安全的”，因为此列表不维护对它的引用。 （
     * 换句话说，即使此列表由数组支持，此方法也必须分配一个新数组）。
     * 因此，调用者可以自由修改返回的数组。
     * 此方法充当基于数组和基于集合的 API 之间的桥梁。
     */
    @Override
    public Object[] toArray() {
        return Arrays.copyOf(data,size);
    }

    /**
     * 将指定集合中的所有元素添加到此集合（可选操作）。
     * 如果在操作正在进行时修改了指定的集合，则此操作的行为是未定义的。
     * （这意味着如果指定的集合是这个集合，并且这个集合是非空的，那么这个调用的行为是未定义的。）
     * @param c collection containing elements to be added to this collection
     */
    @Override
    public boolean addAll(Collection c) {
        return addAll(size,c);
    }


    //未完成
    @Override
    public Iterator iterator() {
        return new Iterator<T>(){
            int Point = -1;
            public boolean hasNext(){
                return data[Point + 1] != null;
            }
            public T next(){
                return (T) data[++Point];
            }

        };
    }

    @Override
    public boolean containsAll(Collection c) {
        if (c==null||c.size()==0) return false;

        for (Object o : c) {
            if (!contains(o))return false;
        }
        return true;
    }

    @Override
    public boolean retainAll(Collection c) {
        return extracted(c,true);
    }

    @Override
    public boolean removeAll(Collection c) {
        return extracted(c,false);
    }

    private boolean extracted(Collection c,boolean comp) {
        if (c ==null || c.size() == 0) return false;

        int r = 0;
        int w = 0;
        boolean flag = false;
        try {
            for (; r < size; r++) {
                if (c.contains(data[r])==comp) data[w++] = data [r];
            }
        }finally {
            if (r!=size) {
                System.arraycopy(data, w, data, r, size - r);
                w += size - r;
            }
            if (w!=size){
                for (int i = w; i < size; i++)
                    data[i] = null;
                size = w;
                flag = true;
            }
        }
        return flag;
    }

    @Override
    public ListIterator listIterator() {

        return null;
    }

    @Override
    public ListIterator listIterator(int index) {
        return null;
    }

}
```

以上是根据自己浅薄理解写出来的MyArrayList,值得注意的是

* ArrayList底层使用数组实现，因此随机查询、修改很快，因为数组的下标可以等同于内存地址，而随机增加和删除会慢一点，这两个操作需要将后续元素一个一个位移，增加了操作量

* 插入有序
* ArrayList结构图如下：

![](images\20200321205116862.png)

* 关于迭代器的思考：迭代器就相当于一个指针一样，指针一开始开始指向-1位置，而不是元素的位置

  下面是迭代器的执行过程

  ![](images\131907b56b9b4b848d641d4976aa4135.gif)

  > 迭代器执行过程详解：
  >
  > 1. 首先得到一个集合的迭代器Iterator iterator = list.iterator();
  > 2. 进入while循环，调用hasNext()判断是否有下一个元素，返回true，Iterator.next()移动一个位置，将该位置的元素111返回。
  > 3. 再次进入while循环，调用hasNext()判断是否有下一个元素，返回true，Iterator.next()移动一个位置，将该位置222的元素返回。
  > 4. 再次进入while循环，调用hasNext()判断是否有下一个元素，返回true，Iterator.next()移动一个位置，将该位置333的元素返回。
  > 5. 再次进入while循环，调用hasNext()判断是否有下一个元素，返回false，循环结束。  

  ~~~
  在 迭代器的遍历过程中先通过hastNext()方法判断是否有下一个元素，  
  如果存在下一个元素再调用next()方法获取元素，在这里next()方法先往后移动一个元素位置， 再返回该位置的元素。  
  因此，在调用next()方法之前必须要调用hastNext()方法进行检测；  
  如果没有调用并且没有下一个元素，直接调用next()方法会抛出 NoSuchElementException异常。
  ~~~



### 17.2、LinkedList

LinkedList使用双向链表实现，他把每个元素看做一个节点，节点内保存有自己的值以及上下两个节点；由于是链表实现方式它查询和修改较慢，而增加和删除较快，因为查询与删除需要一个一个节点“问路” 问过去。而增删只需告诉两边的人中间会来一个人即可。LinkedList结构图如下：

![](images\20200321212118892.png)



在写MyLinkedList的时候需要多次实现根据索引或值定位元素的操作，参考了一下jdk的写法如下：

```java
//Node<E> 用来存储数据的节点类型，该方法大概就是得到索引为index的值
Node<E> node(int index) {
    // assert isElementIndex(index);
    // 判断index位于链表的哪一边size>>1等于size/2
    if (index < (size >> 1)) {
        Node<E> x = first;
        for (int i = 0; i < index; i++)
            x = x.next;
        return x;
    } else {
        Node<E> x = last;
        for (int i = size - 1; i > index; i--)
            x = x.prev;
        return x;
    }
}
```



### 17.3、HashSet

通过查看HashSet的源码我们可以得知：

```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{

private transient HashMap<E,Object> map;
....
....
//底层还是HashMap
public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }
...
...
}
```

HashSet基于HashMap的key来实现，所以它的元素不重复，插入无序，结构图如下：

![](images\20200321224855148.png)



### 17.4、LinkedHashSet

- 继承HashSet，**内部采用的是LinkedHashMap数据结构**。
- 和HashSet一样，内部存储元素不可重复。
- **和HashSet唯一不同的地方是元素有序**。
- 因为内部使用的是LinkedHashMap，它内部的存储元素是LinkedHashMapEntry，源码如下：

~~~java
static class LinkedHashMapEntry<K,V> extends HashMap.Node<K,V> {

    LinkedHashMapEntry<K,V> before, after;

    LinkedHashMapEntry(int hash, K key, V value, Node<K,V> next) {

        super(hash, key, value, next);

    }

}
~~~

- 我们看到LinkedHashMapEntry继承了Node，并且在Node的基础上多了两个指针，before和afer，这两个指针就是用来记录插入的每一个元素的前后元素位置的，这样就保证了顺序。结构图如下： 

![](images\2020032222264240.png)



### 17.5、TreeSet

- 底层采用树结构来进行存储，内部使用的是TreeMap。
- 元素按照字符串顺序存储，元素不允许为null。



### 17.6、HashMap

* 它通过Key的hashCode()做相关的位运算得到数组中的位置索引，如果hash散列算法以及数组长度合适的话，效率很高。
* 它可以存入键为null的键值对
* 它不是线程安全
* 结构图如下：

![](images\20200321224855148.png)



### 17.7、LinkedHashMap

- 它是HashMap的子类，它和HashMap的区别是在HashMap的基础上，增加了一个双向链表来记录存储数据的先后顺序，能够保证遍历数据的时候，首先得到的是先插入的数据。
- LinkedHashMap的结构图如下：

![](images\2020032222264240.png)



### 17.8、TreeMap

* 它实现了SortedMap接口，查看put源码可以看到，key必须实现Comparable接口或者在构造TreeMap传入自定义的Comparator，否则会在运行时抛出java.lang.ClassCastException类型的异常。
* 它能够把保存的记录根据键排序，默认是按键值的升序排序，可以在构造方法中指定排序的比较器，当用Iterator遍历TreeMap时，得到的记录是排过序的。



### 17.9、其他集合

还有一些其他的集合主要是线程安全的集合，由于自己学识浅薄，目前暂不深究



## 18、Collections和Arrays工具类***





## 19、异常



## 20、IO流



## 21、网络编程



## 22、多线程



## 23、jdk8新特性





