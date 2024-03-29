# 算法精粹-经典计算机科学问题的Python实现

## 第 1 章  几个小问题

1. 类型提示对在Python解释器中实际运行代码没有丝毫影响。
2. Python自带了一个内置的装饰器（decorator），可以自动为任何函数缓存结果。  
    每次用新的参数执行fib4()时，该装饰器就会把返回值缓存起来。以后再用相同的参数调用fib4()时，都会从缓存中读取该参数对应的fib4()之前的返回值并返回。  
    @lru_cache的maxsize属性表示对所装饰的函数最多应该缓存多少次最近的调用结果，如果将其设置为None就表示没有限制。
3. 有些人可能会觉得这是为了简洁而牺牲了可读性，还有些人可能会发现简洁本身就更具可读性。
4. 使用元组解包来实现某种变量交换的做法在Python中十分常见。
5. 用函数sys.getsizeof()可以查出Python对象占用的内存字节数。
6. Python只有一种int类型，可以存储任意精度的数值。  
    由于Python对象系统的固有开销，在Python 3.7中无法创建少于28字节（224位）的int类型。  
    每个int类型对象每次可以扩大1个二进制位，但最少也要占用28字节。
7. Python标准库中不包含可处理任意长度位串的现成结构体。此处把位串存储在int类型中。  
    因为Python中的int类型可以是任意长度，所以它可以当成任意长度的位串来使用。  
    为了将位串类型转换回str类型，就需要实现Python的特殊方法 `__str__()`  
    int类型的bit_length()方法给开发带来了很大便利。
8. Python没有真正的私有方法或变量的概念。  
    所有变量和方法都可以通过反射访问到，Python对它们没有严格的强制私有策略。  
    前导下划线只是一种约定，表示类的外部不应依赖其方法的实现。这一类方法可能会发生变化，应该被视为私有方法。  
    如果类的方法或实例变量名用两个下划线开头，Python将会对其进行名称混淆（name mangle），通过加入盐值（salt）来改变其在实现时的名称，使其不易被其他类发现。  
    本书用一条下划线表示“私有”变量或方法，但如果真要强调一些私有内容，或许得用两条下划线才合适。
9. 当左移操作完成后，位串的右侧会加入两个0。在位运算过程中，0与其他任何值执行“或”操作的结果都是把0替换为该值。
10. 因为Python没有switch语句，所以大量采用了if语句。  
    在Python中有时还会出现一种情况，就是高度依靠字典对象来代替大量的if语句，以便对一系列的条件做出处理。  
    有时字典方案的可读性会更好，但可能会带来一定的性能开销。  
    尽管查找字典在技术上的复杂度为 O(1)，但运行哈希函数存在开销，这有时会意味着字典的性能还不如一串 if  
    是否采用字典，取决于具体的if语句做判断时需要进行什么计算。  
    如果在关键代码段中要在多个if和查找字典中做出取舍，或许该分别对这两种方法运行一次性能测试。
11. Python 3的str类型有一种用法可被视为UTF-8字节序列。  
    通过encode()方法可将str转换为UTF-8字节序列（以bytes类型表示）。  
    用bytes类型的decode()方法可将UTF-8字节序列转换回str
12. 能否生成真正随机的数据？大多数计算机的答案都是否定的。  
    Python 3.6 secrets包的伪随机数据生成函数token_ bytes()，在幕后采用的仍然是伪随机数生成器，生成的数据并非是真正随机的。
13. 与对序列中的多字节进行操作相比，对单个int（读作“长位串”）进行位操作将更加简单高效。
14. 在大多数平台中，Python的float类型是64位的浮点数（或C语言中的double类型）。

## 第 2 章  搜索问题

1. 只要组成元组的元素类型是可比较的，Python就内置支持元组的比较操作。
2. in运算符可以与任何已实现__contains__()方法的类型一起使用。  
    Python内置的序列类型（list、tuple、range）都已实现了__contains__()方法，这样就能简单地用in操作符在其中搜索某个指定的数据项。
3. Optional类型表示，参数化类型的值可以被变量引用，或变量可以引用 None
4. Python的list数据结构从右侧弹出的效率较高，但从左侧弹出则不然。  
    在用list的时候，从左侧弹出数据后，每个后续元素都必须向左移动一个位置（等同于Java的ArrayList）。
    deque则从两侧都能够高效地弹出数据。  
    因此，在deque上有一个名为popleft()的内置方法，但在list上则没有与其等效的方法。  
    当然可以找到其他方法来用list作为队列的底层存储结构，但效率会比较低。  
    在deque上从左侧弹出数据的操作复杂度为O(1)，而在list上则为O(n)  
5. Python的标准库中包含了heappush()函数和heappop()函数，这些函数将读取一个列表并将其维护为二叉堆。  
    用这些标准库函数构建一个很薄的封装器，即可实现一个优先队列。

## 第 3 章  约束满足问题

1. 很多要用计算工具来解决的问题基本都可归类为约束满足问题（constraint-satisfaction problem，CSP）  
    CSP由一组变量构成，变量可能的取值范围被称为值域（domain）。要求解约束满足问题需要满足变量之间的约束。  
2. 一般建议不要在自己的代码中使用抽象基类，除非确定要构建的框架不仅仅是供内部使用的类架构，而是要作为供其他人构建的基础。
3. 该约束满足框架的核心将是一个名为CSP的类。CSP是变量、值域和约束的汇集点。  
    本约束满足框架将使用简单的回溯搜索来找到问题的解。  
    回溯的思路如下：一旦在搜索中碰到障碍，就会回到碰到障碍之前最后一次做出判断的已知点，然后选择其他的一条路径。
4. 有时会用super()调用超类的方法，但也可以使用类本身的名称来调用，正如Constraint.__init__([place1, place2])  
    在处理多重继承时，这种用法特别有用，因为这样对于要调用哪个超类的方法非常明了。
