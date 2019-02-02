---
title: Hoist The Jolly Roger
tags: 'JavaScript, ES6'
date: 2019-02-02 06:13:13
---


I recently stumbled across [this article](http://davidshariff.com/blog/preparing-for-a-front-end-web-development-interview-in-2017/) from David Shariff while I was looking for JavaScript exercises for my students at [Bloc](https://www.bloc.io/). His article details topics across the entire front end that are extremely important, but what caught my eye in particular were the JavaScript concepts. In my continuous quest for **JavaScript World Domination**&trade; I thought this list was a great representation of "topics that I sort of understand, but that have to Google when I'm pressed for details" on, so I'm going to work through these concepts one by one to solidify my understanding.

## Is Hoisting Foisting You?

Today I'm going to look at Hoisting. What I'm going to do is distill my own understanding of this topic into some useful examples that hopefully will make the topic clear. The worst case scenario is this only helps me, but maybe you'll find some benefit from these examples as well.

My goto for any grainy, tough JS subject is going to be first and foremost [Kyle Simpson](https://me.getify.com/) because his *You Don't Know JS* series does such a great job thoroughly breaking down these topics. Here are the selections that give the best background for what I'm going to discuss below:

* [Hoisting](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch4.md)
* [ES6 Syntax](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20%26%20beyond/ch2.md)

 What I like to do when I'm bulking up on a topic is to go through his chapter on the relevant subject matter, then recreate all of his (very dry) examples with something that resonates more with me.[^1] I definitely recommend this approach for everyone. It's a great way to start to own the material and to get your dang fingers on the keyboard.

If you find that you enjoy Kyle's style and approach, you might consider picking up the [whole series](https://www.amazon.com/gp/product/B01AY9P0P6/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B01AY9P0P6&linkCode=as2&tag=rolfe-20&linkId=363b17d9b041fe4e217621bdfbe2cee7).  
(I'm trying out the Amazon Affiliate program, so if you purchase the book from that link, I'll get something like 80 cents!)

## Hoisted By My Own Petard

This topic is dear to my heart because it sent me into a near existential crisis when I first learned about it. *What do you mean the JS engine just **moves** my code after I've written it?* past-Alex asked, incredulous at the thought of things happening underneath the surface. **JavaScript *moves* my code around while it runs?!**

No.

Well, not in the way that the term *hoisting* suggests. But it is a useful way to think about this behavior. You'll see the phrase "hoistable" pop up a lot if you go read the ECMA specs for JavaScript[^2], so even the internal team finds the metaphor useful. But what actually happens is a little more nuanced. It warrants a whole post in of itself (see some of the Additional Reading below), but a sufficient summary might be: *JavaScript pulls declarations, but not initializations into memory before executing a block of code*

When we talk about hoisting, we can talk about *Variable* hoisting and *Function* Hoisting. They behave pretty similarly, but there are a few distinctions to keep in mind, and so let's talk about them separately. Crack open your favorite [REPL](https://jsbin.com) environment and let's dive in.

### Variable Hoisting

The first thing to keep in mind when we talk about hoisting is *declaration* versus *initialization*. Typically, you see these steps combined:

```js
var dog = 'Fido';
```

In the line above, we both declare (`var dog`) and initialize (`dog = 'Fido'`) the variable dog. We don't usually think of these are separate steps, but from the perspective of the JS compiler, they are indeed distinct.

Case in point, what logs to the console here?

```js
console.log(dog);
var dog = 'Fido';
```

What about in this essentially identical question, presented with a little bit more of a cheeky setup?

```js
function bar() {
  var baz = 10;
  console.log(foo);
  var foo = baz;
}

var foo = 11;

bar();
```

In both examples, the same principles are at play.

#### Hulk Smash

If you run the above code, you'll find that it returns `undefined` in both cases. This is where we use the "hoisting" metaphor to explain this behavior. Imagine that JavaScript literally *pulls apart* your variable assignments and then *hoists* the declarations up to the top of the scope that they are introduced in.

I like to imagine The Incredible Hulk leaping onto the scene, grabbing hold of your delicate variable, and rending it in two, tossing one half, the declaration, high up onto a building (the top of the block of scope you are in), before bounding off in search of other variables to harass.

The first examples becomes:

```js
var dog;
console.log(dog);
dog = 'Fido';
```

If you think back to how variables behave in JS, you might remember that *any declared but unassigned variable is assigned the data type `undefined` until it is initialized*.

Try the same exercise with my second example. You should come up with the following:

```js

var foo;

function bar() {
  var baz;
  var foo;
  baz = 10;
  console.log(foo);
  foo = baz;
}

foo = 11;

bar();

```

Notice that our hoisted variable declarations are brought to the top of their respective scopes.

### Function Hoisting

Above, we covered the fact that variable declarations are "hoisted" to the top of their scope, but what about functions? At a glance, it seems like functions don't follow this rule of "process the declaration before the initialization."

Put another way, why does this work?

```js
proclaimThisFact("Pizza should not have eggs on it");

function proclaimThisFact(fact) {
  console.log(`${fact}!`);
}
```

Shouldn't we get an error of some sort? Maybe something telling us that `proclaimThisFact` is not a function? Why can we use this function before it is defined?

Remember that there are several ways to define a function in JavaScript. One way, used above, is called a **function declaration**. Alternatively, we could have written our function like this:

```js
proclaimThisFact("Pizza should not have eggs on it");

var proclaimThisFact = function (fact) {
  console.log(`${fact}!`);
}
```

This is called a **function expression**. Armed with your refined knowledge of how variable declarations behave, what do you think the difference is in behavior if you run my second example, modified to use a function expression?

It blows up! Just like we expected the first example to. And pay close attention to the error you get here. It's a `TypeError`, not a `ReferenceError` like you might expect. (To see the difference, try to call a function that doesn't exist at all: `defamePasta("linguini")`).

The error comes from the fact that we are asking our variable to be a function before it has been initialized as one:

```js
var proclaimThisFact; // proclaimThisFact is undefined
proclaimThisFact("Pizza should not have eggs on it"); // JS Engine: "lol wut is this thing"
// TypeError!

proclaimThisFact = function (fact) {
  console.log(`${fact}!`);
}
```

### ES6's let and const

`let` and `const` are ES6 compliments to the original JavaScript syntax. They are interesting, helpful additions to the language for a variety of reasons, but for the purposes of our discussion of hoisting, we'll  just talk about one specific behavior, which is:

#### let and const are not hoisted

When a variable is declared using either `const` or `let`, it is not hoisted in the way that we see above with `var`. Instead, its point of declaration is treated "in-line." Essentially, it behaves closer to the way it looks in the written code.

Take my original example, but we replace `var` with `const`:

```js
console.log(dog);
const dog = 'Fido';
```

Bam! We get hit with a reference error! And I argue, this is preferable to what we see as a result of hoisting + `var`. It's much better to trigger an error like this in development than to chase down strange bugs that are caused as a side effect of your variables sneaking around with `undefined` values. For that reason, I take the approach of always using `const`[^3] for my variables, rather than `var`. I haven't found any compelling reasons to use `var` when `const`/`let` are available to me.[^4]

A best practice that Kyle Simpson suggests in the above mentioned sources is to declare all of the variables you will be using at the tops of their relevant scopes:

```js
bar();

function bar() {
  let foo;
  const baz = 10;
  
  foo = baz;
  console.log(foo);
}
```

### Key Takeaways

* Just use `let` and `const` to avoid this nonsense
* Only declarations are hoisted, not initializations
* Function declarations (`function lookAtThisPizza() { ... }`) are fully hoisted
* In function expressions (`var lookAtThisPizza = function () { ... }`), only the variable is hoisted

While ES6's `const` and `let` allow us a comfortable way to side step this issue by simply declaring our variables at the tops of our scopes and carrying on like civilized people, it is important to understand `var`'s hoisting behavior. For one, JavaScript is the most ubiquitous language in use on the web and there are millions of lines of code running in production today that were created pre-ES6. You may one day have to maintain or refactor some of that code, and knowing why it behaves the way it does will be helpful.

Furthermore, [`let`](https://caniuse.com/#search=let) and [`const`](https://caniuse.com/#search=const) don't have complete browser support, so if you're forced to continue support for IE10 or something similar, you may need to hoist yourself up by your own bootstraps and navigate this quirk of the JavaScript language.

### Additional Reading

* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch4.md)
* [MDN's Glossary Entry](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
* [David Shariff on Execution Context](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)

### Footnotes

[^1]: Generally, food based examples decrying improper pizza toppings or other less worthy snack foods.
[^2]: You know, for fun.
[^3]: I try to default to `const` and then change to `let` only when necessary.
[^4]: Outside of writing examples for people running into this problem, of course.