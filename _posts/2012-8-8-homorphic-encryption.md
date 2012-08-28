---
layout: post
title: Homomorphic Encryption
tags: [Encryption]
date: 2012-08-08
---

{{ page.title }}
================
<p class="meta">8 August 2012</p>

I'm no expert on encryption, but I find the concept of homomorphic encryption very interesting. You can have remote computers perform computation on data, without giving them the plaintext.

From what I've read, to provide arbitrary operations on data we must provide addition and multiplication of the plaintext as primitives. If we work with individual bits addition corresponds to XOR and multiplication corresponds to AND (modulo 2).

So just provide NAND or NOR and be done with it. With NAND we can have:

| NOT x   |=| x NAND x                |
| x OR y  |=| (NOT x) NAND (NOT y)    |
| x AND y |=| NOT (x NAND y)          |
| x XOR y |=| (x OR y) AND (x NAND y) |

I propose encrypting the data client side using a the equivalent of a one-time pad. The client keeps a look-up table where-in we encode values that consititute a 0 and a 1. We send the server a look-up table that provides our function as a map of inputs to output.

One downside is that we only have binary values, and complex results will be a series of 1s and 0s. Any attacker can clearly see which values are the same, and which are not. This means there are only two possibilities, for the plaintext, the plaintext and it's negation. So we've only hid one bit of information (which is easily checked by seeing which possible plaintext is intelligible.)

To encode more we can shift to using larger values (say 32bit integers), but problems still persist. Also, an attacker can examine the look-up tables to determine the relations between the values to break our encoding.

As an alternative (but without ruling out a move to larger values) we can add redundancy to our encoding. Rather than having a unique value for 1 and 0, we can have several encodings for them. This means our NAND look up table will be able to hide which values encode 0 even though there is only one entry for 0[^1]. 

One attack, still possible against NAND (and NOR) tables, is that x NAND x is NOT x. As such, I suggest for each x we pick one x' where x NAND x = x' and x' NAND x' = x. Otherwise an attacker could still segregate 0 values from 1 values.

The remaining problems are:

* how to select secure look-up tables (preventing the attacker from determining their values by how they are connected in the look-up table.)
* how to encode the actual programs
* how to loop (A rather big sticking point of the program representation)
* how to compress look-up tables, especially if we look to larger values

I suggest programs be encoded by providing a list of inputs, and then how they should be placed together. This may or may not need to be secure (perhaps protecting the data is enough.) But if the program is known perhaps that would allow an attacker more information about the data (or data + result) than they really need to.


As an extension, each encoding could count the number of operations performed on it (modulo some limit). This would be a way to verify the correct number of operations occured. 

Binary functions as dispatch
----------------------------

We can also think of binary binary operations[^2] as being a dispatch of the first argument to a unary binary operation on the second argument. The unary binary operations are: false, true, not and identity. This of course generalises, to functions of n inputs.

| input | false | true | not | identity |
|:-----:|:-----:|:----:|:---:|:--------:|
|     0 |     0 |    1 |   1 |        0 |
|     1 |     0 |    1 |   0 |        1 |

NAND and NOR then become a dispatch:

| lhs |      NAND |       NOR |
|:---:|----------:|----------:|
|   0 |  true rhs |   not rhs |
|   1 |   not rhs | false rhs |

Both consist of not, and a constant. The constant is not arbitrary, as it has to be the value that chooses the not path.

[^1]: When adding redundant encodings there will certainly be more 0s.
[^2]: That is operations over binary digits, that have two operands.
