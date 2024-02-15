---
external: false
draft: false
title: Raku, possibly the most underrated language of the century
description: The original successor to the infamous Perl might be a hidden gem of a language.
date: 2024-02-01
---

When you say the name "Perl", many older and senior software engineers recoil in absolute horror
and are immediately triggered from past experiences at having to maintain Perl code from the CGI
days. This is when the internet was beginning to boom: we were transitioning away into Web 2.0 where CGI was common,
and the only major "stack" in the game was LAMP. This was LONG before the advent of "microservices", "frontends" and "backends", and far longer 
before something like [TRPC](https://trpc.io/).

Back then, in order for a website to be "interactive" it had to follow something called the Common Gateway Interface, and Perl was one of those languages
that slotted in best into that.

Unfortunately Perl code isn't the most readable:

```perl
use strict;

# 
# This script also prints the contents of all the listed files, but
# it first scans through the list to check that each file exists and
# is readable.  It will stop if there are any errors.
#

my $bad = 0;
foreach my $fn (@ARGV) {
    if(! -r $fn) {
        # File cannot be read.  See if it exists or not for a better 
        # error message.
        if(-e $fn) {
            print STDERR "You do not have permission to read $fn.\n";
        } else {
            print STDERR "File $fn does not exist.\n";
        }

        # One way or the other, it's bad.
        $bad = 1;
    }
}

# If there was a problem, bail out.
if($bad) { exit 2; }

# Copy all the files.
while(my $fn = shift @ARGV) {

    # Open the file.
    if(!open(INFILE, $fn)) {
        # We know the file is readable, but sometimes something else goes 
        # wrong.  It's safer to check.
        print STDERR "Cannot open $fn: $!\n";
        next;
    }

    # Copy it.
    while(my $l = <INFILE>) {
        print $l;
    }

    close INFILE;
}
# See the man page for the very long list of file tests, some of which are
# Unix-specific.  To find it from perl documentation, choose the
# standard documentation pack, supporting manpages, perlfunc, then
# choose -X (first in the alphabetical list).

```

This is an example of relatively readable Perl code. If you weren't used to Perl syntax and you were 
hired to work on a project it would be very hard to write easy-to-understand Perl code your first go-around,
and when making changes you were very easily liable to screw things up.

The thing is, Perl **excels** in text processing and matching, so much so that Perl's regular expressions quite literally
set the standard for regular expressions across multiple languages. Think about that: much of the standard regular expression libraries
used in languages today are used in reference to this one language.

But because Perl syntax is so terse, changes to code kept adding up, thus creating the meme of Perl being "write-only" code.

## Enter Perl 6

Larry Wall, as a result, set out to create the successor to Perl. It ended up diverging so much from the Perl that existed up until version 5 
that a new name was christened for the language: Raku. It took an incredibly long time and people generally thought for a long time that Perl 6 
was DOA.

But no. Raku is here and I personally think it's here to stay. It's the most underrated language I've seen thus far
and I mean it.

Let's start off with an easy example, a `Point` with an `x` and a `y`:

```
class Point {
  has $.x;
  has $.y;
}
```

in Python this is:

```python
class Point:
    def __initialize__(self, x, y):
        self.x = x 
        self.y = y
```

and in Ruby:

```ruby
class Point
  def initialize(x:, y:)
    @x = x 
    @y = y
  end
end
```

except, there's one slight problem. The Python and Ruby implementations don't have built-in way 
to see whether or not the two points are equal. You have to define `__eq__` in Python and `==` in Ruby.

But Raku? 

```
my $u = Point.new(x => 1, y => 2);
my $v = Point.new(x => 1, y => 2);

$u eqv $v # => True 
```

There's a structural equality operator `eqv` built in to the language. Now of course you have the option of defining 
`==` in Raku to basically do the same thing (don't worry we'll break this down after we revisit `Point`):

```
multi sub infix:<==>(Point:D $a, Point:D $b) {
  $a.x == $b.x && $a.y == $b.y;
}

say $u == $v; 
```

But you'll also notice something else once we revisit `Point`'s definition:

```
class Point { has $x; has $y; }
```

We didn't even write a constructor for this. By default, every class you define has a constructor,
that takes named arguments corresponding to the instance variable names and **automatically assigns** those named
arguments to the instance variables. 

Meaning, we didn't have to write the constructor for this:

```
Point.new(x => 1, y => 2)
```

How cool is that?! Okay okay, let's revisit the other piece of code and break it down.

```
multi sub infix:<==>(Point:D $a, Point:D $b) {
  $a.x == $b.x && $a.y == $b.y;
}
```

- `multi` marks any function as [multiple dispatch](https://en.wikipedia.org/wiki/Multiple_dispatch), meaning that function arguments can be overloaded. The cool thing is that MoarVM, the VM Raku runs on, specifically optimizes for 
multiple dispatch.
- `infix:<==>` is an extended identifier, which contains meta-information about the kind of identifier it is. In this case, the `infix:` tells us this is a function definition for 
an infix operator. Raku allows you to define infix, prefix, postfix, circumfix, and post-circumfix operators. Whatever is in between the `<>` is the sequence of characters that will become the operator.
- `:D` is a type smiley (I swear I didn't make the name up). `:D` at the end of any type tells us that it's a "non-null" value (undefinedness in Raku is handled quite differently from many languages). The point is that you can't pass empty values into this function will be used as an operator. `:D` means defined, and `:U` means undefined.


### Consistency in Syntax

It sounds wild to be saying this about a language that used to be called "Perl 6", but yes, there is remarkable semantic consistency in the language. 

Let's start with an anonymous function:

```
-> $x { say $x }
```

If you're coming from almost any other programming language you're probably thrown off by the fact that the argument is on the pointy end of the arrow.
The way to read this is basically: we're going to capture some value in an outer context and call it `$x`, and with that `$x` we're going to do stuff with it (in this case just print it to the console). 

Let's see what happens when we add two words/terms to this:

```
for 1..10 -> $x { say $x }
```

Well would you look at that, there's our `for` loop. Now you can read the code as "for each number from 1 to 10, capture that value as `$x` and print it to the console". This isn't us just passing in a 
function by the way, this is proper Raku syntax. Even though in Haskell I know you could do something like:

```
forM_ [1..10] $ \x -> do
    print x
```

which is basically passing in a Kleisli function into `forM`. The point that I'm making is there's consistency within the syntax itself, `for` doesn't have all that separate a syntax from a simple lambda function.

Here's another example: 

```
sub maybe-create-something(Int $x) {
  fail "Just kidding" if $x != 42;

  return $x;
}

with maybe-create-something(42) -> $x {
  say "Answer to life";
} else {
  say "Oops"
}
```

`with` basically is the Raku analogue of Rust's (and Swift's) `if let` and Clojure's `if-let`: it takes a possibly empty value and tries to assign it to a variable. But again here we see the lambda function 
syntax being applied to this context: `with maybe-create-something()` is creating a context in which a value is captured and being put into `$x` (potentially in this case).

One final example:

```
react {
  whenever Supply.from-list(1..10) -> $x {
    say $x;
  }
}
```

`react` and `whenever` basically help with concurrency. `react` basically launches a new (green?) thread that loops forever, `whenever` runs whenever the channel or supply on the left hand side of the arrow is provided a new value.
Again, we see the exact same lambda function syntax applied here. Nothing's changed except the context that `$x` is being pulled from. 


