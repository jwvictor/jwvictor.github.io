---
layout: category-post
title:  "Introducing Braids.jl"
date:   2018-02-05 12:00:56 -0800
categories: writing
---

I'm happy to announce the alpha release of [Braids.jl](https://github.com/jwvictor/Braids.jl), a braid theory library for Julia.

[Braid theory](https://en.wikipedia.org/wiki/Braid_theory) is an abstract mathematical study arising from the everyday notion of a braid -- just like in a girl's hair, or in the fibers woven together to form a rope. Braids, organized by the number of strands in the braid, form a [group](https://en.wikipedia.org/wiki/Group_(mathematics)) whose study has recently become of particular interest in cryptography, as some post-quantum encryption standards rely on the difficulty of problems in the [braid group](https://en.wikipedia.org/wiki/Braid_group).

A mathematical braid looks like this:

![](https://upload.wikimedia.org/wikipedia/commons/3/33/Braid_s3.png)

And the braid group is formed by taking these simple braids and stringing them together. Here's another braid:

![](https://upload.wikimedia.org/wikipedia/commons/3/31/Braid_s2.png)

Now, if we take the first one and attach it by its endpoints to the second, follow where the strings go and you'll see what we get is this:

![](https://upload.wikimedia.org/wikipedia/commons/e/e7/Braid_s3s2.png)

This is how we do math with braids. Emil Artin gave specific rules for the manipulation of braids, and those rules allow us to build results about braids without any string at all. For example, two braids can be considered "equivalent" if one can be deformed into the other without breaking any strings. Braid groups give us the ability to mathematically compute whether two braids have this property -- if they are "ambient isotopic," to use the lingo -- given the reduction of Dehornoy implemented in `Braids.jl`, which is based on Artin's rules of braids. 

Braids have been studied extensively as a possible base group for post-quantum cryptography. For example, the conjugacy search problem is (pretty) hard in braids. This problem is essentially, given two braids _a_ and _b_, find a braid _c_ such that composing _c_ then _a_ and then the mirror image of _c_ gives us precisely the braid _b_. It was for some time believed this problem was difficult enough to form the basis of a post-quantum cryptosystem, but algorithms have since been discovered that can attack "weak braids" and make the system not provably secure. 

It is an open question to what extent braids can be applied in cryptography -- and one I personally care a lot about, which is what motivated me to build `Braids.jl`. I hope a platform for manipulating braids on top of Julia -- which gives researchers the power of massively parallel computing if necessary, or near-C speed at the very least -- will help stimulate just that sort of research.

One other cool thing in `Braids.jl`: it turns out that braids can be "represented" (meaning, linearly) as a matrix of integer polynomials in _t_ and _t^-1_. Multiplying together the matrices of two braids will give a matrix corresponding to the product of the two braids. For braid groups on two and three strands, the mapping is one-to-one. For five and more strands, it is known that there are single matrices that correspond to two distinct braids -- in math lingo, the kernel is nontrivial, or the representation is not _faithful_. Interestingly, it remains an open question whether the representation is faithful for _n_ = 4. Perhaps some user of this library will be the one to figure it out!

_Updated September 2, 2020_