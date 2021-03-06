# GO学习笔记

## 语言结构

```go
package main  //第一行代码 package main 定义了包名。你必须在源文件中非注释的第一行指明这个文件属于哪个包，如：package main。package main表示一个可独立执行的程序，每个 Go 应用程序都包含一个名为 main 的包。

import "fmt"  // import "fmt" 告诉 Go 编译器这个程序需要使用 fmt 包（的函数，或其他元素），fmt 包实现了格式化 IO（输入/输出）的函数。

func main() { //main 函数是每一个可执行程序所必须包含的，一般来说都是在启动后第一个执行的函数（如果有 init() 函数则会先执行该函数）。
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}


/*当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的（像面向对象语言中的 protected ）*/
```





* 变量分级比较极端：要么就是包内通行，要么就是函数内部

* 简短变量声明语句的形式可用于声明和初始化局部变量

* 一个指针的值是另一个变量的地址。一个指针对应变量在内存中的存储位置。并不是每一个值都会有一个内存地址，但是对于每一个变量必然有对应的内存地址。通过指针，我们可以直接读或更新对应变量的值，而不需要知道该变量的名字（如果变量有名字的话）。

* 任何类型的指针的零值都是nil。

* 在Go语言中，返回函数中局部变量的地址也是安全的

* new 创建的是指针

* Go语言的自动垃圾收集器对编写正确的代码是一个巨大的帮助，但也并不是说你完全不用考虑内存了。你虽然不需要显式地分配和释放内存，但是要编写高效的程序你依然需要了解变量的生命周期。例如，如果将指向短生命周期对象的指针保存到具有长生命周期的对象中，特别是保存到全局变量时，会阻止对短生命周期对象的垃圾回收（从而可能影响程序的性能）。

* 类型声明语句一般出现在包一级，因此如果新创建的类型名字的首字符大写，则在包外部也可以使用。

* ```go
  type 类型名字 底层类型
  ```

* 这样的init初始化函数除了不能被调用或引用外，其他行为和普通函数类似。在每个文件中的init初始化函数，在程序开始执行时按照它们声明的顺序被自动调用。

* 无符号数往往只有在位运算或其它特殊的运算场景才会使用，就像bit集合、分析二进制文件格式或者是哈希和加密操作等。它们通常并不用于仅仅是表达非负数量的场合。

* 当调用一个函数的时候，函数的每个调用参数将会被赋值给函数内部的参数变量，所以函数参数变量接收的是一个复制的副本，并不是原始调用的变量。因为函数参数传递的机制导致传递大的数组类型将是低效的，并且对数组参数的任何的修改都是发生在复制的数组上，并不能直接修改调用时原始的数组变量。在这个方面，Go语言对待数组的方式和其它很多编程语言不同，其它编程语言可能会隐式地将数组作为引用或指针对象传入被调用的函数。

* 上面其实说明，数组作为参数传递，是值传递。这就很僵硬，所以一般使用切片。这里也和python不同，python的数组和切片是一体两面，go的切片和数组是两种东西，特性都不一样。尽管底层还是数组。

* 切片：[]T,不指定长度

* 也可以这么定义：

  ```go
  months := [...]string{1: "January", /* ... */, 12: "December"}
  // 这样子months[0] 直接被初始化为字符串，实际使用的时候，是从1，开始用的。
  ```

* 如果切片操作超出cap(s)的上限将导致一个panic异常，但是超出len(s)则是意味着扩展了slice，因为新slice的长度会变大

* 因为slice值包含指向第一个slice元素的指针，因此向函数传递slice将允许在函数内部修改底层数组的元素。换句话说，复制一个slice只是对底层的数组创建了一个新的slice别名

* 一个零值的slice等于nil。一个nil值的slice并没有底层数组。一个nil值的slice的长度和容量都是0，但是也有非nil值的slice的长度和容量也是0的，例如[]int{}或make([]int, 3)[3:]。

* 内置的make函数创建一个指定元素类型、长度和容量的slice。容量部分可以省略，在这种情况下，容量将等于长度。

  ```go
  make([]T, len)
  make([]T, len, cap) // same as make([]T, cap)[:len]
  ```

  在底层，make创建了一个匿名的数组变量，然后返回一个slice；只有通过返回的slice才能引用底层匿名的数组变量。在第一种语句中，slice是整个数组的view。在第二个语句中，slice只引用了底层数组的前len个元素，但是容量将包含整个的数组。额外的元素是留给未来的增长用的。

* 从这个角度看，slice并不是一个纯粹的引用类型，它实际上是一个类似下面结构体的聚合类型：

  ```go
  type IntSlice struct {
      ptr      *int
      len, cap int
  }
  ```

  





### Map

* 内置的make函数可以创建一个map：

  ```go
  ages := make(map[string]int) // mapping from strings to ints
  ```

* 也可以附带初始量的初始化：

  ```go
  ages := map[string]int{
      "alice":   31,
      "charlie": 34,
  }
  ```

* ```go
  //另一种初始化方法,注意这种方法创建的是空的map，nil：
  map[string]int{}
  ```

* 基本操作：

  ```go
  //查看
  ages["alice"] = 12
  // 删除
  delete(ages, "alice")
  // 
  ```

* 但是map中的元素并不是一个变量，因此我们不能对map的元素进行取址操作,愿意是可能会rehash

* 遍历：

  ```go
  for name,age := range ages {
      fmt.Printf("%s\t%d\n", name, age)
  }
  ```

* 但是向一个nil值的map存入元素将导致一个panic异常：

  ```go
  ages["carol"] = 21 // panic: assignment to entry in nil map
  ```

* 这个规则很实用，但是有时候可能需要知道对应的元素是否真的是在map之中。例如，如果元素类型是一个数字，你可能需要区分一个已经存在的0，和不存在而返回零值的0，可以像下面这样测试：

  ```go
  age, ok := ages["bob"]
  if !ok { /* "bob" is not a key in this map; age == 0. */ }
  ```

  在这种场景下，map的下标语法将产生两个值；第二个是一个布尔值，用于报告元素是否真的存在。布尔变量一般命名为ok，特别适合马上用于if条件判断部分。

* 是的，GO中没有原生的set类型，需要时，活用go就好





### 结构体

* Go语言有一个特性让我们只声明一个成员对应的数据类型而不指名成员的名字；这类成员就叫匿名成员。匿名成员的数据类型必须是命名的类型或指向一个命名的类型的指针。

* 下面的代码中，Circle和Wheel各自都有一个匿名成员。我们可以说Point类型被嵌入到了Circle结构体，同时Circle类型被嵌入到了Wheel结构体。

  ```go
  type Circle struct {
      Point
      Radius int
  }
  
  type Wheel struct {
      Circle
      Spokes int
  }
  ```

  得益于匿名嵌入的特性，我们可以直接访问叶子属性而不需要给出完整的路径：

  ```go
  var w Wheel
  w.X = 8            // equivalent to w.Circle.Point.X = 8
  w.Y = 8            // equivalent to w.Circle.Point.Y = 8
  w.Radius = 5       // equivalent to w.Circle.Radius = 5
  w.Spokes = 20
  ```

  

* 因为匿名成员也有一个隐式的名字，因此不能同时包含两个类型相同的匿名成员，这会导致名字冲突。



### JSON

* json编码：

  ```go
  data, err := json.Marshal(movies)
  if err != nil {
      log.Fatalf("JSON marshaling failed: %s", err)
  }
  fmt.Printf("%s\n", data)
  ```

* json编码： 可阅读性

  ```go
  data, err := json.MarshalIndent(movies, "", "    ") // 后面两个参数，分别是每一行的前缀缩进，和每一个层的缩进
  if err != nil {
      log.Fatalf("JSON marshaling failed: %s", err)
  }
  fmt.Printf("%s\n", data)
  ```

* 一个结构体成员Tag是和在编译阶段关联到该成员的元信息字符串：

```go
Year  int  `json:"released"`
Color bool `json:"color,omitempty"`
```

* 解码：

  通过定义合适的Go语言数据结构，我们可以选择性地解码JSON中感兴趣的成员。当Unmarshal函数调用返回，slice将被只含有Title信息的值填充，其它JSON成员将被忽略。

  ```go
  var titles []struct{ Title string }
  if err := json.Unmarshal(data, &titles); err != nil {
      log.Fatalf("JSON unmarshaling failed: %s", err)
  }
  fmt.Println(titles) // "[{Casablanca} {Cool Hand Luke} {Bullitt}]"
  ```

  

## 函数

### 函数声明

* 函数声明包括函数名、形式参数列表、返回值列表（可省略）以及函数体。

  ```go
  func name(parameter-list) (result-list) {
      body
  }
  ```

  

* 函数的类型被称为函数的签名。如果两个函数形式参数列表和返回值列表中的变量类型一一对应，那么这两个函数被认为有相同的类型或签名。形参和返回值的变量名不影响函数签名，也不影响它们是否可以以省略参数类型的形式表示。
* 每一次函数调用都必须按照声明顺序为所有参数提供实参（参数值）。在函数调用时，Go语言没有默认参数值，也没有任何方法可以通过参数名指定形参，因此形参和返回值的变量名对于函数调用者而言没有意义。
* 实参通过值的方式传递，因此函数的形参是实参的拷贝。对形参进行修改不会影响实参。但是，如果实参包括引用类型，如指针，slice(切片)、map、function、channel等类型，实参可能会由于函数的间接引用被修改。







### 多返回值

* 在Go中，一个函数可以返回多个值

* 如果一个函数所有的返回值都有显式的变量名，那么该函数的return语句可以省略操作数。这称之为bare return。

  ```go
  func CountWordsAndImages(url string) (words, images int, err error) {
      resp, err := http.Get(url)
      if err != nil {
          return
      }
      doc, err := html.Parse(resp.Body)
      resp.Body.Close()
      if err != nil {
          err = fmt.Errorf("parsing HTML: %s", err)
          return
      }
      words, images = countWordsAndImages(doc)
      return
  }
  func countWordsAndImages(n *html.Node) (words, images int) { /* ... */ }
  ```

  ```go
  // 上面的每一个返回都相当于：
  return words, images, err
  ```

  

* 错误处理策略：

  * 传播错误：

    ```go
    resp, err := http.Get(url)
    if err != nil{
        return nil, err
    }
    ```

    优化：构造新的更详细的信息：

    ```go
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
        return nil, fmt.Errorf("parsing %s as HTML: %v", url,err)
    }
    ```

  * 重试：

    让我们来看看处理错误的第二种策略。如果错误的发生是偶然性的，或由不可预知的问题导致的。一个明智的选择是重新尝试失败的操作。在重试时，我们需要限制重试的时间间隔或重试的次数，防止无限制的重试。

  * 输出错误信息并结束程序。需要注意的是，这种策略只应在main中执行。对库函数而言，应仅向上传播错误，除非该错误意味着程序内部包含不一致性，即遇到了bug，才能在库函数中结束程序。

  * 有时，我们只需要输出错误信息就足够了，不需要中断程序的运行。我们可以通过log包提供函数

  * 可以直接忽略掉错误。



### 函数值

* 函数被看作第一类值（first-class values） 和python 有点像。函数像其他值一样，拥有类型，可以被赋值给其他变量，传递给函数，从函数返回。
* 





### 匿名函数

* 拥有函数名的函数只能在包级语法块中被声明，通过函数字面量（function literal），我们可绕过这一限制，在任何表达式中表示一个函数值。函数字面量的语法和函数声明相似，区别在于func关键字后没有函数名。函数值字面量是一种表达式，它的值被称为匿名函数（anonymous function）

  ```go
  strings.Map(func(r rune) rune { return r + 1 }, "HAL-9000")
  ```

* squares的例子证明，函数值不仅仅是一串代码，还记录了状态。在squares中定义的匿名内部函数可以访问和更新squares中的局部变量，这意味着匿名函数和squares中，存在变量引用。这就是函数值属于引用类型和函数值不可比较的原因。Go使用闭包（closures）技术实现函数值，Go程序员也把函数值叫做闭包。

* 通过这个例子，我们看到变量的生命周期不由它的作用域决定：squares返回后，变量x仍然隐式的存在于f中。





### Deferred函数

* 你只需要在调用普通函数或方法前加上关键字defer，就完成了defer所需要的语法。当执行到该条语句时，函数和参数表达式得到计算，但直到包含该defer语句的函数执行完毕时，defer后的函数才会被执行，不论包含defer语句的函数是通过return正常结束，还是由于panic导致的异常结束。你可以在一个函数中执行多条defer语句，它们的执行顺序与声明顺序相反。
* defer语句经常被用于处理成对的操作，如打开、关闭、连接、断开连接、加锁、释放锁。通过defer机制，不论函数逻辑多复杂，都能保证在任何执行路径下，资源被释放。释放资源的defer应该直接跟在请求资源的语句后。在下面的代码中，一条defer语句替代了之前的所有resp.Body.Close
* 通过这种方式， 我们可以只通过一条语句控制函数的入口和所有的出口，甚至可以记录函数的运行时间，如例子中的start。需要注意一点：不要忘记defer语句后的圆括号，否则本该在进入时执行的操作会在退出时执行，而本该在退出时执行的，永远不会被执行。
* 在循环体中的defer语句需要特别注意，因为只有在函数执行完毕后，这些被延迟的函数才会执行.





### Panic异常

* Go的类型系统会在编译时捕获很多错误，但有些错误只能在运行时检查，如数组访问越界、空指针引用等。这些运行时错误会引起painc异常。
* 一般而言，当panic异常发生时，程序会中断运行，并立即执行在该goroutine（可以先理解成线程，在第8章会详细介绍）中被延迟的函数（defer 机制）。随后，程序崩溃并输出日志信息。

```go
switch s := suit(drawCard()); s {
case "Spades":                                // ...
case "Hearts":                                // ...
case "Diamonds":                              // ...
case "Clubs":                                 // ...
default:
    panic(fmt.Sprintf("invalid suit %q", s)) // Joker?
}
```

* 将panic机制类比其他语言异常机制的读者可能会惊讶，runtime.Stack为何能输出已经被释放函数的信息？在Go的panic机制中，延迟函数的调用在释放堆栈信息之前。
* 





### Recover捕获异常

* 如果在deferred函数中调用了内置函数recover，并且定义该defer语句的函数发生了panic异常，recover会使程序从panic中恢复，并返回panic value。导致panic异常的函数不会继续运行，但能正常返回。在未发生panic时调用recover，recover会返回nil。

  ```go
  func Parse(input string) (s *Syntax, err error) {
      defer func() {
          if p := recover(); p != nil {
              err = fmt.Errorf("internal error: %v", p)
          }
      }()
      // ...parser...
  }
  ```

* deferred函数帮助Parse从panic中恢复。在deferred函数内部，panic value被附加到错误信息中；并用err变量接收错误信息，返回给调用者。我们也可以通过调用runtime.Stack往错误信息中添加完整的堆栈调用信息。











## 方法

### 方法声明

* 在函数声明时，在其名字之前放上一个变量，即是一个方法。这个附加的参数会将该函数附加到这种类型上，即相当于为这种类型定义了一个独占的方法。

  ```go
  package geometry
  
  import "math"
  
  type Point struct{ X, Y float64 }
  
  // traditional function
  func Distance(p, q Point) float64 {
      return math.Hypot(q.X-p.X, q.Y-p.Y)
  }
  
  // same thing, but as a method of the Point type
  func (p Point) Distance(q Point) float64 {
      return math.Hypot(q.X-p.X, q.Y-p.Y)
  }
  ```

  

* 我们可以给同一个包内的任意命名类型定义方法，只要这个命名类型的底层类型（译注：这个例子里，底层类型是指[]Point这个slice，Path就是命名类型）不是指针或者interface。









### 基于指针对象的方法

* 当调用一个函数时，会对其每一个参数值进行拷贝，如果一个函数需要更新一个变量，或者函数的其中一个参数实在太大我们希望能够避免进行这种默认的拷贝，这种情况下我们就需要用到指针了。
* 在现实的程序里，一般会约定如果Point这个类有一个指针作为接收器的方法，那么所有Point的方法都必须有一个指针接收器，即使是那些并不需要这个指针接收器的函数。我们在这里打破了这个约定只是为了展示一下两种方法的异同而已。
* 只有类型（Point）和指向他们的指针`(*Point)`，才可能是出现在接收器声明里的两种接收器。此外，为了避免歧义，在声明方法时，如果一个类型名本身是一个指针的话，是不允许其出现在接收器中的，
* 不管你的method的receiver是指针类型还是非指针类型，都是可以通过指针/非指针类型进行调用的，编译器会帮你做类型转换。
* 在声明一个method的receiver该是指针还是非指针类型时，你需要考虑两方面的因素，第一方面是这个对象本身是不是特别大，如果声明为非指针变量时，调用会产生一次拷贝；第二方面是如果你用指针类型作为receiver，那么你一定要注意，这种指针类型指向的始终是一块内存地址，就算你对其进行了拷贝。熟悉C或者C++的人这里应该很快能明白。
* 就像一些函数允许nil指针作为参数一样，方法理论上也可以用nil指针作为其接收器，尤其当nil对于对象来说是合法的零值时，比如map或者slice。

### 通过嵌入结构体来扩展类型

* 内嵌的结构，对于主体结构而言，可以视为自己的属性
* 内嵌结构的方法，可以将主体结构类型作为接收机使用，调用内嵌结构的方法。





## 接口

* 很多面向对象的语言都有相似的接口概念，但Go语言中接口类型的独特之处在于它是满足隐式实现的。也就是说，我们没有必要对于给定的具体类型定义所有满足的接口类型；简单地拥有一些必需的方法就足够了。这种设计可以让你创建一个新的接口类型满足已经存在的具体类型却不会去改变这些类型的定义；当我们使用的类型来自于不受我们控制的包时这种设计尤其有用。

### 接口约定

* 接口类型是一种抽象的类型。它不会暴露出它所代表的对象的内部值的结构和这个对象支持的基础操作的集合；它们只会表现出它们自己的方法。也就是说当你有看到一个接口类型的值时，你不知道它是什么，唯一知道的就是可以通过它的方法来做什么。

###  接口类型

* 接口类型具体描述了一系列方法的集合，一个实现了这些方法的具体类型是这个接口类型的实例。
* 我们可以用这种方式以一个简写命名一个接口，而不用声明它所有的方法。这种方式称为接口内嵌。





###  实现接口的条件

* 一个类型如果拥有一个接口需要的所有方法，那么这个类型就实现了这个接口。
* 接口指定的规则非常简单：表达一个类型属于某个接口只要这个类型实现这个接口。
* 这看上去好像没有用，但实际上interface{}被称为空接口类型是不可或缺的。因为空接口类型对实现它的类型没有要求，所以我们可以将任意一个值赋给空接口类型。

* 每一个具体类型的组基于它们相同的行为可以表示成一个接口类型。不像基于类的语言，他们一个类实现的接口集合需要进行显式的定义，在Go语言中我们可以在需要的时候定义一个新的抽象或者特定特点的组，而不需要修改具体类型的定义。当具体的类型来自不同的作者时这种方式会特别有用。当然也确实没有必要在具体的类型中指出这些共性。







###  接口值

* 概念上讲一个接口的值，接口值，由两个部分组成，一个具体的类型和那个类型的值。它们被称为接口的动态类型和动态值。

* 在Go语言中，变量总是被一个定义明确的值初始化，即使接口类型也不例外。对于一个接口的零值就是它的类型和值的部分都是nil

* 一个接口值基于它的动态类型被描述为空或非空，所以这是一个空的接口值。你可以通过使用w==nil或者w!=nil来判断接口值是否为空。调用一个空接口值上的任意方法都会产生panic:

  ```go
  w.Write([]byte("hello")) // panic: nil pointer dereference
  ```

* 通常在编译期，我们不知道接口值的动态类型是什么，所以一个接口上的调用必须使用动态分配。因为不是直接进行调用，所以编译器必须把代码生成在类型描述符的方法Write上，然后间接调用那个地址。

* 一个接口值可以持有任意大的动态值。

* 结果可能和图7.4相似。从概念上讲，不论接口值多大，动态值总是可以容下它。

* 接口值可以使用==和!＝来进行比较。两个接口值相等仅当它们都是nil值，或者它们的动态类型相同并且动态值也根据这个动态类型的==操作相等。因为接口值是可比较的，所以它们可以用在map的键或者作为switch语句的操作数。

* **一个不包含任何值的nil接口值和一个刚好包含nil指针的接口值是不同的。这个细微区别产生了一个容易绊倒每个Go程序员的陷阱。**



### sort.Interface接口

* 幸运的是，sort包内置的提供了根据一些排序函数来对任何序列排序的功能。它的设计非常独到。在很多语言中，排序算法都是和序列数据类型关联，同时排序函数和具体类型元素关联。相比之下，Go语言的sort.Sort函数不会对具体的序列和它的元素做任何假设。相反，它使用了一个接口类型sort.Interface来指定通用的排序算法和可能被排序到的序列类型之间的约定。这个接口的实现由序列的具体表示和它希望排序的元素决定，序列的表示经常是一个切片。



### 类型断言

* 类型断言是一个使用在接口值上的操作。语法上它看起来像x.(T)被称为断言类型，这里x表示一个接口的类型和T表示一个类型。一个类型断言检查它操作对象的动态类型是否和断言的类型匹配。

* 这里有两种可能。第一种，如果断言的类型T是一个具体类型，然后类型断言检查x的动态类型是否和T相同。如果这个检查成功了，类型断言的结果是x的动态值，当然它的类型是T。换句话说，具体类型的类型断言从它的操作对象中获得具体的值。

  ```go
  var w io.Writer
  w = os.Stdout
  f := w.(*os.File)      // success: f == os.Stdout
  c := w.(*bytes.Buffer) // panic: interface holds *os.File, not *bytes.Buffer
  ```

* 第二种，如果相反地断言的类型T是一个接口类型，然后类型断言检查是否x的动态类型满足T。如果这个检查成功了，动态值没有获取到；这个结果仍然是一个有相同动态类型和值部分的接口值，但是结果为类型T。换句话说，对一个接口类型的类型断言改变了类型的表述方式，改变了可以获取的方法集合（通常更大），但是它保留了接口值内部的动态类型和值的部分。

  ```go
  var w io.Writer
  w = os.Stdout
  rw := w.(io.ReadWriter) // success: *os.File has both Read and Write
  w = new(ByteCounter)
  rw = w.(io.ReadWriter) // panic: *ByteCounter has no Read method
  ```

  



### 类型分支

* 接口被以两种不同的方式使用。在第一个方式中，以io.Reader，io.Writer，fmt.Stringer，sort.Interface，http.Handler和error为典型，一个接口的方法表达了实现这个接口的具体类型间的相似性，但是隐藏了代码的细节和这些具体类型本身的操作。重点在于方法上，而不是具体的类型上。
* 第二个方式是利用一个接口值可以持有各种具体类型值的能力，将这个接口认为是这些类型的联合。类型断言用来动态地区别这些类型，使得对每一种情况都不一样。在这个方式中，重点在于具体的类型满足这个接口，而不在于接口的方法（如果它确实有一些的话），并且没有任何的信息隐藏。我们将以这种方式使用的接口描述为discriminated unions（可辨识联合）。

* 在最简单的形式中，一个类型分支像普通的switch语句一样，它的运算对象是x.(type)——它使用了关键词字面量type——并且每个case有一到多个类型。一个类型分支基于这个接口值的动态类型使一个多路分支有效。这个nil的case和if x == nil匹配，并且这个default的case和如果其它case都不匹配的情况匹配。

  ```go
  switch x.(type) {
  case nil:       // ...
  case int, uint: // ...
  case bool:      // ...
  case string:    // ...
  default:        // ...
  }
  ```

## Goroutines和Channels

### Goroutines

* 当一个程序启动时，其主函数即在一个单独的goroutine中运行，我们叫它main goroutine。新的goroutine会用go语句来创建。在语法上，go语句是一个普通的函数或方法调用前加上关键字go。go语句会使其语句中的函数在一个新创建的goroutine中运行。而go语句本身会迅速地完成。

  ```go
  f()    // call f(); wait for it to return
  go f() // create a new goroutine that calls f(); don't wait
  ```

* 主函数返回时，所有的goroutine都会被直接打断，程序退出。除了从主函数退出或者直接终止程序之外，没有其它的编程方法能够让一个goroutine来打断另一个的执行，但是之后可以看到一种方式来实现这个目的，通过goroutine之间的通信来让一个goroutine请求其它的goroutine，并让被请求的goroutine自行结束执行。





### Channels

* 如果说goroutine是Go语言程序的并发体的话，那么channels则是它们之间的通信机制。

* 一个channel是一个通信机制，它可以让一个goroutine通过它给另一个goroutine发送值信息。每个channel都有一个特殊的类型，也就是channels可发送数据的类型。一个可以发送int类型数据的channel一般写为chan int。

* 使用内置的make函数，我们可以创建一个channel：

  ```go
  ch := make(chan int) // ch has type 'chan int'
  ```

  和map类似，channel也对应一个make创建的底层数据结构的引用。当我们复制一个channel或用于函数参数传递时，我们只是拷贝了一个channel引用，因此调用者和被调用者将引用同一个channel对象。和其它的引用类型一样，channel的零值也是nil。

* 两个相同类型的channel可以使用==运算符比较。如果两个channel引用的是相同的对象，那么比较的结果为真。一个channel也可以和nil进行比较。

* 一个channel有发送和接受两个主要操作，都是通信行为。一个发送语句将一个值从一个goroutine通过channel发送到另一个执行接收操作的goroutine。发送和接收两个操作都使用`<-`运算符。在发送语句中，`<-`运算符分割channel和要发送的值。在接收语句中，`<-`运算符写在channel对象之前。一个不使用接收结果的接收操作也是合法的。

  ```go
  ch <- x  // a send statement
  x = <-ch // a receive expression in an assignment statement
  <-ch     // a receive statement; result is discarded
  ```

* Channel还支持close操作，用于关闭channel，随后对基于该channel的任何发送操作都将导致panic异常。对一个已经被close过的channel进行接收操作依然可以接受到之前已经成功发送的数据；如果channel中已经没有数据的话将产生一个零值的数据。

  ```go
  close(ch)
  ```

* 以最简单方式调用make函数创建的是一个无缓存的channel，但是我们也可以指定第二个整型参数，对应channel的容量。如果channel的容量大于零，那么该channel就是带缓存的channel。

  ```go
  ch = make(chan int)    // unbuffered channel
  ch = make(chan int, 0) // unbuffered channel
  ch = make(chan int, 3) // buffered channel with capacity 3
  ```

* 当一个channel被关闭后，再向该channel发送数据将导致panic异常。当一个被关闭的channel中已经发送的数据都被成功接收后，后续的接收操作将不再阻塞，它们会立即返回一个零值。关闭上面例子中的naturals变量对应的channel并不能终止循环，它依然会收到一个永无休止的零值序列，然后将它们发送给打印者goroutine。

  没有办法直接测试一个channel是否被关闭，但是接收操作有一个变体形式：它多接收一个结果，多接收的第二个结果是一个布尔值ok，ture表示成功从channels接收到值，false表示channels已经被关闭并且里面没有值可接收。使用这个特性，我们可以修改squarer函数中的循环代码，当naturals对应的channel被关闭并没有值可接收时跳出循环，并且也关闭squares对应的channel.

  ```go
  // Squarer
  go func() {
      for {
          x, ok := <-naturals
          if !ok {
              break // channel was closed and drained
          }
          squares <- x * x
      }
      close(squares)
  }()
  ```

* 在下面的改进中，我们的计数器goroutine只生成100个含数字的序列，然后关闭naturals对应的channel，这将导致计算平方数的squarer对应的goroutine可以正常终止循环并关闭squares对应的channel。（在一个更复杂的程序中，可以通过defer语句关闭对应的channel。）最后，主goroutine也可以正常终止循环并退出程序。

* 其实你并不需要关闭每一个channel。只有当需要告诉接收者goroutine，所有的数据已经全部发送时才需要关闭channel。不管一个channel是否被关闭，当它没有被引用时将会被Go语言的垃圾自动回收器回收。

* 为了表明这种意图并防止被滥用，Go语言的类型系统提供了单方向的channel类型，分别用于只发送或只接收的channel。类型`chan<- int`表示一个只发送int的channel，只能发送不能接收。相反，类型`<-chan int`表示一个只接收int的channel，只能接收不能发送。（箭头`<-`和关键字chan的相对位置表明了channel的方向。）这种限制将在编译期检测。

* 调用counter（naturals）时，naturals的类型将隐式地从chan int转换成chan<- int。





## 基于共享变量的并发

### 竞争条件

* 我们来重复一下数据竞争的定义，因为实在太重要了：数据竞争会在两个以上的goroutine并发访问相同的变量且至少其中一个为写操作时发生。根据上述定义，有三种方式可以避免数据竞争：

  * 第一种方法是不要去写变量

  * 第二种避免数据竞争的方法是，避免从多个goroutine访问变量。某个变量绑定一个goroutine,其他goroutine需要的时候，使用channel 通信

  * 第三种避免数据竞争的方法是允许很多goroutine去访问变量，但是在同一个时刻最多只有一个goroutine在访问。这种方式被称为“互斥”

  * 这种互斥很实用，而且被sync包里的Mutex类型直接支持。它的Lock方法能够获取到token(这里叫锁)，并且Unlock方法会释放这个token：

    ```go
    import "sync"
    
    var (
        mu      sync.Mutex // guards balance
        balance int
    )
    
    func Deposit(amount int) {
        mu.Lock()
        balance = balance + amount
        mu.Unlock()
    }
    
    func Balance() int {
        mu.Lock()
        b := balance
        mu.Unlock()
        return b
    }
    ```

    

## 测试

### go test

* go test命令是一个按照一定的约定和组织来测试代码的程序。在包目录内，所有以`_test.go`为后缀名的源文件在执行go build时不会被构建成包的一部分，它们是go test测试的一部分。

  在`*_test.go`文件中，有三种类型的函数：测试函数、基准测试（benchmark）函数、示例函数。一个测试函数是以Test为函数名前缀的函数，用于测试程序的一些逻辑行为是否正确；go test命令会调用这些测试函数并报告测试结果是PASS或FAIL。基准测试函数是以Benchmark为函数名前缀的函数，它们用于衡量一些函数的性能；go test命令会多次运行基准测试函数以计算一个平均的执行时间。示例函数是以Example为函数名前缀的函数，提供一个由编译器保证正确性的示例文档。我们将在11.2节讨论测试函数的所有细节，并在11.4节讨论基准测试函数的细节，然后在11.6节讨论示例函数的细节。