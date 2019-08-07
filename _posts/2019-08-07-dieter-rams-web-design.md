---
layout: post
title:  "Applying Dieter Rams' design principles on the web"
---

The iconic industrial designer [Dieter Rams](https://en.wikipedia.org/wiki/Dieter_Rams) has ten principles of good design [[1](https://web.archive.org/web/20120904120034/http://www.sfmoma.org/about/press/press_exhibitions/releases/880)]. During his career, these were meant for physical products (going out on a limb and assuming he didn't dabble in software), but I figure they should be just as applicable on the web.

I am not a designer, so my knowledge is limited. These notes are my attempt at condensing his principles into rules that are explicit (but general) and easy to follow, without any particular design knowledge.

#### Good design is innovative

Without innovation, there is no progress. Web design should always strive to be simpler and more inclusive for both people and platforms, but take care to not compromise usability. Design should be instinctive to convey understanding and usability.

#### Good design makes a product useful

For me, this boils down into two categories: accessibility and information density. A website is useful, if it 1) can be used by everyone [[2](https://developer.mozilla.org/en-US/docs/Learn/Accessibility)] and 2) conveys its purpose efficiently and  pedagogically. 

#### Good design is aesthetic

I would argue that this comes naturally, if following the rest of the principles. Form follows function.

#### Good design makes a product understandable

The user should, when your website has just finished loading, understand at a glance of what can be done on your site and where it can be done. Every element's function should be obvious. User flow should be logical, intuitive, and follow a consistent pattern.

#### Good design is unobtrusive

Fast load time, no subscription popups, no obtrusive ads, no autoplay, no bloated javascript. Convey what the user wants, nothing else.

#### Good design is honest

- Don't hide your cookie tracking behind a maze of clicks when displaying your GDPR notice (or preferably, don't track cookies at all)
- Be mindful of dark patterns and make sure you never use them [[3](https://en.wikipedia.org/wiki/Dark_pattern)]

#### Good design is long lasting

Simplicity > innovation. Keep it simple, rather than forcing yourself to innovate (and later revert).

#### Good design is thorough down to the last detail

Have a coherent design: pick **one** color scheme, pick **one** set of icons, pick **one** set of buttons. Frameworks are great if you don't want to spend time on this - their creators have been thorough so that you don't have to (assuming you are consistent in using them).

#### Good design is environmentally friendly

Apart from choosing whether your website runs on oil or sun, the best you can do is to not bloat (serverside and clientside).

#### Good design is as little design as possible

Focus on what your site does. Remove everything else that steals attention and requires unnecessary thought.
