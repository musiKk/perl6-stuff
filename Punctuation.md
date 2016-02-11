# Perl 6 Punctuation

* [Motivation](#motivation)
* ==>, <== [Feed Operators](#feed-operators)
* { ... } [Block](#block)
* -> { ... }, -> $foo { ... } [Pointy Block](#pointy-block)
* <-> $foo { ... } [Double-Pointy Block](#double-pointy-block)
* |(1, 2, 3), |@foo [List Flattening](#list-flattening)
* $foo: [Method Invocant](#method-invocant)
* method foo(Foo:) { ... } [Invocant Type](#invocant-type)
* -->: [Return Specification](#return-specification)

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

## Method Invocant

    $foo:

Allows one to specify the invocant of a method. This can be used to give another name to the invocant than `self`. `self` is available in both cases.

    class Foo {
        method foo { say self }
        method bar($me:) { say $me }
        method baz($me:) { say self }
    }
    # produce the same output
    Foo.new.foo;
    Foo.new.bar;
    Foo.new.baz;

Source: http://doc.perl6.org/type/Signature#Parameter_Separators

## Invocant Type

    method foo(Foo:) { ... }
    method foo(Foo: $arg1) { ... }

Allows to specify the type on which the method is invoked on. This is usually the type the method is defined in but can be any other type as well. This is sometimes done for the documentation to make things explicit but not actually needed in code. Note that the first argument is not separated by a comma.

## Return Specification

    sub foo($foo, --> Foo) { ... }
    my @foo[--> Foo];

Allows to specify the return type of a routine and is therefore an alternative syntax to

    sub foo($foo) returns Foo { ... }

Allows the creation of typed arrays and is therefore an alternative syntax to

    my Foo @foo;

Source: http://design.perl6.org/S06.html#Named_subroutines, http://design.perl6.org/S09.html#Typed_arrays
