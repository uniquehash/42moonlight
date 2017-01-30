
#Day 03: Learning JavaScript, the “right” way: Closures

##Update

##Day 03: Learning JavaScript, the “right” way: Closures

![This will make sense in a little bit.](https://medium2.global.ssl.fastly.net/max/10602/1*0jWWc5bh0hbMEbzQOIC_dg.jpeg)*This will make sense in a little bit.*

##Update

I’ve decided that I want to only increase the day number for this blog when I spend time exclusively to JavaScript and decide to write a post. This will probably persist until my schedule get’s a little less hectic.

**NOTE:** This post has been in my Drafts for almost a month now, as you can see from this my Schedule is far from less hectic.

##Closures

I pretty much already had the gist of closures, but a couple things with my understanding of them were wrong. I’ll go over three mindsets I used to better understand closures.

1. Functions are Closures in JS (This statement changes with other languages: [https://goo.gl/emG5w5](https://goo.gl/emG5w5))

1. Most importantly, understanding the Lexical Scope in JavaScript. JavaScript is function scoped, which means anything outside of a function is considered Global. Anything inside a function is Local. However, when you have a function inside of a function, the most inner function has access to it’s Parent’s Scope, the Global Scope, AND it’s own Local Scope.

1. I like thinking of each function as a walled-off region in my code. The contents in the walled-off region has access to everything outside of it, but nothing outside of it can get past it’s walls. However, when you call the function, you now have access past it’s walls.

##Example

<iframe src="https://medium.com/media/0d95f0479dd28f44cd00ebfd69bdecff" frameborder=0></iframe>

In this code we have a Global variable, a Parent variable (inside parameter of the parent function), and a Local variable (inside the function that is returned by the parent function).

To get access to our closure inside the parent function we assign it to the variable access and call access.

When the function access is called it executes the closure inside of parent. This closure has access to the Global Scope, it’s Parent’s Scope and it’s Local Scope. This means it alerts, “global variable, parent variable, local variable”.

##Conclusion

I hope this gave you a slightly better idea of what Closures are. For more a more detailed description of Closures + some more examples visit the sources below. Also, please feel free to give me feedback on the info. in this post because I’m still learning so if anything is not accurate please say so. Thanks!

##Sources
[**Understand JavaScript Closures With Ease**
*Closures allow JavaScript programmers to write better code. Creative, expressive, and concise. We frequently use…*javascriptissexy.com](http://javascriptissexy.com/understand-javascript-closures-with-ease/)

<iframe src="https://medium.com/media/e70b8acdd143d1b22769398f864e3a3f" frameborder=0></iframe>
