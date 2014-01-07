---
title: My Story About Learning Functional Programming
author: bepitulaz
layout: post
permalink: /2013/12/story-about-learn-functional-programming/
categories:
  - Blog
  - Programming
tags:
  - erlang
  - functional programming
  - meetup
comments: true
---
The first time I heard about functional programming was when I read about Foursquare use Lift framework (a framework written in Scala language) a few years ago. At that time, I was not digging about FP deeper. But after I read about Twitter also migrated from Ruby (Rails) to Scala in 2011, and never seen that infamous Fail Whale anymore after it, this FP thing was getting my attention. But I was not sure, was it worth to learn? Then in 2013, I read some articles that mention about Go, a new language from Google, and especially this article about <a href="http://highscalability.com/blog/2013/3/13/ironio-moved-from-ruby-to-go-28-servers-cut-and-colossal-clu.html" target="_blank">reducing 30 Rails servers to only 2 servers with Go</a>. Ok, I decide to learn functional programming.

<!--more-->

First attempt to learn functional programming I tried Haskell. Because some articles mention about Haskell is a pure functional programming language. But, not in a very long time, I gave up. Lol. I can&#8217;t understand the concept (yeah my brain was refusing the concept). I stopped learning for a few months, but I still curious about it and read many materials about functional programming concept. Then I realised, I made a mistake because I tried to learn the language, not tried to understand the concept.

2 weeks ago, I started to learn it again. I discovered about Scala, Clojure, Go, Erlang, Elixir, and Haskell. But my choice was Erlang, because Scala and Clojure is on top of JVM (I&#8217;m not Java hater, but I&#8217;m too lazy to install it on my laptop); I can&#8217;t find books about Go (I&#8217;m a text book learner); I didn&#8217;t find a commercial project that use Haskell; Elixir is on top of Erlang VM and can use Erlang library and code directly, therefore better if I learn Erlang first. Now I understand that functional programming has very simple concept, those are avoiding state and data immutable.

I&#8217;m not good at math, but this is a basic math lesson in the school that show you the concept about immutable data and the basic concept in functional programming.

<pre>x + y = 6
x - y = 2

so, x must be 4 and y must be 2</pre>

But when we learn imperative programming like C, PHP, Javascript, Ruby, etc, we were shown the stuff like this and it&#8217;s normal.

<pre>x = 3
x = x + 1

now x is 4
x is like a box that can be changed its value</pre>

That&#8217;s not allowed in functional programming, because functional programming is like a function in math.

<pre>f(x) = x + 1

f is function name
(x) is input
x + 1  is output

x = 3
f(3) = 3 + 1 

x value is not changed, it still 3. Only the result of the function is changed.</pre>

Now, I&#8217;ll show you one more simple example, it&#8217;s taken from Erlang book.

<pre>X = (2+4).
the result is 6

Y = 10.
the result is 10

X = 6.
it is true, X is 6 indeed

X = Y.
You will get error. Why? Because the value of X it does not same with the value of Y.</pre>

In Erlang (and most of any other FP languages), once you assign value to a variable, you can&#8217;t change it. That is called immutable. By the way, = is not an assignment operation likes in most of imperative programming language, = is a pattern matching operation. Now can you see the different between mutable and immutable data?

One tip from me, if you are interested to learn functional programming. Empty your mind, assume that you don&#8217;t know anything about programming, and start learn the concept. If you live in Jakarta, Indonesia, let&#8217;s meet up and share about functional programming. <a title="Functional Programmer Meetup Jakarta" href="http://www.meetup.com/Functional-Programmers-Jakarta/" target="_blank">Join and come here</a> <img src="http://asep.co/wp-includes/images/smilies/icon_biggrin.gif" alt=":D" class="wp-smiley" />