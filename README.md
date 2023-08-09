下面的内容均来自Android第一行代码 第三版，这里整理出来方便自己后续查看。

kotlin每一行代码后面无需加;

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

```
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



