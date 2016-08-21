---
layout: post
title: The Problem with window.onload()
date: 2016-08-19
---


if you've ever used JQuery you are no doubt familiar with the `$(document).ready()` function. As
you may be aware, the `$(document).ready()` function is an event listener with that says "hey, when
all the DOM elements on a page are rendered and ready, execute the function I'm passing to you."
Without the use of the `$(document).ready()` function your script may run without all the elements on the page being ready, which will, of course, result in an error, so it's a pretty useful thing. The equivalent of `$(document).ready()` in plain 'ol javascript is `window.onload()`. If you're like
me and tend to avoid JQuery wherever possible, then you might have run into some issues with
`window.onload()` specifically if you are passing in multiple callbacks that reference the
same window.


Let's say we we want to `doSomething()` once the window loads, we've defined that in the
doSomething.js file that looks like this.


```javascript

(function(window) {

  window.onload( function() {

    function doSomething() {
      ...body...
    }
  }
})(window)

```


Later, we add some other functionality in the doSomethingElse.js file. In this
file we also pass `doSomethingElse()` to `window.onload()` what happens? Well only the last functionthat is hooked to the window.onload event will be executed when the page loads. Not great,
especially considereing the usefulness of `window.onload()`.


What to do? Well it turns out the answer is pretty simple just use:

`window.addEventListener('load', doSomething, false);`

instead. `window.onload` is a bit of an archaic construct since it only allows one hook per page. plplus it's pretty ugly. In my opinion this is much nicer and easier to follow.


```javascript

(fucntion(window) {

  function doSomething() {
    ...body...
  }

  function doSomethingElse() {
    ...body...
  }

  window.addEventListener('load', doSomething, false);
  window.addEventListener('load', doSomethingElse, false);

})(window)

```
