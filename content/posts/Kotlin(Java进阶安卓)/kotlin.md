# 一.语法(仅记录与Java不同的地方)
## (1)基础语法
### 1. hello world!
~~~
fun main(){
    println("Hello world,Hello My Darling~")
}
~~~
1. fun用以**定义函数**,function嘛
2. println比起java不需要system了
3. 并且结尾不需要使用分号结尾了
### 2. 变量：
#### 2.1. 声明：
1. val声明的变量表示不可变的变量(常量)
   val myLover = "胡欣琦"
   或val myLover:string = "hxq"
   其数据类型会自动识别，也可手动设置
2. var声明的变量表示可变的变量
   var lunch = 5
### 3.1 函数：(Java中类比于 方法 )
1. 主函数:
~~~
fun main(){
    val myLover = "胡欣琦"
    println(sayLove(myLover))
    println(sayLove("胡欣琦"))
    println(sayLove(Darling = "胡欣琦"))
    println(sayLove())
}
~~~
2. 普通函数:
~~~
fun sayLove(name:String = "Darling"):Int{
    println("$name,I love you~")
    return 520
}
~~~
可以给变量赋默认值,括号后面添加返回值的类型
3. 特殊情况,函数体只有一行(可略写return,返回值类型和大括号):
~~~
fun sayLove(name:String = "Darling"):"$name,I love you~"
~~~
### 3.2 拓展函数/属性:
1. 拓展函数:
~~~
fun 拓展函数名.自定义拓展名(参数):返回类型{
    ...
}

fun String.isPalindrome():Boolean{
    return this == this.reversed()
}
~~~
2. 拓展属性:不能有后台字段(无法在类储存状态)
    等价于无法真的储存东西,更像是一种方法而非空间字段,
    更多作为**只读属性**存在
~~~
val String.firstChar:Char
    get() = get(0)
...
class Person(var name:String)

var Person.displayName:String
    get() = "Person:${name}"
    //可直接通过displayName获取字段"Person:${name}"
    set(value){
        name = value
    }
    //可直接通过displayName传入字段给name
~~~



### 4. 流程:
#### 4.1. if:
1. 直接if简写:
~~~
fun ifTest1(a:Int,b:Int):Int{
    return if (a>b) a else b
}   
~~~
2. 整个函数简写:
~~~
fun ifTest(a:Int,b:Int) = if (a>b) a else b
~~~
#### 4.2 when(类似于java的 switch)
1. when正常式:
~~~
fun ifTest1(a:Int,b:Int):Int{
    return when{
        a>b -> a
        a<b -> b
        else -> c//此处必须要有else,除非能把所有条件枚举遍
        //也就是说when包含了一个判断条件的所有可能性
    }
}   
~~~
2. when简化:
~~~
fun ifTest1(a:Int,b:Int) = when{
    a>b -> a
    a<b -> b
    else -> c//此处必须要有else,除非能把所有条件枚举遍
    //也就是说when包含了一个判断条件的所有可能性
}  
~~~
#### 4.3循环(while与java一样)
for循环:与python类似:(**区间左闭右开**)
~~~
for (i in 1 until 10 step 2){
    println("${i}")
}
//for (i in 1 .. 10 step 2)
//降序:for (i in 10 downTo 1 step 2)
~~~
### 5. 类和对象
#### 5.1 类的继承与构造:
~~~
open class People(var name:String,var age:Int = 18){
    //需要构造的写在小括号里,可加默认值
    //添加val/var表示其成为类的 属性,可通过方法访问,否则仅为函数构造的参数
    //若父类已添加val/var使其变成属性,则子类无需添加
    //若不写open,则默认是final的无法继承
    //需要重写的方法,属性也要写open
    open val beauty = ""
    open fun sayLove(){
        ...
    }
}
...
class Lover(name:String,age:Int):People(name,age){
    //以People为父类,继承它
    //此处复用了父类的属性
    //重写的方法要加override关键字
    open val beauty = "cute"
    override fun sayLove(){
        println("${name},I love you~")
    }
    //在后续没有别的字符什么的可以省略${name}的{}
    //不过建议还是写一下,更明显一点
}
...
fun main(){
    val myLover = Lover("hxq",20)
    myLover.sayLove()
}
~~~
### 6. 抽象:
abstract修饰的类,属性,方法,**父类无需标注open**,当然,abstract是需要的
### 7. 接口:
~~~
interface People{
    fun say()
}
...
class Lover:Perple{
    override fun say(){
        ...
    }
}
~~~
### 8. List集合:
1. listOf(不可变的集合):
    val list1 = listOf(1,2,"hxq")
    val list100 = listOf\<Int>(1,2,3)//添加了泛式,只能有Int元素
2. mutableListOf(可变的集合):
    val list2 = mutableListOf(1,2,3)
    list2.add(4)
### 9. 函数式API(关键):
1. 各自函数(部分):
sortedDescending():降序
max():最大
min():最小
sum,average:求和,求平均
filter{条件}:筛选符合条件的元素
any{条件}:判断是否存在元素满足条件,true/false
all{条件}:判断是否所有元素满足条件,true/false
joinToString(separator = "什么符号分割",prefix = "最前面的符号",postfix = "最后面的符号"):拼接字符串
2. 示例(部分):
~~~
fun main(){
    val list = listOf(1,3,1,4,5,2,0,-9)
    val joinToString = list.joinToString(separator = ".",prefix = "[",postfix = "]")
    println("list的元素是 : ${joinToString}")
    //list的元素是:[1.3.1.4.5.2.0.-9]

    println("list的最大值是:${list.max()}")
    //list的最大值是:5

    val listFilter = list.filter{i>-1}
    //listFilter:{1,3,1,4,5,2,0}
}
~~~
### 10.空安全(指针,变量等):
~~~
val name:String = null
//报错
    
val name2:Stirng? = null
val length1 = name2?.length
val length2 = name2?.length ?:0
//可以输出,输出为null需要加个"?"
//可以在后面添加"?:默认值",以此来输出默认值而非null

println(length1)
//null
println(length2)
//0
~~~
### 11. lateinit(延迟初始化/赋值)
允许一个非空属性(var,不可空)先不初始化,之后再赋值
保证是非空的,所以编译器不会额外去校验,也不用反复添加?
~~~
lateinit val element:String
~~~
### 12. 泛型:
~~~
class Box<T>(var content: T){
    fun getContents():T{
        return contents\
    }
}
~~~






