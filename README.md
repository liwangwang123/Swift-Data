# Swift-Data
Swift 数据结构与算法

# Swift 标准库

在深入阅读本书其余部分之前，您将首先了解Swift语言中包含的一些数据结构。*Swift*标准库是指定义Swift语言核心组件的框架。在里面，你会发现各种各样的工具和类型来帮助你构建你的Swift应用程序。
在本章中，您将关注标准库提供的两种开箱即用的数据结构:**Array**和**Dictionary**。

## Array

数组是用于存储元素集合的通用容器，通常用于各种*Swift*程序中。您可以使用数组创建文字数组，它是一个逗号分隔的值列表，周围用方括号括起来。例如:

`let perple = [“Brian”, “Stanley”, “Ringo”]`

这是一个泛型类型，因为它对任何元素类型进行抽象。元素类型没有正式的要求;它可以是任何东西。在上面的例子中，编译器类型推断元素是字符串类型。

*Swift*使用协议定义数组。这些协议中的每一个都在数组上增加了更多的功能。例如，数组是一个序列，这意味着您至少可以遍历它一次。它也是一个集合，这意味着它可以被多次遍历，不受破坏，并且可以使用下标操作符访问它。Array也是一个随机存取集合，它保证了效率。
例如，*count*属性被保证是一个“廉价的”写为O(1)的常量时间操作。这意味着无论数组变得多大，计算*count*总是需要相同的时间。

数组对于元素的排序也很有用。

### 命令
数组中的元素是显式有序的。使用上述 *perple* 数组。例如，"*Brian*"在"*Stanley*"之前。
数组中的所有元素都有对应的从零开始的整数索引。例如，上面例子中的*people*数组有三个索引，一个对应于每个元素。

您可以通过编写以下代码来检索数组中某个元素的值:
```
perple[0]
perple[1]
```
顺序是由数组数据结构定义的，不应视为理所当然。一些数据结构，如**Dictionary**，有较弱的顺序概念。

### 随机存取
如果数据结构能够在固定的O(1)时间内处理元素检索，那么它就可以声明为随机访问。例如，从*people*数组中获取“*Ringo*”需要常数时间。同样，这种表现也不应视为理所当然。其他数据结构，如链表和树，没有固定的时间访问。

数组的性能
除了作为一个随机访问集合之外，还有其他一些性能领域是开发人员感兴趣的，特别是当数据结构中包含的数据量需要增长时，数据结构的性能如何?对于数组，这取决于两个因素。

### 插入的位置
第一个因素是选择在数组中插入新元素的位置。向数组中添加元素的最有效场景是在数组的末尾追加元素:

`perple.append(“Charles”)`

使用append方法插入“*Charles*”将把字符串放在数组的末尾。这是一个常量时间操作，是向数组中添加元素的最有效方法。但是，有时可能需要在特定位置插入一个元素，比如在数组的正中央。在这种情况下，这是一个O(n)操作。

为了帮助说明为什么会出现这种情况，请考虑下面的类比。你在排队去剧院。又来了一个新人，加入了队伍。什么地方最容易把人加入阵容?
当然是在最后!
如果这个新来的球员想插到队伍中间，他们就必须说服一半的球员重新洗牌以腾出位置。
如果他非常粗鲁，他会试图排在最前面。这是最坏的情况，因为队列中的每个人都需要重新洗牌，为前面的新成员腾出空间!

这就是数组的工作方式。从数组末尾以外的任何位置插入新元素都会迫使元素向后移动，为新元素腾出空间:

`perple.insert(“Andy”, at:0)`

准确地说，每个元素必须向后移动一个索引，这需要*n*个步骤。正因为如此，它被认为是一个线性时间或O(n)操作。如果数组中的元素数量翻倍，则此插入操作所需的时间也将翻倍。

如果在集合前插入元素是程序的常见操作，则可能需要考虑使用不同的数据结构来保存数据。

决定插入速度的第二个因素是数组的容量。如果超过数组预先分配的空间(容量)，则必须重新分配存储并复制当前元素。这意味着，如果复制完成，任何给定的插入，即使是在最后，也可能需要n个步骤来完成。然而，标准库使用了一个微妙的技巧来防止追加时间为O(n)。每当它耗尽存储空间并需要复制时，它的容量就会加倍。这样做使得数组的平摊代价仍然是常数时间*O(1)*。

## Dictionary
**dictionary**是另一个包含键值对的泛型集合。例如，这里有一个字典包含用户的名字和他们的分数:

`var scores: [String: Int] = [“Eric”:9, “Mark”:12, “Wayne”: 1]`

字典没有任何顺序概念，也不能插入特定索引。他们还对钥匙类型提出了一个要求，它是可*Hashable*的。幸运的是，几乎所有的标准类型都已经具备了可*Hashable*性，在最新的*Swift*版本中，采用可*Hashable*协议现在变得很简单。
您可以使用以下语法向字典添加一个新条目:

`scores["Andrew"] = 0`

这将在字典中创建一个新的键值对:

`["Eric": 9, "Mark": 12, "Andrew": 0, "Wayne": 1]`

“*Andrew*”键被插入字典的某个地方。字典是无序的，所以你不能保证新条目会放在哪里。可以根据集合协议多次遍历字典的键值。这个顺序虽然没有定义，但是每次遍历它时都是相同的，直到集合被更改(突变)。
无序的劣势带来了一些优点。与数组不同，字典不需要担心元素的变化。字典插入元素总是O(1)。查找操作也在O(1)时间内完成，这比数组中特定元素的O(n)查找时间快得多。

## 从这里到哪里?

本章将介绍Swift中最常见的两种数据结构，并简要介绍两者之间的一些权衡。本书的其余部分将介绍其他具有独特性能特征的数据结构，这些数据结构在某些方面具有优势
场景。令人惊讶的是，使用这两个简单的标准库基础可以高效地构建许多其他数据结构。

### 链表
链表是按线性单向序列排列的值的集合。链表相对于连续存储选项(如Swift Array)有几个理论上的优势:
* 从列表的前面插入和删除需常数时间。
* 可靠的性能特征。

![链表](https://github.com/liwangwang123/Swift-Data/blob/master/images/B5B3CAB1-DB73-4374-9358-84C41883C1F0.png)

如图所示，链表是节点链。节点有两个职责:
* 持有一个值。
* 保存对下一个节点的引用。空值表示列表的末尾。

![链表](https://github.com/liwangwang123/Swift-Data/blob/master/images/4242962A-C494-4D14-9994-84005DD34261.png)

## 节点
在*Sources*目录中创建一个新的Swift文件，并将其命名为*Node.swift*。将以下内容添加到文件中:
```
public class Node<Value> {
  public var value: Value public var next: Node?
  public init(value: Value, next: Node? = nil) { 
    self.value = value
    self.next = next
  }
}


extension Node: CustomStringConvertible { 
  public var description: String {
    guard let next = next else { 
      return "\(value)"
    }
    return "\(value) -> " + String(describing: next) + " "
  }
}
```

导航到playground页面，并加入以下内容:
```
example(of: "creating and linking nodes") { 
  let node1 = Node(value: 1)
  let node2 = Node(value: 2) 
  let node3 = Node(value: 3)
  
  node1.next = node2 
  node2.next = node3
 
  print(node1)
}
```

您刚刚创建了三个节点并将它们连接起来:

![链表](https://github.com/liwangwang123/Swift-Data/blob/master/images/7FE5712A-9E5E-4130-A12A-C8C0C432AED3.png)

在控制台中，您应该看到以下输出:

---Example of creating and linking nodes--- 1 -> 2 -> 3

就实用性而言，目前的构建列表的方法还有很多需要改进的地方。您可以很容易地看到，以这种方式构建长列表是不切实际的。缓解这个问题的一种常见方法是构建一个管理节点对象的LinkedList。你会那样做的!


### LinkedList

在Sources目录中，创建一个新文件，并将其命名为LinkedList.swift。将以下内容添加到文件中:
```
public struct LinkedList<Value> {
  public var head: Node<Value>? 
  public var tail: Node<Value>?
  public init() {}
  public var isEmpty: Bool { 
    return head == nil
  }
}

extension LinkedList: CustomStringConvertible { 
  public var description: String {
    guard let head = head else { 
      return "Empty list"
    }
   return String(describing: head)
  }
}
```
链表有头和尾的概念，分别指链表的第一个节点和最后一个节点:
![列表](https://github.com/liwangwang123/Swift-Data/blob/master/images/703CEF39-5D73-49C8-9002-115A23C61EE4.png)

### 向列表添加值
如前所述，您将提供一个接口来管理节点对象。首先要处理的是添加值。向链表添加值有三种方法，每种方法都有自己独特的性能特征:

1. push: 在列表的前面添加一个值。
2. append: 在列表末尾添加一个值。
3. insert(after:): 在列表的特定节点之后添加值。

在下一节中，您将实现每一个功能，并分析它们的性能特征。

### push

在列表的前面添加值称为 *push* 操作。这也被称为头先插入。它的代码非常简单。

在LinkedList文件中添加以下方法:
```
public mutating func push(_ value: Value) {
  head = Node(value: value, next: head) 
  if tail == nil {
    tail = head
  }
}
```
在 push 进空列表的情况下，新节点同时拥有列表的头和尾。
回到 *playground* 页面，添加以下内容:
```
example(of: "push") {
  var list = LinkedList<Int>() 
  list.push(3)
  list.push(2) 
  list.push(1)
  print(list)
}
```

您的控制台输出应该显示以下内容:
`1->2->3  `

### append
您将看到的下一个操作是 *append*。这意味着在列表末尾添加一个值，称为尾端插入。
回到LinkedList.swift，在下面添加以下代码 *push*:
```
public mutating func append(_ value: Value) {
  // 1
  guard !isEmpty else { 
    push(value)
    return
  }
  // 2
  tail!.next = Node(value: value)
  // 3
  tail = tail!.next
}
```

这段代码相对简单:

1. 与之前一样，如果列表为空，则需要将head和tail更新到新节点。由于在空列表中 *append* 与 *push* 在功能上是相同的，所以只需调用 *push* 就可以为你完成这项工作。
2. 在所有其他情况下，只需在尾节点之后创建一个新节点。强制展开保证成功，因为您使用上面的保护语句在 *isEmpty* 情况下进行了 *push* 操作。
3. 由于这是尾端插入，所以新节点也是列表的尾节点。

回到 *playground* ，在底部添加以下内容:
```
example(of: "append") {
  var list = LinkedList<Int>() 
  list.append(1) 
  list.append(2) 
  list.append(3)
  print(list)
}
```

会在控制台中看到以下输出:
`1->2->3`

### insert(after:)

添加值的第三个也是最后一个操作是 `insert(after:)`。该操作在列表的特定位置插入一个值，需要两个步骤:
1. 在列表中找到特定节点。
2. 插入新节点。

首先，实现代码来查找要插入值的节点。
回到LinkedList.swift文件，在 `append` 下面添加以下代码:
```
public func node(at index: Int) -> Node<Value>? {
  // 1
  var currentNode = head
  var currentIndex = 0
  // 2
  while currentNode != nil && currentIndex < index {
    currentNode = currentNode!.next
    currentIndex += 1
  }
  return currentNode
}
```
