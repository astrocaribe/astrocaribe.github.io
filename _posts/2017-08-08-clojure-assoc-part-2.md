---
layout: post
title:  "Clojure Assoc-in, Part 2"
date:   2017-08-08 11:56:00
author: astrocaribe
category: dev
tags: [clojure assoc]
---

In a previous post, [Clojure Assoc-in][assoc-post], I discussed adding items to
a Clojure nested map. I came up with an alternative solution.

Let's define the original map (again):

{% highlight clojure %}
(def movie {:movie {:info {:name "Little Trouble in Big Manhattan"
                           :year "2020"
                           :format "BR-1000"}}})
=> #'ns-movies-db/movie
{% endhighlight %}

To recap, we want to add the following to the above map, under the `:info` key:
{% highlight clojure %}
{:actor "JeSuis VonBraun"
 :language "French"}
{% endhighlight %}

Here's the previous solution:
{% highlight clojure %}
(assoc-in (assoc-in movie [:movie :info :actor] "JeSuis VonBraun")
          [:movie :info :language] "French")
=>{:movie {:info {:name "Little Trouble in Big Manhattan",
                  :year "2020",
                  :format "BR-1000",
                  :actor "JeSuis VonBraun",
                  :language "French"}}}
{% endhighlight %}

And now, the updated one (drumroll please!):
{% highlight clojure %}
(-> (assoc-in movie [:movie :info :actor] "JeSuis VonBraun")
    (assoc-in [:movie :info :language] "French"))
=>
{:movie {:info {:name "Little Trouble in Big Manhattan",
                :year "2020",
                :format "BR-1000",
                :actor "JeSuis VonBraun",
                :language "French"}}}
{% endhighlight %}

Ok, I know, not terribly exciting, but it's a little easier to understand (at
least for me). And yes, we are using a [thread-first][thread-first] macro like
we discussed in a previous post on [threading macros][thread-post].

### In Closing...
I am still not happy with this solution, and I'm sure there is still a more
elegant way to solve this one. Stay tuned!


[assoc-post]: {{ site.baseurl }}{% post_url 2017-08-03-clojure-assoc-in %}
[thread-first]: https://clojuredocs.org/clojure.core/-%3E
[thread-post]: {{ site.baseurl }}{% post_url 2017-07-30-clojure-threading-macros %}
