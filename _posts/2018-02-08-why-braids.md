---
layout: category-post
title:  "Why braids? Quantum vs. crypto."
date:   2018-02-05 12:00:56 -0800
categories: writing
---

I recently [announced](https://jwvictor.github.io/writing/2018/02/05/introducting-braids-jl.html) the alpha release of [Braids.jl](https://github.com/jwvictor/Braids.jl), my new braid theory library for Julia. Admittedly, my introduction to the library was largely about the abstract meaning of braids.

After that post, a few people asked me, Jason, why would somebody interested in cryptocurrencies and machine learning care about the abstract topological theory of [braid groups](https://en.wikipedia.org/wiki/Braid_group)?

It's pretty simple after all, just:

![](https://upload.wikimedia.org/wikipedia/commons/3/33/Braid_s3.png) <sup style="font-size: 48px; margin-bottom: 42px; color: #3890df">+</sup> ![](https://upload.wikimedia.org/wikipedia/commons/3/31/Braid_s2.png) <sup style="font-size: 48px; margin-bottom: 42px; color: #3890df">=</sup> ![](https://upload.wikimedia.org/wikipedia/commons/e/e7/Braid_s3s2.png)

Here's why. (It's the cryptocurrency part.)

I don't talk much openly about my thoughts on cryptocurrencies, but I'm going to a cryptocurrency conference today so the subject is on the brain. Roughly, the **entire world of cryptocurrencies** -- or rather, a vast part of it -- is at imminent risk for major disruption by quantum computing. I'm not going to go through the detail of how quantum attacks work against different cryptosystems, or the mechanisms to defend against them (and they do exist), but suffice it to say, the cryptoverse would be a very different place if cracking ECC was as easy as 1-2-3. Indeed, most of the commonly used cryptosystems have a near-identical Achilles heel.

Now, what can we do that's different? Well, one thing we can do is use different base groups for the cryptographic trickery. In the case of RSA, for example, the base group is simply the integers: a particularly well-understood mathematical object, and one with a number of exploitable symmetries and curiosities also not worth going into here. A quantum computer can readily crack RSA, and in the next 3-6 years, we'll likely see the first quantum computer capable of cracking RSA in the key strengths deployed in the wild. Without going into details, they can crack pretty much everything we currently trust. In the very worst case, a quantum computer can use Grover's algorithm to do whatever a classical computer would do in square root the time complexity. And that's if it had to enumerate every possible solution, which usually is not the case with quantum computers. 

**And no, "change your address every time" is not quantum resistance. It makes it very hard to do business with transient addresses.**

We sometimes forget the practical in light of the theoretical, and so I don't blame that line of thinking one bit -- for many applications, Bitcoin could remain secure by adapting user flows. But, that's not a great user experience for many, if not most, applications.

One of the major lines of thinking to come out has been regarding the use of noncommutative groups instead of well-understood abelian (commutative) groups, like the integers. Obviously the groups have to be infinte in order for things to remain feasible, so noncommutative infinite groups are the key candidates. One that became of particular interest was precisely _Artin's braid group_, the subject of my Julia lib.

Now, soon after the early braid-based cryptosystems came out, weaknesses were found -- special cases of braids that didn't provide security, attacks based on solving easier problems in the linear space under some representation, and the like. 

Most of this early work was based on the presumed difficulty of the _conjugacy search problem_, a theoretical problem in which you essentially are given two braids _a_ and _c_, and need to find another braid _b_ such that braiding _b_ then _a_ and then the oppositive of _b_ gives precisely the braid _c_. It turns out, that problem can be solved _reasonably_ efficiently, and thus it is not the basis for a good cryptosystem.

The most prominent attack against the conjugacy search problem is based on so-called "super summit sets." This technique works _sometimes_, and works more than we'd like with braids believed to be secure against so-called "length attacks," but is by no means a provably efficient algorithm. The other main weakness is based on the discovery that the various representation of braids (as matrices of polynomials) gave rise to easier-than-expected solutions in many practical circumstances -- but of course, it remains an open question whether the resulting matrix-valued solutions can be readily inverted back to braid space. 

Other other proposals for post-quantum encryption based on braids have been made, such as those based on the difficulty of [so-called e-multiplication](https://www.securerf.com/wp-content/uploads/2017/01/SecureRF-GTDH-Quantum-Resistant-12-16.pdf), and it's possible even more cryptographic potential lies in braid groups. For example, the minimal braid problem is known to be co-NP -- could that problem be used as the basis for a quantum-resistant... _something_? It's widely believed that the square-root problem in braids is hard enough to be used for a cryptosystem, but exactly how is not yet unerstood.

In order to find out, we'll need to do more research into the practical difficulty of different problems in braids, and greater minds than I will likely be the ones to solve the problem. Hopefully, `Braids.jl` will give researchers and the open-source community a platform to experiment with braid-based cryptography.
