---
layout: post
title:  "Clojure Test Fixutes"
date:   2017-08-08 10:33:00
author: astrocaribe
category: dev
tags: [clojure unittests intellij idea]
---

In the past few months, I have really gotten into unit testing. I have done it
in the past for Node apps in earnest, but in the past month or so, I have done a
head-first dive into it, using [clojure test][clojure-test].

I have just come across `use-fixtures`, which I have seen before, but never
used. I have come up with a quick example of how this works, I'll share what I
have here!

### Clojure Test API
The Clojure [test API][clojure-test] provides a unit testing framework that is
easy to use; I like it because it can be integrated with [IntelliJ IDEA][IDEA]
and [Cursive][cursive-testing]. Using this I can quickly run the test suite,
see the failures in the tests, and get immediate feedback as to why it/they
failed, by showing a right vs. left comparison, assuming the `is` comparator is
being used.

### Use-Fixtures
`use-fixtures` is used mainly to create setup/teardown tasks for unit test, and
are defined once:

{% highlight clojure %}
(defn my-test-fixure [f]
  (println "My setup function here")
  (f)
  (println "My teardown function here")
  (println))
{% endhighlight %}

So, `my-test-fixure` takes a function `f`, performs any function described (in
this case, prints **"My setup function here"**), then executes function `f` that
is passed in. Finally, it prints **"My teardown function here"**, as defined above.

Now we define how the fixture is to be used:

{% highlight clojure %}
(use-fixtures :each my-test-fixure)
{% endhighlight %}

`:each` means that we want to use this fixture for **every** unit test found. If
you want to execute the fixture only once for the entire test suite (at the
beginning and the end), use `:once`. We'll see the effect of these shortly.

Say we have a unit test ready to go:

{% highlight clojure %}
(deftest a-test
  (testing "FIXME, I fail."
    (println "My first test here!")
    (is (= 1 1))))
{% endhighlight %}

Running our tests with `lein test` yields:

```
Testing test-example.core-test
My setup function here
My first test here!
My teardown function here


Ran 1 tests containing 1 assertions.
0 failures, 0 errors.
```
### :once
What happens if we add a second test? This is where the fixture frequency comes
in. First, let's add another test:

{% highlight clojure %}
(deftest b-test
  (testing "FIXME 2, I fail more!"
    (println "My second test here!")
    (is (= 6 6))))
{% endhighlight %}

And, set up the fixture to execute once for the test suite (begin/end):
{% highlight clojure %}
(use-fixtures :once my-test-fixure)
{% endhighlight %}

With `lein test` once again, we now get:
```
Testing test-example.core-test
My setup function here
My second test here!
My first test here!
My teardown function here


Ran 2 tests containing 2 assertions.
0 failures, 0 errors.
```

Yep, just as we expected! How about executing the fixture for each test?

### :each
Let's set it up to run for every test in the suite:
{% highlight clojure %}
(use-fixtures :each my-test-fixure)
{% endhighlight %}

```
Testing test-example.core-test
My setup function here
My second test here!
My teardown function here

My setup function here
My first test here!
My teardown function here


Ran 2 tests containing 2 assertions.
0 failures, 0 errors.
```

And there it is!

### In Closing...
This will be a neat tidbit when setting up a test suite that requires setting up
and tearing down tasks, without requiring them to be done in the test
explicitly. It gives you a centralized place to perform these tasks, making the
actual writing of unit tests easier.



[clojure-test]: https://clojure.github.io/clojure/clojure.test-api.html
[IDEA]: https://www.jetbrains.com/idea/
[cursive-testing]: https://cursive-ide.com/userguide/testing.html
