---
title: Fun With Generators
date: 2018-12-21 15:48:26
tags:
---

## What is a generator function?

Some kind of trickery, surely. What is that `*` doing, and why should I care?

Take this for example:

```javascript
function* generatingInterest(i) {
  yield i;
  yield i + 10;
}
```
