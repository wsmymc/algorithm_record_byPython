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

