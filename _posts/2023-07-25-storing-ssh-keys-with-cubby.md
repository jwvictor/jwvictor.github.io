---
layout: category-post
title:  "How I Store Sensitive SSH Keys in Cubby"
date:   2023-07-25 12:00:56 -0800
categories: writing
---

_This article has been cross-posted from [Cubby](https://public.cubbycli.com/v1/post/jason/ssh-key-in-cubby/view)._

# How I put sensitive SSH keys in Cubby

_By Jason Victor_
July 25, 2023

One of the cool things about Cubby is you get total control of your encryption keys. In particular, you can create as many keys as you want and use them for different types of blobs/files to create segregation between resources of varying sensitivity.

## How cryptographic keys work in Cubby

In order to specify an encryption key, you provide Cubby with a human-readable word or phrase called the "passphrase." Cubby calculates an AES key from this passphrase using a specialized key derivation algorithm, which guarantees a bunch of favorable security properties for the keys. This way, instead of having to worry about copying key files around, you just have to remember the passphrase that generates a particular key.

Most of the time, this passphrase is provided by the `symmetric-key` config line set up in your `~/.cubby/cubby-client.yaml`. But if you want to interact with a particular blob using a different key, you can pass a custom encryption passphrase with the `-K` flag. (Alternatively, you can set the `CUB_CRYPTO_SYMMETRIC_KEY` environment variable or the `CryptoPass` setting in the Neovim plugin, i.e. `:CubbySet CryptoPass <passphrase>`.)

I encrypt most of my blobs with the passphrase configured in cleartext in my `cubby-client.yaml`. After all, that's totally secure as long as someone doesn't get into my filesystem. However, for my most security-sensitive blobs, I use alternative encryption passphrases that are passed to Cubby explicitly when I create or use them. This way, the compromise of my filesystem (and `cubby-client.yaml` passphrase with it) still leaves my most important data safe from prying eyes.

One of those sensitive things is the SSH keys that can access my AWS servers. So, here's a quick rundown on how I store those.

*IMPORTANT*: all of this assumes you do not leave your shell history on like a noob. Obviously, if your shell is recording everything you type, none of this will be secure, since a hacker that gets into your laptop can pull your passphrases from `.bash_history`. In Bash, you can do this with `set +o history`.

## My setup

I made a blob called `etc:ssh` to store all my ssh keys, and then I attach them all to that one blob, but using a different key than usual. 

To put a new key into the blob, you'd do something like:

```bash
cubby attach -K <passphrase> etc:ssh key.pem
```

Then, I can pull them down like:

```bash
cubby attachments etc:ssh -K <passphrase> -F <filename>
```

If you forget which keys are available, all you gotta do is:

```bash
cubby attachments etc:ssh
```

to list the attached filenames out.

And voila, `key.pem` is now attached with a custom passphrase to that blob. One other thing I like to do is put some reminders in `etc:ssh` -- reminders of the usernames for different keys, and of course a reminder to `chmod 600` the key after pulling it down. Anything I might need to remember when logging in using one of these keys.

Broadly, I prefer this setup to one child blob per key, simply because it's less typing. No deep technical reason. I don't need to edit these things so it wouldn't be useful to have them in blob bodies anyway.


## A plan for the paranoid

Now, when you start up a new container or cloud node or whatever, you can:

1. Run the Cubby install script.
2. Turn off bash history (or have it disabled by default).
3. Pull down whichever keys you need.
4. Delete them when you're done.

This is a good way to go for the truly paranoid. If that box gets compromised, these keys don't get swallowed along with it.

