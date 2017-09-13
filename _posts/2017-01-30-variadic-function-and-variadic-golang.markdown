---
title: Variadic Function and Variadic Argument Golang
date: 2017-01-30 09:52:00 Z
categories:
- golang
layout: post
meta_keywords: golang variadic, variadic function in golang
meta_description: Explain with example variadic function and variadic argument in
  Golang
comments: true
---

Let's start the first note!

In this week, I'm exploring Golang. It is an opensource programming language. I'm interested in learning it because Go offers concurrency, compiled code and run everywhere.

The interesting one is about variadic function. Variadic function is an function with that have unlimited arguments. So it can be called with any number of trailing arguments.

{% highlight golang %}
package main

import "fmt"

func sum(numbers ...int) int {
    total := 0
    for _, number := range numbers {
        total += number
    }
    return total
}

func main() {
    fmt.Println(sum(1, 2, 3, 4, 5))
}
{% endhighlight %}

In Go you can also use variadic argument to help you pass values from an array to a variadic function parameter.

{% highlight golang %}
package main

import "fmt"

func sum(numbers ...int) int {
    total := 0
    for _, number := range numbers {
        total += number
    }
    return total
}

func main() {
    numbers := []int{1, 2, 3, 4, 5}
    fmt.Println(sum(numbers...))
}
{% endhighlight %}

This is what I get when learning Go, let's see what I get in the next couple weeks.