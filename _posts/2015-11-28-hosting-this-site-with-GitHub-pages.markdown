---
layout: post
title:  "Hosting this site with GitHub pages and a custom domain"
date:   2015-11-28
categories: tech
comments: true
---

## For those who have a few minutes to read...
Not too long ago, I decided I wanted to create a new personal site where I could share some information about myself and blog. I ended up settling on [Jekyll](https://jekyllrb.com/){:target="_blank"}.  I just started trying to write about my reasons for choosing Jekyll, then I realized that's not why I'm writing this post.  If you're curious about some arguments for using Jekyll, here's some reading material:

- [5 reasons you should use Jekyll](http://cloudcannon.com/jekyll/2015/03/04/5-reasons-you-should-use-jekyll.html){:target='_blank'} *- by CloudCannon*
- [YesWeJekyll](http://yeswejekyll.com/){:target='_blank'} *- by Juho Vepsäläinen*
- [WordPress vs. Jekyll: Why You Might Want to Switch](http://www.sitepoint.com/wordpress-vs-jekyll-might-want-make-switch/){:target='_blank'} *- by David Turnbull*

The actual motivation for this post came this morning, when I finally decided to re-purchase this domain (matthewalanhiggins.com) and set up this site to work with it.

If you take a look at the few links I posted above, one of the main reasons people like Jekyll is because it works so well with GitHub. Taking your site live is as simple as creating a new branch called "gh-pages" and pushing it to GitHub.

Since this site isn't really a project page (generally used to describe, you know, a project), I wasn't too keen on the idea of having users navigate to `http://mh120888.github.io/portfolio-site/` just to take a look at it. I suppose it's not the worst web address in the world but I think it's nicer to see my name there instead - is that vain?

Having modified more DNS records than I ever wanted to in my job at Scorpion, I've come to the conclusion that despite having a name I continue to insist is creppy, GoDaddy provides the best combination of support and admin interfaces of all the domain registrars and hosting companies I've encountered.  Needless to say, I registered my domain name with GoDaddy.

A few minutes later, I had my zone file open and ready to modify - some quick googling got me the IP address for GitHub pages. All that was left was to create a CNAME file in the top-level directory of my site. Honestly I wasn't expecting it to be quite so straightforward, but all that's required is a file named `CNAME` into which I placed a single record (`www.matthewalanhiggins.com)`.

What was slightly more annoying was realizing that I needed to modify the baseurl for the site to reflect the new domain. Since it had previously been set up to work with the github.io url that GitHub automatically provides, changing the domain name broke links all over the place, including links to assets like my stylesheets! Not to say this is the greatest design in the world, but it certainly looks nicer than what you'd be seeing with just user-agent styles.

## tl;dr
- You should use Jekyll for blogs
- GitHub Pages make Jekyll even better
- GitHub Pages + custom domain is very easy to set up