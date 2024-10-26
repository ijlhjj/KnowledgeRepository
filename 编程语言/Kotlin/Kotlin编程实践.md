# Kotlin编程实践

## 第 1 章 安装并运行 Kotlin

1. Kotlin 目前是 Android 开发的首选语言，但它是一种更广泛的通用语言。  
    你可以在任何原先使用Java的地方以及其他更多场景中使用它。
2. 被编译后的文件采用首字母大写的类名，然后在末尾加上Kt作为文件名。  
    这些可以通过注解来控制。
3. 使用 Kotlin 编写的脚本文件扩展名为 .kts  
    脚本包含通常会显式在类的标准 main 方法中的代码。
4. Kotlin 可以作为 JVM 上的脚本语言来使用。

## 第 2 章 Kotlin 基础

1. Kotlin 最有魅力的特征可能就是它消除了几乎所有可能的空值。  
    在 Kotlin 中如果你定义的变量不在尾部添加问号，则编译器将要求该值不为空。  
    如果你想让一个变量可空，可以在类型声明的后面加一个问号。  
    Kotlin 最主要的特征之一就是使类型系统在编译时强制实现可空性。  
    如果你声明一个类型为 String 的变量，则它永不为空，反之，如果它的类型被声明为 String? ，则可以为空。
2. val 声明的变量不可改变，var 声明的变量可变。
3. 操作符：

    ```text
        !!          非空断言        可能抛出 NullPointerException 异常
        ?.          安全调用        值为空时返回 null
        ?:          Elvis          左值不为空返回左值，左值为空返回右值
        as?         安全转换        转换失败返回 null，避免发生 ClassCastException 异常
        ===         恒等操作符      两个实例是同一个对象
    ```

4. Kotlin 在多种情况下会尝试进行智能转换。
5. 许多库都参考了 JSR-305 的兼容性注解。  
    JSR-305 中定义的 @NonNull 注解采用名为 when 的属性。
6. 可选或可空属性应放在函数签名的末尾，以便在使用位置参数调用函数时可以将其忽略。  
7. 如果将注解 @JvmOverloads 添加到函数中，生成的类将支持所有的函数重载。  
    构造器重载必须显式声明 constructor ，这样才能向它添加 @JvmOverloads 注解。  
    调用被 @JvmOverloads 标记的构造函数时，不会调用具有相同数量参数的 super 。相反，他们使用提供的默认值调用完整的构造函数。
8. Kotlin 中没有原始类型，所有的数值都是类对象的实例。
9. Kotlin 支持：  
    运算符重载  
    解构操作  
    形参默认值
10. actual 关键字是指在多平台项目中的平台指定实现。
11. 虽然 Kotlin 中只能重载预定义的操作符（运算符），但是可以通过将其括在反引号中来伪造一个，作为中辍运算符，这样可能会更好。
12. 使用 Hamcrest 匹配器 closeTo 来避免对 double 进行相等性比较。
13. Kotlin 使用函数进行移位运算：shl 有符号左移， shr 有符号右移， ushr 无符号右移。  
    ushr 在求两个大整数的平均值时可以部分避免数值溢出问题。
14. 位运算符：

    ```text
        and         按位与
        or          按位或
        xor         按位异或
        inv         按位取反
    ```

15. 位运算与它们对应的逻辑运算之间的唯一区别是位运算不会短路。
16. Pair 表示拥有两个元素的数据类。Triple 表示拥有三个元素的数据类。
17. to 函数是任意泛型 A 上的扩展函数，并接受任意泛型 B 的值作为参数，并返回一个结合 A 与 B 值的 Pair 实例。

## 第 3 章 Kotlin 中的面向对象编程

1. Kotlin 是面向对象编程语言。
2. 可使用 const 定义常量。  
    const 必须与 val 一起使用。
3. 默认情况下，Kotlin 所有一切都是公有的。
4. 在构造函数中声明的属性必须包含类型，即使它们具有默认值。
5. 如果一个属性使用默认生成的 getter 或 setter ，又或者如果一个自定义 getter 或 setter 通过 field 属性引用了它，则该属性会生成一个幕后字段。  
    field 标识符被用于引用生成的幕后字段，仅能被用于自定义的 getter 与 setter 中。
6. Kotlin 使用 data 定义数据类。  
    将关键字 data 添加到类定义中会使编译器生成一系列函数，包括一致性的 equals 与 hashCode 函数，用于显式类与属性值的 toString 函数、 copy 函数，以及用于解构的 component 函数。  
    在数据类上调用 copy 将进行浅拷贝而不是深拷贝。
7. 使用扩展函数可以对已有的类添加新的函数方法。
8. lateinit 与 lazy：  
    lazy 只能用于 val 属性，lateinit 只能用于 var 属性。  
    lazy 委托使用 lambda 表达式，在首次访问这个属性时执行；lateinit 属性可以在任何该属性可见的地方初始化。  
    如果初始化非常昂贵或该属性也许永远不会被使用时，推荐使用 lazy  
    lateinit 修饰的属性不能拥有自定义的 getter 和 setter ，被修饰的类型必须是非空的且不能是原始类型。  
    lateinit 在依赖注入的情况下很有用，应该在首次使用前初始化，否则会引发异常。
9. 在 Kotlin 中，== 操作符会自动调用 equals 函数。
10. equals 的约定要求实现是自反的、对称的以及可传递的，并且具有一致性，并适当地处理空值。  
    hashCode 的约定要求如果两个对象使用 equals 函数判定是等价的，则它们也应该具有相同的 hashCode 。  
    如果 equals 函数被覆盖，则 hashCode 函数也应该被覆盖。
11. 在 Kotlin 中，使用 object 关键字代替 class 来实现单例模式。
12. Nothing 类没有任何实例，你可以用 Nothing 来代表一个不存在的值。  
    当你将变量赋值为 null 而不给它显式声明类型时，类型被推断为 Nothing  
    抛出异常的返回类型为 Nothing  
    在 Kotlin 中，Nothing 类实际上是所有其他类型的子类型。

## 第 4 章 函数式编程

1. fold 函数是一种规约操作，可以应用于数组或可迭代对象。
2. reduce 函数与 fold 函数几乎完全相同，并且其用途也相同。  
    它们的最大区别是 reduce 函数没有为累加器提供初始值的参数。因此，累加器将使用集合中的第一个值进行初始化。  
    仅当可以使用集合的第一个值初始化累加器且对其他值不执行任何其他处理时，才应该使用 reduce
3. tailrec 关键字告诉编译器优化递归调用。同样的算法使用 Java 表示仍然是递归调用，并且会遇到同样的内存限制。
4. 要使函数符合 tailrec 修饰符的使用规则，要求如下：  
    该函数必须将调用它自己作为最后一步操作；  
    不能在 try/catch/finally 块中使用 tailrec  
    尾递归仅在 JVM 后端支持（新版本已支持其他平台）。

## 第 5 章 集合

1. Kotlin 提供了一个名为 arrayOf 的简单工厂方法来创建数组，尽管它使用相同的语法访问元素，但在 Kotlin 中 Array 却是一个类。
2. Array 类中仅有一个公共构造函数，它接收两个参数：

    ```text
        * Int 类型的 size
        * 一个类型为 (Int)->T 的 lambda 表达式 init
    ```

    在创建数组时，会在每个索引上调用该 lambda 表达式。
3. 虽然 Kotlin 没有显式地指明原生类型，但当值可为空时，生成的字节码也会使用 Java 包装类，如果不为空，则使用原生类型。
4. 使用 listOf 、 setOf 、 mapOf 可以产生不可变集合；使用 mutableListOf 、 mutableSetOf 、 mutableMapOf 可以产生可变集合。
5. 使用 toList 、 toSet 、 toMap 方法可以创建只读集合，将可变集合赋予 List 、 Set 、 Map 类型的变量可以创建只读视图。
6. associate 和 associateWith 函数可以将 Set 转化为 Map
7. 当集合为空或字符串为空时，使用 ifEmpty 和 ifBlank 函数可以返回默认值。
8. 使用 coerceIn 函数可以将变量限制在给定区间。小于最小值返回最小值，大于最大值返回最大值。
9. 如果要将集合切分为相同的部分，则使用 chunked 函数；如果希望将块沿集合滑动给定间隔的块，则使用 windowed 函数。  
    chunked 函数与 windowed 函数对于分阶段处理时序数据很有用。
10. 解构是通过将对象的值分配给变量的集合来从中提取值的过程。
11. sortedWith 函数和 compareBy 函数组合可以实现多字段排序。  
    sortBy 和 sortWith 函数对元素在集合内进行排序，因此需要可变的集合。
12. 在 Java 中， for-each 循环可以迭代实现 Iterable 的任何类。在 Kotlin 中， for-in 循环可以运行相似的约束。
13. filterIsInstance 函数使用了实化类型，因此生成的集合属于已知类型，并且你无须在使用其属性之前检查类型。
14. 在 Kotlin 中使用双点操作符可以创建一个区间，例如 1..5 ，这将创建一个 IntRange 。 Kotlin 中的区间都是闭区间，即包含两个端点。
15. 可以基于实现 Comparable 的任何类来创建区间，并且已经有支持该区间的基础结构。  
    使用双点操作符创建一个区间，该区间支持使用 forEachIndexed 函数进行迭代。

## 第 6 章 序列

1. 序列是惰性的：当使用序列来处理数据时，每个元素都会在处理下一个元素之前完成整个流水线处理流程。
2. 与 Java 流不同，Kotlin 序列可以被遍历多次。
3. 序列与 Java 中的流类似，都拥有过渡操作或末端操作。  
    过渡操作返回一个新的序列，末端操作返回任意其他值。  
    当你在序列上通过函数调用创建了一个流水线时，在进行末端操作之前不会有数据通过序列。
4. yield 函数的工作是为迭代器提供一个值，并挂起直到请求下一个值。  
    由于 yield 是一个挂起函数，意味着它可以很好地与协程配合使用。  
    Kotlin 运行时可以提供一个值，然后将当前协程保留，直到请求下一个值。  
    通过 yield 与 yieldAll 内部挂起的组合，可以轻松地将序列自定义为任何所需的一组生成值。

## 第 7 章 作用域函数

1. aplly 函数是任意泛型 T 的扩展函数，该函数使用它的接收者调用给定的代码块，并在执行完毕后将其返回。  
    它最常用于对已实例化的对象进行额外的配置。
2. also 函数是任意泛型 T 的扩展函数，该类型的接收者在执行给定的代码块后将其返回。  
    它最常用于将函数调用链接到对象上。  
    在代码块内部，使用 it 引用该对象。
3. let 函数在执行给定的代码块后返回代码块执行结果，而不是上下文对象本身。

## 第 8 章 Kotlin 委托

1. 使用委托实现组合：创建包含委托方法的接口，并在类中实现这些接口，然后使用关键字 by 来用它们构建包装器类。
2. 标准库中的 lazy 函数是顶层函数，其余大多数委托函数是 Delegates 实例的一部分。
3. 单例类、工厂方法以及私有实现类的这种组合在 Kotlin 中是常见的习惯用法。如果你提供自己的自定义委托，请考虑遵循此模式。
4. inline 关键字将告知编译器避免仅仅为了调用这个函数而创建一个完整的新对象，而是将实际的源代码替换到调用处。
5. Kotlin 映射实现了成为委托所必需的 getValue 与 setValue 函数。所以可以使用映射实现类的属性委托。
6. 可以通过实现 ReadOnlyProperty 或 ReadWriteProperty 接口来实现自己的属性委托。

## 第 9 章 测试

1. Kotlin 没有 static 关键字，需要使用伴生对象实现类似 Java 的一些静态行为。
2. JUnit 5 允许使用 @TestInstance 指定测试类的生命周期，默认值为 PER_METHOD

## 第 10 章 输入 / 输出

1. Kotlin 不支持 try-with-resources 语法，Kotlin 将扩展函数 use 添加到了 Closeable 上，并将 useLines 添加到了 Reader 与 File 上。
2. use 块是 Execute Around Method 设计模式的一个示例，该基础结构代码已内置到库中，一个提供的 lambda 表达式用于完成实际工作。
3. writeText 与 appendText 函数将实现委托给了 writeBytes 与 appendBytes，它们都利用 use 函数来确保写入完成后关闭文件。

## 第 11 章 其他

1. 可以使用 KotlinVersion.CURRENT 获取当前使用的 kotlin 版本号。
2. 可以使用标准库中的 repeat 函数重复执行一个 lambda 表达式（ (Int) -> Unit 类型的函数 ）。
3. kotlin 的 when 语句类似于 Java 的 switch 语句，但它不需要在每个子句中使用 break
4. String 类的 replace 函数要实现正则表达式替换效果需要先使用 toRegex 函数进行转换。
5. Int 类可以使用扩展函数 toString 转换为二进制字符串。String 类的 toInt 函数执行相反操作。
6. 通过提供 invoke 操作符函数，可以通过在引用后添加括号来直接执行实例。
7. kotlin.system 包的 measureTimeMillis 和 measureNanoTime 函数可以用来测量代码的执行时间。
8. kotlin.concurrent.thread 函数可以创建并启动线程。  
    使用 isDaemon 参数可以创建守护线程；  
    调用 join 方法可以使所有线程按顺序执行。
9. 使用 TODO 函数可以标记当前未实现的功能。
10. 给定相同的因子，Random.nextInt 将获得相同的随机数序列。
11. Kotlin 中的所有异常均被视为非受检异常，这意味着编译器不会要求你处理它们。  
    可以使用 @Throws 注解解决与 Java 的集成问题。

## 第 12 章 Spring 框架

1. 默认情况下，Kotlin 是静态绑定的，这意味着你无法覆盖方法，甚至不能继承类，除非已使用 open 关键字将其标记为可继承。  
    为了解决这样的问题，Kotlin 有一个被称为全开放式插件的插件。该插件使带有指定注解的类可继承，而无需在类及其包含的函数中显式添加 open 关键字。
2. no-arg 编译器插件向 Kotlin 类添加了一个合成的默认构造函数，这意味着它不能在 Java 或 Kotlin 中调用，但是 Spring 可以使用反射来调用它。
3. 如果在 Spring 管理的 bean 中只有一个构造函数，Spring 将自动注入所有参数。
4. 可以在 JUnit 5 测试中使用非默认构造函数。

## 第 13 章 协程与结构化并发

1. runBlocking 是一个顶层函数，而 launch 与 async 是 CoroutineScope 类型上的扩展函数。  
    如果你需要启动协程以执行单独的任务，但又不需要从中取得返回值，可以使用 launch 构建器。  
    在需要返回值的常见情况下，使用 async 构建器。  
    CoroutineContext 用于与其他协程共享状态。  
    coroutineScope 函数是一个挂起函数，它会等待所有包含的协程执行完毕后才退出。它的优点是不阻塞主线程（与 runBlocking 不同），但是必须将其作为挂起函数的一部分来调用。  
    coroutineScope 的好处在于，不必轮询即可查看协程是否执行完毕，它会自动等待所有子协程执行完毕。  
    在 coroutineScope 内运行所有协程以确保如果一个失败，所有协程将被取消的惯例称为结构化并发。
2. 在实践中，从 coroutineScope 开始以建立所包含的协程的作用域，在代码块内部，你可以使用 launch 或 async 来处理各个任务。然后，该作用域将等待所有协程执行完毕，然后退出，并且如果任何协程失败，还将取消其他协程。这样就可以在控制和错误处理之间实现很好的平衡，而不必轮询以查看协程是否已经执行完毕，并且可以防止协程失败时发生内存泄漏。
3. 在实践中，可以使用 withContext 替换在 async 调用后立即调用 await 的代码。
4. Dispatchers.Default 调度器使用一个普通的可共享的后台线程池。它适合消耗大量计算资源的协程。  
    Dispatchers.IO 调度器使用按需创建的共享线程池，用于运行 I/O 密集型的阻塞任务，例如文件 I/O 或阻塞式的网络 I/O  
    Android 还包含一个 Dispatchers.Main 的调度器。
5. Android 通常建议在 Android KTX 库的 viewModelScope 函数上启动协程。
6. 使用 use 函数，在代码块结束时，将关闭调度器，同时还将关闭基础线程池。
7. 使用 withTimeout 函数在执行超时时抛出 TimeoutCancellationException  
    使用 withTimeoutOrNull 在超时时返回 null
