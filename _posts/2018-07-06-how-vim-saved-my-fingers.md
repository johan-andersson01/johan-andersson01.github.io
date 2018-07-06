---
layout: post
title: 'How Vim saved my fingers'
---

At work I have set out to migrate a large codebase from Bootstrap 3 to Bootstrap 4. There are a lot of small changes that make this a daunting task. One example is breadcrumbs.

In the codebase, there were 39 instances (each in a separate file) of Bootstrap's [breadcrumb](https://getbootstrap.com/docs/4.0/components/breadcrumb/).

In Bootstrap 3, the following code would work:

```
<ol class="breadcrumb">
	<li><a href="/home/johan/">Home</a></li>
	<li><a href="/home/johan/code/">Code</a></li>
</ol>
```

However, in Bootstrap 4, a tiny change has been introduced that completely breaks this layout. In Bootstrap 4, children of `breadcrumb` need to belong to the class `breadcrumb-item` like so:

<ol class="breadcrumb">
	<li class="breadcrumb-item"><a href="/home/johan/">Home</a></li>
	<li class="breadcrumb-item"><a href="/home/johan/code/">Code</a></li>
</ol>

In migrating from Bootstrap 3 to 4, this was not the hardest change. But it was a mind-numbing change to do. If I had to go through each file and manually replace these instances, I would have become a zombie.

So what did I do? To the experienced Vim user it comes as no surprise that I used [macros](http://vim.wikia.com/wiki/Macros).

Macros let you record keystrokes, so that you can repeat patterns of editing. In this case, I wanted to record a macro that updates a `breadcrumb` from Bootstrap 3 to 4.

Having opened the file I did the following:

1. /breadcrumb<E> (jump to the first occurrence of "breadcrumb")
2. j (go one line down)
3. qq (start a recording and bind it to q)
4. 0 (go to line beginning)
5. /li<E> (jump to the first occurrence of "li")
6. ea (go to the end of "li" and start appending text)
7. <space> class="breadcrumb-item"
8. <Esc> (exit append mode)
9. q (finish recording)

I performed the above sequence one time, and then used it for all 39 files, which sped up the process immensely and also made it fun since I was learning more about Vim.

Having recorded the macro, it was time to use it. With two terminal windows open in split screen, one with vim and one with the output of `rg breadcrumb`, I went through all files and performed the upgrade. To switch files inside Vim, and thus retaining my macro, I used [fzf](https://github.com/junegunn/fzf) which I had bound to shift+f.



