---
title: Going all in on Raku
author: Arun Vickram
pubDatetime: 2025-12-12T01:34:26.491Z
slug: going-all-in-on-raku
featured: true
draft: true
tags:
  - raku
  - fully
# ogImage: ../../assets/images/Camelia-Raku.png # src/assets/images/example.png
ogImage: "https://upload.wikimedia.org/wikipedia/commons/thumb/8/85/Camelia.svg/2880px-Camelia.svg.png" # remote URL
description: Hedging all bets on the success of the not-so-little language that could.
# canonicalURL: https://example.org/my-article-was-already-posted-here
---

[Raku](https://raku.org/) has quickly become my favorite programming language over the past year or so for a number of reasons:

## Raku Pros:

### Standard library
Raku has one of the most feature-packed standard libraries of any programming language, with basically everything you could possibly need (aside from things like JSON parsing which has several libraries). 

Oh, and you don't have to import *anything*. It's all right there, ready for you whenever you need it.
### Functional programming

Let's start with examples, cause they speak louder than words. I want you to keep in mind that Raku isn't a language with functional programming as its *principal* feature.
#### Function Composition

Haskell:
```haskell
f x = x + 1
g x = x + 2
h = g . f
```

Clojure: 
```clojure
(defn f [x] (inc x))
(defn g [x] (+ 2 x))
(def h (comp g f))
```

Raku:
```raku
sub f($x) { $x + 1 } # can also be written as our &f = { $^x + 1 }
sub g($x) { $x + 2 }
our &h = &g o &f; # yes, the composition operator in Raku is a lowercase `o`
```

Some explanations:
- Raku has this notion of *variable containers* and *sigils* to represent those variable containers, which it inherited from Perl.
	- Scalar variables are variables that generally hold exactly one thing, and are prefixed with a `$`.
	- Variables prefixed with `@` such as `@elements` are variables containing multiple values that can be indexed by an integer: essentially these are variables that are a list or array
	- Variables prefixed with a `%` such as `%dictionary` are associative data types, basically map-like data structures.
	- Variables prefixed with a `&` are function references. Every time you declare a function in Raku you can reference that function by prefixing it with a `&` like we do with `&f` and `&g`.
- `our` is a keyword in Raku similar to the variable declarator `my`, but the difference is that it declares a package-level variable rather than a lexically scoped variable. This is in kind of what declaring `sub`s does.

**BONUS:**
The function `comp` in Clojure is a variadic function that takes any number of functions and returns the composition of all those functions.

```clojure
(def composed (comp f g h a b c)) ; (composed x) = (f (g (h (a (b (c x))))))
```

You can actually do the exact same thing in Raku if you wanted:

```raku
our &composed = [o] &f, &g, &h, &a, &b, &c;

our &composed-2 = ([o] &f, &g, &h, &a, &b, &c) # if you want to make it more lisp-like;
```

Daniel Sockwell, another fellow Clojurian turned Rakuteer, [wrote a whole article on Raku's surprisingly good Lisp impression](https://www.codesections.com/blog/raku-lisp-impression/)


#### Pipe Operators

F#/OCaml:
```fsharp
let addBy y x = y + x

let add3 x = 
    x
    |> addBy 1
    |> addBy 2
```

Clojure:
```clojure
(defn add-by [y x] (+ y x))

(defn add3 [x]
    (->> x
         (add-by 1)
         (add-by 2)))
```

Raku:
```raku
sub add_by($y, $x) { $y + $x }

sub add-3($x) {
    $x ==> add_by 1
       ==> add_by 2;
}
```

> Note: the pipe operator in Raku is called the feed operator.

#### Partial Function Application

Functions are curried by default in languages like Haskell and OCaml/F#. So all function definitions with multiple parameters are by default functions that return functions:

```haskell
add x y = x + y
add' = \x -> \y -> x + y -- add' and add are equivalent
```

This allows you to *partially apply* (not plug in every parameter into) these functions so that you actually return functions that can be used in other functions like `map`.

Clojure (and Lisps in general) and Raku, in contrast, do not take this route: functions require you to pass all parameters by default.

```clojure
(defn add [x y] (+ x y))
(defn add' [x] (fn [y] (+ x y))) ; add and add' are not equivalent
```

```raku
sub add($x, $y) { $x + $y }
sub add-curried($x) { -> $y { $x + $y } } # these are not equivalent functions
```

To compensate for this, Clojure has a function called `partial` which takes a function name and a variable number of arguments and returns a new partially applied function.

```clojure
(defn add [x y] (+ x y))
(def add-2 (partial add 2))
(add-2 3) ; => 5
```

In Raku, this is achieved through the `.assuming` method and the Whatever star `*`, which is a "variable" (it's complicated) that changes depending on the context. In this case, `*` represents a variable that hasn't been applied yet.

```raku
sub add($x, $y) { $x + $y }
our &add-2 = &add.assuming(2, *);
add-2(3) # => 5
```


### Multimethods
Multi-methods/multi-subs. They're incredibly powerful in Elixir and they're extremely powerful here.
### Subsets
Subsets. An incredibly powerful feature that allows you to build what I think are more practical versions of [refined types](https://en.wikipedia.org/wiki/Refinement_type). Subsets paired with multi-methods are an **absurdly** powerful way of solving your domain problem declaratively
### Junctions
[Junctions](https://docs.raku.org/type/Junction). Raku might be the only language I have come across that has anything like this and it's surprisingly convenient. 
### Gradual Typing
Gradual typing. Raku is one of the few languages that is actually gradually typed: you can gradually add types to function signatures and variable declarations and the Raku compiler will type check them both at runtime and compile time.
### Slangs
Slangs. You can modify the Raku syntax if you feel like there's a construct that's missing
### ...and more
- And a whole lot more.

## Raku Cons:
Raku unfortunately has a lot of really important things going against it right now
### No Static Analysis/LSP implementation
The lack of solid statically analyzing LSP implementation. I think this There's [this](https://github.com/bscan/RakuNavigator) from bscan which works well enough if you use VSCode, but there's not much static analysis going on here and for a language as complex as Raku. For a language as complex and feature-rich as Raku, this is a *huge* problem for major adoption from new users who might lose patience when dealing with errors.
### Speed
Speed. The speed of the Rakudo compiler and MoarVM has been getting better over time especially thanks to really good work on RakuAST (another ridiculously powerful feature coming to Raku soon, macros and AST manipulation). My biggest issues so far have largely been the feedback loop when building Cro applications, from what I understand there is no fast incremental compilation because Cro applications are basically just Zef libraries that compile. A Raku script then loads the total library after being compiled.
### Documentation
Documentation. Documentation for Raku is fantastic overall, but there are some things like creating new HOWs using the Metamodel system that I think need documentation. There have been multiple tutorials which work well enough for the basics, but it's one of those things where the language is so complex that what is considered extensive documentation for other languages won't cut it for Raku.

I really want this language to succeed. I originally started with a [currency library](https://raku.land/zef:arunvickram/Moneys) just to dip my toes in with some of the cool features that Raku has like operator definitions, and then I started going further with implementing a [Datastar](https://data-star.dev/) SDK in [Raku](https://raku.land/zef:arunvickram/DataStar).
