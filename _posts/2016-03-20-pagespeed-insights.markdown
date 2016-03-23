---
layout: post
title:  "Why You Shouldn’t Obsess About PageSpeed Insights"
date:   2016-03-20
categories: tech
comments: true
---

## Short Reason:

Sometimes it simply tells you to do things that don’t make sense.

PageSpeed Insights for this site gives a score of 55 for desktop, which seems awful.

![PageSpeed Insights](http://i.imgur.com/DCXat4R.png)

If you take a look at the most important aspects of the page to fix, it suggests compressing five different images for a total size reduction of 98%.

Take a minute to think about that - PageSpeed Insights is suggesting that 98% of these images can be removed and the (visual) experience will essentially be the same. Doesn’t seem too logical, does it?

Let’s compare one of the original images from this list with the compressed version that PageSpeed Insights provides as a download.

### Original Version:

![Original Version](http://i.imgur.com/8s6dHQ1.png)

### “Optimized” Version:

![Optimized Version](http://i.imgur.com/95aOOuM.png)

Not really something that you’d want on anyone’s site, let alone a high-value client.

## Longer Reason

According to some, there have historically been four major categories of page performance metrics.  All have their uses, but it’s important to understand how each actually relates performance that users might care about.

The most basic page performance metrics are quantitative metrics.  This includes things like the number of images, number of javascript files, number of HTTP redirects, total page weight, etc. - things that are objectively true and can be easily represented with numbers.

There are a variety of reasons why this is true, but generally speaking the fewer bytes that are sent over the wire the faster a page will load, all things being equal.  Given that this is the case, it’s easy to see how quantitative metrics can be used to describe performance - the more stuff you have on your site, the worse it will perform.

On the other hand, quantitative metrics can be used to misconstrue the truth.  Not all files are created equal, in file size or in importance to your users.  Just because you have twenty image files doesn’t mean the page will take a long time to load.  Likewise, a single hero image can have a disproportionate impact on the perceived performance of your site (due to its size and its placement at the top of the page, where users can see it - or not).

Quantitative metrics on their own aren’t terribly useful then.  To help us make sense of quantitative metrics, we have what are called *rule-based metrics*.

Rule-based metrics take quantitative metrics for your site and perform some analysis on top of them, measuring information about your page against current best practices to make sure you’re doing everything you can to help your site’s performance.  This includes things like compressing images, bundling and minifying static assets like javascript and CSS, enabling HTTP caching, etc.  **This is what PageSpeed Insights does.**

In the example above, I showed one suggestion that came from PageSpeed Insights that pretty clearly is not a good idea.  This isn’t to say that PageSpeed Insights is not a useful tool for diagnosing potential performance problems and finding solutions to them, but focusing on the score that comes back is generally not the best use of your time **if you’re mostly concerned with your users’ experience.**

Although there are a lot of factors that go into the PageSpeed Insights grade that may be reflective of site performance, that is not necessarily the case.  The score is calculated based on how well Google thinks you’re adhering to web performance optimization best practices.

In this example for Imhoff & Associates, Google decided that the images should have been much smaller (98% smaller) and consequently dropped the PageSpeed score significantly.  Even if the images could be compressed to 2% of their original size and still look good, that isn’t really the point.  If we were to compress these images but add something else to the page that equaled the page weight that were saved by the image compression (~600kb here), the PageSpeed score would shoot up even if the page loaded at the same rate, or slower.

To put it in a nutshell, **the PageSpeed Insights score is a reflection of how closely your site implements current best practices, as determined by Google.**  It is not necessarily indicative of things like how long it takes for the page to render (how long it takes for a user to actually see something on the page). For this we need another type of performance metric, milestone metrics.

Milestone metrics measure how long it takes from initiating a request (a user clicks on a link) to some clearly defined action happening, like how long it takes for the DOMContentLoaded event to fire.  This type of metric is very useful because it’s represented in a way that is easy for most anyone to understand - “it took 2.7s for event X to happen”.  Since we’re measuring in time, it’s easier to see whether changes you’ve made to your site are having a positive or negative impact on a given milestone metric.

Although time is easy to comprehend in this situation, the events that are measured in milestone metrics can be very technical and not have much meaning to your users.  For example, DOMContentLoaded is an event that fires when the HTML for a page has been received and parsed by the browser, but it does not take CSS, images, javascript, etc. into account.

The load event is probably closer to what we think of as the moment when a page is finished rendering, but even that is a bit of an oversimplification. For more traditional website and web applications that use server-side rendering to fully generate HTML, load is more or less indicative of when users can see stuff on your page (technically it fires when all of the content on a page has loaded, including its dependent resources like images, CSS, etc.).

For sites that modify the page with javascript, load might very well be the beginning of when things start happening visually, especially if you’re using a front-end framework like Angular, Ember, etc. You might have two sites that have a load time of 2.7s, but if one uses server-side rendering and the other doesn’t, users will *perceive* the first to be faster even though the milestone metrics are identical.  This brings us to our fourth type of metric, perceived metrics.

Perceived metrics attempt to build on top of the idea of milestone metrics, focusing on what users perceive to be happening. There are a lot of different ways to do that, all of which are somewhat subjective, but one of the best methods for leveraging this type of metric can be found at [www.webpagetest.org](http://www.webpagetest.org/).

WebPagetest provides a number of useful metrics that might sound familiar, including:

- **Optimization grades** - letter grades for basic optimizations like compressing images, caching static content, keep-alive connections etc.
- **High-level metrics** - metrics for things like load time, time to first byte, document complete, etc. for both first-time visitors (no resources are cached) and return visitors

WebPagetest also provides something called a Speed Index, which is just a number that represents the average time in ms at which visible parts of the page are displayed (this means that’s it’s dependant on the size of the viewport).

The default algorithm for calculating the Speed Index is based upon the visual completeness over time.  It captures a video of the page loading (10 frames per second) and calculates a “visual completeness percentage” at that point in time, which gets plotted onto a line graph.  Once the page has finished loading, the area under the curve on the graph that was created is measured and becomes the basis for the Speed Index.

*This is a greatly oversimplified explanation of what goes into calculating the Speed Index - you can [read more about it here](https://sites.google.com/a/webpagetest.org/docs/using-webpagetest/metrics/speed-index).*

One of the nice things about this algorithm is that is can distinguish between two sites with the same load time but drastically different performance.  Imagine two sites both load in five seconds, but the first loads 99% of the visual content in the first .5s while the second loads 20% of the visual content per second.  The first site will be perceived as being faster by a user even though our milestone metric of page load is identical for both sites - this is exactly the type of problem that perceived metrics try to solve.

In the description of the Speed Index, its creators say that it “should be used in combination with the other metrics (load time, start render, etc) to better understand a site's performance.”  I definitely think that’s the most important take-away point.

There is no single type of metric that will be able to fully describe the performance of a site. Web performance is changing all the time, both the underlying technologies/best practices for performance and our users’ expectations about how things should function.  Any single number or metric that can be measured is only part of the story.  If you obsess over it, you might miss the bigger picture.

And you might go crazy.