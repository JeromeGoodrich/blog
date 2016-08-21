---
layout: post
title: Closures in Javascript Event Listeners
date: 2016-08-18
---


Javascript has Closure behavior, which means that any functions are allowed access to the scope theywere created in, indefinitely. This creates a pretty powerful dynamic. Because Javascript functions are objects that can be passed around to different contexts, we can have a function created one
context be passed to another context, but still have access to it's original context. The
implications of this dynamism are manifold.


Consider the following case: You are adding some functionality to a forum that facilitates
conversation between native French and English speakers. The feature you are adding is the ability
to toggle the language in which a post in a thread appears. Each thread can have many posts and the user should be able to toggle posts individually.


```javascript

function switchPostLanguage() {
  \\get the an arrray of all the divs that wraps both the french and english content
  var allPosts = document.querySelectorAll('.content-container');
  allPosts.forEach(function(post) {
    var toggleToFrenchButton = post.querySelector('.toFrenchButton'),
        toggleToEnglishButton = post.querySelector('.toEnglishButton'),
        frenchContent = post.querySelector('.frenchContent'),
        englishContent = post.querySelector('.englishContent');

   toggleToFrenchButton.addEventListener('click', function () {
     toggleVisibility([toggleToFrenchButton, toggleToEnglishButton, frenchContent, englishContent])
   }, false)

   toggleToEnglishButton.addEventListener('click', function () {
     toggleVisibility([toggleToFrenchButton, toggleToEnglishButton, frenchContent, englishContent])
   }, false)
  }
}

```

Each invocation of the callback function passed to`forEach` is its own closure. The parameter passed
to the callback is array element specific to that iteration. The key thing to notice is the second
closure. The anonymous function defined as a parameter within the `addEventListener` invocations.
If we were to call the toggleVisibility function directly in this scenario, our buttons would become useless because the function does not have the same context as variables it takes as parameters. Thanks to the use of a closure though we can now define that function elsewhere and still be able to
pass it variables that are defined within a different context.


Given the example we might be tempted to define the `toggleVisibility` function within the body of
`switchPostLanguage`. However, what would happen if we now needed to toggle the visibility of
something else on the page, instead of duplciating a lot of logic in `switchSomethingElse` function we could now extract `toggleVisibilty` and use it wherever we need to toggle the visibility of an
element. Pretty powerful stuff, and it's all thanks to closures!
