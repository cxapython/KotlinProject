下面的内容均来自Android第一行代码 第三版，这里整理出来方便自己后续查看。

kotlin每一行代码后面无需加;

# 基础部分

### 1.搭个架子

开发一个kotlin版本的demo app，界面选择选择Empty,然后在MainActivity.kt同级目录下创建LearnKotlin.kt内容如下

```kotlin
package com.example.ktproject

import java.lang.Integer.max

fun main() 
    println("hello world")
}
```

### 2.变量

比较简单看代码注释就行

```kotlin
fun main() {
    //val声明不可变的变量,通过:可以指定类型
    val a: Int = 10
    //var声明可变类型，后面方便进行继续操作
    var b = 20
    b = b * b
    println("a=" + a)
    println("b=" + b)
    var c=largeNumber(a,b)
    println("c=" + c)
    //优先使用val，val满足不了再用var

```

### 3.函数

*基本形式*

fun 函数名 (参数1，参数2): 返回值类型1,返回值类型2 

如果没有返回值可以不写。

一段代码

```kotlin
fun largeNumber(num1:Int,num2:Int): Int {

   return max(num1,num2);
}

```

如果方法的内容就一行可以进行如下优化

第一次优化

```kotlin
fun largeNumber(num1:Int,num2:Int):Int = max(num1,num2)
```

继续优化

```kotlin
fun largeNumber(num1:Int,num2:Int) = max(num1,num2)
```



### 4.逻辑控制

Kotlin里有两个逻辑控制语句if和when，下面一一展示。

#### if

```kotlin
fun largeNumber2(num1:Int,num2:Int): Int {
    var value = 0
    if(num1>num2){
       value = num1
    }else{
        value = num2
    }
   return value;
}
```

到目前为止它的用法和java基本上一样，它也可以像go一样使用。

```kotlin
fun largeNumber2(num1:Int,num2:Int): Int {
    var value = if(num1>num2){
        num1
    }else{
        num2
    }
    return value;
}
```

继续优化

```kotlin
fun largeNumber2(num1:Int,num2:Int): Int {
   return if(num1>num2){
        num1
    }else{
        num2
    }
    return value;
}
```

结合上面的函数优化，这里可以继续优化为下面的形式

```kotlin
fun largeNumber2(num1: Int, num2: Int) = if (num1 > num2) {
   num1
} else {
    num2
}

```

继续优化，但是可读性感觉差了

```kotlin
fun largeNumber2(num1: Int, num2: Int) = if (num1 > num2) num1 else num2
```

#### When

when类似java的switch语句

```kotlin
fun getScore(name:String) = when(name){
    "Zhao" ->86
    "Qian" -> 77
    "Sun" -> 100
    else-> 0
}
```

如果不想写when括号的name参数，可以像下面一样

```kotlin
fun getScore(name:String) = when{
    name=="Zhao" ->86
    name=="Qian" -> 77
    name==" Sun" -> 100
    else-> 0
}
```

使用这种方法的好处，可以在匹配的时候对name做操作，比如

```kotlin
fun getScore(name:String) = when{
		//并一定是用等于号才行
    name.startsWith("Zhao") ->86
    name=="Qian" -> 77
    name==" Sun" -> 100
    else-> 0
}
```



除此之外还可以使用when进行类型匹配

```kotlin
fun checkNumber(num:Number){
    when(num){
        is Int-> println("number is int")
        is Double-> println("number is Strinf")
        else -> println("unkonw type")
    }
}
```

这里的 is 相当于java的instanceof关键字。

### 5.循环

循环分为 for 和 while，其中while和java的使用基本相同，不过多展开

```kotlin
fun aaa() {
    var a = 0

    while (a < 3) {
        println("in aaa:" + a)
        a += 1
    }
}
```

#### for

for-- in 可以循环遍历区间

```kotlin
fun main() {
    for (i in 0..200){
        println("num is:"+i)
    }
    for (i in 0 until 20 step 4){
        println("num is:"+1)
    }
    for (j in 20 downTo 0 step 2 ){
        println("j is:"+j)

    }
}

```

 0..200 :表示 0到200

0 until 20 表示:左闭右开。即包括0不包括20。

可以通过step设置每次遍历的步数，像python的range第三个参数。



downTo:可以设置成降序遍历。

### 6.面向对象

新创建一个Person.kt文件，创建一个父类，基本形态如下:

```kotlin
package com.example.ktproject

open class Person {
    var name=""
    var age = 0
    fun eat(){
        println(name+"is eating.He is "+age+" year old.")
    }
}
```

open的用法，kotlin默认的类是不可以继承，通过open关键词就可以了。

在之前的main方法进行实例化

```kotlin
fun main() {
    val p = Person()
    p.name = "Li"
    p.age = 20
    p.eat()
}
```

#### 继承

创建一个子类名为Student



```kotlin
package com.example.ktproject

class Student(val sno:String,val grade:Int): Person() {
    init {
        println("sno is"+sno)
        println("grade is"+grade)
    }
    
}
```

kotlin中的继承关键字是:，

在main中调用

```kotlin
  val s = Student("1",3)
```

Person类后面的括号的作用:涉及到主构造函数和次构造函数，每个类默认都有一个不带参数的主构造函数

主构造函数没有方法体，如果想写一些逻辑可以使用init结构体。就像上面的Student。

下面对父类和子类做一些变换，给父类显示的指明参数，

Person.kt

```kotlin
package com.example.ktproject

open class Person(val name:String,val age:Int) {
    fun eat(){
        println(name+"is eating.He is "+age+" year old.")
    }
}
```

kotlin和java一样必须遵守下面的规定：子类的构造函数必须调用父类的构造函数。

这里就使用到父类后面括号了。但是子类有没有父类的这两个字段，所以最终字子类变为下述模式。

```kotlin
package com.example.ktproject

class Student(val sno:String,val grade:Int,name:String,age:Int):
    Person(name,age) {
        init {
            println("nam is"+name)
        }
}
```

关于次构造函数后续再补充。

### 7.接口

接口中的函数不要求有函数体

创建一个Study.kt 文件

```kotlin
package com.example.ktproject

interface Study{
    fun readBook()
    fun doHomeWork()
    fun elseFun(){
        println("这个函数不强制要求重写")
    }
}
```

其中readBook和doHomeWork是必须要继承的，elseFun函数不强制要求继承。

下面是Student.kt

```kotlin
package com.example.ktproject

class Student(val sno:String,val grade:Int,name:String,age:Int):
    Person(name,age),Study {
    override fun doHomeWork() {
        TODO("Not yet implemented")
    }

    override fun readBook() {
        TODO("Not yet implemented")
    }
}
```

继承和接口都一样统一是:打头，但是多个同时出现时使用,进行分割，如上。

### 8.数据类(独有)

kotlin还有一个类似python的dataclass功能的data，其形式为

```kotlin
data class CellPhone(val brand:String,val price:Double)
```

使用

```kotlin
fun main(){
  val phone1=CellPhone("xiaomi","9999")
  val phone2=CellPhone("xiaomi","9999")
  println(phone1==phone2)
}
```

因为data class帮我们实现了equals,hashCode(),toString()等固定且无实际逻辑意义的方法，从而帮我们大大降低了工作量。

### 9.单例创建

kotlin创建单例类很简单，将class关键改为object即可，即下面这样

```kotlin
package com.example.ktproject

object Singleton {
    fun singleton(){
            println("singleton is called")        
    }
}
```

调用代码,类似java中的静态方法

```kotlin
Singleton.singleton()
```

# Lambda编程

### 1.集合的创建与遍历

#### List和Set

========

##### List

1).创建

在Java中我们这样创建一个list

```java
val list = ArrayList<String>()
list.add("Apple")
list.add("Banana") 
```

kotlin中可以使用内置的listOf函数，来简化初始化操作。

```kotlin
val list = listOf("Apple", "Banana")
```

这里仅用一行代码就完成了集合的初始化操作。注意listOf()函数创建的是一个不可变的集合，只能用于读取。

可以使用mutableListOf替换listOf创建可变集合

```kotlin
val list = mutableListOf("Apple", "Banana")
list.add("Orange")
```

2).遍历

不管是可变集合还是不可变集合，我们都可以使用之前提到的for..in进行遍历。

```kotlin
fun main() {
    for(i in listOf("Apple", "Banana")){
        println("i is $i")

    }
}
```



##### Set

set使用上和list一样，将上面的list或为set既可，listOf改为setOf,mutableListOf改为mutableSetOf.

set的还一个特性就是内部元素不会出现重复，常用作数组去重。



#### Map

##### 用法1:



*创建*

首先和Java中的Map写法一样

```kotlin
val map = HashMap<String, Int>() 
map.put("Apple", 1)
map["Banana"] = 2
```

可以看到赋值有两种形式，推荐=赋值方式。

*读取*

```kotlin
val number = map["Banana"]
```

可以使用get或者上面的方法读取。



区别于java，kotlin和提供了类似上面list和set一样的创建集合的方法。mapOf和mutableMapOf

##### 用法2

*创建*

```kotlin
val map = mapOf("Apple" to 1, "Banana" to 2)
```

关于to关键字后续再补充

*遍历*

不管是方法1还是方法2都可以使用下面的形式进行遍历

```kotlin
for ((fruit, number) in map) {
	println("fruit is  $fruit number is $number")
}
```

可以看到我们在遍历的时候，每一个子项包括键和值，像python的字典使用items方法遍历一样。

#### 2.集合的函数式API

Lambda表达式的完整语法结构

```
{参数名1: 参数类型, 参数名2: 参数类型 -> 函数体}
```

一个例子

```kotlin
val list = listOf("Apple", "Banana")
val lambda = { fruit: String -> fruit.length }
val maxLengthFruit = list.maxBy(lambda)
```

上面的例子是遵循lambda表达式的语法结构来的，但是并不需要使用lambda表达式完整的语法结构。可以进行如下简化

maxBy是找里面最大的元素，参数是一个lambda表达式。(感觉有点像C#中的linq)。

```kotlin
val maxLengthFruit = list.maxBy({ fruit: String -> fruit.length })
```

Kotlin规定，当Lambda参数是函数的最后一个参数时，可以将Lambda表达式移到函数括号的外面。

(这里参数`{ fruit: String -> fruit.length } `是maxBy唯一的参数，那肯定也是最后一个了)

此时代码变为

```kotlin
val maxLengthFruit = list.maxBy() { fruit: String -> fruit.length }
```

接下来，如果Lambda参数是函数的唯一一个参数的话，还可以将函数的括号省略:

```kotlin
val maxLengthFruit = list.maxBy { fruit: String -> fruit.length }
```

emmm 开始变得不太好理解了。

像Python使用lambda一样，Lambda表达式中的参数列表其实在大多数情况下不必声明参数类型

```kotlin
val maxLengthFruit = list.maxBy { fruit -> fruit.length }
```

最后，参数列表只有一个的时候也可以进行省略，但是要使用it关键字代替。

```kotlin
val maxLengthFruit = list.maxBy { it.length }
```

注意it关键词这个地方不能替换为别的。它表示当前的项。



```kotlin
fun main() {
    val list = listOf(1,2,3,4,5,6,7)
    val result = list.filter{
        it%2==0
    }.map { it.xor(3) }.first()
    println(result)
}
```

可以看到这个是链式调用的，可以一直调用其他方法。

除此之外还有很多的函数式api不用一一记住比如any,all等方法。现用现查就行，大部分函数式api在大多数语言中表述的含义是一样的。

### 3.Java函数式Api

如果我们在Kotlin代码中调用了一个 Java方法，并且该方法接收一个Java单抽象方法接口参数，就可以使用函数式API。

不太好理解这段话的含义，举例子

Java原生API中有一个最为常见的单抽象方法接口——Runnable接口

```java
public interface Runnable {
    void run();
}
```

我们知道new Thread可以接收一个Runnable参数。我们就使用java创建子线程举例子。

```java
new Thread(new Runnable() {
    @Override
public void run() { 
  System.out.println("Thread is running");
}
} ).start();
```

翻译成kotlin版本如下

```kotlin
Thread(object : Runnable {
    override fun run() {
println("Thread is running") }
}).start()
```

由于Kotlin完全舍弃了new关键字，因此创建匿名类 实例的时候就不能再使用new了,而是改用了object关键字。

我们可以进一步精简

```kotlin
Thread(Runnable {
println("Thread is running")
}).start()
```

因为Runnable类中 *只有一个* 待实现方法，即使这里没有显式地重写run()方法，Kotlin也能自动明白Runnable后 面的Lambda表达式就是要在run()方法中实现的内容。

如果一个Java方法的参数列表中有且仅有一个Java单抽象方法接口参数，我们还可以将接口名进行省略。

继续简化代码

```kotlin
Thread({
println("Thread is running")
}).start()
```

接下来我们看Thread接收的参数是一个lambda表达式了。根据之前的分析，这里lambda参数是函数的唯一一个参数自然也是方法的最后一个参数，所以进一步优化为

```kotlin
Thread {
		println("Thread is running")
}.start()
```

再举一个常用的例子，创建button按钮并点击的时候

一般我们这么写

```java
button.setOnClickListener(new View.OnClickListener() { 
    @Override
    public void onClick(View v) {
} });
```

 观察发现它又是一个单抽象方法接口

```kotlin
public interface OnClickListener {
    void onClick(View v);
}
```

结合之前的代码分析，最终可以简化为

```kotlin
button.setOnClickListener {
}
```



简单记忆:

Java单抽象方法接口指的是接口中只有一个待实现方法。直接优化成{}之后写内容。



### 空指针

Kotlin将空指针异常的检查提前到了编译时期

#### 判空辅助

第一个 ?.

```kotlin
a?.doSomething()
```

就是TS语法一样，如果a不为空就调用doSomething。

对于函数的可空参数我们可以像下面这样使用

```kotlin
fun doStudy(study: Study?) { 
			study?.readBooks()
      study?.doHomework()
}
```



第二个 ?:

可以当作三元运算符去理解,举个例子

```kotlin
val c = if (a ! = null) { a
} else { b
}
```

可以优化为

```kotlin
val c = a ?: b
```



#### let函数

这个函数提供了函数式API，示例代码如下

```kotlin
obj.let { obj2 ->
// 编写具体的业务逻辑
}
```

通过例子进行理解

```kotlin
fun doStudy(study: Study?) { 
if (study != null) {
			study.readBooks()
		study.doHomework()
} 
}
```

我们可以使用?.结合let将代码改为

```kotlin
fun doStudy(study: Study?) {
  study?.let { stu ->
  stu.readBooks()
  stu.doHomework() }
}
```

另外我们知道lambda表达式的参数列表中只有一个参数时可以不用声明参数，用it关键字代替即可。

```kotlin
fun doStudy(study: Study?) {
study?.let {
it.readBooks()
it.doHomework()
}
}
```



### 其他技巧

1.SecondActivity::class.java的写法就相当于Java中SecondActivity.class的写法。

标准函数**with**、**run**和**apply**

一段代码

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape") 
val builder = StringBuilder()
builder.append("Start eating fruits.\n")
for (fruit in list) {
		builder.append(fruit).append("\n") 
}
builder.append("Ate all fruits.") 
val result = builder.toString()
println(result)
```



下面分别使用with，run以及apply实现代码相同的功能。



#### with

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val result = with(StringBuilder()) {
    append("Start eating fruits.\n") for (fruit in list) {
    append(fruit).append("\n") }
    append("Ate all fruits.")
    toString()
}
println(result)
```

首先我们给with函数的第一个参数传入了一个StringBuilder对象，那么接下来整个Lambda表达式的上下文就会是这个 StringBuilder对象。于是我们在Lambda表达式中就不用再像刚才那样调用 builder.append()和builder.toString()方法了，而是可以直接调用append()和 toString()方法。Lambda表达式的最后一行代码会作为with函数的返回值返回，最终我们将结果打印出来。

#### run

将上面的with改为run的形式如下

run函数通常不会直接调用， 而是要在某个对象的基础上调用;其次run函数只接收一个Lambda参数，并且会在Lambda表 达式中提供调用对象的上下文。

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape") 
val result = StringBuilder().run {
			append("Start eating fruits.\n")
      for (fruit in list) {
		      append(fruit).append("\n") }
     			append("Ate all fruits.")
      		toString()
}
println(result)
```



#### apply

apply函数无法指定返回值，而是会自动返回调用对象本身，上面的代码改为apply之后

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape") 
val result = StringBuilder().apply {
			append("Start eating fruits.\n")
      for (fruit in list) {
		      append(fruit).append("\n") }
     			append("Ate all fruits.")
      		toString()
}
println(result)
```



接下来对代码进行一下修改

````kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val result = StringBuilder().apply {
  append("Start eating fruits.\n") 
  for (fruit in list) {
  		append(fruit).append("\n") }
  		append("Ate all fruits.")
}
println(result.toString())
````

会发现修改之后的结果和我们一开始的不一样，这是因为apply和run的区别在于,apply函数无法指定返回值，只能返回调用对象本身，因此这里的 result实际上是一个StringBuilder对象，所以我们在最后打印的时候还要再调用它的 toString()方法才行。

#### 静态方法



##### 注解方式

Kotlin确实没有直接定义静态方法的关键字，但是提供了一些语法特性来支持类 似于静态方法调用的写法。

```kotlin
class Util {
    fun doAction1() {
        println("do action1")
    }
    @JvmStatic
    companion object {
        fun doAction2() {
            println("do action2")
        }
    }
}
```

可以通过 companion object代码块，将函数包裹使它具有类似静态方法的功能。这里doAction1调用的时候需要使用实例化Util的实例才能调用，但是doAction2就可以通过类名Util直接调用。

companion object这个关键字实际上会在Util类的内部创建一个伴生类，而doAction2()方法就是定义在这个伴生类里面的实例方法。只是Kotlin会保证Util类始终只会存在一个伴生类对象，因此调用Util.doAction2()方法实际上就是调用了Util类中伴生对象的doAction2()方法。

其中@JvmStatic注解，让Kotlin编译器将这些方法编译成真正的静态方法，它只能加在单例类或companion object中的方法上。

##### 顶层方法

除了使用@JvmStatic注解，kotlin还可以使用java没有顶层方法，这个就和在python的类上面单独定义了一个方法一样。

如何在java中进行调用？

比如我们创建的那个文件kotlin文件叫Helper.kt，Kotlin编译器会自动创建一个叫作HelperKt的Java类， 在Java中使用HelperKt.doSomething()的写法来调用就可以了。

#### 变量延迟初始化

延迟初始化使用的是lateinit关键字，它可以告诉Kotlin编译器，我会在晚些时候对这个变量 进行初始化，这样就不用在一开始的时候将它赋值为null了。

### 泛型和委托

#### 泛型

定一个泛型的类

```kotlin
class MyClass<T> {
    fun method(params:T):T{
        return params
    }
}
```

下面开始调用这个类以及方法 其中的T需要我们指定类型。这里指定其为Int类型。



```kotlin
val myclass = MyClass<Int>();
val result = myclass.method(123)
```

如果我们只想定义一个泛型的方法而不是泛型的类。

````kotlin
class MyClass {
    fun <T> method(param: T): T {
        return param
} 
}
````

调用方法有所变化

```kotlin
val myClass = MyClass()
val result = myClass.method<Int>(123)
```

因为Kotlin有类型推导机制我们可以进一步简化

```kotlin
val myClass = MyClass()
val result = myClass.method(123)
```

另外可以限定method传入的类型

```kotlin
   fun <T> method(param: T)
```

如果限定为数字类型可以改为下面这样，

```kotlin
   fun <T：Number> method(param: T)
```

Number表示Int，Double以及Float等数字类型

默认的限制类型其实是Any?

#### 委托

委托是一种设计模式，它的基本理念是:操作对象自己不会去处理某段逻辑，而是会把工作委 托给另外一个辅助对象去处理。

Kotlin委托功能分为了两种:类委托和委托属性。

##### 类委托

委托的关键字为by，类委托的核心思想是将一个类的具体实现委托给另一个类去完成。

一段代码示例

```kotlin
class MySet<T>(val helperSet: HashSet<T>) : Set<T> {
    override val size: Int get() = helperSet.size
    override fun contains(element: T) = helperSet.contains(element)
    override fun containsAll(elements: Collection<T>) = helperSet.containsAll(elements) override fun isEmpty() = helperSet.isEmpty()
    override fun iterator() = helperSet.iterator()
}
```

这里我们自己定义了一个方法实现了Set。通过类委托上面的代码可以改为



```kotlin
class MySet<T>(val helperSet: HashSet<T>) : Set<T> by helperSet { 
		fun helloWorld() = println("Hello World")
		override fun isEmpty() = false
}
```

不仅实现了原来Set的功能，还重写了自己需要方法。

#### 委托属性

委托属性的语法结构

```kotlin
class MyClass {
    var p by Delegate()
}
```

上面代码表示将p属性的具体实现委托给了Delegate类去完成,当调用p属性的时候会自动调用Delegate类的getValue()方法，当给p属性赋值的时候会自动调用Delegate类的 setValue()方法。所以我们需要实现一个Delegate类才行。

```kotlin
class Delegate {
		var propValue: Any? = null
		operator fun getValue(myClass: MyClass, prop: KProperty<*>): Any? {
      return propValue
		}
		operator fun setValue(myClass: MyClass, prop: KProperty<*>, value: Any?) {
      propValue = value
		} 
	}
```

在Delegate类中我们必须实现getValue()和setValue()这两个方法，并且都要使用operator关键字进行声明。

getValue()方法要接收两个参数:第一个参数用于声明该Delegate类的委托功能可以在什么类中使用，这里写成MyClass表示仅可在MyClass类中使用;第二个参数KProperty<*>是 Kotlin中的一个属性操作类，可用于获取各种属性相关的值，在当前场景下用不着，但是必须在方法参数上进行声明。另外，<*>这种泛型的写法表示你不知道或者不关心泛型的具体类型，只是为了通过语法编译而已，有点类似于Java中<?>的写法。至于返回值可以声明成任何类型，根据具体的实现逻辑去写就行了，上述代码只是一种示例写法。

setValue()要接收3个参数。前两个参数和getValue()方法是相同的，最后一个参数表示具体要赋值给委托属性的值，这个参数的类型必须和getValue()方法返回值的类型保持一致。

整个委托属性的工作流程就是这样实现的，现在当我们给MyClass的p属性赋值时，就会调用 Delegate类的setValue()方法，当获取MyClass中p属性的值时，就会调用Delegate类的 getValue()方法。

### 泛型高级用法

#### 泛型实化

*类型擦除机制*

Java的泛型功能是通过类型擦除机制来实现的。什么意思呢?就是说泛型对于类 型的约束只在编译时期存在，运行的时候仍然会按照JDK 1.5之前的机制来运行，JVM是识别不 出来我们在代码中指定的泛型类型的。例如，假设我们创建了一个List<String>集合，虽然 在编译时期只能向集合中添加字符串类型的元素，但是在运行时期JVM并不能知道它本来只打算 包含哪种类型的元素，只能识别出来它是个List。

Kotlin提供了一个内联函数的概念，内联函数中的代码会在编译的时候自动被替换到调用它的地方，这样的话也就不存 在什么泛型擦除的问题了，因为代码在编译之后会直接使用实际的类型来替代内联函数中的泛型声明。

泛型实化的一个代码示例

```kotlin
inline fun <reified T> getGenericType() {
}
```

函数必须是内联函数才行，也就是要用inline 关键字来修饰该函数。其次，在声明泛型的地方必须加上reified关键字来表示该泛型要进行 实化。





