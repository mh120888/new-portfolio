---
layout: post
title:  "Material Design Ripple Button"
date:   2015-10-26
categories: design
---

##What is it?
When a user clicks on a button, a ripple appears on the surface of the button that radiates outward from the location of the click and then fades away, much like a ripple appears on the surface of still water when someone tosses in a pebble.

Here's a quick demo:

<p data-height="268" data-theme-id="0" data-slug-hash="XmEPPM" data-default-tab="result" data-user="matthewalanhiggins" class='codepen'>See the Pen <a href='http://codepen.io/matthewalanhiggins/pen/XmEPPM/'>Ripple Button (v2)</a> by Matthew Higgins (<a href='http://codepen.io/matthewalanhiggins'>@matthewalanhiggins</a>) on <a href='http://codepen.io'>CodePen</a>.</p>

<script async='async' src="//assets.codepen.io/assets/embed/ei.js"></script>

##Why?
The simplest answer is that sometimes it’s just nice to see something a bit cool and different.

That being said, there’s a certain kind of satisfaction a user gets from immediately receiving visual feedback from having clicked on a button.  Additionally, having the ripple effect start at the location of the click makes the interaction feel more personal.

Microinteractions are important!

##How does it work?
Overall there’s not very much code that’s needed to make this work, after leveraging the awesome power of GSAP and jQuery (for convenience).

The basic strategy - behind every button, there is also an svg canvas containing a circle.  Most of the time this circle is 2px by 2px and completely transparent, so you don’t see it, but when a user clicks on the button we are able to increase the circle’s opacity and scale it out.

###Any external dependencies?
Yes. We are using GSAP’s TweenMax library to handle the animation of the svg.  While there are certainly ways to accomplish this using only vanilla js and CSS, the simplicity and cross-browser support that GSAP provides make it well worth the added weight (it’s actually only 34kb).

We’re also using jQuery for convenience, but that could easily be replaced if needed.  GSAP is not dependant on any other libraries, but it does work well with jQuery.

###Start with HTML
We start off with an svg symbol, which functions kind of like a template for the ripple.

{% highlight html %}
<div style="height: 0; width: 0; position: absolute; visibility: hidden;" aria-hidden="true">
	<svg version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" focusable="false">
		<symbol id="ripple-shape" viewBox="0 0 100 100">
			<circle id="ripple-shape" cx="1" cy="1" r="1" />
		</symbol>
	</svg>
</div>
{% endhighlight %}

What this allows us to do is to define the circle element once and then use it throughout our page without having to redefine it over and over.  Besides not having to type as much, this has the added benefit of being easy to modify in the future if we want to use a shape other than a circle for the ripple.  Obviously the “template” version of the circle is going to be hidden from view (inline styles for clarity).

Assuming each button on the page is actually an `<a>` instead of the more semantically correct `<button>`, our button markup will look something like this:

{% highlight html %}
<a href='#' class="btn">
	Click for Ripple
	<svg class="ripple-obj">
		<use width="100" height="100" xlink:href="#ripple-shape" class="js-ripple"></use>
	</svg>
</a>
{% endhighlight %}

As I mentioned, instead of redefining the same circle inside of each button, we’re going to use the `<use>` element - read more about it [here](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/use) - to pull in our circle.

###Then CSS

In addition to whatever styles we have to make our buttons look the way we’d like, we need to ensure that the svg canvas (the `<svg>` element) for each button is “behind” the button itself by adding position: relative to the button, then position: absolute to the svg and matching its dimensions to those of the button.

We also need to give the svg a fill color - this is going to be the interior color of the elements on the svg canvas, which in other words is the color of our circle.  

{% highlight css %}
body {
	background-color: #FFF;
}

.btn {
	display: inline-block;
	text-decoration: none;
	color: #FFF;
	font-family: sans-serif;
	background-color: #000;
	padding: .75em 3em;
	position: relative;
	transition: .3s all ease;
	border-radius: .25em;
}

.btn:hover {
	background-color: #333;
}

.ripple-obj {
	height: 100%;
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	z-index: 0;
	fill: #FFF;
}

.ripple-obj use {
	opacity: 0;
}
{% endhighlight %}

###And Finally Javascript
Most of the “magic” happens in the javascript layer, but as I promised it’s not going to be overly complicated.

We want to bind the click event for every button on the page and define a callback function that includes the animation logic for our ripple.

Inside of this callback function, we want to initially set up a couple of variables.  The first is our ripple, the circle svg shape inside of our button.  Just some simple DOM querying.

Next we want to figure out exactly where the user clicked so we know the where to set the epicenter of our animation.  Fortunately we can grab the x and y coordinates of the click from the event object, and even more fortunately, these coordinates are defined relative to the target on which the event was fired.

Lastly we want to create a new TimelineMax instance, which is a GSAP object that allows us to define our animation sequence, which we’ll do next.

TimelineMax provides the fromTo method which accepts four arguments - the element you want to animate, the duration of the animation, an object literal containing the starting attributes of the animation, and an object literal containing the final attributes of the animation.

We want to animate our ripple by specifying its x and y position that we grabbed from the event object to give the appearance that the ripple grows from the exact spot that was clicked. We want to start at scale: 0 and opacity: 1 and end at some larger scale (I’m using 200 here, which means that when the animation finishes the circle has a radius of 400px) and opacity: 0, so the ripple appears to grow larger and weaken as it expands outward.


{% highlight javascript %}
function rippleAnimation(event) {
  var ripple = $(this).find('.js-ripple'),
  tl = new TimelineMax();
  x = event.offsetX,
  y = event.offsetY,
  scale_ratio = 200

tl.fromTo(ripple, 1, {
  x: x,
  y: y,
  transformOrigin: '50% 50%',
  scale: 0,
  opacity: 1,
  ease: Linear.easeIn
}, {
  scale: scale_ratio,
  opacity: 0
});

return tl;
}
{% endhighlight %}
