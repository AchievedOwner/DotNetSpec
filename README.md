# .NET 编程开发手册 v1.1
-- 《代码评审参考手册》

## 目录

| 变更记录 |
| --- |
| 前言 |
| 编程规范 |
| 1.命名规范 |
| |
| 3.OOP 规范 |
| 4.控制语句 |
| 5.并发处理 |
| 6.集合 |
| 7.注释规范 |
| 8.其他 |
| 异常日志 |
| 1.异常处理 |
| 2.日志规范 |
| 单元测试 |
| 项目结构 |
| 1.应用分层 |
| 2.第三方库 |
| 参考文献 |
| |

#

# 变更记录

| 版本 | 变更内容 | 变更人 | 日期 | 备注 |
| --- | --- | --- | --- | --- |
| 1.0 | 初步拟定内容 | 周大侠 | 2019-6-1 |   |
| 1.1 | 细化 ViewModel 的命名规范 | 周大侠 | 2019-8-7 |   |

#

#

#

# 前言

现代软件行业的高速发展对开发者的综合素质要求越来越高，因为不仅是编程知识点，其它维度的知识点也会影响到软件的最终交付质量。比如：数据库的表结构和索引设计缺陷可能带来软件上的架构缺陷或性能风险；工程结构混乱导致后续维护艰难；没有鉴权的漏洞代码易被黑客攻击等等。本手册以开发人员为中心，定义了编程规范、异常日志、单元测试、项目结构4个维度。根据约束力强弱及故障敏感性，规范依次分为【强制】、【推荐】、【参考】三大类。对于条目的延伸信息中，`说明`对内容做了适当扩展和解释；`正例`提倡什么样的编码和实现方式；`反例`说明需要提防的雷区，以及真实的错误案例。

本手册的愿景是 **高质量代码才是高效率的提现。** 现代软件架构都需要协同开发完成，高效 协作即降低协同成本，提升沟通效率，所谓无规矩不成方圆，无规范不能协作。众所 周知，制订交通法规表面上是要限制行车权，实际上是保障公众的人身安全。试想如果没有限速，没有红绿灯，谁还敢上路行驶。对软件来说，适当的规范和标准绝不是 消灭代码内容的创造性、优雅性，而是限制过度个性化，以一种普遍认可的统一方式 一起做事，提升协作效率。代码的字里行间流淌的是软件生命中的血液，质量的提升是尽可能少踩坑，杜绝踩重复的坑，切实提升质量意识。

本手册只以 .NET 平台下的 C# 语言进行规范，不对其他语言进行规范。并且为了与现代技术信息同步，运行时框架一律使用 .NET Core 2.1 和.NET Standard 2.0以上版本，支持最低C#语法版本为 7.0，务必使用 Visual Studio 2017 15.6+ 版本，推荐使用 Visual Studio 2019 版本，点击 [http://www.visualstudio.com](http://www.visualstudio.com. 下载最新版本的Visual Studio，以及 [https://dotnet.microsoft.com/download/dotnet-core](https://dotnet.microsoft.com/download/dotnet-core.下载相关的 .NET Core SDK。



# 编程规范

## 命名规范

代码的命名规则必须遵守驼峰命名法

变量和参数使用【Camel】命名规范，即第一个单词首字母小写，其余单词首字母大写；

其他都必须使用【Pascal】命名规范，即每个单词首字母必须大写。

1. 【强制】代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。

    >说明：正确的英文拼写和语法可以让阅读者易于理解，避免歧义，可借助翻译软件，并写明注释。

    正例：tencent / taobao / json / ip / huawei 等国际通用的名称，可视同为英文

2. 【强制】类的成员变量，必须使用前缀`\_`开头，但前缀之后的字符不能以数字、特殊字符开头，也不能以下划线、特殊字符结尾。

    正例：\_member / \_context / \_userId / \_fileManagerFactory

    反例：\_$dollar / \_@name / name\_ / name@\_

3. 【强制】方法里的成员变量和方法的签名，不允许使用下划线、特殊字符、数字开头，也不能以下换线、特殊字符结尾。

    正例：member / context / userId / fileManagerFactory

    反例：\_$dollar / @name / name\_ / name@ / name$

    >说明：尽量不要使用编译器所支持的关键字来命名变量和参数。

4. 【推荐】变量声明使用关键字 **`var`** 来替代强制声明的数据类型，并务必对变量进行初始化。这样声明类型可以根据初始化的值，由编译器自动推断数据类型。这一点在代码重构时就能更高效地提现出来。

    正例：`var users = GetUserList(3.;`

5. 【强制】参数名称，不允许使用下划线、特殊字符、数字开头，也不能以下换线、特殊字符结尾。

    正例：member / context / userId / fileManagerFactory

    反例：\_$dollar / @name / name\_ / name@ / name$

    >说明：尽量不要使用编译器所支持的关键字来命名参数。

6. 【强制】常量的名称，所有字母必须是大写，每一个单词用下划线分隔，且不允许使用下划线、数字、特殊字符开头和结束。

    正例：VERSION\_NUMBER / STATIC\_FILE\_NAME

    反例：version\_number / staticFileName

    >说明：常量必须使用关键字 **`const`** 来修饰，力求语义表达完整清楚，不要嫌名字长。

7. 【强制】接口第一个字母必须是英文字母`I`开头

    正例：IEnumerable, IMyService

8. 【强制】抽象类使用`Base`单词结尾，异常类命名使用 Exception 结尾；测试类命名以它要测试的类的名称开始，以 Test 结尾。

10. 【推荐】布尔值的变量、属性或方法，首个单词以`Is、Has、Can`开头。

    正例：isSystem / HasNamed / CanClick

11. 【强制】命名空间的每个级别使用`.`分隔，之间有且仅有一个自然语义的英语单词。
13. 【强制】杜绝完全不规范的缩写，避免望文不知义。

    反例：AbstractClass`缩写`命名成 AbsClass；condition`缩写`命名成 condi，此类随意缩写严重降低了代码的可阅读性。

14. 【推荐】为了达到代码自解释的目标，任何自定义编程元素在命名时，使用尽量完整的单词组合来表达其意。

    正例：从远程仓库拉取代码的类命名为 PullCodeFromRemoteRepository。

    反例：变量 `int a;` 的随意命名方式。

15. 【推荐】如果模块、接口、类、方法使用了设计模式，在命名时体现出具体模式。 说明：将设计模式体现在名字中，有利于阅读者快速理解架构设计理念。

    正例： `public class OrderFactory;`, `public class LoginProxy;` , `public class ResourceObserver;`

16. 【强制】枚举声明使用英文名词复数形式，枚举项采用【Pascal】命名规范，并且设置每一个项的值。

    正例：
    ```
    public enum MemberTypes
    
    public enum SqlOptions
    {
      SqlServer = 0,
      MySql = 1
    }
    ```
    反例：

    ```
    public enum SqlTypeEnum
    ```

17. 【推荐】枚举项的要有一个未知的项，并且值用 0 来表示。可以使用 None或 Unknow 来作为第一项的枚举选项。

    正例：
    ```
    public enum MemberTypes
    {
      Unknow = 0,
      Manager = 1,
      User = 2
    }
    ```

    >说明：当枚举项没有赋值但被使用，其值是0，若代码中没有判断就很可能存储错误的值。

18. 【推荐】应使用异步方法，并返回 `Task` 或 `Task<T>`，方法名称必须以 `Async` 结尾。

    正例：`public Task<int> GetUserAsync(int id);`

    反例：`public Task InsertFile(string fileName);`

19. 【推荐】扩展方法的类名称，必须使用 `Extensions` 结尾。

    正例：`public static class FileNameExtensions`

20. 【参考】分层命名参考：

    **业务逻辑方法命名规范：**
  
  * 获取数据：以 `Get/Find` 开头，获取多条数据，以名词复数结尾，如：`GetUsers`
  * 新增数据：以 `Create/Add` 开头
  * 更新数据：以 `Update` 开头
  * 删除数据：以 `Delete/Remove` 开头

## 代码格式

1. 【强制】大括号使用规范。如果大括号内容为空，则简介地写成｛｝即可，但应避免这样的代码出现；若大括号内容不为空，则：
    
    * 左大括号前换行
    * 左大括号后换行
    * 右括号前换行
    * 右大括号后必须换行。

2. 【强制】左小括号和字符之间不出现空格，同样，右小括号和字符之间不出现空格。

    反例：`if(空格a == b空格)`

3. 【强制】`if/for/while/switch/do` 等保留字与括号之间都必须加空格。
4. 
5. 【强制】操作符的两端必须要有空格。

6. 【强制】采用4个空格的缩进，禁止使用tab字符

    >说明：如果使用 tab 缩进，必须设置 1 个 tab 为 4 个空格。

    正例（以上1-5）：
    ```
        static void Main(string[] args)
        {
            // 缩进 4 个空格
            String say = `hello`;
            
            // 运算符的左右必须有一个空格
            int flag = 0;

            // 关键词 if 与括号之间必须有一个空格，括号内的 f 与左括号，0 与右括号不需要空格

            if (flag == 0)
            {
                Console.WriteLine(say);
            }

            // 左大括号前加空格且换行
            if (flag == 1)
            {
                Console.WriteLine(`world`);
                // 右大括号前换行
            }
            else
            {
                Console.WriteLine(`ok`);
                // 在右大括号后直接结束，则必须换行
            }
        }
        ```

7. 【强制】注释的双斜线与注释内容之间有且仅有一个空格。

    正例：
    ```
    // 注释内容
    ```

8. 【推荐】如果只有一句代码，尽量使用方法主体语句。

    正例：
    ```
    public Task<User> GetById(int id)
        => \_context.GetAsync(id);
    
    public int CountSaveChanges()
        => \_context.Count();
    ```

9. 【强制】单行字符数限制不超过120个，超过需要换行，换行时务必遵守以下原则：

    * 第二行相对第一行缩进 4 个空格，从第三行开始，不再继续缩进；
    * 运算符与下文一起换行；
    * 方法调用的点符号与下文一起换行；
    * 方法调用时，多个参数，需要换行时，在逗号后换行，换行的参数要和一个参数对齐；
    * 参数超过5个以后，要求每一个参数换行并与第一个参数对齐显示；
    * 在括号前不要换行，见反例。

    正例：
    ```
    var sb = new StringBuilder();
    sb.Append("xxx")
        .Append("xxxx")
        .Append("xxx")
        .Append("xxx")
            ...

    // 参数超过了5个，每个参数换行对齐
    public void Add(object arg1,
                 object arg2,
                 object arg3,
                 object arg4,
                 object arg5,
                 object arg6.
    {
        // to do
    }
    ```

    反例：
    ```
    var sb = new StringBuilder();
    // 超过 120 个字符的情况下，不要在括号前换行
    sb.Append("zi").Append("xin")...Append
    (`huang`);
    ```

10. 【强制】方法参数在定义和传入时，多个参数逗号后边必须加空格。
11. 正例：下例中实参的`a`,后边必须要有一个空格。
    ```
    method("a", "b", "c");
    ```

12. 【推荐】没有必要增加若干空格来使某一行的字符与上一行对应位置的字符对齐。

    正例：
    ```
    int a = 3;
    long b = 4L;
    float c = 5F;
    StringBuilder sb = new StringBuilder();
    ```
    
    >说明：增加 sb 这个变量，如果需要对齐，则给 a、b、c 都要增加几个空格，在变量比较多的情况下，是一种累赘的事情。

13. 【推荐】方法体内的执行语句组、变量的定义语句组、不同的业务逻辑之间或者不同的语义之间插入一个空行。相同业务逻辑和语义之间不需要插入空行。

    >说明：没有必要插入多个空行进行隔开。

14. 【推荐】 类内方法定义顺序依次是：公有方法或保护方法 > 私有方法 > 属性，并且使用 `#region` 来分类所占区域

>说明：公有方法是类的调用者和维护者最关心的方法，首屏展示最好；保护方法虽然只是子类关心，也可能是`模板设计模式`下的核心方法；而私有方法外部一般不需要特别关心，是一个 黑盒实现；因为承载的信息价值较低，而属性则放在类体最后。

15. 【推荐】当一个类有多个构造方法，或者多个同名方法，这些方法应该按顺序放置在一起，便于阅读。

## OOP 规范

1. 【推荐】引用类型的属性要在声明后给一个默认实例化。

    正例：
    ```
    public MyEntity Table { get; set; } = new MyEntity();
    
    public IEnumerable<UserInfo> Users { get; set; } = new HashSet<UserInfo>();
    ```

    >说明：如果外部使用该引用属性忘记实例化，则会抛出空引用异常。

2. 【强制】已经被广泛使用的接口（interface）如果要增加方法或修改参数，要使用扩展方法来对原有方法进行扩展。

 >说明：接口中已定义好的方法或参数被直接更改，会导致程序里的所有调用处都报异常，使用扩展方法可以有效避免此事情的发生。唯一缺点就是不能被重新实现。

3. 【强制】不要使用已过期的类或方法。

 >说明：接口提供方既然明确是过时接口，那么有义务同时提供新的接口；作为调用方来说，有义务去考证过时方法的新实现是什么。

4. 【强制】`Object` 的 `equals` 方法容易抛空指针异常，应使用常量或确定有值的对象来调用 `Equals`。

    正例：`"test".Equals(object);`

    反例：`object.Equals(`test`);`

5. 【强制】任何情况下，禁止使用字符串拼接。特别是在循环内部组装字符串时，要使用 `StringBuilder` 类。

    正例：
    ```
    var sb = new StringBuilder();
    foreach (var span in collection)
    {
        sb.Append($"{span},");
    }
    ```

    反例：
    ```
    string str = string.Empty;
    foreach (var span in collection)
    {
        str += span + ",";
    }
    ```

    >说明：每进行一次字符串拼接，CLR 其实就进行了一个 StringBuilder 的实例化，非常消耗资源。 **如果是简单的字符串连接，推荐使用模板字符串或格式化字符串。**

    - 模板字符串：`$"{变量1}xxxxxxx{变量2}"`
    - 格式化字符串：`string.Format("{0}xxxxxxx{1}", arg0, arg1);`

6. 【强制】类、类的成员与方法访问控制从严：

    - 如果不允许外部直接通过 `new` 来创建对象，那么构造方法必须是 `private`。
    - 工具类应该用 `static` 修饰类型，应尽量使用扩展方法来表达工具类型。
    - 类的成员变量必须是 `private`，不允许共享给任何其他类。
    - 类的属性并且与子类共享，必须是 `protected`。
    - 类 `static` 成员变量如果仅在本类使用，必须是 `private`。
    - 若是 `static` 成员变量，必须考虑是否为 `readonly`。
    - 类成员方法只供类内部调用，必须是 `private`。
    - 类成员方法只对继承类公开，那么限制为 `protected`。
    - 慎用 `static` 的属性，并确保属性仅仅提供只读功能。

    >说明：任何类、方法、参数、变量，严控访问范围。过于宽泛的访问范围，不利于模块解耦。

    > 思考：如果是一个 private 的方法，想删除就删除，可是一个 public 的方法，或者一个 public 的成员变量，删除一下，不得手心冒点汗吗？变量像自己的小孩，尽量在自己的    视线内，变量作用域太大，无限制的到处跑，那么你会担心的。

7. 【推荐】减少使用 `Tuple` 类，而应该使用元祖返回值

    > 说明：因为 Tuple 仅返回 Item1, Item2, ItemX，调用者根本不能直观的知道 Item1 Item2 到底是什么意思。使用 TupleValue 可以直接返回字段，而不是 Item1 Item2 ItemX，但该版类型需要 C# 7.0 的语法支持。

    正例：
    ```
    public (int age, string name. GetTuple()
        => (age: 1, name: `xyz`);
    
    var item = GetTuple();
    Console.WriteLine(item.age);
    Console.WriteLine(item.name);
    ```


## 控制语句

1. 【强制】在控制语句中，即使只有一条语句，也不能省略大括号。

    正例：
    ```
    if(a == 1)
    {
        throw new ArgumentOutOfRangeException();
    }
    ```

    反例：
    ```
    if(a == 1)
        throw new ArgumentOutOfRangeException();
    ```

2. 【强制】在一个 `switch` 块内，每个 `case` 要么通过 `break/return` 等来终止，要么注释说明程序将继续执行到哪一个 `case` 为止；在一个 `switch` 块内，都必须包含一个 `default` 语句并且放在最后，即使它什么代码也没有。

3. 【推荐】表达异常的分支时，少用 if-else 方式，这种方式可以改写成：
    ```
    if (condition)
    {
        // ...
        return obj;
    }
    // 接着写 else 的业务逻辑代码;
    ```

    >说明：如果非得使用 `if-else-if` 方式表达逻辑，
    
4. 【强制】避免后续代码维护困难，请勿超过 3 层。

    正例：超过 3 层的 `if-else` 的逻辑判断代码可以使用卫语句、策略模式、状态模式等来实现，

    其中卫语句示例如下：
    ```
    public void Today()
    {
        if (IsBusy())
        {
            Console.WriteLine("change time.");
            return;
        }
        
        if (IsFree())
        {
            Console.WriteLine ("go to travel.");
            return;
        }
        
        Console.WriteLine ("stay at home to learn C# Coding Guidelines.");
        return;
    }
    ```

5. 【推荐】除属性外，不要在条件判断中执行其它复杂的语句，将复杂逻辑判断的结果赋值给一个有意义的布尔变量名，以提高可读性。

    >说明：很多 if 语句内的逻辑相当复杂，阅读者需要分析条件表达式的最终结果，才能明确什么样的条件执行什么样的语句，那么，如果阅读者分析逻辑表达式错误呢？

    正例：
    ```
    var canCreate = (Directory.Exists("xxxx") && File.Exists("xxx") || (……)  ||  (……);
    
    if(canCreate)
    {
        ……
    }
    ```

    反例：
    ```
     if ((Directory.Exists("xxxx") && File.Exists("xxx")  ||  (……)  ||  (…….)
     {
      ……
     }
    ```
    
6. 【推荐】循环体中的语句要考量性能，以下操作尽量移至循环体外处理，如定义对象、变量、获取数据库连接，进行不必要的 `try-catch` 操作（这个 `try-catch` 是否可以移至循环体外）。

## 并发处理

1. 【强制】获取单例对象需要保证线程安全，其中的方法也要保证线程安全。

    >说明：资源驱动类、工具类、单例工厂类都需要注意。

## 集合

1. 【强制】如果重写了 `Equals` 方法，就必须重写 `GetHashCode` 方法。

2. 【推荐】如果需要泛型集合类型进行值传递时，要使用 `IEnumerable<T>` 类型，而不是 `List<T>`。

    >说明：`List<T>` 是列表，有添加和移除功能，可能会被调用方私自改写集合的内容，而造成不可预期的错误，而 `IEnumerable<T>` 只是一个迭代集合，仅可获取，不可修改。

3. 【推荐】在属性中，给 IEnumerable<T> 进行初始化赋值时，应该使用 `HashSet<T>` 而不是 `List<T>`。

    正例：
    ```
    public IEnumerable<T> Users { get; set; } = new HashSet<T>();
    ```

    >说明：`HashSet<T>` 的效率要明显高于 `List<T>`

4. 【强制】集合判断是否有数据，使用 `System.Linq` 的 `Any` 方法。

    正例：
    ```
    if(list.Any())
    {
        // …
    }
    ```
5. 【推荐】使用 `System.Linq` 的扩展方法来对集合进行处理。

## 注释规范

1. 【强制】类、接口、属性、事件、委托、结构体、公开字段等，都需要使用 `///<summary> … ///</summary>` 的形式添加注释。不允许使用 `// 注释`。

    >说明：VS 会根据三斜杠的注释，在调用方调用时，将注释显示在候选列表中，以帮助调用者理解使用当前对象的意义。

2. 【强制】方法的签名每一个参数都必须使用 `<param name="参数">注释</param>` 写注释。

    >说明：试想一下，如果你在调用某个方法时，搞不清楚某个参数的使用方式，你是不是会无奈？

3. 【强制】所有的方法，如果方法内部抛出异常，必须使用 `/// <exception cref="异常">抛出异常的原因</exception>` 在方法顶部标记出来。

4. 【强制】如果方法有返回值，必须使用 `<returns>返回值的说明</returns>` 并写清楚返回的值的注释。

    正例：
    ```
    /// <summary>
    /// 创建指定名称的文件夹。
    /// </summary>
    /// <param name=`name`>要创建的文件夹名称。</param>
    /// <exception cref=`ArgumentNullException`><paramref name=`name`/> 是 null。</exception>
    /// <returns>创建成功返回 <c>true</c>;否则返回 <c>false</c>。</returns>
    public bool MakeDir(string name)
    {
        if (name == null)
        {
            throw new ArgumentNullException(nameof(name));
        }
    
        // to do
        return false;
    }
    ```

5. 【强制】所有的枚举类型字段必须要有注释，说明每个数据项的用途。

6. 【推荐】与其 **“半吊子”** 英文来注释，不如用中文注释把问题说清楚。专有名词与关键字保持英文原文即可。

    反例："TCP 连接超时" 解释成 "传输控制协议连接超时"，理解反而费脑筋。

7. 【推荐】代码修改的同时，注释也要进行相应的修改，尤其是参数、返回值、异常、核心逻辑等的修改。

    >说明：代码与注释更新不同步，就像路网与导航软件更新不同步一样，如果导航软件严重滞后，就失去了导航的意义。

8. 【参考】谨慎注释掉代码。在上方详细说明，而不是简单地注释掉。如果无用，则删除。 说明：代码被注释掉有两种可能性：
    - 后续会恢复此段代码逻辑。
    - 永久不用。前者如果没有备注信息，难以知晓注释动机。后者建议直接删掉（代码仓库保存了历史代码）。

9. 【参考】对于注释的要求：
    * 第一、能够准确反应设计思想和代码逻辑；
    * 第二、能够描述业务含义，使别的程序员能够迅速了解到代码背后的信息。
    >完全没有注释的大段代码对于阅读者形同天书，注释是给自己看的，即使隔很长时间，也能清晰理解当时的思路；注释也是给继任者看的，使其能够快速接替自己的工作。

10. 【参考】好的命名、代码结构是自解释的，注释力求精简准确、表达到位。避免出现注释的一个极端：过多过滥的注释，代码的逻辑一旦修改，修改注释是相当大的负担。

    反例：
    ```
    // put elephant into fridge
    Put(elephant, fridge);
    ```

    方法名 put，加上两个有意义的变量名 elephant 和 fridge，已经说明了这是在干什么，语义清晰的代码不需要额外的注释。

13. 【参考】特殊注释标记，请注明标记人与标记时间，并且使用 #region #endregion 将范围包含起来。

    >说明：注意及时处理这些标记，通过标记扫描， 经常清理此类标记。线上故障有时候就是来源于这些标记处的代码。

    - 待办事宜（ **TODO** ）:（ 标记人，标记时间，[预计处理时间]） 表示需要实现，但目前还未实现的功能。
    - 错误，不能工作（ **FIXME** ）:（标记人，标记时间，[预计处理时间]） 在注释中用 FIXME 标记某代码是错误的，而且不能工作，需要及时纠正的情况。

## 其他

1. 【推荐】任何数据结构的构造或初始化，都应指定大小，避免数据结构无限增长吃光内存。

2. 【推荐】及时清理不再使用的代码段或配置信息。

    >说明：对于垃圾代码或过时配置，坚决清理干净，避免程序过度臃肿，代码冗余。

    正例：对于暂时被注释掉，后续可能恢复使用的代码片断，在注释代码上方，统一规定使用多行注释(/\*\*/.来说明注释掉代码的理由。



# 异常日志

## 异常处理

1. 【强制】不允许直接使用catch 来对常规检查进行异常捕获，比如 `NullReferenceException`, `InvalidOperationException` 等。

    > 说明：无法使用常规检查除外。

    正例：
    ```
    if(obj == null)
    {
        // ……
    }
    ```
    反例：
    ```
    try
    {
        obj.Invoke();
    }
    catch (NullReferenceException)
    {
        // ……
    }
    ```

2. 【强制】异常不要用来做流程控制，条件控制，因为异常的处理效率比条件分支低。

3. 【强制】对大段代码进行 `try-catch`，这是不负责任的表现。`catch` 时请分清稳定代码和非稳定代码，稳定代码指的是无论如何不会出错的代码。对于非稳定代码的 `catch` 尽可能进行区分异常类型，再做对应的异常处理。

4. 【强制】捕获异常是为了处理它，不要捕获了却什么都不处理而抛弃之，如果不想处理它，请将该异常抛给它的调用者。最外层的业务使用者，必须处理异常，将其转化为用户可以理解的内容。

5. 【强制】有 `try` 块放到了事务代码中，`catch` 异常后，如果需要回滚事务，一定要注意手动回滚事务。

6. 【强制】`finally` 块必须对资源对象、流对象进行关闭，有异常也要做 `try-catch`。

7. 【强制】不能在 `finally` 块中使用 `return`，`finally` 块中的 `return` 返回后方法结束执行，不会再执行 `try` 块中的 `return` 语句。

8. 【强制】捕获异常与抛异常，必须是完全匹配，或者捕获异常是抛异常的父类。

    >说明：如果预期对方抛的是绣球，实际接到的是铅球，就会产生意外情况。

9. 【推荐】方法的返回值可以为 `null`，不强制返回空集合，或者空对象等，必须添加注释充分说明什么情况下会返回 null 值。调用方需要进行 `null` 判断防止 `NRE` 问题。

    >说明：本手册明确防止 NRE 是调用者的责任。即使被调用方法返回空集合或者空对象，对调用者来说，也并非高枕无忧，必须考虑到远程调用失败、序列化失败、运行时异常等场景返回 null 的情况。

10. 【推荐】防止 NRE，是程序员的基本修养，注意 NRE 产生的场景：

    - 返回类型为基本数据类型，return 包装数据类型的对象时，自动拆箱有可能产生 NPE。

    反例：`public int f() { return  (int.对象}`， 如果为 null，自动拆箱抛 NRE。
    
    - 数据库的查询结果可能为 null。
    - 集合里取出的数据元素也可能为 null。
    - 远程调用返回对象时，一律要求进行空指针判断，防止 NRE。
    - 对于 Session 中获取的数据，建议 NPE 检查，避免空指针。
    - 级联调用 obj.GetA(..GetB(..GetC(.；一连串调用，易产生 NRE。

    正例：使用可为空表达式 **（?.）** 来防止NRE的问题。

12. 【参考】避免出现重复的代码（Don&#39;t Repeat Yourself），即 DRY 原则。

    >说明：随意复制和粘贴代码，必然会导致代码的重复，在以后需要修改时，需要修改所有的副本，容易遗漏。必要时抽取共性方法，或者抽象公共类，甚至是组件化。

## 日志规范

1. 【强制】应用程序中，不可以直接使用日志系统的 API，而应该依赖于 `Microsoft.Extensions.Logging` 日志框架的 `ILogging` 或 `ILogging<T>` 接口或 `ILoggingFactory` 来创建以上两个接口。使用门面模式的日志框架，有利于维护和各个类的日志处理方式统一。

2. 【推荐】谨慎地记录日志。生产环境禁止输出 `debug` 日志；有选择地输出info日志；如果使用 `warn` 来记录刚上线时的业务行为信息，一定要注意日志输出量的问题，避免把服务器磁盘撑爆，并记得及时删除这些观察日志。

    >说明：大量地输出无效日志，不利于系统性能提升，也不利于快速定位错误点。记录日志时请思考：这些日志真的有人看吗？看到这条日志你能做什么？能不能给问题排查带来好处？

3. 【强制】异常信息应该包括两类信息：案发现场信息和异常堆栈信息。如果不处理，那么通过关键字 `throws` 往上抛出。

    正例：`logger.LogError(ex, "各类参数或者对象_{0}",  e.Message());`

4. 【参考】可以使用 `warn` 日志级别来记录用户输入参数错误的情况，避免用户投诉时，无所适从。注意日志输出的级别，`error` 级别只记录系统逻辑出错、异常等重要的错误信息。如非必要，请不要在此场景打出 `error` 级别。



# 单元测试

1. 【强制】好的单元测试必须遵守 AIR 原则。

    >说明：单元测试在线上运行时，感觉像空气（AIR）一样并不存在，但在测试质量的保障上，却是非常关键的。好的单元测试宏观上来说，具有自动化、独立性、可重复执行的特点。

    - A：Automatic（自动化）
    - I：Independent（独立性）
    - R：Repeatable（可重复）

2. 【强制】单元测试应该是全自动执行的，并且非交互式的。测试框架通常是定期执行的，执行过程必须完全自动化才有意义。输出结果需要人工检查的测试不是一个好的单元测试。单元测试中不准使用 `Console.WriteLine` 来进行人肉验证，必须使用 `Assert` 来验证。

3. 【强制】保持单元测试的独立性。为了保证单元测试稳定可靠且便于维护，单元测试用例之间决不能互相调用，也不能依赖执行的先后次序。

    反例：method2 需要依赖 method1 的执行，将执行结果做为 method2 的输入。

4. 【强制】单元测试是可以重复执行的，不能受到外界环境的影响。

    >说明：单元测试通常会被放到持续集成中，每次有代码 check in 时单元测试都会被执行。如果单测对外部环境（网络、服务、中间件等）有依赖，容易导致持续集成机制的不可用。

    正例：为了不受外界环境影响，要求设计代码时就把 SUT 的依赖改成注入，在测试时用 DI 框架注入一个本地（内存）实现或者 Mock 实现。

5. 【强制】对于单元测试，要保证测试粒度足够小，有助于精确定位问题。单测粒度至多是类级别，一般是方法级别。

    >说明：只有测试粒度小才能在出错时尽快定位到出错位置。单测不负责检查跨类或者跨系统的交互逻辑，那是集成测试的领域。

6. 【强制】核心业务、核心应用、核心模块的增量代码确保单元测试通过。

    >说明：新增代码及时补充单元测试，如果新增代码影响了原有单元测试，请及时修正。

7. 【强制】单元测试代码必须写在如下工程目录：test，不允许写在业务代码目录下。

    >说明：源码构建时会跳过此目录，而单元测试框架默认是扫描此目录。

8. 【推荐】单元测试的基本目标：语句覆盖率达到 70%；核心模块的语句覆盖率和分支覆盖率都要达到 100%。

9. 【推荐】编写单元测试代码遵守 BCDE 原则，以保证被测试模块的交付质量。

    - B：Border，边界值测试，包括循环边界、特殊取值、特殊时间点、数据顺序等。
    - C：Correct，正确的输入，并得到预期的结果。
    - D：Design，与设计文档相结合，来编写单元测试。
    - E：Error，强制错误信息输入（如：非法数据、异常流程、非业务允许输入等），并得到预期的结果。

10. 【推荐】对于数据库相关的查询，更新，删除等操作，不能假设数据库里的数据是存在的，或者直接操作数据库把数据插入进去，请使用程序插入或者导入数据的方式来准备数据。

    反例：删除某一行数据的单元测试，在数据库中，先直接手动增加一行作为删除目标，但是这一行新增数据并不符合业务插入规则，导致测试结果异常。

11. 【推荐】和数据库相关的单元测试，可以设定自动回滚机制，不给数据库造成脏数据。或者对单元测试产生的数据有明确的前后缀标识。

12. 【推荐】对于不可测的代码建议做必要的重构，使代码变得可测，避免为了达到测试要求而书写不规范测试代码。

13. 【推荐】在设计评审阶段，开发人员需要和测试人员一起确定单元测试范围，单元测试最好覆盖所有测试用例（UC）。

14. 【推荐】单元测试作为一种质量保障手段，不建议项目发布后补充单元测试用例，建议在项目提测前完成单元测试。

15. 【参考】为了更方便地进行单元测试，业务代码应避免以下情况：

    - 构造方法中做的事情过多。
    - 存在过多的全局变量和静态方法。
    - 存在过多的外部依赖。
    - 存在过多的条件语句。

16. 【参考】不要对单元测试存在如下误解：

    - 那是测试同学干的事情。本文是开发手册，凡是本文内容都是与开发同学强相关的。
    - 单元测试代码是多余的。汽车的整体功能与各单元部件的测试正常与否是强相关的。
    - 单元测试代码不需要维护。一年半载后，那么单元测试几乎处于废弃状态。
    - 单元测试与线上故障没有辩证关系。好的单元测试能够最大限度地规避线上故障。

# 项目结构
## 分层模型规范

- DO（Domain Object）：与数据库表一一对应的领域实体；

    【强制】使用名词复数形式命名并不能带有前缀，如 Users / Companies

- DTO（Data Transfer Object）：数据传输对象，Manager 与 Web 交互的对象；

    【强制】以 Info 作为后缀，如 UserInfo / CompanyInfo 等。
    
- VO（View Object）：视图对象，通常是 Web 与外界交互的传输对象；
    
    【强制】以 ViewModel 作为后缀，并遵循格式【模块名称 + 操作分类 + [页面/功能位置(可选.] + ViewModel】的形式
    
    - 模块名称：User, Notam, Flight…
    - 操作分类：Create(创建./Edit(编辑./Delete(删除./Display(展示./Query(查询.
    - 页面/功能位置：List(列表页面./Detail(详情页面. --可选的
    
    说明：特殊命名：公共的则直接进行名称 + ViewModel，如：PaginationViewModel 表示用于分页的ViewModel
    
    正例：
    
    * `AshtamDisplayListViewModel`(火山灰列表展示视图模型.
    * `UserCreateViewModel`(用户创建视图模型.
    * `CompanyRouteEditViewModel`(公司航路编辑视图模型.
    * `FlightCalculationDetailViewModel`(飞行计划计算详情视图模型.
    
- `Specification Object`(SO.：规约对象，各层接收上层的查询请求。注意超过3个参数的查询就要考虑封装成规约对象。
    
    【强制】以 Spec 作为后缀，如 UserListSpec / FlightPlanSpec

3. 【强制】项目命名空间每一层的名字作为第三级，最多不能超过5级。

    - UI层：UerInterfaces
    - API层：Exposes
    - 请求处理层：Web
    - 应用逻辑层：Managers
    - 领域服务层：Services
    
    >说明：如果需要单独为各层的传输对象建立项目和命名空间，则需要按照每层命名规范，并按以下规范命名：

    - VO：ViewModels
    - SO：Specifications
    - DTO：DataTransferObjects
    - DO：DataObjects

    【参考】由于 CIF 库已经封装某一层的功能，因此可以根据项目成本、时间、大小等因素考虑对于项目分层的划分。
    
    - Service 如果仅仅是数据存储的操作，可省略，由 CIF.Storage 来完成；
    - 如果无复杂的 Manager ，则可以省略，并将简单的业务逻辑写在 Web 层；
    
    >说明：没有一层不变的架构，需要根据业务的扩展性、时间的推移适当地对项目分层进行重构，以适应当前需求。

## 第三方库

1. 【强制】暴露对外的 API 一定要提供被调用方重写的功能，这样客户端可以根据需求改写内部调用算法。

    >说明：一个优秀的库，可以在需要的时候随时被客户端进行改写以满足二次开发的需要。这就需要设计者需要想得更多，尝试的更多，假设的更多。

2. 【强制】版本号命名方式：主版本号.次版本号.修订号

    - a.主版本号：产品方向改变，或者大规模API不兼容，或者架构不兼容升级。
    - b.次版本号：保持相对兼容性，增加主要功能特性，影响范围极小的API不兼容修改。
    - c.修订号：保持完全兼容性，修复BUG、新增次要功能特性等。

    >说明： 注意起始版本号 必须 为： 1.0.0，而不是 ，而不是 ，而不是 0.0.1 正式发布的类库必须先去中央仓库进行查证，使版本号有延续性，正式版本号不允许覆盖升级。如当前版本：1.3.3，那么下一个合理的版本号：1.3.4 或 1.4.0 或 2.0.0

3. 【强制】依赖于一个二方库群时，必须定义一个统一的版本变量，避免版本号不一致。

4. 【强制】库的发布，必须使用Release方式，并且包中必须带有.xml的注释文件，打包前要删除pdb文件或不进行生成。

# 参考文献

- 阿里巴巴Java编程规范1.3