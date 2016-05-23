---
layout: post
title: "[SP-6] Learning Swift"
categories: 
    - project
tags: 
    - swift
published: true
---

I want to learn a bit about Swift iOS programming in case I 
need to handle with the mobile computing. It is always good 
to have the ability to make some mobile apps. 

I will record some links of learning resources and maybe I 
will write down some comments. 


[1]   Bret Victor - "[Inventing on Principle](https://vimeo.com/36579366)"

**My Comments:** You can see the design principle of Swift 
from this talk. Wow! The binary search example surprises me 
a lot, which is an excellent demo. 

[2]   A Swift Tour in "The Swift Programming Language"

**My Comments:** For me, the whole idea is similar to iPython
for interactive programming, which is very popular to embed
code into an article. But Swift's playground is a real-time
interpreter. A drawback of this is that there is a small 
delay to wait for showing the result. And Swift combines 
a lot features from different programming languages. You 
may feel quite familiar about its syntax and grammar. 

[3]   [Stanford - Developing iOS 8 Apps with Swift [2015]](https://www.youtube.com/playlist?list=PLy7oRd3ashWodnpf8rjfYEkTgwbOEsKfU)

**My Comments:** I found this course in Itunes U and 
this is what I need. 

[4]   [A tutorial for developing a Tetris game](https://www.bloc.io/tutorials/swiftris-build-your-first-ios-game-with-swift#!/chapters/675)

**My Comments:** A very cool game. This tutorial is great, which not only
teaches you how to design this game, but also links related knowledge of 
Swift. 

[5]   [Swift Education](http://swifteducation.github.io/)

**My Comments:** It is recommended by the official website. It aims to 
guide teachers to prepare for their classes in Swift app development.
The website has many cool projects that contain all necessary stuff,
e.g., code and spec, for a class. 

All following examples come from [2]: 

**A Swift Tour**

*   [Simple Values](#sec01)
*   [Control Flow](#sec02)
*   [Functions and Closures](#sec03)
*   [Objects and Classes](#sec04)
*   [Enumerations and Structures](#sec05)
*   [Protocols and Extensions](#sec06)
*   [Generics](#sec07)


#### <a name="sec01"></a>Simple Values

{% highlight csharp %}
println("Hello, world!")  // print a line

// variable and constant
var myVariable = 42
myVariable = 50
let myConstant = 42

// type inference
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70

// no implicitly casting, needs explicitly casting
let label = "The width is "
let width = 94
// if remove '+', you will get an error that binary 
// operator cannot be applied to 'string' and 'int'
let labelWidth = label + String(width)

// \(item) convert to string
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples"
let fruitSummary = "I have \(apples + oranges) pieces of fruit."

// [] for array and dictionary
var shoppingList = ["catfish", "water", "tulips", "blue paint"]
shoppingList[1] = "bottle of water"

var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"

// initializer to create empty array and dictionary
let emptyArray = [String]()
let emptyDictionary = Dictionary<String, Float>()

// or, use [] and [:]
let emptyArray = []
let emptyDictionary = [:]

{% endhighlight %}



#### <a name="sec02"></a>Control Flow

{% highlight csharp %}
// bracnch selection statement
// selection: if and switch
// loop: for-in, for, while, do-while
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
println(teamScore)


// nullable variable
// combine 'if' and 'let', it is convinient to hanldle nullable variable. 
var optionalString: String? = "Hello"
optionalString == nil

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    greeting = "Hello, \(name)"
}

// flexiable 'switch'
let vegetable = "red pepper"
switch vegetable {
case "celery":
    let vegetableComment = "Add some raisins and make ants on a log."
case "cucumber", "watercress":
    let vegetableComment = "That would make a good tea sandwich."
case let x where x.hasSuffix("pepper"):
    let vegetableComment = "Is it a spicy \(x)?"
default:
    let vegetableComment = "Everything tastes good in soup."
}

// 'for-in' iterates dictionary
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (kind, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
println(largest)


// 'while' and 'do-while'
var n = 2
while n < 100 {
    n = n * 2
}
println(n)

var m = 2
do {
    m = m * 2
} while m < 100
println(m)


// traditional 'for' loop, use '..<' to generate a range
// ..< => half-open range
// ... => closed range
var firstForLoop = 0
for i in 0..<4 {
    firstForLoop += i
}
println(firstForLoop)

var secondForLoop = 0
for var i = 0; i < 4; ++i {
    secondForLoop += i
}
println(secondForLoop)

{% endhighlight %}



#### <a name="sec03"></a>Functions and Closures

{% highlight csharp %}

// function and closure
// 'func'
func greet(name: String, day: String) -> String {
    return "Hello \(name), today is \(day)."
}
greet("Bob", "Tuesday")


// 'Tuple' returns multiple values
func calculateStatistics(scores: [Int]) -> (min: Int,
        max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0
    
    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }
            
    return (min, max, sum)
}
let statistics = calculateStatistics([5, 3, 100, 3, 9])
println(statistics.sum)
println(statistics.2)

func getGasPrices() -> (Double, Double, Double) {
    return (3.59, 3.69, 3.79)
}
getGasPrices()

// function with varying arguments
func sumOf(numbers: Int...) -> Int {
    var sum = 0
    for number in numbers {
        sum += number
    }
    return sum
}
sumOf()
sumOf(42, 597, 12)


// a nested function
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()


// a function can be a return value, or passing by argument
func makeIncrementer() -> (Int -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)


func hasAnyMatches(list: [Int], condition: Int -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(numbers, lessThanTen)

// Closure
// function is a special closure, '{}' defines lambda function
numbers.map({
    (number: Int) -> Int in
    let result = 3 * number
    return result
})

let mappedNumbers = numbers.map({number in 3 * number})
println(mappedNumbers)


// use arguments' positions
let sortedNumbers = sorted([1, 5, 3, 12, 2]){ $0 > $1 }
println(sortedNumbers)

{% endhighlight %}



#### <a name="sec04"></a>Objects and Classes

{% highlight csharp %}
// objects and classes
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}

var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()

class NamedShape {
    var numberOfSides: Int = 0
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    // use 'deinit' to create a deinitializer if you 
    // need to perform some cleanup before the object
    // is deallocated.
    
    // 'override'
    
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}


class Square: NamedShape {
    var sideLength: Double
    
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }
    
    func area() -> Double {
        return sideLength * sideLength
    }
    
    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}

let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()


class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0
    
    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }
    
    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }
    
    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}


var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
println(triangle.perimeter)
triangle.perimeter = 9.9
println(triangle.sideLength)


class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}

var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
println(triangleAndSquare.square.sideLength)
println(triangleAndSquare.triangle.sideLength)
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
println(triangleAndSquare.triangle.sideLength)

class Counter {
    var count: Int = 0
    func incrementBy(amount: Int, numberOfTimes times: Int) {
        count += amount * times
    }
}
var counter = Counter()
counter.incrementBy(2, numberOfTimes: 7)

let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
// if optionalSquare is nil, then sideLength is nil
let sideLength = optionalSquare?.sideLength
{% endhighlight %}





#### <a name="sec05"></a>Enumerations and Structures

{% highlight csharp %}
enum Suit {
    case Spades, Hearts, Diamonds, Clubs
    func simpleDescription() -> String {
        switch self {
        case .Spades:
            return "spades"
        case .Hearts:
            return "hearts"
        case .Diamonds:
            return "diamonds"
        case .Clubs:
            return "clubs"
        }
    }
}

let hearts = Suit.Hearts
let heartsDescription = hearts.simpleDescription()

// why structure? 
// One of the most important differences between structures
// and classes is that structures are always copied when they
// are passed around in your code, but classes are passed by
// reference. 
struct Card {
    var rank: Rank
    var suit: Suit
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}

let threeOfSpades = Card(rank: .Three, suit: .Spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()


enum ServerResponse {
    case Result(String, String)
    case Error(String)
}

let success = ServerResponse.Result("6:00 am", "8:09 pm")
let failure = ServerResponse.Error("Out of cheese.")

switch success {
case let .Result(sunrise, sunset):
    let serverResponse = "Sunrise is at \(sunrise) and sunset is at \(sunset)."
case let .Error(error):
    let serverResponse = "Failure... \(error)"
}

{% endhighlight %}


#### <a name="sec06"></a>Protocols and Extensions

{% highlight csharp %}
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}

class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += " Now 100% adjusted."
    }
}

var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription

// extend a type 'Int'
// use 'extension' to add functionality to an existing type
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    
    mutating func adjust() {
        self += 42
    }
}
println(7.simpleDescription)

let protocolValue: ExampleProtocol = a
println(protocolValue.simpleDescription)
// println(protocolValue.anotherProperty)
{% endhighlight %}

#### <a name="sec07"></a>Generics

{% highlight csharp %}
func repeat<Item>(item: Item, times: Int) -> [Item] {
    var result = [Item]()
    for i in 0..<times {
        result.append(item)
    }
    return result
}
repeat("knock", 4)

// Reimplement the Swift standard library's optional type
enum OptionalValue<T> {
    case None
    case Some(T)
}
var possibleInteger: OptionalValue<Int> = .None
possibleInteger = .Some(100)

func anyCommonElements <T, U where T: SequenceType, U: SequenceType,
    T.Generator.Element: Equatable, T.Generator.Element == U.Generator.Element>
    (lhs: T, rhs: U) -> Bool {
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}
anyCommonElements([1,2,3], [3])
{% endhighlight %}