# Perl 6 Punctuation

* [Motivation](#motivation)
* ==>, <== [Feed Operators](#feed-operators)
* { ... } [Block](#block)
* -> { ... }, -> $foo { ... } [Pointy Block](#pointy-block)
* <-> $foo { ... } [Double-Pointy Block](#double-pointy-block)
* |(1, 2, 3), |@foo [List Flattening](#list-flattening)

## Motivation

Again and again I forget what certain punctuation characters mean. Many are operators but not all so [the operators documentation](http://doc.perl6.org/language/operators) does not always help (and some are even mentioned but not documented). In addition this is notoriously hard to google. This is my attempt at collecting all punctuation that crosses my way.

NOTE: All code samples are Perl 6 except where noted.

## Feed Operators

    ==>
    <==

Allow to transform a chain of the form

    sort { ... }  map { ... }  grep { ... }  @array; # Perl 5
    sort { ... }, map { ... }, grep { ... }, @array; # Perl 6

to

    sort { ... } <== map { ... } <== grep { ... } <== @array;

    @array ==> grep { ... } ==> map { ... } ==> sort { ... };

thereby making the flow of data more explicit and allowing to write it more naturally from left to right.

Source: https://perl6advent.wordpress.com/2010/12/10/day-10-feed-operators/

## Block

    { ... }

Executable raw block of type [`Block`](http://doc.perl6.org/type/Block). Executed immediately when used as a statement similar to other C like languages. Execution can be enforced with `do { ... }` when used as part of an expression.

Can be used for control expressions such as `if` and `while`.

Source: http://doc.perl6.org/language/control, http://design.perl6.org/S06.html#Blocks

## Pointy Block

    -> { ... }
    -> $foo { ... }

Hybrid between anonymous sub and raw blocks. Can optionally declare parameters without parentheses. Creates an anonymous sub but can be used anywhere where a raw block may be used such as in `if` and `while` expressions.

Can declare multiple parameters in a `for` loop which binds successive values to the variables. Total number of elements looped over has to be a multiple of the number of parameters:

    for ^10 -> $a, $b { say $a, $b } # works
    for ^10 -> $a, $b, $c { say $a, $b } # fails during last iteration

Declared parameters are by default `readonly`, use [double-pointy blocks](#double-pointy-block) for a default of `rw`.

Source: http://design.perl6.org/S06.html#%22Pointy_blocks%22

## Double-Pointy Block

    <-> $foo { ... }

Same as [pointy blocks](#pointy-block) but adds the `rw` trait to all parameters by default. The following pointy blocks are thus equivalent:

    -> $foo as rw { ... }
    <-> $foo { ... }

Source: http://design.perl6.org/S06.html#%22Pointy_blocks%22

## List Flattening

    |(1, 2, 3)
    |@foo

In contrast to Perl 5 lists don't automatically flatten. If you want to flatten a list into the surrounding list, put a pipe in front of it. Flattening is only done one level deep, it is not recursive.

    (1, (2, 3)) # Perl 5: (1, 2, 3)

    (1, (2, 3))       # Perl 6: (1, (2, 3))
    (1, |(2, 3))      # Perl 6: (1, 2, 3)
    (1, |(2, (3, 4))) # Perl 6: (1, 2, (3, 4))

This also works for parameter lists:

    say (1, 2, 3)  # (1 2 3)
    say |(1, 2, 3) # 123

Technically the pipe is equivalent to creating a [`Slip`](http://doc.perl6.org/type/Slip).

Source: http://doc.perl6.org/language/list