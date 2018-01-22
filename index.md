#Removal of Nullable Prefixes and Suffixes in ICGrep

##Introduction

I chose to take a look at the **RemoveNullablePrefix/Suffix** passes made by ICGrep.
Both functions perform very similar tasks so I will focus on **RemoveNullablePrefix** for simplicity although the statements can easily be applied to **RemoveNullableSuffix**.

##What it does

**RemoveNullablePrefix** looks for leading repetitions and will reduce them to the
smallest repetition needed to perform the same function.

A RE such as ````a*bc```` can be replaced with ````bc````. The latter will match
the same texts but instead of starting at the first a, it will start to match
at the b.

\t **aaaabc** -> aaaa**bc**

In general, a leading repetition of the form ````a{min, max}```` will be
replaced by ````a{min, min}````

For example matching ````a{2,4}bc```` is equivalent to matching ````a{2,2}bc````

\t **aaaabc** -> aa**aabc**
