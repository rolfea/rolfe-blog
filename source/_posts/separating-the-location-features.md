---
title: Separating the Location Features
url: 28.html
id: 28
categories:
  - Uncategorized
date: 2017-01-15 14:17:27
tags:
---

[You Work for Me](http://youworkforme.netlify.com/) is an app I’m building as a way to learn React. YWFM uses the Web API’s geolocation capabilities alongside the Sunlight Lab Congress API to determine the user’s congressional representatives, supplying them with contact information. My mentor, [Brian](https://briandouglas.me/), and I decided that the functionality that determines the location and correct legislators would be a good candidate to abstract away from this app into a separate library. I decided to use the [SurviveJS component boiler plate](https://github.com/survivejs/react-component-boilerplate) to generate the base for my library. I made this decision for the sake of time. I know I will learn a lot when I set up my own boilerplate, but for now I’d rather have my project ready to demonstrate at an interview than get stuck in the weeds trying to configure a whole mess of dependencies just to get off the ground. The SurviveJS boilerplate is very simple to set up. As per their instructions, just clone the repo, remove the .git file and run `git init` to hook up the cloned folder to your own repository instead. Finally, use `npm install` to install the dependencies. I want to hone down my library to do one specific task really well, so rather than offering a suite of location based services, the library will provide the congressional district of the user, based on the location returned by the Geolocation API. Because a user must give the browser permission, the library needs to have a graceful default if permission isn’t granted or if geolocation isn’t enabled or possible for some reason. The first steps I need to complete in order to have this library functioning are as follows:

*   Test the API that returns district numbers, seeing what form the returned data takes
    
*   Insert that API into my current app to bring in the district numbers
    
*   ID the correct rep. for the user and provide their contact information
    

After some ugly moments in which I remembered how much fun it is to play with promises in JavaScript, the API is finally returning what I need: an object with state and district values. This example uses the JavaScript Fetch API to issue a call to the Sunlight Labs’ district lookup method. We pass in the sample latitude and longitude (or replace with your own) from the API documentation. This call returns a promise object, so we’ll use the `.then` method to pass a callback that waits for the response object and then calls `.json()` on it when received. Because `.json` also returns a promise, we’ll use `.then()` again and just log the results of our response so we can look at it. If you’re following along, you can test the logic right now. Just open the developer tools in your browser and try out the commands below:

```js
const lat = 42.96
const long = -108.09

fetch(\`https://congress.api.sunlightfoundation.com/districts/locate?latitude=${lat}&longitude=${long}\`).then(
  (res) => res.json().then(
    (jsonObject) => console.log(jsonObject.results\\\[0\\\])
  )
)
```

(Also take a moment to appreciate how nice this looks with ES6 arrow functions!) In the following post I’ll walk through pulling out the location service, the political api, and wrapping it into a single library. I’ll also cover writing tests for that library and publishing it to NPM.