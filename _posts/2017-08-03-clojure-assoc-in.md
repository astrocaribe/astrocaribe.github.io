---
layout: post
title:  "Clojure Assoc-in"
date:   2017-08-03 16:12:00
author: astrocaribe
category: dev
tags: [clojure]
---

I came across a scenario where I needed to add a nested clojure map into an
existing map; something like:

#### Original:
{% highlight clojure %}
  {:movie {:info {:name "Little Trouble in Big Manhattan"
                  :year "2020"
                  :format "BR-1000"}}}
{% endhighlight %}

Suppose I wanted to add an actor, and a language for the film? So in the end,
I'd like something like this:

#### Desired:
{% highlight clojure %}
  {:movie {:info {:name "Little Trouble in Big Manhattan"
                  :year "2020"
                  :format "BR-1000"
                  :actor "JeSuis VonBraun"
                  :language "French"}}}
{% endhighlight %}

Here's my solution:

- First, define the original map to make it easier:
{% highlight clojure %}
(def movie {:movie {:info {:name "Little Trouble in Big Manhattan"
                           :year "2020"
                           :format "BR-1000"}}})
=> #'ns-movies-db/movie
{% endhighlight %}

- Insert **:actor**:
{% highlight clojure %}
(assoc-in movie [:movie :info :actor] "JeSuis VonBraun")
=> {:movie {:info {:name "Little Trouble in Big Manhattan",
                   :year "2020",
                   :format "BR-1000",
                   :actor "JeSuis VonBraun"}}}
{% endhighlight %}

- Similarly, **:language** can be added in the same way:
{% highlight clojure %}
(assoc-in movie [:movie :info :language] "French")
=> {:movie {:info {:name "Little Trouble in Big Manhattan",
                   :year "2020",
                   :format "BR-1000",
                   :language "French"}}}
{% endhighlight %}

But what if I want to add both? My current strategy is to wrap one `assoc-in`
inside of another, using the result of the inner as the map for the outer:
{% highlight clojure %}
(assoc-in (assoc-in movie [:movie :info :actor] "JeSuis VonBraun")
          [:movie :info :language] "French")
=>{:movie {:info {:name "Little Trouble in Big Manhattan",
                  :year "2020",
                  :format "BR-1000",
                  :actor "JeSuis VonBraun",
                  :language "French"}}}
{% endhighlight %}

Voila!! I get the result I'm after! I'm must admit, however, that this does not
feel like an elegant solution. I'll keep digging to refactor this, and post
a better solution once I find it!
