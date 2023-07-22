---
layout: category-post
title:  "Javascript Futures and Monads Belong Together"
date:   2023-07-21 12:00:56 -0800
categories: writing
---

We recently posted a little code snippet on the [Toggle](https://www.hellotoggle.com) devlog explaining why we like using a simple Either (a.k.a. "Result") monad with our Node.js futures: it makes it much cleaner to build long chains of asynchronous processing in a way that is idiomatic and terminates early in the vent of errors.

We'll update this page if we put it up on a Github gist, but for now you can read the whole post and get the code for the Node.js Result monad here: 

[Read full article](https://www.hellotoggle.com/devlog-javascript-futures-and-result-monads)
