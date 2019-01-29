---
title: Hoist The Jolly Roger
tags:
---

## Hoisting Be Foisting

I recently stumbled across [this article](https://www.linkedin.com/pulse/preparing-front-end-web-development-interview-2017-david-shariff/) from David Shariff while I was looking for JavaScript exercises for my students at [Bloc](https://www.bloc.io/). His article details topics across the entire front end that are extremely important, but what caught my eye in particular were the JavaScript concepts. In my continuous quest for **JavaScript World Domination**&trade; I thought this list was a great representation of "topics that I sort of understand, but that have to Google when I'm pressed for details" on, so I'm going to work through the concepts one by one.

 Today I'm going to look at Hoisting. What I'm going to do[^1] is try to distill my own understanding of this topic into some useful examples that hopefully will make the topic clear. My goto for any grainy, tough JS subject is going to be first and foremost Kyle Simpson[^2] because his You Don't Know JS series does such a great job thoroughly breaking down these topics.

 This topic is dear to my heart because it sent me into a near existential crisis when I first learned about it. *What do you mean the JS engine just **moves** my code after I've written it?* past-Alex asked, increduous at the mere thought that things were happening underneath the surface.

When we talk about hoisting, we can talk about *Variable* hoisting and *Function* Hoisting, and so let's talk about them separately.

Case in point:

```js
console.log(x);
var x = 10;
```

This returns `undefined`. That is perplexing until you know that JavaScript .

### Key Takeaways

* Just use `let` and `const`
* Only function declarations are hoisted
* Functions are hoisted before variables

### Additional Reading:

* [^2]: [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch4.md)

### Footnotes

[^1]: And what pretty much all software blogs do