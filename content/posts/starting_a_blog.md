---
title: "Starting a blog"
date: 2018-07-15T17:10:27+03:00
type: post
draft: true
---

Starting a blog has always been a daunting task for me. No only because it
requires sharing your personal experience, but also because I want it to be a
blog that will be easy for me to write and easy for you to read.

I am making a simple blog, so most of the posts will be texts files, Markdown
with some code snippets and some figures. I also want:

- own domain
- Fast loading time even on slow mobile network
- HTTPS
- "Fire and forget" - as little as possible maintenance 


## Content

There are a lot of blog systems and services out there like Wordpress, blogspot,
hexo and [hugo][hugo]. I have been using all of them at certain moment and for
this blog I am going with hugo and here is why.


Wordpress will do the job, but is a bit of an overkill for that and requires
real hosting - it needs a database, a PHP capable webserver and so on. Not only
it will cost more, but it will require more maintenance from my side.
I know that you can get a blog at *.wordpress.com* for free, but you lose the
chance to have your own domain and it will display ads.

The last argument goes for blogspot.com - it is a nice service again, but you
can not change a lot like the theme and so on. (was that true?)

I have been using hexo for the site of a course about C++ I led at Sofia
University - I got the job done, but it wasn't as easy as I wanted. I spent
hours to get everything to work as I wanted it - having slides shows inside
the site, showing the slides as articles as well pagination. The biggest
source of frustration was incompatible versions of the [jade](LINK) language -
during the year, something in the parser changed and files that were fine
according to one version, could not be rendered by the newer one, so I had to
stick to the old one.  The other was that hexo uses node.js and every module
brings a ton of dependencies inside the node_modules folder.  The good things
about hexo is that it allowed me to have my blog contain text files with code
snippets and figures, version all of that easily on github and host it there
without any cost.  It has a great preview feature - you have the file, it
re-renders the site and refreshes a browser that is looking at it.

So here is why I choose hugo for my this blog - it has all the things I like in
hexo, without some of the drawbacks:

 - it is fast.
 - it is a single binary - goodbye node_modules and it is easy to install.
 - it has somewhat better collection of themes - more on that in a bit.

There lots of other static site generators out
[there](https://www.staticgen.com/) and probably most of them have the same
qualities as hexo and hugo.


## Themes

Almost everything in your blog depends on the theme that you chose for it. So be
sure to chose one that you like and works well for your content on all possible
devices. When you go to the site of any CMS or static generator, there will be
lots of themes, even hundreds of them and the list is not always well curated.
Some of them include everything, some of them a too minimalistic to be useful.
Sooner or later you will need to change something in the theme, so make sure it
is well organized, so that will be easier. An indicator of a quality of a theme
is how many stars it has in github. Forks too show the popularity - but many
forks may be a symptom that changes were necessary.


## Hosting and domain

The options I had for the hosting of the blog content were Amazon S3 and Github
Pages. Amazon S3 is pretty cheap, but still not free. And serving an S3 bucket
on the web requires some configuration. I had used Github Pages before, so their
almost zero configuration won me. The only thing I had to configure was to make
them work with a custom domain.

About the domain - it is almost impossible to find your domain free, but I did
it :). I had been Amazon Route53 before and they gave me the lowest price, so
it was an easy choice.

## The setup

There is no point in replicating the documentation of hugo, Github Pages and
Amazon Route53, so here are the links:

1. [Hugo Quick Start][quick]
2. theme setup - [hugo-future-imperfect][theme]
3. [Hugo hosting on Github Pages][deploy]
4. [Using a custom domain with GitHub Pages][domain] - This seems a bit lot at
   first, but it covers all the possible usages of GitHub Pages (for an user,
   for an organization, a project) and supported types of domains.
5. Amazon Route53 can be complicated, but for this setup you'll need to create
only CNAME and A records


# The result

So far, hugo is great, hosting and deployment is easy.

I still have to force Hugo to minimize all the generated files, but that is
solved in 0.43, so it is up to the theme not (or me).

As you will notive the blog is still not entirely static - it uses Disqus for
comments. Loading the comments form is one of the slower parts of the blog at
the moment, so I am thinking of getting rid of comments altogether or using a
"static" system for comments like - ??? 

[hugo]: https://gohugo.io/
[quick]: https://gohugo.io/getting-started/quick-start/
[theme]: https://github.com/jpescador/hugo-future-imperfect
[deploy]: https://gohugo.io/hosting-and-deployment/hosting-on-github/
[domain]: https://help.github.com/articles/using-a-custom-domain-with-github-pages/

