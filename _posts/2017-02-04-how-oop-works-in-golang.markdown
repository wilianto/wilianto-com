---
layout: post
title: "How OOP works in Golang"
date: 2017-02-04 06:58:00 +0700
categories: golang
meta_keywords: golang oop
meta_description: "Golang for developer who familiar with Object Oriented Programming"
comments: true
---

My previous and current stack are PHP, Java and Ruby. All of them are Object Oriented ready. So when I learn Golang this week, I try to implement the concept of OOP in Golang. These are some of OOP concept that I've tried to implemented in Golang.

- Class
- Attribute
- Method
- Inheritance
- Overiding
- Interface
- Polymorphism

## Class in Golang
In Golang we use struct to make our own data types. We can also make class with struct.

### Class Example in Golang
{% highlight golang %}
package main

import (
  "fmt"
)

type Shape struct {
  Name string
  Color string
}

func main() {
  shape := Shape{"General Shape", "Blank"}

  fmt.Printf("%T\n", shape) //print the object type
  fmt.Println(shape.Name) //print the object attribute
  fmt.Println(shape.Color)
}

{% endhighlight %}

**Output: Class Example**
```
main.Shape
General Shape
Blank
```

In OOP, class usually has methods. In go it's called receiver.

### Example method / receiver in Golang
{% highlight golang %}
package main

import (
  "fmt"
)

type Shape struct {
  Name string
  Color string
}

func (shape Shape) Area() float64 {
  return 0.0
}

func main() {
  shape := Shape{"General Shape", "Blank"}

  fmt.Printf("%T\n", shape) //print the object type
  fmt.Println(shape.Name) //print the object attribute
  fmt.Println(shape.Color)

  fmt.Println(shape.Area()) //print the object method / receiver
}
{% endhighlight %}

**Output: Method / Receiver Example**
```
main.Shape
General Shape
Blank
0
```
## Inheritance in Golang
We cannot separate inheritance with OOP. In Golang we can also do inheritance and it's called embedding.

### Inheritance Example in Golang
{% highlight golang %}
package main
import (
  "fmt"
)

type Shape struct {
  Name string
  Color string
}

func (shape Shape) Area() float64 {
  return 0.0
}

type Square struct {
  Shape //embedding
  Side float64
}

type Rectangle struct {
  Shape //embedding
  Width float64
  Length float64
}

func main() {
  square := Square{
    Shape{"Squarepant", "Yellow"},
    4.0,
  }

  fmt.Printf("%T\n", square) //print the object type
  fmt.Println(square.Name) //print the parent attribute
  fmt.Println(square.Color)
  fmt.Println(square.Side)
  fmt.Println(square.Area()) //print the object method / receiver

  fmt.Println("------------------")

  rectangle := Rectangle{
    Shape{"Rectangular", "Red"},
    4.0,
    6.0,
  }

  fmt.Printf("%T\n", rectangle) //print the object type
  fmt.Println(rectangle.Name) //print the parent attribute
  fmt.Println(rectangle.Color)
  fmt.Println(rectangle.Width)
  fmt.Println(rectangle.Length)
  fmt.Println(rectangle.Area()) //print the object method / receiver
}
{% endhighlight %}

**Output: Inheritance Example**
```
main.Square
Squarepant
Yellow
4
0
------------------
main.Rectangle
Rectangular
Red
4
6
0
```

## Overiding in Golang
In the example above, square and rectangle has Area receiver from parent. But it still returns 0, we should overide the receiver Area() because calculating area of rectangle and square have different formula.

### Overide Example in Golang

{% highlight golang %}
package main

import (
  "fmt"
)

type Shape struct {
  Name string
  Color string
}

func (shape Shape) Area() float64 {
  return 0.0
}

type Square struct {
  Shape
  Side float64
}

//overide
func (square Square) Area() float64 {
  return square.Side * square.Side
}

type Rectangle struct {
  Shape
  Width float64
  Length float64
}

//overide
func (rectangle Rectangle) Area() float64 {
  return rectangle.Width * rectangle.Length
}

func main() {
  square := Square{
    Shape{"Squarepant", "Yellow"},
    4.0,
  }

  fmt.Printf("%T\n", square) //print the object type
  fmt.Println(square.Name) //print the parent attribute
  fmt.Println(square.Color)
  fmt.Println(square.Side)
  fmt.Println(square.Area()) //print the object method / receiver

  fmt.Println("------------------")

  rectangle := Rectangle{
    Shape{"Rectangular", "Red"},
    4.0,
    6.0,
  }

  fmt.Printf("%T\n", rectangle) //print the object type
  fmt.Println(rectangle.Name) //print the parent attribute
  fmt.Println(rectangle.Color)
  fmt.Println(rectangle.Width)
  fmt.Println(rectangle.Length)
  fmt.Println(rectangle.Area()) //print the object method / receiver
}

{% endhighlight %}

**Output: Overide Example**
```
main.Square
Squarepant
Yellow
4
16
------------------
main.Rectangle
Rectangular
Red
4
6
24
```

## Interface in Golang
We have receiver Area() but in the parent class we returns it with 0. It's not a good implementation, because the shape should not have method Area() if there is no formula to calculate it. So we should refactor it to use interface.

{% highlight golang %}
package main

import (
  "fmt"
)

type Shape struct {
  Name string
  Color string
}

//create interface
type Calculable interface {
  Area() float64
}

type Square struct {
  Shape
  Side float64
}

//overide
func (square Square) Area() float64 {
  return square.Side * square.Side
}

type Rectangle struct {
  Shape
  Width float64
  Length float64
}

//overide
func (rectangle Rectangle) Area() float64 {
  return rectangle.Width * rectangle.Length
}

func main() {
  square := Square{
    Shape{"Squarepant", "Yellow"},
    4.0,
  }

  fmt.Printf("%T\n", square) //print the object type
  fmt.Println(square.Name) //print the parent attribute
  fmt.Println(square.Color)
  fmt.Println(square.Side)
  fmt.Println(square.Area()) //print the object method / receiver

  fmt.Println("------------------")

  rectangle := Rectangle{
    Shape{"Rectangular", "Red"},
    4.0,
    6.0,
  }

  fmt.Printf("%T\n", rectangle) //print the object type
  fmt.Println(rectangle.Name) //print the parent attribute
  fmt.Println(rectangle.Color)
  fmt.Println(rectangle.Width)
  fmt.Println(rectangle.Length)
  fmt.Println(rectangle.Area()) //print the object method / receiver
}

{% endhighlight %}

**Output: Interface Example**

```
main.Square
Squarepant
Yellow
4
16
------------------
main.Rectangle
Rectangular
Red
4
6
24
```

The output still the same, but our code has better implementation.

## Polymorphism in Golang
We already use interface in our current code. We can get the benefit of interface, it is polymorphism. It's a bit hard for me to explain it in words. You can see the explaination [here](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)). But the main point is we can call interface receiver at all of struct that already implemented it.

## Polymorphism Example in Golang

{% highlight golang %}
package main

import (
  "fmt"
)

type Shape struct {
  Name string
  Color string
}

//create interface
type Calculable interface {
  Area() float64
}

type Square struct {
  Shape
  Side float64
}

//overide
func (square Square) Area() float64 {
  return square.Side * square.Side
}

type Rectangle struct {
  Shape
  Width float64
  Length float64
}

//overide
func (rectangle Rectangle) Area() float64 {
  return rectangle.Width * rectangle.Length
}

func main() {
  square := Square{
    Shape{"Squarepant", "Yellow"},
    4.0,
  }

  rectangle := Rectangle{
    Shape{"Rectangular", "Red"},
    4.0,
    6.0,
  }

  calculableShapes := []Calculable{square, rectangle}

  for _, calculableShape := range calculableShapes {
    fmt.Printf("Im a %T and my area is %g\n", calculableShape, calculableShape.Area())
  }
}

{% endhighlight  %}

**Output: Polymorphism Example**
```
Im a main.Square and my area is 16
Im a main.Rectangle and my area is 24
```

See how it works?

That's what I've learned this week. If you have some input for me about OOP in Golang, please tell me on the comment below.

Reference:
- [https://golang.org/pkg](https://golang.org/pkg)

