---
layout: category-post
title:  "Startup Lessons: The Decline of Scott Toilet Paper"
date:   2023-07-22 12:00:56 -0800
categories: writing
---

# How to take notes like a Chad with Neovim

_By Jason Victor_
_7/25/23_

If you want to get chicks _and_ take notes on the command line, then this post is for you. We're going to explain in detail how to set up the awesome cloud storage tool by the name of [Cubby](https://www.github.com/jwvictor/cubby), as well as its [Neovim plugin](https://www.github.com/jwvictor/nvim-cubby). 

With this setup, you'll be able to make end-to-end encrypted notes (called "blobs" in Cubby) that get stored in the cloud. These blobs are accessible from the command line or inside Neovim, they maintain full version histories, they can be grepped (even if encrypted), they can be published to the Internet with a single command, and they can be used with UNIX pipes just like local files. And, all of this can be done with the paranoid security profile a true Chad deserves, wherein no sensitive data is ever stored to disk. 

## Installing Cubby

The easiest way to install Cubby is to run the quick install script:

```bash
curl -s -S -L https://www.cubbycli.com/static/install.sh | bash
```

Now if you're a real Chad, and I know you are if you've read this far, you're probably sketched by this. Well fear not dear reader. A quick inspection will demonstrate that all this script does is download the Cubby binary for your architecture, copy it to `~/.cubby/bin`, and prompt you to set up your config files.

When prompted to create a new account, answer yes. Keep in mind you will be providing two separate credentials when you create your account: a _password_ and a default _encryption passphrase_.

The _password_ protects your cubby account -- i.e. the ability to list blobs and perform actions on unencrypted blobs. The default encryption _passphrase_ is a human-readable string from which a default encryption key will be derived. Unless you specify otherwise, this is the key that Cubby will use when reading/writing your blobs. (Later in this post, we'll see examples of how to override this passphrase when necessary.) 

Last step, restart your shell and the `cubby` binary should now be available in your `$PATH`.

## Installing nvim-cubby

The Neovim plugin for Cubby is in the Github repo `jwvictor/nvim-cubby`, and most Neovim plugin managers can download it directly from there. For example, if you're a Nvchad user (which, being a Chad, you probably are) you would list the repo under `default_plugins` in `~/.config/nvim/lua/plugins/init.lua`. 

## Playing with Cubby

To get our feet wet, let's start on the command line. First we'll make a parent blob (like a directory) to store the stuff from this tutorial. Let's call that blob `chad`:

```bash
$ cubby put chad
```

That wasn't that hard. Now let's take a look at our blobspace:

```bash
$ cubby list
```

You'll see there's only the one `chad` blob. So let's make some child blobs (like files in this new folder). But before we do this, let's make sure our editor is properly configured:

```bash
$ export EDITOR=nvim
```

And now, we'll put our first child blob into Cubby:

```bash
$ cubby put chad:hello-world
```

Since we didn't specify any encryption options, the defaults were used: the blob is encrypted with your default encryption passphrase. Let's open it up and put some data in there:

```bash
$ cubby get chad:hello-world
```

This will open your blob in `nvim`. Make some edits, and then save and quit. Your changes are now reflected in the blob.

But your hands might be getting tired typing these keys like `chad:hello-world` over and over. Fortunately, you can provide "prefix keys" to Cubby instead. Effectively, instead of typing `hello-world`, you can type just enough characters that it's unambiguous which blob you want. In our case, there are no other blobs starting with `h` under `chad`, so we could equally well do:

```bash
$ cubby get chad:h
```

And this works at all levels of the key! Since the `chad` parent blob is the only blob in the root of our blobspace beginning with `c`, we could simply use:

```bash
$ cubby get c:h
```

Cubby is smart enough to figure out that only one blob's path matches the prefixes "c" and "h".

Next, we may not always want to open blobs in an editor -- for example, if we're stringing the blob together with other UNIX commands. To take a quick peek at a blob's contents via standard out, run:

```bash
$ cubby cat chad:hello-world
```

Like UNIX `cat`, this will print the contents to standard output. And this brings up a good point: Cubby can serve as a drop-in replacement for files when using UNIX pipes. Let's replace the current contents of our blob with the output from a Linux command:

```bash
$ ls -l | cubby set chad:hello-world
```

Now if we `cat` the blob again, you'll see its contents have been replaced by the directory listing from `ls -l`.

But perhaps we don't like that output -- or we did something unintentionally. We can revert to the prior revision of this blob with:

```bash
$  cubby revert chad:hello-world
```

This will give you a list of revisions. Choose one of your earlier revisions and re-run `cubby cat`.

Now, until this point, we've been using the defaults for encryption. But the encryption is one of the best features of Cubby! So let's learn more about how to use the cryptography features.

First, let's convince you that this data is really encrypted as-is. To see the raw data that Cubby stores, you can run:

```bash
$  cubby cat -b=false chad:hello-world
```

The `-b=false` flag tells Cubby to return the _raw_ blob data, not just the decrypted body. You'll see something that looks like this:

```
  "id": "02c03935-490f-4ac0-bd53-89784da0a019",
    "title": "test",
    "type": "markdown",
    "data": "",
    "raw_data": [
        {
            "description": "",
            "data": "StJ4cwMFlbUTCM0Qot3gTwpQCijg0hrvLQt1pQZSLdNs8iszK1/FuSZBNegOuvauvom3xPU8IIJCpfrcnvW4k88elefbb8ptBQQ2ns1i5kWRorcNjr46CKGjfWGW8URq8f8WO5yqSYWRmS
/b72r5/9HdWfgG1pnfBe4iItiBmaiEYc4YwyP1ectOnifXMp8n7q9Nl2/oBo76uExE4/2yg8EbESKniQPk1eZudzN3NvepiJeO1nzsnpWi7SXaxm8dDpahm1KbioJV8i0zEwAfHU/3+H+GT00L0cOhdWoaedJ8LG3yd
rC3tl9VdSF9Ig/WKoZOBa4huVKzvcNuXz/NQ2B/xXhbXBwNNWJwBhiIVacRSSNRe1rJYT4y1d3ZTru0miBzGIr5sFSDV7i/fZd7rLxX4Y6v0aoCScZR27r5qwJukmAy5pl22FI4e1f2Hq3CdJHn3hoAlKiAtXYyM4uW
4Yowej0YcYiX7xdOfNSz0NdSYm/yKxBXVus=",
            "type": "encrypted_body"
        }
    ],
    "importance": 0,
    "tags": [],
    "deleted": false,
    "owner_id": "224a1216-76f0-4d27-8867-251ac7b79ce1",
    "version_history": [

...
```
As you can see, our data has been encrypted and stored as base64.

To override the encryption passphrase used for interacting with a blob, we can use the `-K` flag. Let's try making a blob encrypted with the alternative passphrase `suchachad`:

```bash
$ cubby put -K suchachad chad:other-key
```

Now, if you try to `cat` this blob normally, you'll get a bunch of garbage data. Instead, you need to specify the blob's encryption passphrase like so:

```bash
$ cubby cat -K suchachad chad:other-key
```

But what if we want to store a blob with no encryption at all, say, to share with others? In that case, we disable encryption entirely by passing `-C none`:

```bash
$ cubby put -C none chad:hello-public-internet
```

This will make an _unencrypted_ blob. Why would we want to make a blob unencrypted? As mega-Chad 10x engineers, we want to share our brilliant thoughts with the world. So instead of messing around with blogging platforms for the technical peasantry, we're just going to share a Markdown blob from the command line, and Cubby will render it as HTML for us. Let's start by putting some markdown in our blob:

```bash
curl "https://raw.githubusercontent.com/jwvictor/nvim-cubby/main/README.md" | cubby set chad:hello-public-internet
```

So that took the Markdown from the nvim-cubby README file and sent it into our unencrypted blob. To publish this for real, simply run:

```bash
$ cubby publish put chad:hello-public-internet
```

And you get:

```
No permissions provided - defaulting to public...
Published successfully, getting URL...
Web: https://public.cubbycli.com:443/v1/post/jason/hello-public-internet/view
API: https://public.cubbycli.com:443/v1/post/jason/hello-public-internet
```

Now if you go to that web link, you'll see your post rendered out as HTML. Amazing!

But what happens if we share an encrypted blob? Let's find out. Our `chad:other-key` blob is encrypted with the key `suchachad` -- not our default passphrase. This means we can safely give that passphrase to intended recipients of the blob.

So let's publish this blob:

```bash
$ cubby publish put chad:other-key
```

Now, follow the web link as before, and you'll see that the post page is requesting a passphrase. If you provide the correct passphrase, the data will be decrypted inside your browser and rendered as HTML. This is because Cubby blobs are truly end-to-end encrypted, and the page you're looking at only has access to the ciphertext for your post.

There are many other interesting features in Cubby, but this should be enough for you to clown on the beta male `nano` users.

## Using Cubby with Neovim

So now you're probably convinced that a true uber-Chad will use Cubby for everything from taking notes to blogging to storing crypto keys or passwords. But what's this about Neovim, the official editor of Chads everywhere?

The `nvim-cubby` plugin you installed allows you to interact with Cubby from inside Neovim. To see a listing of your blobs, run: 

```bash
:CubbyList
```

Look through this list and put your cursor on the row of the `hello-public-internet` blob. Now, run: 

```bash
:CubbyGo
```

The plugin will determine the ID of the blob on the row you've selected and open it. Now that it's open in a new buffer, you can edit it, and when you want to commit your changes, run:

```bash
:CubbySave
```

If you want to open a blob directly based on its key in Neovim, you can use:

```bash
:CubbyGet <key>
```

This will open the blob in a new buffer. As before, you can use `:CubbySave` to commit your changes up to the server.

But what if you have certain blobs that use special encryption passphrases? Easy -- just set the `CryptoPass` config variable like so:

```bash
:CubbySet CryptoPass suchachad
```

Now, when we go to open or write an encrypted blob, Cubby will use the passphrase "suchachad" from earlier.

By default, our blobs have been opening in a new buffer, which is created by invoking `enew` in Neovim. But what if we want to set `nvim-cubby` to open in a skinny vertical pane instead? Then we simply do:

```bash
:CubbySet OpenWith 25 vnew
```

Any command that can open a new buffer can be used with the setting `OpenWith` to customize where your blobs are displayed in Neovim. Here, we're using `25 vnew`, which creates a vertical pane 25% of the screen width wide.

## Chad-grade security

But what's _really_ cool about nvim-cubby, at least from the perspective of an uber-Chad, is the security properties. Many of us 10x ninjas work for companies that have paranoid, invasive security policies, which results in 23-year-old IT interns examining any kind of personal information that comes across the wire. Cubby can't help much with social media -- but it helps a lot with things like personal notes, credentials, keys, and so on.

Cubby (as used with `nvim-cubby`) provides a solution to this problem by (i.) end-to-end encrypting your data and (ii.) only storing the decrypted plaintext in RAM, never on your filesystem.

This means that any kind of crazy company spyware on your work computer won't be able to see any of the stuff you work on in Cubby. (Just be sure to follow the instructions for Cubby opsec on [my blog](https://jwvictor.github.io), e.g. don't leave your BASH history on while typing passphrases.)

## Conclusion

So if you're ready to take the dive into managing your personal data like an actual boss and not a neophyte, I encourage you to check out Cubby. It has all the sauce your tech-savvy user needs without any of the headaches. And best of all, it's 100% free and open-source and always will be.
