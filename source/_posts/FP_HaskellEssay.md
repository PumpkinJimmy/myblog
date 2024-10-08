---
abbrlink: '0'
title: 基于Haskell的函数式入门随笔
---
# 基于Haskell的函数式入门随笔

FP这块的一些关键词：
- *Pure Function*：纯函数，没有副作用的函数，可以精确地映射到lambda算子上
- *Type System*：类型系统，现代函数式语言往往配合强大的类型使用。这样的类型系统往往并不将复合类型/容器类型内置在语言中，而是使用类型系统的复合来严格定义，这跟一般变成语言很不一样
- *Monad*：单子，范畴论的术语，函数式语言中用来支持副作用、
- *First-Class Function*：函数式里函数是一级成员，函数类型和普通类型的对等的抽象概念。这也说明高阶函数（返回函数的函数，输入函数的函数）会经常出现
- *Curry*：柯里化，纯函数有且只有一个入参和一个出参，curry化是指通过返回函数的纯函数来变相实现多入参的纯函数

FP的好处
- 数学对应：无副作用，容易转换为数据证明，所以容易分析和形式化证明
- Lazy：由于是Pure，所以无副作用的数据计算理论上可以延迟到真正使用的时刻
- 容易并行：无副作用意味着求值顺序很多时候是任意的，很适合并行
- 优化潜力：逻辑高度抽象，没有指明硬件细节，理论上足够强大的编译器可以生成跟过程式语言一样的高效字节码，且因为无副作用优化潜力更大

## Haskell语法笔记
- 注释
  - 单行注释：双横杠`--`
- 类型声明：双冒号，比如`myNumber :: Int`
- 赋值：没有赋值，使用`a = b`给表达式b起一个别名a
- 函数定义与调用：基于Lambda算子的书写
  ```Haskell
  Sqare :: Int -> Int
  Sqare x = x*x
  result = Sqare 32  
  ``` 

  注意：
  - 纯函数符号`->`在类型书写中是个右结合的运算符，也即`String -> Int -> int`等效`String -> (Int -> Int)`
  - “调用”函数不需要书写括号，一对没有内容的括号通常表示“空表达式”

- Curry化
    ```Haskell
    times::Int->Int->Int
    times x y = x * y
    result = times 3 5

    -- times x y = x * y是如下原语书写的语法糖
    timesX::Int->Int
    timesX x = y -> x * y
    times = (TimesX x) y
    ```

    思考：其实纯函数的Curry化暗示了常规变成语言中按位置传参的参数顺序不能调换的本质：数学上两个参数其实是嵌套在高阶函数里的如`x->y->x*y`，函数嵌套可是不能调换顺序的

    注意：这个语法糖很容易跟函数复合混淆，必须记清楚，这是个语法糖，它底层的纯函数从来不允许多个函数参数
    
    容易混淆的例子：
    ```
    f :: (Int->Int)->Int->Int
    g :: Int->Int
    result = f g 3
    -- f是一个Curry化书写的双入参函数

    f :: Int -> Int
    g :: Int -> Int
    result = f (g 3)
    --后面的括号是必要的，因为Curry写法是左结合的，这里不是Curry化
    ```

- 一些数据操作原语
  - 字符串拼接：`"StringX" <> "StringY"`
  - 数组拼接：`[1, 2] ++ [3]`

- 自定义数据类型：`data`
    ```Haskell
    -- 定义类型
    data Student = Student String Int
    -- 在类型声明中使用
    myStudent :: Student
    -- 创建该类型的实例
    myStudent = Student "pumpkin" 85

    -- 通过取值集合定义新类型，其实类似枚举；这里的True/False其实应该是构造器，只是构造器不需要任何子类型而已
    data Bool = True | False

    -- 通过多个类型的并联构成类似共用体的新类型
    data JsonValue = JsonString String
        | JsonInt Int
        | JsonFloat Float

    ```

    注
    - `data`左侧是*类型名*，右侧是*类型构造器名*，构造器后面跟的是“成员类型”（或者说是构造器的参数类型）
    - 类型名和构造器名往往是相同的（也可以不同），但它们的性质不一样。前者只出现在`::`右侧，而后者用于构造该类型的表达式
    - 也即类型构造器可以看作是一个`Args -> CustomType`的函数
    - 可以看出，纯函数背景下的“自定义类型”等效于其他类型的复合，只有这么简单我们才能发挥形式化的威力

    工程化：继承接口
    
    ```
    -- 定义类型，从Show接口泛化，可以被标准库的show转为转为字符串
    data JsonValue = JsonString String
        | JsonInt Int
        | JsonFloat Float
        deriving Show
    ```

- `data`语法糖
    ```
    -- 具名字段
    data Student
        = Student
            {
            name::String,
            score::Int
            }

    -- 按名构造
    myStudent = Student { name = "bbpumpkin", score = 85}

    -- 访问成员
    studentName = name(myStudent)
    ```  

- 类型别名`type`，比如`type A = B`，其实就是A是类型B的别名，没有创建新类型
- 条件分支`if then else`基本操作，只是这次整个都是表达式了，没有副作用
  - 注意到这里的分支其实是一种特殊的`->`映射，所以分支的每个块中都必须是一个表达式
  - 突然能理解为什么Rust整出不带分号的表达式当作返回值的操作了
- 模式匹配：`case`
  - 说明：这个case不全是用来分支的，它还是使用自定义类型的重要手段
  - 举例：一般匹配
    ```Haskell
    myFunc :: String -> Int
    myFunc c = 
        case c of 
            "a" -> 1
            "b" -> 2
            "c" -> 3
            --这里的模式写任意非字面量的合法变量名都行，Pattern就当作变量名，c会赋值给把这个Pattern
            _ -> 4
    ``` 
  - 举例：访问自定义类型的成员
    ```Haskell
    data Student = Student String Int
    getStudentName :: Student -> String
    getStudentName stu = 
        case stu of
            Student name score -> name
    
    -- 语法糖：直接将Pattern写在入参的位置
    getStudentName (Student name score) = name
    ```
- 元组类型`Tuple`
  - 包含三个元素的元组可以直接写作：`("pumpkin", 85, True)`
  - 上述写法是`Tuple "pumpkin" 85 True`的语法糖，`Tuple`是内置的一个高阶类型，前述元组的实际类型是`Tuple String Int Bool`
- 链表类型：`[MyType]`
  - `[MyType]`声明一个以`MyType`为元素的链表
    - 之所以是链表，是因为函数式没有循环，实际使用容器都是尾递归展开的
  - 字面量：`[a, b, c]`直接书写，算是语法糖了
  - 使用模式匹配来访问链表：`:`记号书写模式

    举例：
    ```Haskell
    safeHead :: [a] -> Maybe a
    safeHead list =
    case list of
        -- Empty list
        [] -> Nothing

        -- Cons cell pattern, will match any list with at least one element
        x : _ -> Just x
    ```

    说明：就像多数类型论教材那样，`:`是书写序列类型的记号，既可以用于书写模式也可以用于构造序列

- 高阶类型与泛型（待完善）
  - 允许声明高阶类型（二阶类型，从给定一阶类型构造另一个类型），举例：
    ```
    data Maybe a
        = Nothing
        | Just a
    ``` 
    注意，这里的`a`是泛型，`Maybe`类似C++的模板，是一个类型构造器，给定一个具体类型可以构造类似`Maybe Int`这样的一阶类型
    - 这里将`Maybe`看作一个二阶类型意义不大，因为`Maybe`不是这个类型系统的一阶公民了，我们没有一个变量可以是单纯的`Maybe`类型的，我们也没有不允许复合`Maybe`“类型”来构造其他类型
    - 正式地说，这种接受一个泛型来构造一个一阶类型的泛型构造器在Haskell中被称为*类型类*
    - 类型了可以接受声明泛型约束，比如一个可比较的泛型类，并实现它的接口：
      ```Haskell

      -- 定义布尔数据类型
      data Bool = True | False

      -- 使用class定义一个类型类
      class Equtable a where
        isEqual :: a -> a -> Bool

      -- 使用instance为Bool实现比较接口
      instance Equtable Bool where
        isEqual True  True  = True
        isEqual False False = True
        isEqual _     _     = False

      -- 使用类型类声明某个泛型函数的泛型约束
      myFunc :: Equatable a => a -> a -> String

      ```

      注意相关原语：`class, where, instance, =>`

- 处理IO与副作用
  - 注意到，标准输入“函数”`getLine`并不是一个真的函数，它是一个`IO String`类型的“数据”
    - 实际上，Haskell使用内建的`IO`类型类来实现各种IO功能，而不是一般的纯函数
  - 特殊类型类`IO`
    - 内建的类型类，专门用于处理IO相关的副作用
  - 关于“串联”
    - 注意到我们在纯函数中我们没有任何“顺序指令流”，整个Main如果不考虑IO操作的话就是一个表达式
    - 这个表达式中所有`let`里的变量都是助记符号，没有求值顺序，不允许相互依赖
    - 运算顺序的要求来自于函数复合的顺序
    - 所以我们发现，我们在纯函数中“串联”各种操作的媒介其实是`->`，通过串联函数的输入输出来构造指令流（注意if和switch都是`->`的特殊形式而已
    - 所以我们马上发现问题：IO功能实现为了类型类`IO`，怎么串联IO操作（更一般的，有副作用的操作）
  - 记号 `>>=`
    - 定义：`(>>=) :: (IO a) -> (a -> IO b) -> (IO b)`
    - 解释：`>>=`是一个泛型二元运算函数
      - 左边是一个`IO a`类型的对象，右侧是一个函数，输入类型`a`输出类型`IO b`的对象
      - 这个运算符表达式的结果是一个`IO b`类型的对象
      - 这个泛型运算符用于串联IO操作，其中`IO a`通常是一个输入`a`类型数据的操作，而`a -> IO b`则是一个输出类型`a`的数据的操作
    - 用例：读取一个行并立刻输出
      
      ```Haskell
      getLine :: IO string
      putStrLn :: String -> IO ()

      -- echo串联了两个IO操作
      echo :: IO ()
      echo = getLine >>= \line -> putStrLn line
      ```
    - 形象的比喻：`>>=`右边其实类似于处理输入Input结果的回调函数

  - `*>`记号和`>>`记号
    - `(*>) :: IO a -> IO b -> IO b`
    - 解释：`*>`也可以用于在不关心运算符左侧的IO操作的返回值的时候，串联两个IO操作，比如`putStrLn "Hello" *> putStrLn "World"`
    - 一个直觉上的等效：`a *> b = a >>= \_ -> b`
    - `>>`运算符跟`*>`几乎一致，保留的愿意只是向后兼容
  - `pure`记号
    - `pure :: a -> IO a`
    - `pure`其实类似于void函数中的return，它专用于某些依赖于IO结果的函数的实现中。它可以快速使用一般类型`a`的值构造`IO a`用于函数的结果中
  - `fmap`记号
    - `fmap :: (a -> b) -> IO a -> IO b`
  - 一些规律：输入功能一般实现为`IO a`，输出功能一般实现为`a -> IO ()`
  - `IO`的传染性
    - `IO`实际上是完全不透明的特殊设施，它就是有其独特的限制
    - 一旦类型中染上IO，语言构造上没有任何办法再摘掉，也即`myExecute :: IO a -> a`是无法实现的
    - `IO a`和`a`的接口不再能直接匹配使用，比如`getLine ++ "MyString"`是不合法的，因为`IO String ++ String`的操作不存在
    - 传染性导致`IO`相关功能最好只出现在系统代码的边缘，避免污染内部
  
  - Do与纯函数书写
    - 复合要求的表达式构件，在Do块中可以当作过程式变成中的语句来使用
    - 可以认为Do语句块中是一个允许包含`IO a`的表达式序列，系统自动使用`*>`和`>>=`以及`->`正确地顺序串联其中所有表达式 
    - 三类表达式可以当作语句使用
      - `IO ()`类型的，比如`putStrLn "haha"`
      - `let`声明函数和变量（不需要`in`）
      - `var <- myVal`赋值语句，其中`myVal :: IO a`，而`var`就是IO操作的返回值
     
    - Do语句块本身本身等价于一个`IO a`类型的表达式，可以任意嵌套
    - Do语句块地取值取决于整个语句块中最后一个表达式的取值

- 工程化：Module
  - 导出
    ```Haskell
    module MyModule (
        MyType1,
        MyType2,
        MyFunc1,
        MyFunc2
        ) where
    
    -- where后面是类型和函数的实现代码
    ```
  - 导入：`import MyModule`

- 常见语法糖（待完善）

  - 局部变量
    ```Haskell
    Distance::Int->Int->Int
    Distance a b = 
        let
            sqareA = a*a
            sqareB = b*b
        in
            Sqrt (sqareA + sqareB)
    ```
  - 函数复合

    其他语言中的点记号`.`这里被用于函数复合。灵感来自数学中的映射复合符号$\circ$，比如$f$和$g$的复合写作$f \circ g$，所以Haskell中有：
    ```Haskell
    -- 等效如下定义
    (.) :: (b -> c) -> (a -> b) -> a -> c
    (.) f g x = f (g x)

    -- 用例
    double :: Int -> Int
    square :: Int -> Int
    result :: Int

    -- result = 36
    result = square . double 3
    -- 等效写法
    result = square (double 3)
    ``` 

    注意：
    - 跟数学中的函数复合类似，复合函数是右结合的，所以`f.g x`中是先计算`g x`再计算`f (g x)`
    - 复合运算符本身也是右结合的

    复合记号的价值，除了可以链式书写函数复合，还有快速书写类型构造，比如：
    ```Haskell
    data Name = Name string
    data Student = Student Name Int
    myFunc :: String -> Student
    myFunc name = Student . Name name
    ``` 

  - 真·匿名函数
    ```Haskell
    -- result的值为16
    result = (\num -> num * 2) 8
    ```

    注意，我们不使用一个专门记号来保存表达式中的函数定义，而是直接书写，这里反斜杠的必要性在于我们相当于不书写入参类型的情况下直接书写函数定义，让编译器现场推到这个反斜杠的入参的类型。而正常函数书写，定义和类型是需要分开书写的

  - 单分支的case简化：
    ```
    data Student = Student String Int
    getStudentName :: Student -> String
    -- 完整书写
    getStudentName stu = 
        case stu of
            Student name score -> name
    
    -- 简略书写：直接将Pattern写在入参的位置
    getStudentName (Student name score) = name
    ```

  - `$`记号
    
    用法一：减少函数嵌套调用的使用产生的括号
    ```
    -- 等效putStrLn(show(3+4))
    putStrLn $ show $ 3+4
    ``` 

    用法二：绑定多参函数的第二个成员

    ```
    div :: Float -> Float -> Float
    div a b = a / b
    -- 朴素的绑定第一个参数的办法：
    div1st x = div x 
    -- 绑定第二个参数的语法糖：
    div2nd y = div $ y
    ```

## 纯函数的Monad
- **Monad可以看作一种暂时打不开的盒子。理论上一切异步过程和副作用都可以建模为Monad**
- 某种意义上说，**Monad是一种设计模式**
- Haskell中最典型的Monad就是IO，但Monad其实不只有IO。Monad在Haskell中可以看作一个类型类（类型构造器）：

  ```Haskell
  class Monad m where
    pure :: a -> m a
    (>>=) :: m a -> (a -> m b) -> m b
  ``` 

  其中`>>=`操作也叫`bind`操作，是`Moand`的核心操作。它看起来跟对单个元素执行变换的`map`有点像：`map :: a -> (a -> b) -> b`。如果把`Monad`看作一个“箱子”，那`>>=`操作就是允许在不拆箱子的情况下变换箱子里的东西的操作，所以也有些人将其称为`flatmap`

  另一个隐藏的特点是，Monad应该是自然解开嵌套的，就是我们一般希望对于某个`Monad`，`Monad Monad T`等效`Monad T`（整个操作又是也称为`flatten`）

  从上面的定义来看，Monad跟IO毫无关系，IO只是Monad的特例

- Monad举例（以下以C#/C++语法书写了）
  - Maybe就是Monad，相当于C#的可空类型。
    - C#中，`bind`操作就是我们允许将`U?`的对象变换为`V?`类型（比如说安全地对`T?`串联解引用），而不需要拆掉问号；当然我们很容易从`T`构造`T?`
  - 很多Lazy求值可以建模为Monad
    - 原则上我们应该允许对`Lazy<T>`执行所有`T`能支持的操作，只不过延迟到`Lazy`异步完成再执行这些操作
    - 我们考虑`Lazy<T>`，如果Lazy是Monad，支持`flattenTransform(Lazy<T>, Function<U, Lazy<U>>)`操作，则我们可以使用`flattenTransform`执行任意操作，直到真的想取出结果再执行什么`Lazy<TResult>.GetValue`
  - Future/Promise可以是Monad
    - `Promise<T>/Future<T>`是对输入执行某些操作后，承诺未来某个时间点能拿到`T`类型的结果 
    - 我们通过串联使用`Promise<T>.then()`来将嵌套代码变成串行代码
      - 使用回调
        ```javascript
        firstRequest(response =>
        secondRequest(response, nextResponse =>
            thirdRequest(nextResponse, finalResponse =>
                console.log('Final response: ' + finalResponse);
                , failureCallback),
            failureCallback),
        failureCallback);
        ```  

    - 使用Promise
       ```javascript
        firstRequest()
        .then(response      => secondRequest(response))
        .then(nextResponse  => thirdRequest(nextResponse))
        .then(finalResponse => console.log('Final response: ' + finalResponse))
        .catch(failureCallback);
        ```  
    
    - 之所以能从嵌套中解脱，是因为`then`会返回一个Future，这正式Monad的特点

  - 甚至整个`async/await`协程模型都可以看作是某种Monad驱动的异步编程框架
    - 我们编写的协程中，每个*Awaitable*都可以看作Monad，这次Monad这个箱子里装的东西就是整个函数的上下文（或者说它的栈帧）
    - 我们的`async`协程代码就是对绕过Awaitable这个箱子对箱子里的东西进行操作（也就平凡地实现了`>>=`），然后整个`async`函数也是一个Awaitable
    - 在实现中，我们允许在还没等到之前挂起等待，来处理Awaitable中的值暂时拿不到的问题
    - 当然，代价就是现代`async/await`协程模型跟Monad一样具有传染性，工程上来说总是面临困难
 
  - Parser也是一个Monad
  - 实际上FP中的List也可以视作一个Monad


- 到底Monad是怎么跟消除副作用/建模状态/异步扯上关系的
  - State Monad如何建模“状态”
    - Haskell代码
      ```
        -- State是一个泛型函数的别名，runState方便将这个函数从别名中取出来
        newtype State s a = State {
            runState :: s -> (a, s)
        }

        -- returnState是泛型函数，其中美元符号是为了节省括号
        returnState :: a -> State s a
        returnState a = State $ \s -> (a, s)

        -- bindState是Monad需要的标准bind操作了，其中m是绑定了a值类型的State
        -- m :: State s a
        -- k :: a -> State s b
        -- k a :: State s b
        -- runState (k a) :: s -> (b, s)
        -- runState (k a) s' :: (b, s')

        bindState :: State s a -> (a -> State s b) -> State s b
        bindState m k = State $ \s -> runState (k a) s'
            where (a, s') = runState m s

        instance Monad (State s) where
            m >>= k = bindState m k
            return a = returnState a
      ```
    - StateMonad把状态转移当作状态本身，这样做有效的原因是：给定一个初始输入和状态转移函数的序列，我们总是可以通过路径上所有的转移的复合来计算出当前的“返回值”
    - 本质上是将状态机的中间状态看作是求值过程的中间取值，**将状态机整个生命周期的输入看作是一整个固定的函数输入，将时序逻辑化归为纯函数的组合逻辑**，并在实际运行中该表达式的输入并不是一开始就到位，若相关的输入尚未到位，则相关的转移函数的计算只能阻塞等待/挂起等待，直到输入得到更新，由此通过纯函数建模了时序逻辑
    - 其实这方面最好的示例是使用State Monad实现栈
  - Monad能消除副作用的原因是：
    - Monad可以将状态的每次转移和状态的记忆，等价替换为，对起始输入以及一连串后续输入一连串变换和纯函数的复合
    - `IO String`代表的其实是用户在程序整个生命周期中的所有输入。由于程序不可能预测用户的所有输入，所以每次bind函数求值时，如果缓冲区没有内容，则需要等待输入，而“表达式”的“求值”就暂停了；用户读出某些内容后`IO String`这个缓冲区的抽象并没有实质改变，而是作为下一个输入处理函数的输入状态，通过“挂起”将组合逻辑拆分成时序逻辑
  - 综上，在使用Monad设计模式时，我们考虑的**Monad其实就是Context**，这不过这个Monad在函数式里式“不变”的Context——每个跟副作用相关的函数都需要输入Context然后输出一个变换后的Context罢了
  - 最后，其实在没有OOP的C语言中，Monad有一个更加直白的近似物：*全局变量*

- Monad只是其中一种抽象，设计模式上还有很多类似Monad的东西
  - 比如在异步中使用Proxy，如果`Proxy<T>`代理了所有`T`的接口，则`Proxy`也可以算Monad。这意味着异步的事实可以被Proxy彻底隔离，用户代码可以几乎无感地对异步或同步加载的`T`类型对象执行相同的操作
    - 当然，代价就是，事实上Proxy需要以某种方式录制所有操作，并在异步完成的时刻重放它们
    - 工程实践中，可以通过收窄Proxy的接口来避免无止境的重复编码
    - 不过有一说一，如果Proxy真的实现了`T`的全部接口，那上层永远也不需要真正取出`T`来，只需要操作Proxy即可。Future也是同理的

## 题外话：为啥Monad是“自函子范畴上的一个幺半群”
- 
- 幺半群条件
  - 群元：函数`a -> b`（注意这里把高阶函数也看作函数）
  - 群乘法：`>>=`组合运算
  - 单位元：`return :: a -> M a`运算，需要验证这个运算是单位元
  - 结合律：需要保证成立`f >= g >= h = f >= (g >= h)`

- 自函子
  - 函子：
    ```
    class Functor f where
        fmap :: (a -> b) -> f a -> f b
    ```

    将类型类`Monad`看作是函数类型的映射，这样的映射就是函数
  - 自函子：
    - 由于类型系统只有一阶类型，所有的函数都是同一类的，所以`Monad`这个函子就是自我映射的函子，就是自函子