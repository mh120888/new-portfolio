---
layout: post
title:  "Exploring Flex - Part 1"
date:   2016-03-22
categories: tech
---

For about as long as I’ve known about it, I’ve been opposed to using flexbox.  Although I think we need more modern methods for implementing complex layouts on the web, I’ve been very much turned off by the inconsistencies in implementation and even in syntax between different versions; as a developer who needs to support further back than just the latest version of IE/Edge, they’re pretty much a deal-breaker.

That being said there will likely come a time when flex is the norm, even if we incorporate it into our toolkits at different rates. Despite being somewhat familiar with how to use it (and despite the fact that I don’t), I thought it would be a useful exercise to actually look at the [flexbox spec](https://www.w3.org/TR/css-flexbox-1/) to learn more about it. Distaste, or outright hatred, should come from a place of authority, not ignorance.

Happily, I found the spec to be mostly readable.  It does a great job of explaining what parts of the document are really geared towards implementors (people making browsers) rather than the rest of us, and it makes the intention of the spec much clearer than any tutorial I’ve seen.

So what is flexbox, or flex, or the “flex layout model”?  I really like this description straight from the abstract of the spec, so I’ll just quote it:

`In the flex layout model, the children of a flex container can be laid out in any direction, and can “flex” their sizes, either growing to fill unused space or shrinking to avoid overflowing the parent.`

Well that explains the name.

The abstract also states that “horizontal and vertical alignment of the children (of a flex container) can be easily manipulated”.  Yes, vertical alignment can actually be a thing on the web that doesn’t involve annoying hacks.

Lastly, the abstract mentions that we are able to *nest* flex containers, and use them on both horizontal (normal) and vertical axes.

Just before getting into the meat of the spec, we’re reminded that this is a Candidate Recommendation, soon to become a W3C Recommendation, and hopefully a standard one day.  But until that time comes, don’t be surprised to see things change.

**Coming up - delving into the introduction**
