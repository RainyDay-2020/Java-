2020/9/11

+++

### ==和equals

**`==`** : 它的作用是判断两个对象的地址是不是相等。即判断两个对象是不是同一个对象。(**基本数据类型==比较的是值，引用数据类型==比较的是内存地址**)

> 因为 Java 只有值传递，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是引用类型变量存的值是对象的地址。

**`equals()`** : 它的作用也是判断两个对象是否相等，它不能用于比较基本数据类型的变量。

```java
public class test1 {
    public static void main(String[] args) {
        String a = new String("ab"); // a 为一个引用
        String b = new String("ab"); // b为另一个引用,对象的内容一样
        String aa = "ab"; // 放在常量池中
        String bb = "ab"; // 从常量池中查找
        if (aa == bb) // true
            System.out.println("aa==bb");
        if (a == b) // false，非同一对象
            System.out.println("a==b");
        if (a.equals(b)) // true
            System.out.println("aEQb");
        if (42 == 42.0) { // true
            System.out.println("true");
        }
    }
}
```

**说明**

1. String中的 equals 方法是被**重写**过的，因为 Object的 equals 方法比较的是对象的内存地址，而 ==String的 equals 方法比较的是对象的值。==
2. 当创建String类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 String 对象。
3. `String a = new String("ab")`会创建一个新的对象，而`String b =  "ab"`，Java在编译时，首先会从常量池中寻找，如果有则指向常量池中的对象；没有则放到常量池中。

### hashCode()与equals()

1. 为什么重写equals方法时必须重写hashCode()方法
   + hashCode()的作用是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。因为hashCode定义在JDK的Object类中，所以Java的任何类中都含有该函数。

2. 为什么要有hashCode?
   + 以HashSet 如何检查重复”为例子来说明为什么要有 hashCode？
     1. 当你把对象加入HashSet时，HashSet会先计算对象的 hashcode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashCode 值作比较。
     2. 如果没有相符的 hashCode，HashSet会假设对象没有重复出现。
     3. 但是如果发现有相同 hashcode 值的对象**（也就是说hashCode只是用来缩小查找样本）**，这时会调用 equals() 方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，HashSet就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。
   + 这样我们就大大减少了 equals 的次数，相应就大大提高了执行速度。

3. 为什么重写equals时必须重写hashCode方法？

   + 如果两个对象相等，则 hashcode 一定也是相同的。

   + 两个对象相等,对两个对象分别调用 equals 方法都返回 true。

   + 但是，两个对象有相同的 hashcode 值，它们也不一定是相等的 。**因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖。**

     > `hashCode()`的默认行为是对堆上的对象产生独特值。如果没有重写 `hashCode()`，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）

### 基本数据类型

1. Java 里使用 long 类型的数据一定要在数值后面加上 **L**，否则将作为整型解析

2. Java 基本类型的包装类的大部分都实现了常量池技术，即==Byte,Short,Integer,Long,Character,Boolean==；前面 4 种包装类默认创建了数值[-128，127] 的相应类型的缓存数据，==Character==创建了数值在[0,127]范围的缓存数据，==Boolean== 直接返回True Or False。**如果超出对应范围仍然会去创建新的对象。**`两种浮点数类型的包装类 Float,Double 并没有实现常量池技术`

   1. Integer i1=40；Java 在编译的时候会直接将代码封装成 Integer i1=Integer.valueOf(40);，从而使用常量池中的对象。

   2. Integer i1 = new Integer(40);这种情况下会创建新的对象。

      ```java
        Integer i1 = 40;
        Integer i2 = new Integer(40);
        System.out.println(i1 == i2);//输出 false
      ```

### 函数

> 1. 按值调用(call by value)表示方法接收的是调用者提供的值
>
> 2. 而按引用调用（call by reference)表示方法接收的是调用者提供的变量地址。
> 3. 一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值。
> 4. Java中只有值传递

1. **值传递小结：**

   1. 一个方法**不能修改一个基本数据类型的参数**（即数值型或布尔型，形参交换参数值不会影响实参）。

   2. 一个方法**可以改变一个对象参数的状态**。（如字符数组中实参是arr，形参array被初始化为arr引用的拷贝也就是一个对象的引用。也就是array和arr指向的是同一个数组对象。因此外部对象引用对象的改变会反映到所对应的对象上）

   3. 一个方法不能让对象参数引用一个新的对象（不能引用其它的对象）。

      ```java
      public class Test {
      
      	public static void main(String[] args) {
      		// TODO Auto-generated method stub
      		Student s1 = new Student("小张");
      		Student s2 = new Student("小李");
      		Test.swap(s1, s2);
      		System.out.println("s1:" + s1.getName());
      		System.out.println("s2:" + s2.getName());
      	}
      
      	public static void swap(Student x, Student y) {
      		Student temp = x;
      		x = y;
      		y = temp;
      		System.out.println("x:" + x.getName());
      		System.out.println("y:" + y.getName());
      	}
      }
      
      //输出结果
      
      x:小李
      y:小张
      s1:小张
      s2:小李
      
      ```

      

2. **成员变量与局部变量的区别**

   1. 量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰。
   2. 从变量在内存中的存储方式来看:如果成员变量是使用static修饰的，那么这个成员变量是属于类的，如果没有使用static修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
   3. 从变量在内存中的生存时间上看:成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动消失。
   4. 成员变量如果没有被赋初值:则会自动以类型的默认值而赋值（一种情况例外:被 final 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。

3. **对象实体与对象引用有何不同?**

   1. new 创建对象实例（**对象实例在堆内存中**）

   2. 对象引用指向对象实例（对象引用存放在栈内存中）。

      如：一个对象引用可以指向 0 个或 1 个对象（一根绳子可以不系气球，也可以系一个气球）;一个对象可以有 n 个引用指向它（可以用 n 条绳子系住一个气球）。

### 面向对象

1. 封装：把一个对象的属性隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是提供一些可以被外界访问的方法来操作属性。
2. 继承
   1. 子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，**只是拥有**。
   2. 子类可以拥有自己属性和方法，即子类可以对父类进行扩展。
   3. 子类可以用自己的方式实现父类的方法
3. 多态：父类的引用指向了子类的实例
   1. 方法具有多态性，属性不具有多态性；

`StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。`StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。