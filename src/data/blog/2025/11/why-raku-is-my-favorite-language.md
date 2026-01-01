---
title: Why Raku is my favorite language
author: Arun Vickram
pubDatetime: 2025-11-08T01:34:26.491Z
slug: why-raku-is-my-favorite-language
featured: true
draft: true
tags:
  - raku
  - language
  - favorite
  - perl
# ogImage: ../../assets/images/Camelia-Raku.png # src/assets/images/example.png
ogImage: "https://upload.wikimedia.org/wikipedia/commons/thumb/8/85/Camelia.svg/2880px-Camelia.svg.png" # remote URL
description: This is an impassioned rant of a developer battered down by big corpo who rediscovered his love of coding through Raku
# canonicalURL: https://example.org/my-article-was-already-posted-here
---

I don't know how to start this but I'll just start listing the coolest things I've seen in Raku and maybe the intro will come later.

It'll go from most useful to most quirky.

### Multimethods

### Gradual Typing

The three most popular dynamically typed languages are Javascript, Python, and Ruby. The benefit of having types only be realized at runtime
is obvious: prototyping and rapid development are much easier in dynamic languages like those three and other niche languages like Clojure,
Scheme, Smalltalk, etc.

The downside to dynamic typing is that when you write more and more code, and especially when you write code that
others depend on, the following tends to happen:

- Because there are no type annotations in your code, unless you explicitly add the name of the type into the names of the function parameters (which by the way is the convention in Scheme),
there often tends to be ambiguity vis-à-vis what the function accepts.
  - The way Clojure and other Lisps get away with this is by specifically emphasizing development using a REPL (Read-Evaluate-Print-Loop) for an extremely tight feedback loop. While Python, Javascript, and Ruby have REPLs, generally speaking when
  developing, say, Django or FastAPI applications the REPL is not specifically emphasized as the preferred tool of development in the same way that Lisps do tend to emphasize it. The REPL actually encourages people to play with libraries and get a feel for the functions via trial and error and the aforementioned instant feedback.
  - Smalltalk gets away with dynamic typing for a very similar reason: when writing Smalltalk code, you don't write your code using your generally preferred IDE or editor;
  you use a whole suite of code inspection and browsing tools in a virtual machine (the VirtualBox kind). You basically have a REPL and an IDE that is hopped on the most artisanal performance enhancing drugs. I'll make a different blog post on Smalltalk later, because I think we've fundamentally misunderstood
  OOP.
- You now have to write tests and runtime checks to check that you're passing in the right types (again the Lisps and Smalltalks are in many cases exempt from this due to really clever language design decisions).

#### Javascript

There were two major solutions that were each put forth by a large FAANG compoany: Facebook put out [Flow](https://flow.org/) and Microsoft put out [Typescript](https://www.typescriptlang.org/).

Let's talk about the far more popular of the two solutions: Typescript. Typescript's approach to solving this problem is to be a syntactic superset of ECMAScript, which is the standard that Javascript conforms to.

The way you work with existing Javascript libraries as a Typescript developer is that you're *supposed* to write type definition files for those libraries, basically trying to map Typescript annotations and declarations to the actual kinds of values being passed around functions in Javascript.
Similar to how ATS works with C, while you can just port your JS code to TS with the highest fidelity, bugs and all (as [deech](https://www.youtube.com/playlist?list=PLEQTpgQ9eFCGULzlOITbueBQW6R7VtIKO) would say) by telling the Typescript compiler to ignore certain code, you wouldn't
be deriving the full benefits of the Typescript type system.

And on that note, I want to say something about the Typescript type system: for what it is, it's actually quite clever. It's just very unfortunate that we're bolting on a type system to Javascript post-mortem. Like Typescript has *real* union types unlike the sum types of Haskell and the ML family.
So when Rich Hickey [talks about loosening requirements and strengthening guarantees](https://www.youtube.com/watch?v=YR5WdGrpoug&pp=ygUVbWF5YmUgbm90IHJpY2ggaGlja2V5), Typescript actually doesn't have the problem of Haskell where you introduce breaking changes while trying to actually do the opposite.

Typescript's [mapped types](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html) are a great example of the kind of thought that went into the type system of Typescript which revolves heavily around structural typing as opposed to nominal typing.

That all being said, I would be lying to you if I said this didn't look a bit insane:

```typescript
export type ThunkAction<
  R, // Return type of the thunk function
  S, // state type used by getState
  E, // any "extra argument" injected into the thunk
  A extends Action // known types of actions that can be dispatched
> = (dispatch: ThunkDispatch<S, E, A>, getState: () => S, extraArgument: E) => R
```

Obviously in Typescript, you don't have to have single-letter type parameter names, so this could be rewritten as:

```typescript
export type ThunkAction<
  ThunkResult, // Return type of the thunk function
  State, // state type used by getState
  ExtraThunkArg, // any "extra argument" injected into the thunk
  ActionType extends Action // known types of actions that can be dispatched
> = (dispatch: ThunkDispatch<State, ExtraThunkArg, ActionType>, getState: () => State, extraArgument: ExtraThunkArg) => ThunkResult
```

This is a great example of one of the biggest gripes I have mainly with Typescript conventions: slapping a name on a function type. The ease with which you can slap a name on an
object type in TS to add checks is Typescript's greatest strength and its greatest weakness. I generally don't have a problem with this for very simple function types, but the moment we start doing the above
*so frequently* that it actually obscures what the type actually is underneath the hood, is when I believe we've lost the plot.


#### Python

Python, because it has a benevolent dictator for life in the form of Guido van Rossum,

#### Ruby

I think out of these three languages, Ruby has the best excuse for avoiding type annotations at all costs, and it's because Ruby is basically trying to bring Smalltalk's object model into the mainstream. Unlike Javascript and Python, in Ruby *everything*, including what we
would expect to be primitive datatypes like integers, floats, etc. is *some* instance of *some* class.


#### Raku's solution


### Junctions
<!--Junctions are possibly the quirkiest data type I've seen in a while. They're conceptually based on set theory:

```raku
my $or-junction = 1|2|3|4;
my $and-junction = 1&2&3&4;
```

The variable `$or-junction` can *sort of* be likened to the following (we'll explain the nuances in a second):

$$
\{1\} \cup \{2\} \cup \{3\} \cup \{4\}
$$

And `$and-junction` is sort of like:

$$
\{1\} \cap \{2\} \cap \{3\} \cap \{4\}
$$

This isn't a sum type like `Either` that you would see in Haskell, and it's also not a union type that you would see in Scala 3 like `A | B`. All junctions are instances of the class `Junction`.

No, we're effectively looking at a glorified list with one of four tags (`none`, `any`, `all`, `one`) attached to it. In fact, the `|` and `&` operators we see in between the numbers, are actually just sugar for the following:

```
my $or-junction = any(1, 2, 3, 4); #=> 1|2|3|4
my $and-junction = all(1, 2, 3, 4); #=> 1&2&3&4

### Or, as we'll see:

my @numbers = 1, 2, 3, 4;
my $or-junction = any(@numbers);
my $and-junction = all(@numbers);
```

So, why does this even exist? What is the raison-d'être of the quirky `Junction`? Let's look at a more real-world example:

```raku
class Vec2 {
    has Int $.x;
    has Int $.y;

    method is_origin {
        $!x == 0 && $!y = 0
    }
}

my @vecs =
    Vec2.new(x => 10, y => 10),
    Vec2.new(x => 2, y => 2),
    Vec2.new(x => 0, y => 0);

if any(@vecs).is_origin {
    say "We got the origin vector";
}

### OUTPUT:
### We got the origin vector
```

"Wait a second," you're probably thinking. "`is_origin` isn't a method defined on the class `Junction`, right?"

And you would be correct.

Junctions are *distributive* types,
meaning that any operation or method call on a junction, is automatically applied to every member of that junction.

Let's actually look at what's happening under the hood:

```
my @vecs =
    Vec2.new(x => 10, y => 10),
    Vec2.new(x => 2, y => 2),
    Vec2.new(x => 0, y => 0);

#`(
    any(@vecs).is_origin

  = any(Vec2.new(x => 10, y => 10),
        Vec2.new(x => 2, y => 2),
        Vec2.new(x => 0, y => 0)).is_origin

  = any(Vec2.new(x => 10, y => 10).is_origin,
        Vec2.new(x => 2, y => 2).is_origin,
        Vec2.new(x => 0, y => 0).is_origin)

  = any(False, False, True)

  ...
)

if any(@vecs).is_origin {
  # the result of `any(@vecs).is_origin`, because it follows an `if`,
  # gets coerced to a boolean which ultimately evaluates to `True`.
  #
  # so therefore the following statement gets printed.
  say "We got the origin vector";
}
```

I have never seen a language (other than Ruby) where you can test a list *this* succinctly.

This would be the equivalent in Ruby:
```ruby
# let's assume we've defined Vec2 already which has a method origin?

vecs = [Vec2.new(10, 10), Vec2.new(2, 2), Vec2.new(0, 0)]

# i'm almost positive you can't just pass a method name as a keyword to .any? but if I'm wrong
# then i'm more than happy to accept it and modify this example.
if vecs.any(&:is_origin)
  puts "Got origin in the list"
end
```

and in Clojure:

```clojure
; clojure gets to cheat a little bit here because
; it's more idiomatic to use hashmaps
(defn origin? [{:keys [x y]}] (and (zero? x) (zero? y)))

(let [vecs [{:x 10 :y 10} {:x 2 :y 2} {:x 0 :y 0}]]
  (if (some origin? vecs)
    (prn "Got origin")))
```

Both Clojure and Ruby are fairly succinct languages in general, but even *they're* limited by the fact
that you cannot succinctly test for the existence of something without explicitly passing a function.

Let's say for the sake of argument that instead of looking for the origin vector in a list, you want to see if
at least one of the vector's x values is greater than 1.

How would we do that in Ruby and Clojure?

```ruby
if vecs.any({ |vec| vec.x > 1 })
  puts "We got a vector whose x > 1"
end
```

```clojure
(if (some? #(> (:x %) 1) vecs)
   (prn "We got a vector whose x > 1))
```

I want to make it perfectly clear: the above code in both Ruby and Clojure is *great*, and if I were to read that in
some codebase I'd understand it clearly. The point is it's just not as clean as this:

```
if any(@vecs).x > 1 {
  say "We got a vector whose x > 1";
}
```

### Raku <3 i18n

Raku has the deepest integration with the Unicode standard I have ever seen.

I want to start with an example. My heritage tongue is Tamil. Let's say that I'm trying to teach a person in Tamil Nadu, India how to code, but they're
most comfortable reading in the Tamil script, which has its own typographic rules.

Just as a shot in the dark, let's see what happens when I try and name a variable using Tamil letters in Python:

```python
கணக்கு = 100 # this means 'account' in Tamil

print(கணக்கு)
```

Cool! It works! However, it would be nice to have that `print` be replaced with a Tamil word: பதிப்பி, meaning "print".

Well, we can define a function in Python that basically does the same thing:

```python
def பதிப்பி(*சொற்கள்):
    print(*சொற்கள்)

பதிப்பி(கணக்கு)

# 100
```

Very cool, this works as well. But wouldn't it *also* be nice if we could replace that `def` with a native word? Hold that thought.

We can't do much about reserved words in Python. so practically speaking we're kind of limited in what we're able to do:

```python
if கணக்கு > 100:
    பதிப்பி("உங்களது கணக்கிலிருக்கும் பணம் அதிகம்")
```

I'll be perfectly honest, this is one of those instances where I give Python a D-; at least it didn't break when I tried to name the function and variable names using UTF-8 characters.

#### Raku understands numerals in every godsdamned language

Let's take an **extreme** example. There's an endangered language called Sora (alternatively Savara), that has its own script and set of Hindu-Arabic positional numerals.

Let's see what happens in Python when I write 4000 using Sora numerals ("𑃴𑃰𑃰𑃰") into the Python REPL:

```
$ python3
>>> 𑃴𑃰𑃰𑃰
  File "<python-input-9>", line 1
    𑃴𑃰𑃰𑃰
    ^
SyntaxError: invalid character '𑃴' (U+110F4)
```

Oops. I think we pushed Python to its limits. Ruby?

```
$ irb
irb(main):004:0> 𑃴𑃰𑃰𑃰
(irb):4:in `<main>': undefined local variable or method `𑃴𑃰𑃰𑃰' for main:Object (NameError)
	from /Users/arun/.local/share/mise/installs/ruby/3.2.9/lib/ruby/gems/3.2.0/gems/irb-1.6.2/exe/irb:11:in `<top (required)>'
	from /Users/arun/.local/share/mise/installs/ruby/3.2.9/lib/ruby/site_ruby/3.2.0/rubygems.rb:319:in `load'
	from /Users/arun/.local/share/mise/installs/ruby/3.2.9/lib/ruby/site_ruby/3.2.0/rubygems.rb:319:in `activate_and_load_bin_path'
	from /Users/arun/.local/share/mise/installs/ruby/3.2.9/bin/irb:25:in `<main>'
```

I guess that's better than Python outright disallowing the characters entirely.

Alright, let's see how Raku handles it:

```
$ rlwrap raku
[0] > 𑃴𑃰𑃰𑃰
4000
```

No matter how tiny a community speaks your language, Raku is smart and accomodating enough to handle it, and I think that represents an ethos in the Raku community
that just doesn't exist anywhere else.-->
