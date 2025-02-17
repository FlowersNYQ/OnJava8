[TOC]

# 第三章 万物皆对象

> 如果我们说另外一种不同的语言，我们会发觉一个不同的世界！— Ludwig Wittgenstein (1889-1951)

相比 C++ ，Java 是一种更纯粹的面向对象编程语言。虽然它们都是混合语言，但在 Java 中，设计者们认为混合的作用并非像在 C++ 中那般重要。混合语言允许多种编程风格，这也是 C++ 支持向后兼容 C 的原因。正因为 C++ 是 C 语言的超集，所以它也同时包含了许多 C 语言不具备的特性，这使得 C++ 在某些方面过于复杂。

 Java 语言假设你只进行面向对象编程。开始学习之前，我们需要将思维置于面向对象的世界。本章你将了解到 Java 程序的基本组成，学习在 Java 中万物（几乎）皆对象的思想。

<!-- You Manipulate Objects with References -->

## 对象操纵

“名字代表什么？玫瑰即使不叫玫瑰，也依旧芬芳”。（引用自 莎士比亚，《罗密欧与朱丽叶》）。

所有的编程语言都会操纵内存中的元素。有时程序员必须要有意识地直接或间接地操纵它们。在 C/C++ 中，对象的操纵是通过指针来完成的。

Java 利用万物皆对象的思想和单一一致的语法方式来简化问题。虽万物皆可为对象，但我们所操纵的标识符实际上只是对对象的“引用” [^1]。 举例：我们可以用遥控器（引用）去操纵电视（对象）。只要拥有对象的“引用”，就可以操纵该“对象”。换句话说，我们无需直接接触电视，就可通过遥控器（引用）自由地控制电视（对象）的频道和音量。此外，没有电视，遥控器也可以单独存在。就是说，你仅仅有一个“引用”并不意味着你必然有一个与之关联的“对象”。 

下面来创建一个 **String** 引用，用于保存单词或语句。代码示例：

```java
    String s;
```

这里我们只是创建了一个 **String** 对象的引用，而非对象。直接拿来使用会出现错误：因为此时你并没有给变量 `s` 赋值--指向任何对象。通常更安全的做法是：创建一个引用的同时进行初始化。代码示例：

```java
    String s = "asdf";
```

Java 语法允许我们使用带双引号的文本内容来初始化字符串。同样，其他类型的对象也有相应的初始化方式。

<!-- You Must Create All the Objects -->
## 对象创建

“引用”用来关联“对象”。在 Java 中，通常我们使用`new`操作符来创建一个新对象。`new` 关键字代表：创建一个新的对象实例。所以，我们也可以这样来表示前面的代码示例：

```java
    String s = new String("asdf");
```
以上展示了字符串对象的创建过程，以及如何初始化生成字符串。除了 **String** 类型以外，Java 本身自带了许多现成的数据类型。除此之外，我们还可以创建自己的数据类型。事实上，这是 Java 程序设计中的一项基本行为。在本书后面的学习中将会接触到。

<!-- Where Storage Lives -->
### 数据存储

那么，程序在运行时是如何存储的呢？尤其是内存是怎么分配的。有5个不同的地方可以存储数据：

1. **寄存器**（Registers）最快的存储区域，位于 CPU 内部 [^2]。然而，寄存器的数量十分有限，所以寄存器根据需求进行分配。我们对其没有直接的控制权，也无法在自己的程序里找到寄存器存在的踪迹（另一方面，C/C++ 允许开发者向编译器建议寄存器的分配）。

2. **栈内存**（Stack）存在于常规内存 RAM（随机访问存储器，Random Access Memory）区域中，可通过栈指针获得处理器的直接支持。栈指针下移分配内存，上移释放内存。这是一种仅次于寄存器的非常快速有效的分配存储方式。创建程序时，Java 系统必须知道栈内保存的所有项的生命周期。这种约束限制了程序的灵活性。因此，虽然在栈内存上存在一些 Java 数据（如对象引用），但 Java 对象本身的数据却是保存在堆内存的。

3. **堆内存**（Heap）这是一种通用的内存池（也在 RAM 区域），所有 Java 对象都存在于其中。与栈内存不同，编译器不需要知道对象必须在堆内存上停留多长时间。因此，用堆内存保存数据更具灵活性。创建一个对象时，只需用 `new` 命令实例化对象即可，当执行代码时，会自动在堆中进行内存分配。这种灵活性是有代价的：分配和清理堆内存要比栈内存需要更多的时间（如果可以用 Java 在栈内存上创建对象，就像在 C++ 中那样的话）。随着时间的推移，Java 的堆内存分配机制现在已经非常快，因此这不是一个值得关心的问题了。

4. **常量存储**（Constant storage）常量值通常直接放在程序代码中，因为它们永远不会改变。如需严格保护，可考虑将它们置于只读存储器 ROM （只读存储器，Read Only Memory）中 [^3]。

5. **非 RAM 存储**（Non-RAM storage）数据完全存在于程序之外，在程序未运行以及脱离程序控制后依然存在。两个主要的例子：（1）序列化对象：对象被转换为字节流，通常被发送到另一台机器；（2）持久化对象：对象被放置在磁盘上，即使程序终止，数据依然存在。这些存储的方式都是将对象转存于另一个介质中，并在需要时恢复成常规的、基于 RAM 的对象。Java 为轻量级持久化提供了支持。而诸如 JDBC 和 Hibernate 这些类库为使用数据库存储和检索对象信息提供了更复杂的支持。

<!-- Special Case: Primitive Types -->
### 基本类型的存储

有一组类型在 Java 中使用频率很高，它们需要特殊对待，这就是 Java 的基本类型。之所以这么说，是因为它们的创建并不是通过 `new` 关键字来产生。通常 `new` 出来的对象都是保存在堆内存中的，以此方式创建小而简单的变量往往是不划算的。所以对于这些基本类型的创建方法，Java 使用了和 C/C++ 一样的策略。也就是说，不是使用 `new` 创建变量，而是使用一个“自动”变量。 这个变量直接存储"值"，并置于栈内存中，因此更加高效。

Java 确定了每种基本类型的内存占用大小。 这些大小不会像其他一些语言那样随着机器环境的变化而变化。这种不变性也是 Java 更具可移植性的一个原因。

| 基本类型  |    大小 |  最小值  | 最大值  | 包装类型 |
| :------: | :------: | :------: | :------: | :------: |
| boolean | —  | — | — | Boolean |
| char | 16 bits | Unicode 0  | Unicode 2<sup>16</sup> -1  | Character |
| byte | 8 bits | -128 | +127 | Byte |
| short | 16 bits | - 2<sup>15</sup> | + 2<sup>15</sup> -1 | Short |
| int | 32 bits | - 2<sup>31</sup> | + 2<sup>31</sup> -1 | Integer |
| long | 64 bits | - 2<sup>63</sup> | + 2<sup>63</sup> -1 | Long |
| float | 32 bits | IEEE754 | IEEE754 | Float |
| double | 64 bits |IEEE754 | IEEE754 | Double |
| void | — | — | — | Void |

所有的数值类型都是有正/负符号的。布尔（boolean）类型的大小没有明确的规定，通常定义为取字面值 “true” 或 “false” 。基本类型有自己对应的包装类型，如果你希望在堆内存里表示基本类型的数据，就需要用到它们的包装类。代码示例：

```java
char c = 'x';
Character ch = new Character(c);
```
或者你也可以使用下面的形式：

```java
Character ch = new Character('x');
```

基本类型自动转换成包装类型（自动装箱）

```java
Character ch = 'x';
```

相对的，包装类型转化为基本类型（自动拆箱）：

```java
char c = ch;
```

个中原因将在以后的章节里解释。

<!-- High-Precision Numbers -->
### 高精度数值

在 Java 中有两种类型的数据可用于高精度的计算。它们是 `BigInteger` 和 `BigDecimal`。尽管它们大致可以划归为“包装类型”，但是它们并没有对应的基本类型。

这两个类包含的方法提供的操作，与对基本类型执行的操作相似。也就是说，能对 int 或 float 做的运算，在 BigInteger 和 BigDecimal 这里也同样可以，只不过必须要通过调用它们的方法来实现而非运算符。此外，由于涉及到的计算量更多，所以运算速度会慢一些。诚然，我们牺牲了速度，但换来了精度。

BigInteger 支持任意精度的整数。可用于精确表示任意大小的整数值，同时在运算过程中不会丢失精度。
BigDecimal 支持任意精度的定点数字。例如，可用它进行精确的货币计算。

关于这两个类的详细信息，请参考 JDK 官方文档。

<!-- Arrays in Java -->
### 数组的存储

许多编程语言都支持数组类型。在 C 和 C++ 中使用数组是危险的，因为那些数组只是内存块。如果程序访问了内存块之外的数组或在初始化之前使用该段内存（常见编程错误），则结果是不可预测的。

Java 的设计主要目标之一是安全性，因此许多困扰 C 和 C++ 程序员的问题不会在 Java 中再现。在 Java 中，数组使用前需要被初始化，并且不能访问数组长度以外的数据。这种范围检查，是以每个数组上少量的内存开销及运行时检查下标的额外时间为代价的，但由此换来的安全性和效率的提高是值得的。（并且 Java 经常可以优化这些操作）。

当我们创建对象数组时，实际上是创建了一个引用数组，并且每个引用的初始值都为 **null** 。在使用该数组之前，我们必须为每个引用指定一个对象 。如果我们尝试使用为 **null** 的引用，则会在运行时报错。因此，在 Java 中就防止了数组操作的常规错误。

我们还可创建基本类型的数组。编译器通过将该数组的内存全部置零来保证初始化。本书稍后将详细介绍数组，特别是在数组章节中。

<!-- Comments -->
## 代码注释

Java 中有两种类型的注释。第一种是传统的 C 风格的注释，以 `/*` 开头，可以跨越多行，到 `*/ ` 结束。注意，许多程序员在多行注释的每一行开头添加 `*`，所以你经常会看到：

```java
/* 这是
* 跨越多行的
* 注释
*/
```

但请记住，`/*` 和 `*/` 之间的内容都是被忽略的。所以你将其改为下面这样也是没有区别的。

```java
/* 这是跨越多
行的注释 */
```

第二种注释形式来自 C++ 。它是单行注释，以 `//` 开头并一直持续到行结束。这种注释方便且常用，因为直观简单。所以你经常看到：

```java
// 这是单行注释
```

<!-- You Never Need to Destroy an Object -->
## 对象清理

在一些编程语言中，管理变量的生命周期需要大量的工作。一个变量需要存活多久？如果我们想销毁它，应该什么时候去做呢？变量生命周期的混乱会导致许多 bug，本小结向你介绍 Java 是如何通过释放存储来简化这个问题的。

<!-- Scoping -->
### 作用域

大多数程序语言都有作用域的概念。作用域决定了在该范围内定义的变量名的可见性和生存周期。在 C、 C++ 和 Java 中，作用域是由大括号 `{}` 的位置决定的。例如：

```java
{
    int x = 12;
    // 仅 x 变量可用
    {
        int q = 96;
        // x 和 q 变量皆可用
    }
    // 仅 x 变量可用
    // 变量 q 不在作用域内
}
```

Java 的变量只有在其作用域内才可用。缩进使得 Java 代码更易于阅读。由于 Java 是一种自由格式的语言，额外的空格、制表符和回车并不会影响程序的执行结果。在 Java 中，你不能执行以下操作，即使这在 C 和 C++ 中是合法的：

```java
{
    int x = 12;
    {
        int x = 96; // Illegal
    }
}
```

在上例中， Java 编译器会在提示变量 x 已经被定义过了。因此，在 C/C++ 中将一个较大作用域的变量"隐藏"起来的做法，在 Java 中是不被允许的。 因为 Java 的设计者认为这样做会导致程序混乱。

<!-- Scope of Objects -->
### 对象作用域

Java 对象与基本类型具有不同的生命周期。当我们使用 `new` 关键字来创建 Java 对象时，它的生命周期将会超出作用域。因此，下面这段代码示例：

```java
{
    String s = new String("a string");
} 
// 作用域终点
```

上例中，引用 s 在作用域终点就结束了。但是，引用 s 指向的字符串对象依然还在占用内存。在这段代码中，我们无法在这个作用域之后访问这个对象，因为唯一对它的引用 s 已超出了作用域的范围。在后面的章节中，我们还会学习怎么在编程中传递和复制对象的引用。

只要你需要，`new` 出来的对象就会一直存活下去。 相比在 C++ 编码中操作内存可能会出现的诸多问题，这些困扰在 Java 中都不复存在了。在 C++ 中你不仅要确保对象的内存在你操作的范围内存在，还必须在使用完它们之后，将其销毁。

那么问题来了：我们在 Java 中并没有主动清理这些对象，那么它是如何避免 C++ 中出现的内存被填满从而阻塞程序的问题呢？答案是：Java 的垃圾收集器会检查所有 `new` 出来的对象并判断哪些不再可达，继而释放那些被占用的内存，供其他新的对象使用。也就是说，我们不必担心内存回收的问题了。你只需简单创建对象即可。当其不再被需要时，能自行被垃圾收集器释放。垃圾回收机制有效防止了因程序员忘记释放内存而造成的“内存泄漏”问题。

<!-- Creating New Data Types: class -->
## 类的创建

### 类型

如果一切都是对象，那么是什么决定了某一类对象的外观和行为呢？换句话说，是什么确定了对象的类型？你可能很自然地想到 `type` 关键字。但是，事实上大多数面向对象的语言都使用 `class` 关键字类来描述一种新的对象。 通常在 `class` 关键字的后面的紧跟类的的名称。如下代码示例：

```java
class ATypeName {
 // 这里是类的内部
}
```

在上例中，我们引入了一个新的类型，尽管这个类里只有一行注释。但是我们一样可以通过 `new` 关键字来创建一个这种类型的对象。如下：

```java
ATypeName a = new ATypeName();
```

到现在为止，我们还不能用这个对象来做什么事（即不能向它发送任何有意义的消息），除非我们在这个类里定义一些方法。

<!-- Fields -->
### 字段

当我们创建好一个类之后，我们可以往类里存放两种类型的元素：方法（method）和字段（field）。类的字段可以是基本类型，也可以是引用类型。如果类的字段是对某个对象的引用，那么必须要初始化该引用将其关联到一个实际的对象上（通过之前介绍的创建对象的方法）。每个对象都有用来存储其字段的空间。通常，字段不在对象间共享。下面是一个具有某些字段的类的代码示例：

```java
class DataOnly {
    int i;
    double d;
    boolean b;
}
```

这个类除了存储数据之外什么也不能做。但是，我们仍然可以通过下面的代码来创建它的一个对象：

```java
    DataOnly data = new DataOnly();
```

我们必须通过这个对象的引用来指定字段值。格式：对象名称.方法名称或字段名称。代码示例：

```java
    data.i = 47;
    data.d = 1.1;
    data.b = false;
```

如果你想修改对象内部包含的另一个对象的数据，可以通过这样的格式修改。代码示例：

```java
    myPlane.leftTank.capacity = 100;
```

你可以用这种方式嵌套许多对象（尽管这样的设计会带来混乱）。

<!-- Default Values for Primitive Members -->
### 基本类型默认值

如果类的成员变量（字段）是基本类型，那么在类初始化时，这些类型将会被赋予一个初始值。

| 基本类型 | 初始值 |
| :-----: |:-----: |
| boolean | false |
| char | \u0000 (null) |
| byte | (byte) 0 |
| short |(short) 0 |
| int | 0 |
| long | 0L |
| float | 0.0f |
| double | 0.0d |

这些默认值仅在 Java 初始化类的时候才会被赋予。这种方式确保了基本类型的字段始终能被初始化（在 C++ 中不会），从而减少了 bug 的来源。但是，这些初始值对于程序来说并不一定是合法或者正确的。 所以，为了安全，我们最好始终显式地初始化变量。

这种默认值的赋予并不适用于局部变量 —— 那些不属于类的字段的变量。 因此，若在方法中定义的基本类型数据，如下：

```java
    int x;
```

这里的变量 x 不会自动初始化为0，因而在使用变量 x 之前，程序员有责任主动地为其赋值（和 C 、C++ 一致）。如果我们忘记了这一步， Java 将会提示我们“编译时错误，该变量可能尚未被初始化”。 这一点做的比 C++ 更好，在后者中，编译器只是提示警告，而在 Java 中则直接报错。

<!-- Methods, Arguments,and Return Values -->
### 方法使用

在许多语言（如 C 和 C++）中，使用术语 *函数* (function) 用来命名子程序。在 Java 中，我们使用术语 *方法*（method）来表示“做某事的方式”。

在 Java 中，方法决定对象能接收哪些消息。方法的基本组成部分包括名称、参数、返回类型、方法体。格式如：

```java
 [返回类型] [方法名](/*参数列表*/){
     // 方法体
 }
```

#### 返回类型

方法的返回类型表明了当你调用它时会返回的结果类型。参数列表则显示了可被传递到方法内部的参数类型及名称。方法名和参数列表统称为**方法签名**（signature of the method）。签名作为方法的唯一标识。

Java 中的方法只能作为类的一部分创建。它只能被对象所调用 [^4]，并且该对象必须有权限来执行调用。若对象调用错误的方法，则程序将在编译时报错。

我们可以像下面这样调用一个对象的方法：

```java
[对象引用].[方法名](参数1, 参数2, 参数3);
```

若方法不带参数，例如一个对象引用 `a` 的方法 `f` 不带参数并返回 **int** 型结果，我们可以如下表示：

```java
int x = a.f();
```

上例中方法 `f` 的返回值类型必须和变量 `x` 的类型兼容 。调用方法的行为有时被称为向对象发送消息。面向对象编程可以总结为：向对象发送消息。

<!-- The Argument List -->
#### 参数列表

方法参数列表指定了传递给方法的信息。正如你可能猜到的，这些信息就像 Java 中的其他所有信息 ，以对象的形式传递。参数列表必须指定每个对象的类型和名称。同样，我们并没有直接处理对象，而是在传递对象引用 [^5] 。但是引用的类型必须是正确的。如果方法需要 String 参数，则必须传入 String，否则编译器将报错。

```java
int storage(String s) {
    return s.length() * 2;
}
```

此方法计算并返回某个字符串所占的字节数。参数 `s` 的类型为 **String** 。将 s 传递给 `storage()` 后，我们可以把它看作和任何其他对象一样，可以向它发送消息。在这里，我们调用 `length()` 方法，它是一个 String 方法，返回字符串中的字符数。字符串中每个字符的大小为 16 位或 2 个字节。你还看到了 **return** 关键字，它执行两项操作。首先，它意味着“方法执行结束”。其次，如果方法有返回值，那么该值就紧跟 **return** 语句之后。这里，返回值是通过计算

```java
s.length() * 2
```
产生的。在方法中，我们可以返回任何类型的数据。如果我们不想方法返回数据，则可以通过给方法标识 `void` 来表明这是一个无需返回值的方法。 代码示例：

```java
boolean flag() { 
    return true; 
}

double naturalLogBase() { 
    return 2.718; 
}

void nothing() {
     return;
}

void nothing2() {

}
```

当返回类型为 **void** 时， **return** 关键字仅用于退出方法，因此在方法结束处的 **return** 可被省略。我们可以随时从方法中返回，但若方法返回类型为非 `void`，则编译器会强制我们返回相应类型的值。

上面的描述可能会让你感觉程序只不过是一堆包含各种方法的对象，在这些方法中，将对象作为参数并发送消息给其他对象。大部分情况下确实如此。但在下一章的运算符中我们将会学习如何在方法中做出决策来完成更底层、详细的工作。对于本章，知道如何发送消息就够了。

<!-- Writing a Java Program -->
## 程序编写

在看到第一个 Java 程序之前，我们还必须理解其他几个问题。

### 命名可见性

命名控制在任何一门编程语言中都是一个问题。如果你在两个模块中使用相同的命名，那么如何区分这两个名称，并防止两个名称发生“冲突”呢？在 C 语言编程中这是很具有挑战性的，因为程序通常是一个无法管理的名称海洋。C++ 将函数嵌套在类中，所以它们不会和嵌套在其他类中的函数名冲突。然而，C++ 还是允许全局数据和全局函数，因此仍有可能发生冲突。为了解决这个问题，C++ 使用附加的关键字引入了*命名空间*。

Java 采取了一种新的方法避免了以上这些问题：为一个类库生成一个明确的名称，Java 创建者希望我们反向使用自己的网络域名，因为域名通常是唯一的。因此我的域名是 MindviewInc.com，所以我将我的 foibles 类库命名为 com.mindviewinc.utility.foibles。反转域名后，`.` 用来代表子目录的划分。

在 Java 1.0 和 Java 1.1 中，域扩展名 com、 edu、 org 和 net 等按惯例大写，因此类库中会出现这样类似的名称：Com.mindviewinc.utility.foibles。然而，在 Java 2 的开发过程中，他们发现这会导致问题，所以现在整个包名都是小写的。此机制意味着所有文件都自动存在于自己的命名空间中，文件中的每个类都具有唯一标识符。这样，Java 语言可以防止名称冲突。

使用反向 URL 是一种新的命名空间方法，在此之前尚未有其他语言这么做过。Java 中有许多这些“创造性”地解决问题的方法。正如你想象，如果我们未经测试就添加一个功能并用于生产，那么在将来发现该功能的问题再想纠正，通常为时已晚（有些错误太严重了就得从语言中删除新功能。）

使用反向 URL 将命名空间与文件路径相关联不会导致BUG，但它却给源代码管理带来麻烦。例如在 `com.mindviewinc.utility.foibles` 这样的目录结构中，我们创建了 `com` 和 `mindviewinc` 空目录。它们存在的唯一目的就是用来表示这个反向的 URL。

这种方式似乎为我们在编写 Java 程序中的某个问题打开了大门。空目录填充了深层次结构，它们不仅用于表示反向 URL，还用于捕获其他信息。这些长路径基本上用于存储有关目录中的内容的数据。如果你希望以最初设计的方式使用目录，这种方法可以从“令人沮丧”到“令人抓狂”，对于生产级的 Java 代码，你必须使用专门为此设计的 IDE 来管理代码。例如 NetBeans，Eclipse 或 IntelliJ IDEA。实际上，这些 IDE 都为我们管理和创建深层次空目录结构。

对于这本书中的例子，我不想让深层次结构给你的学习带来额外的麻烦，这实际上需要你在开始之前学习熟悉一种重量级的 IDE。所以，我们的每个章节的示例都位于一个浅的子目录中，以章节标题为名。这导致我偶尔会与遵循深层次方法的工具发生冲突。

<!-- Using Other Components -->
### 使用其他组件

无论何时在程序中使用预先定义好的类，编译器都必须找到该类。最简单的情况下，该类存在于被调用的源代码文件中。此时我们使用该类 —— 即使该类在文件的后面才会被定义（Java 消除了所谓的“前向引用”问题）。而如果一个类位于其他文件中，又会怎样呢？你可能认为编译器应该足够智能去找到它，但这样是有问题的。想象一下，假如你要使用某个类，但目录中存在多个同名的类（可能用途不同）。或者更糟糕的是，假设你正在编写程序，在构建过程中，你想将某个新类添加到类库中，但却与已有的类名称冲突。

要解决此问题，你必须通过使用 **import** 关键字来告诉 Java 编译器具体要使用的类。**import** 指示编译器导入一个包，也就是一个类库（在其他语言中，一个库不仅包含类，还可能包括函数和数据，但请记住 Java 中的所有代码都必须写在类里）。大多数时候，我们都在使用 Java 标准库中的组件。有了这些构件，你就不必写一长串的反转域名。例如：

```java
import java.util.ArrayList;
```

上例可以告诉编译器使用位于标准库 **util** 下的 ArrayList 类。但是，**util** 中包含许多类，我们可以使用通配符 `*` 来导入其中部分类，而无需显式得逐一声明这些类。代码示例：

```java
import java.util.*;
```

本书中的示例很小，为简单起见，我们通常会使用 `.*` 形式略过导入。然而，许多教程书籍都会要求程序员逐一导入每个类。 

<!-- The static Keyword -->
### static关键字

类是对象的外观及行为方式的描述。通常只有在使用 `new` 创建那个类的对象后，数据存储空间才被分配，对象的方法才能供外界调用。这种方式在两种情况下是不足的。

1. 有时你只想为特定字段（注：也称为属性、域）分配一个共享存储空间，而不去考虑究竟要创建多少对象，甚至根本就不创建对象。

2. 创建一个与此类的任何对象无关的方法。也就是说，即使没有创建对象，也能调用该方法。

**static** 关键字（从 C++ 采用）就符合上述两点要求。当我们说某个事物是静态时，就意味着该字段或方法不依赖于任何特定的对象实例 。 即使我们从未创建过该类的对象，也可以调用其静态方法或访问其静态字段。相反，对于普通的非静态字段和方法，我们必须要先创建一个对象并使用该对象来访问字段或方法，因为非静态字段和方法必须与特定对象关联 [^6] 。

一些面向对象的语言使用类数据（class data）和类方法（class method），表示静态数据和方法只是作为类，而不是类的某个特定对象而存在的。有时 Java 文献也使用这些术语。

我们可以在类的字段或方法前添加 `static` 关键字来表示这是一个静态字段或静态方法。 代码示例：

```java
class StaticTest {
    static int i = 47;
}
```

现在，即使你创建了两个 `StaticTest` 对象，但是静态变量 `i` 仍只占一份存储空间。两个对象都会共享相同的变量 `i`。 代码示例：

```java
StaticTest st1 = new StaticTest();
StaticTest st2 = new StaticTest();
```

`st1.i` 和 `st2.i` 指向同一块存储空间，因此它们的值都是 47。引用静态变量有两种方法。在前面的示例中，我们通过一个对象来定位它，例如 `st2.i`。我们也可以通过类名直接引用它，这种方式对于非静态成员则不可行：

```java
StaticTest.i++;
```

`++` 运算符将会使变量结果 + 1。此时 `st1.i` 和 `st2.i` 的值都变成了 48。

使用类名直接引用静态变量是首选方法，因为它强调了变量的静态属性。类似的逻辑也适用于静态方法。我们可以通过对象引用静态方法，就像使用任何方法一样，也可以通过特殊的语法方式 `Classname.method()` 来直接调用静态字段或方法 [^7]。 代码示例：

```java
class Incrementable {
    static void increment() { 
      StaticTest.i++; 
    }
}
```

上例中，`Incrementable` 的 `increment()` 方法通过 `++` 运算符将静态数据 `i` 加 1。我们依然可以先实例化对象再调用该方法。 代码示例：

```java
Incrementable sf = new Incrementable();
sf.increment();
```

当然了，首选的方法是直接通过类来调用它。代码示例：

```java
Incrementable.increment()；
```

相比非静态的对象，`static` 属性改变了数据创建的方式。同样，当 `static` 关键字修饰方法时，它允许我们无需创建对象就可以直接通过类的引用来调用该方法。正如我们所知，`static` 关键字的这些特性对于应用程序入口点的 `main()` 方法尤为重要。

<!-- Your First Java Program -->
## 小试牛刀

最后，我们开始编写第一个完整的程序。我们使用 Java 标准库中的 **Date** 类来展示一个字符串和日期。

```java

// objects/HelloDate.java
import java.util.*;

public class HelloDate {
    public static void main(String[] args) {
        System.out.println("Hello, it's: ");
        System.out.println(new Date());
    }
}

```

在本书中，所有代码示例的第一行都是注释行，其中包含文件的路径信息（比如本章的目录名是 **objects**），后跟文件名。我的工具可以根据这些信息自动提取和测试书籍的代码，你也可以通过参考第一行注释轻松地在 Github 库中找到对应的代码示例。

如果你想在代码中使用一些额外的类库，那么就必须在程序文件的开始处使用 **import** 关键字来导入它们。之所以说是额外的，因为有一些类库已经默认自动导入到每个文件里了。例如：`java.lang` 包。

现在打开你的浏览器在 [Oracle](https://www.oracle.com/) 上查看文档。如果你还没有从 [Oracle](https://www.oracle.com/) 网站上下载 JDK 文档，那现在就去 [^8] 。查看包列表，你会看到 Java 附带的所有不同的类库。

选择 `java.lang`，你会看到该库中所有类的列表。由于 `java.lang` 隐式包含在每个 Java 代码文件中，因此这些类是自动可用的。`java.lang` 类库中没有 **Date** 类，所以我们必须导入其他的类库(即 Date 所在的类库)。如果你不清楚某个类所在的类库或者想查看类库中所有的类，那么可以在 Java 文档中选择 “Tree” 查看。

现在，我们可以找到 Java 附带的每个类。使用浏览器的“查找”功能查找 **Date**，搜索结果中将会列出 **java.util.Date**，我们就知道了 **Date** 在 **util** 库中，所以必须导入 **java.util.\*** 才能使用 **Date**。

如果你在文档中选择 **java.lang**，然后选择 **System**，你会看到 **System** 类中有几个字段，如果你选择了 **out**，你会发现它是一个静态的 **PrintStream** 对象。 所以，即使我们不使用 **new** 创建， **out** 对象就已经存在并可以使用。 **out** 对象可以执行的操作取决于它的类型： **PrintStream** ，其在文档中是一个超链接，如果单击该链接，我们将可以看到 **PrintStream** 对应的方法列表（更多详情，将在本书后面介绍）。 现在我们重点说的是 **println()** 这个方法。 它的作用是 “将信息输出到控制台，并以换行符结束”。既然如此，我们可以这样编码来输出信息到控制台。 代码示例：

```java
System.out.println("A String of things");
```

每个 java 源文件中允许有多个类。同时，源文件的名称必须要和其中一个类名相同，否则编译器将会报错。每个独立的程序应该包含一个 `main()` 方法作为程序运行的入口。其方法签名和返回类型如下。代码示例：

```java
public static void main(String[] args) {
}
```

关键字 **public** 表示方法可以被外界访问到。（ 更多详情将在 **隐藏实现** 章节讲到）
**main()** 方法的参数是一个 字符串（**String**） 数组。 参数 **args** 并没有在当前的程序中使用到，但是 Java 编译器强制要求必须要有， 这是因为它们被用于接收从命令行输入的参数。

下面我们来看一段有趣的代码：

```java
System.out.println(new Date());
```

上面的示例中，我们创建了一个日期（**Date**）类型的对象并将其转化为字符串类型，输出到控制台中。 一旦这一行语句执行完毕，我们就不再需要该日期对象了。这时，Java 垃圾回收器就可以将其占用的内存回收，我们无需去主动清除它们。

查看 JDK 文档时，我们可以看到在 **System** 类下还有很多其他有用的方法（ Java 的牛逼之处还在于，它拥有一个庞大的标准库资源）。代码示例：

```java
// objects/ShowProperties.java
public class ShowProperties {
    public static void main(String[] args) {
        System.getProperties().list(System.out);
        System.out.println(System.getProperty("user.name"));
        System.out.println(System.getProperty("java.library.path"));
    }
}
```

输出结果(前20行):

```text
java.runtime.name=Java(TM) SE Runtime Environment
sun.boot.library.path=C:\Program
Files\Java\jdk1.8.0_112\jr...
java.vm.version=25.112-b15
java.vm.vendor=Oracle Corporation
java.vendor.url=http://java.oracle.com/
path.separator=;
java.vm.name=Java HotSpot(TM) 64-Bit Server VM
file.encoding.pkg=sun.io
user.script=
user.country=US
sun.java.launcher=SUN_STANDARD
sun.os.patch.level=
java.vm.specification.name=Java Virtual Machine
Specification
user.dir=C:\Users\Bruce\Documents\GitHub\on-ja...
java.runtime.version=1.8.0_112-b15
java.awt.graphicsenv=sun.awt.Win32GraphicsEnvironment
java.endorsed.dirs=C:\Program
Files\Java\jdk1.8.0_112\jr...
os.arch=amd64
java.io.tmpdir=C:\Users\Bruce\AppData\Local\Temp\
```

`main()` 方法中的第一行会输出所有的系统字段，也就是环境信息。 **list()** 方法将结果发送给它的参数 **System.out**。在本书的后面，我们还会接触到将结果输出到其他地方，例如文件中。另外，我们还可以请求特定的字段。该例中我们使用到了 **user.name** 和 **java.library.path**。 

<!-- Compiling and Running -->
### 编译和运行

要编译和运行本书中的代码示例，首先必须具有 Java 编程环境。 第二章的示例中描述了安装过程。如果你遵循这些说明，那么你将会在不受 Oracle 的限制的条件下用到 Java 开发工具包（JDK）。如果你使用其他开发系统，请查看该系统的文档以确定如何编译和运行程序。 第二章还介绍了如何安装本书的示例。 

移动到子目录 **objects** 下并键入：

``` bash
javac HelloDate.java
```

此命令不应产生任何响应。如果我们收到任何类型的错误消息，则表示未正确安装 JDK，那就得检查这些问题。

若执行不报错的话，此时可以键入：

```java
java HelloDate
```
我们将会得到正确的日期输出。这是我们编译和运行本书中每个程序（包含 `main()` 方法）的过程 [^9]。此外，本书的源代码在根目录中也有一个名为 **build.gradle** 的文件，其中包含用于自动构建，测试和运行本书文件的 **Gradle** 配置。当你第一次运行 `gradlew` 命令时，**Gradle** 将自动安装（前提是已安装Java）。

<!-- Coding Style -->
## 编码风格

Java 编程语言编码规范（Code Conventions for the Java Programming Language）[^10] 要求类名的首字母大写。 如果类名是由多个单词构成的，则每个单词的首字母都应大写（不采用下划线来分隔）例如：

```java
class AllTheColorsOfTheRainbow {
    // ...
}
```

有时称这种命名风格叫“驼峰命名法”。对于几乎所有其他方法，字段（成员变量）和对象引用名都采用驼峰命名的方式，但是它们的首字母不需要大写。代码示例：

```java
class AllTheColorsOfTheRainbow {
    int anIntegerRepresentingColors;
    void changeTheHueOfTheColor(int newHue) {
        // ...
    }
    // ...
}

```

在 Oracle 的官方类库中，花括号的位置同样遵循和本书中上述示例相同的规范。

## 本章小结

本章向你展示了简单的 Java 程序编写以及该语言相关的基本概念。到目前为止，我们的示例都只是些简单的顺序执行。在接下来的两章里，我们将会接触到 Java 的一些基本操作符，以及如何去控制程序执行的流程。

[^1]: 这里可能有争议。有人说这是一个指针，但这假定了一个潜在的实现。此外，Java 引用的语法更类似于 C++ 引用而非指针。在 《Thinking in Java》 的第 1 版中，我发明了一个新术语叫“句柄”（handle），因为 C++ 引用和Java 引用有一些重要的区别。作为一个从 C++ 的过来人，我不想混淆 Java 可能的最大受众 —— C++ 程序员。在《Thinking in Java》的第 2 版中，我认为“引用”（reference）是更常用的术语，从 C++ 转过来的人除了引用的术语之外，还有很多东西需要处理，所以他们不妨双脚都跳进去。但是，也有些人甚至不同意“引用”。在某书中我读到一个观点：Java 支持引用传递的说法是完全错误的，因为 Java 对象标识符（根据该作者）实际上是“对象引用”（object references），并且一切都是值传递。所以你不是通过引用传递，而是“通过值传递对象引用”。人们可以质疑我的这种解释的准确性，但我认为我的方法简化了对概念的理解而又没对语言造成伤害（嗯，语言专家可能会说我骗你，但我会说我只是对此进行了适当的抽象。）

[^2]: 大多数微处理器芯片都有额外的高速缓冲存储器，但这是按照传统存储器而不是寄存器。

[^3]: 一个例子是字符串常量池。所有文字字符串和字符串值常量表达式都会自动放入特殊的静态存储中。

[^4]: 静态方法，我们很快就能接触到，它可以在没有对象的情况下直接被类调用。

[^5]: 通常除了前面提到的“特殊”数据类型 boolean、 char、 byte、 short、 int、 long、 float 和 double。通常来说，传递对象就意味者传递对象的引用。

[^6]: 静态方法在使用之前不需要创建对象，因此它们不能直接调用非静态的成员或方法（因为非静态成员和方法必须要先实例化为对象才可以被使用）。

[^7]: 在某些情况下，它还为编译器提供了更好的优化可能。

[^8]: 请注意，此文档未包含在 JDK 中;你必须单独下载才能获得它。

[^9]: 对于本书中编译和运行命令行的每个程序，你可能还需要设置 CLASSPATH 。

[^10]: 为了保持本书的代码排版紧凑，我并没完全遵守规范，但我尽量会做到符合 Java 标准。

<!-- 分页 -->
<div style="page-break-after: always;"></div>
