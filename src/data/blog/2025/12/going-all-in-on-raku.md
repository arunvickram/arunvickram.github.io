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
description: Hedging all bets on the success of the language that could
# canonicalURL: https://example.org/my-article-was-already-posted-here
---


[Raku](https://raku.org/) has quickly become my favorite programming language over the past year or so for a number of reasons:

## Raku Pros:

### Standard library
Raku has one of the most feature-packed standard libraries of any programming language, with basically everything you could possibly need (aside from things like JSON parsing which has several libraries). 

Oh, and you don't have to import *anything*. It's all right there, ready for you whenever you need it.
### Extensive & Practical Functional Programming Capabilities
Raku's functional programming capabilities compared to its sister language Perl and its competitors Ruby and Python are a cut above the rest. It has a function composition operator `o`, pipe operators `==>` and `<==` which allow you to define variables at any point during the pipe for debugging purposes, the `.assuming` method which allows you to create partial functions, immutable classes/structs by default, and more.
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
