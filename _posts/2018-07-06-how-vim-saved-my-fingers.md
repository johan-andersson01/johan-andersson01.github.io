---
layout: post
title: 'How Vim saved my fingers'
---

At work I am currently migrating a large codebase from Bootstrap 3 to Bootstrap 4. There are a lot of small changes that make this a daunting task and one of those is the change to breadcrumbs.

In the codebase, there were 39 instances (each in a separate file) of Bootstrap's [breadcrumb](https://getbootstrap.com/docs/4.0/components/breadcrumb/), with a total of 127 `li` elements inside.

In Bootstrap 3, the following code would work:

```
<ol class="breadcrumb">
    <li><a href="/home/johan/">Home</a></li>
    <li><a href="/home/johan/code/">Code</a></li>
</ol>
```

In Bootstrap 4, however, a tiny change has been introduced that completely breaks this layout. In the new version, children of `breadcrumb` are required to have the class `breadcrumb-item` like so:

```
<ol class="breadcrumb">
    <li class="breadcrumb-item"><a href="/home/johan/">Home</a></li>
    <li class="breadcrumb-item"><a href="/home/johan/code/">Code</a></li>
</ol>
```

In migrating from Bootstrap 3 to 4, this was definitely not the hardest change I had to do. But it was here I first discovered the magic and wonders of Vim. Since it seemed such a mind-numbing task to do, I had to figure out how I could automate at least some part of it, lest I wished to become a zombie.

So what did I do? To the experienced Vim user it comes as no surprise that I used [macros](http://vim.wikia.com/wiki/Macros).

Macros let you record keystrokes, so that you can repeat patterns of editing. In this case, I wanted to record a macro that updates a `breadcrumb` from Bootstrap 3 to 4.

Having opened the file I did the following:

1. `/breadcrumb<E>` (jump to the first occurrence of "breadcrumb")
2. `j` (go down one line)
3. `qq` (start a recording and bind it to q)
4. `0` (go to line beginning)
5. `/li<E>` (jump to the first occurrence of "li")
6. `ea` (go to the end of "li" and start appending text)
7. `<space> class="breadcrumb-item"` (insert the change)
8. `<Esc>` (exit append mode)
9. `q` (finish recording)

Having recorded the macro, it was time to use it. With two terminal windows open in split screen, one with Vim and one with the output of `rg breadcrumb`, I went through all files and performed the upgrade. To switch files inside Vim, and thus retaining my macro, I used [fzf](https://github.com/junegunn/fzf) which I had bound to `<s-f>` (in the following demonstration I used `:find` instead, but it's practically the same).

*(In reality, I recorded two macros: the one below, and another that changed `li class="active"` element into `li class="breadcrumb-item active"`)*

<script src="https://asciinema.org/a/QYVOm17UhbYKaVr5VDcQGdILi.js" id="asciicast-QYVOm17UhbYKaVr5VDcQGdILi" async></script>

I performed the above sequence one time, and then used it in each of the 39 files, 127 times in total, which understandably sped up the process while preventing it from becoming a boring task.

While I didn't manage to do it with one keystroke per file or with one keystroke for all files (there is however a [tradeoff](https://xkcd.com/1205/)), I am still giddy that I managed to reduce the cognitive load of updating the `breadcrumbs`.

It was a defining moment for me, since it was here I caught my first glimpse into why and how Vim is more powerful than the regular editors that I have previously used.


---

I also learned (accidentally) that you can make recursive macros. I have yet to figure out why you would want to do so though.

<script src="https://asciinema.org/a/RvPUqeg4X1lNe8lJr0JwqITt3.js" id="asciicast-RvPUqeg4X1lNe8lJr0JwqITt3" async></script>
