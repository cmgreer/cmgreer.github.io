# Removal of Nullable Prefixes and Suffixes in ICGrep


## Introduction

I chose to take a look at the **RemoveNullablePrefix/Suffix** passes made by ICGrep.
Both functions perform very similar tasks so I will focus on **RemoveNullablePrefix** for simplicity although the statements can easily be applied to **RemoveNullableSuffix**.

## What it does

**RemoveNullablePrefix** looks for leading repetitions and will reduce them to the
smallest repetition needed to perform the same function.

A RE such as ````a*bc```` can be replaced with ````bc````. The latter will match
the same texts but instead of starting at the first a, it will start to match
at the b.

  ``aaaabc`` -> aaaa``bc``

In general, a leading repetition of the form ````a{min, max}```` will be
replaced by ````a{min, min}````

For example matching ````a{2,4}bc```` is equivalent to matching ````a{2,2}bc````

  ``aaaabc`` -> aa``aabc``

## Benefits

By shortening or completely removing leading and trailing repetitions in a
regular expression, the matching algorithm will have less work to do and will
run faster.

## Shortcomings

Some information from the original regular expression is lost.

Some grep programs will highlight the matching portion of a string in the
output, since ICGrep uses a modified regular expression, it is
non-trivial to highlight output as the user would expect.

## Further Optimizations

When given nested leading and/or trailing repetitions, **RemoveNullablePrefix/Suffix**
does not always fully reduce a regular expression.

For example the RE: ````(a{2,5}b*)+c```` will produce the following results:

````
Parser:
(Seq[Rep(Name "\1" =((Seq[Rep(CC "CC_61" [97]/Unicode,2,5),CC "CC_62" [98]/Unicode])),1,Unbounded),CC "CC_63" [99]/Unicode])
...
RemoveNullablePrefix:
(Seq[Name "\1" =((Seq[Rep(CC "CC_61" [97]/Unicode,2,5),CC "CC_62" [98]/Unicode])),CC "CC_63" [99]/Unicode])
````
