---
layout: post
title:  "Unscrewing a Git merge"
date:   2017-07-31 17:00:00
author: astrocaribe
category: dev
tags: [git clojure]
---

Yep. I screwed up a git merge today. It started with a sinking feeling that
something wasn't quite right, and ended with confirmation that "**Yep, I really
did screw that up!**".

### TL;DR

So how did I fix it? After much reading and rationalizing, it did a simple
thing; I threw away the last commit that included the merge by executing a hard
reset, one commit back:

{% highlight bash %}
  git reset --hard HEAD~1
{% endhighlight %}

### Setup

A co-worker and I have been working off the `dev` branch of our repo, and I am a
fan of feature branch work. Due to the nature of what we're doing, I am happy to
let him continue posting work directly to the `dev` branch, while I continue
feature branch work. Now since this is mostly to bolster our Clojure unit tests,
really, it's not a big deal right now.

The issue comes when I decide to merge dev work into my branch. How hard can it
be, right? Without thinking, I commit my feature branch changes (**mistake \#1**)
and proceed to merge `dev` into my branch (**mistake \#2**):

{% highlight shell %}
  git checkout dev
  git pull  #pull latest changes
  git checkout feature-branch
  git merge dev
{% endhighlight %}

Welp, git did exactly as I asked, but not what I wanted. The result was broken
unit tests (that I expected), but more importantly, to my detriment, conflicts.
As in merge conflicts. **Mistake \#3** now comes into play here; without taking
the time to understand the errors that the merge conflict resolution caused, I
started wholesale chipping away at the merge blocks that I didn't think I needed.
What you get at the end of that is a giant mess...

### Processing...

After a couple of hours trying to fix the conflicts, I give up, and resign to
rewriting my morning's work; from scratch. The biggest issue of all is that the
initial merge sorted the timeline such that all commit we're in chronological
order, including a few older ones. My co-worker's code intwined in a dance with
mine.

I panicked, because there was no way I could untangle that manually. I stopped,
thought about it for a few minutes, and came up with maybe trying a `cherry-pick`
or `git rebase` to save me. Then I got desperate enough to try the impossible:
how about blowing away that merge commit itself? Pretend it didn't happen?

### The Problem Solver

I took a deep breath, copied code from the mangled merge that I knew I would
need "just in case". And then uttered the magic words (*a prayer. What I
muttered, really, was a prayer to The Coding Gods*):

{% highlight unix %}
git reset --hard HEAD~1
{% endhighlight %}

The response on my terminal staring back at me? Exactly what I needed! It was as
if my terminal took all my code back to a time machine, hit a temporal
`-1 commit` button on my timeline, and took me back to a state where nothing was
amiss; back to a good state from the morning! **HOORAY!!!**

And the best part? No intertwined code from the `dev` branch from my co-worker!
Nothing to unravel; just getting a second chance to do it right this time.
