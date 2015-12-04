---
layout: post
title:  "Turning a JS Cookie into a JS Object"
date:   2015-12-03
categories: tech
---

Most of the time, you don't have to deal with cookies directly. Frameworks like Rails provide an easy way to set cookies, session variables, etc. without ever having to leave Ruby world.

Even if you're only working with client-side code, the Web Storage API gives us some easy-to-use ways to store information in the browser that will persist between page loads (sessionStorage) or even after the browser is closed (localStorage) in the form of key-value pairs inside of a Storage object.

If you can't use the Web Storage API (i.e. you need to support < IE8) or for whatever reason you don't want to, there's always the option of using plain old `document.cookie`.  There's a whole host of reasons why you probably don't want to do this, but if you find yourself in a position where you are, you'll soon learn something (perhaps) surprising - the cookie is not a key-value store like sessionStorage, but rather a single string "containing a semicolon-separated list of all cookies" (source [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie#View_Cookie_Demo)), in the format `key=value`.

I once found myself working on a client project where I unfortunately had no choice but to use cookies, and so I created this simple utility function for turning the cookie string into a regular JS object.

{% highlight javascript %}
    function turnCookieIntoObject(cookieString) {
        cookieString = cookieString.split('; ');
        var cookieObject = {};
        for (var i = 0; i < cookieString.length; i++) {
            var currentCookie = cookieString[i].split('=');
            cookieObject[currentCookie[0]] = currentCookie[1];
        }
        return cookieObject;
    }
{% endhighlight %}

There's no magic, no real cleverness, but it reminded me that sometimes it's a good idea to be kind to myself and create a tool that makes my job just the slightest bit easier.