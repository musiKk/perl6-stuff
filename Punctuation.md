# Perl 6 Punctuation

* [Motivation](#motivation)
* [Feed operators ==>, <==](#feed-operators--)

## Motivation

Again and again I forget what certain punctuation characters mean. Many are operators but not all so [the operators documentation](http://doc.perl6.org/language/operators) does not always help (and some are even mentioned but not documented). In addition this is notoriously hard to google. This is my attempt at collecting all punctuation that crosses my way.

## Feed operators ==>, <==

Allow to transform a chain of the form

    sort { ... }  map { ... }  grep { ... }  @array; # Perl 5
    sort { ... }, map { ... }, grep { ... }, @array; # Perl 6

to

    sort { ... } <== map { ... } <== grep { ... } <== @array;

    @array ==> grep { ... } ==> map { ... } ==> sort { ... };

thereby making the flow of data more explicit and allowing to write it more naturally from left to right.