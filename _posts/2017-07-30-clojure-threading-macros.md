---
layout: post
title:  "Clojure Threading Macros"
author: astrocaribe
date:   2017-07-30 00:00:00
categories: clojure
---

***Reading time: {{ page.content | reading_time }}***

I've been coding Clojure for ~ 8 months, and it's only been in the few recent
months that some of the concepts are beginning to click. Only after a deed dive
of impressively written code, do some of the nuances begin to emerge.

One way that I learn about language concepts is often through math (aren't all
languages, on one level or another, based on math?). So when I came across
Clojure's threading macros ([thread first][thread-first] and
[thread last][thread-last]), I was throw for a loop. But then, it clicked while
trying to rationalize a math problem.

Consider the following mundane equation:
```math
  6^2 / 2 + 7 = 25
```
I know, nothing fancy. But what would this look like in Clojure?
{% highlight clojure %}
  (+ (/ (* 6 6) 2) 7)
  => 25
{% endhighlight %}

Using the [order of operations][order-of-operations], you should come to `25` as
your answer. But from the snippet above, that's a bit hard to follow. How about
we use a threading macro?

### Thread first (->)
Usage: `(-> x & forms)`  

A [thread first][thread-first] macro essentially places `x` as the second item
in the first form, and the results become the second item in the following form,
ad infinitum. Let's write our previous expression as a thread first macro:

{% highlight clojure %}
  (-> (* 6 6)
      (/ 2)
      (+ 7))
  => 25
{% endhighlight %}

So, the first form is evaluated `(* 6 6) = 36`; which then becomes the second
element in the next form:

{% highlight clojure %}
  (-> (/ 36 2)
      (+ 7))
  => 25
{% endhighlight %}

Then finally:

{% highlight clojure %}
  (-> (+ 18 7))
  => 25
{% endhighlight %}

Ain't that something? Which form is easier to understand? For me, the threading
example is the one that makes me happy! So how is the thread last macro
different from thread first?

### Thread last (->>)
Usage: `(->> x & forms)`  

Unlike the `->` macro, x is inserted as the last element in the following form,
and so on. So taking our original mundane equation:

{% highlight clojure %}
  (->> (* 6 6)
       (/ 2)
       (+ 7))
  => 127/18
{% endhighlight %}

Yes, the math is indeed different! In non-macro form:

{% highlight clojure %}
  (+ 7 (/ 2 (* 6 6)))
  => 127/18
{% endhighlight %}

Apply the same logic as we did for the thread first macro, and you should be
able to verify the result we get above.

The threading macros makes things easier to read, but most importantly, I
finally understand the macros! May sound simplistic, but it got me where I
needed to understand; maybe it can help you too!


[thread-first]: https://clojuredocs.org/clojure.core/-%3E
[thread-last]: https://clojuredocs.org/clojure.core/-%3E%3E
[order-of-operations]: https://en.wikipedia.org/wiki/Order_of_operations#Mnemonics
