---
layout: category-post
title:  "Why I Wrote Cubby"
date:   2022-06-29 12:00:56 -0800
categories: writing
---

_This article was cross-posted from [Cubby](https://public.cubbycli.com/v1/post/jason/note-from-the-author/view)_

## Why I wrote Cubby 

*By Jason Victor* [Last updated: June 29, 2022 at 8am]


[Cubby](https://github.com/jwvictor/cubby) was written, first and foremost, to solve my personal needs. The default Cubby server now
publicly available at `public.cubbycli.com` was previously for personal use only. But as I've
used Cubby more and more, I've come to think that it's worth sharing with the world in hopes that you
all find it useful as well.

I wanted a way to interact with my personal data -- notes, to-do lists, config files, and so on -- that was fast, efficient, end-to-end encrypted, and centered around
a command-line interface. To be clear, I don't mean a CLI _utility_, a companion to some larger app
or service, but rather an experience designed from the ground up for the command-line. With Cubby, all
my personal data is just a few keystrokes away; my default editor is the highly-powered `nvim`, which
lets me edit my stuff -- again -- _efficiently_. With the help of macros and buffers and yanks.

Likewise, important files, such as cryptographic keys and identity files, are immediately available
at every computer I have. Simply running

```bash
cubby get bashrc 
cubby get sshkeys 
```

pulls down these critical pieces of data that I need to copy to every computer, cloud server, and container I run.

Aside from being an extremely fast an efficient way to work with personal data, Cubby is useful for
managing _personal_ notes in which you may sometimes mention matters from work. For example, you remember
something to do for a client at work, and you want to write "get back to client X" on your to-do
list. Exposing that information to the eyes of whatever service provider you're using _could_ violate
your company's security policies.

So, Cubby encrypts your data _at the source_ and only sends ciphertext up to the server. When you
want to view the data, Cubby pulls down the ciphertext and uses a decryption key you provide to
decrypt the data locally. As such, the name of "client X" was never exposed to Cubby's servers,
keeping you safe if you need to mention confidential information.

The other problem of mine that Cubby solves is sharing. Sharing covers everything from sharing a
password or cryptographic key with a single coworker to publishing a blog post to the entire world.

I've often felt that the mere inertia of blogging and the pressure to compile a suitable "blog"
to host my _oeuvres_ discouraged me from publishing my thoughts to the world. With Cubby,
any Markdown blob you store can be swiftly published with: 

```bash
cubby publish put blog:hello-world 
```

A unique URL is generated that you can share with the world, where readers can view your
(now-rendered) Markdown post in all its glory.

And the same applies for sharing secrets with other people. You can set a custom encryption
key for a blob containing a password with `-K` and share it with your coworker using:

```bash
cubby publish put -r my.friend@gmail.com secrets:crypto-key
```

As long as your coworker has a Cubby account and
you tell them the encryption passphrase you used (if any), they will be able to pull down
the ciphertext and decrypt it.

This base set of features lets me build all sorts of interesting little tools and workflows
that make my job easier. It allows me to keep myself organized and provides me with a
"cloud filesystem" of sorts that I can tightly integrate into existing scripts and
workflows -- and one that, importantly, allows me to use the tools with which I'm most
efficient.


