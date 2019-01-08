---
title: 'Enter the Eloquence - Eloquent JavaScript, Chapter 2'
url: 20.html
id: 20
categories:
  - Uncategorized
date: 2016-11-29 17:09:21
tags:
---

I'm aiming to bring my JavaScript chops up a level or two, and I've decided to address this first by working through all of [Eloquent JavaScript](http://eloquentjavascript.net/) by Marijin Haverbeke. I'm going to do each chapter's exercises and post them here with my thoughts. This will have the benefit of forcing me to clarify my thinking on the solutions, and it could also help someone else who might be struggling with the ideas. The book is definitely a dense read. So far, the first two chapters are written in a style that is not quite conversational and not quite formal. It has the odd effect on me of an almost instant-glaze over, which is requiring a slower reading pace. This probably exacerbated by the fact that I'm using a e-book version, not a hard copy. Slowing down my pace of reading is fine though, since I don't want to miss any details in my deep-dive into this language! So, without further ado:  
**Loops to Make a Triangle**

```js
// Looping a Triangle
// For Loop
var hashTriangle = ""
for (var i = 0; i <= 7; i++) {
  hashTriangle += "#";
  console.log(hashTriangle);
}
// While Loop
hashTriangle = ""
while (hashTriangle.length <= 7) {
  hashTriangle += "#";
  console.log(hashTriangle);
}
```

Not much to say here. I decided to try two simple implementations, one with a for-loop and the other with a while-loop. **The Fizz Buzz Problem**

 ```js
// Fizz Buzz
for (var i = 1; i <= 20; i ++) {
  divByThree = (i % 3 == 0);
  divByFive = (i % 5 == 0);

  if (divByThree && divByFive) {
    console.log("FizzBuzz");
  }
  else if (divByFive) {
    console.log("Buzz");
  }
  else if (divByThree) {
    console.log("Fizz");
  }
  else {
    console.log(i);
  }
}
```

Whenever I work on a FizzBuzz type of challenge, I have a nagging feeling in the back of my mind that I'm doing extremely naive work and that there is a much fancier, elegant way to solve the problem than I have discovered so far. [It would seem that this is the case](http://ditam.github.io/posts/fizzbuzz/), but I'm not going to worry about that for now. This being my third time thinking through the problem at some length greater than 30 minutes, I'm happy with my smallish refactoring, that being that I saved the results of the modulo operations to variables to avoid repeating the operation unnecessarily. **Create a Chess Board String**

```js
// Chess Board String
var chessBoard = "";
var size = 8;
// set height
for (var i = 0; i < size; i++) {
  // set each row
  for (var j = 0; j < size / 2; j++) {
    if (i % 2 == 0) {
      chessBoard += "# ";
    } else {
      chessBoard += " #";
    }
  }
  chessBoard += "\\n";
}
console.log(chessBoard);
```

This was an interesting problem to play with. I think my biggest hang-up was simply that I struggled to get away from the notion of implementing this as a 2D array, rather than a formatted string. I may come back to this, as I feel there are more interesting, compact solutions available than what I came up with. So, there are my solutions to the three exercises from Eloquent JavaScript, chapter 2. If you found interesting ways to solve any of them, please let me know. I love being shown how weak my Kung-Fu is, so have at me!