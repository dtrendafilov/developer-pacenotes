---
title: "Starting a blog"
date: 2018-09-06T17:10:27+03:00
type: post
categories: [ "web" ]
draft: false
---
# Starting a blog

Starting a blog has always been a daunting task for me. Not only because it
requires sharing my personal experiences, but also because I want it to be
a blog that will be easy for me to write and easy for you to read.

I am making a simple blog, so most of the posts will be simply text with some
code snippets and possibly some figures. I also want:

- fast loading time even on a slow mobile network connection
- my own domain
- HTTPS
- *fire and forget* - as little as possible maintenance 
- free or as cheap as possible
- ad free

Here is how I choose to create the blog you are reading at the moment.

## Content Management System (CMS)

[CMS](https://en.wikipedia.org/wiki/Content_management_system) like Wordpress
and Ghost make it very easy for a lots of people to create and edit content,
with minimal effort. I have experience with Wordpress, so my impressions apply
for that. CMS have some form of visual WYSIWYG editor that makes creating
content even simpler and faster. However they require a database server for
storing the posts in all of their versions. For anything outside of just
creating content (and not built-in into the theme) they require adding a plugin.
If you really want to get a blog or a site with highlighted code snippets,
analytics, search optimization, social sharing and so on (and you probably do
want these, in order to have readers on your blog) you easily get at least four
plugins, each of them adding at least JavaScript or CSS files to be loaded by
the browser on each page.

In the end to show a single post to the reader, your CMS fetches the content
from the database, filters it through the theme and all of the plugins and
serves it to the browser. The browser makes ten more requests to fetch all the
styles and scripts and finally renders the post to the reader. To get a recent
experience, you'll need a beefy machine to host the CMS and good internet
connection for all of the requests. Sure, there are ways to optimize this such
as using caching and Content Delivery Networks (CDN). Suddenly having
a performing personal blog requires quite a lot of effort and knowledge of
deploying websites.

There hosting services that will do that for you, but they will:

- charge you for that
- or serve some ads
- require you to use their domain

## Static site generators

Static site generators take a bunch of files in a folder, filter them through
the theme with a text template engine and spill out a folder with a bunch of
HTML, JavaScript and CSS files. You upload this folder to a simple web server
and you get a blog.

The simplicity is great, but it comes with some trade-offs.

You have to write all the content in a text editor. The trick here is to choose
a generator that has live preview. The live preview starts a local web server
and you can open it in a browser to see the final post. I keep the browser
opened on half of the screen, the text editor on the other half and whenever
I save the file - it immediately shows me the final content.

To get things like version history, you will need a version control system like
Git, but that is a non-issue for me.

There are no plugins, so if you want something that is not present in the theme
already - you have to code it yourself.

Since the site is static, i.e. there is no database behind it, there are no
comments out of the box. There are services like [Disqus](https://disqus.com)
that provide a comment section for each blog post. You can find a overeview of
some of them [here](https://darekkay.com/blog/static-site-comments/) and some
[here](https://gohugo.io/content-management/comments/).

### Choosing a static site generator

Here is a [list](https://www.staticgen.com/) static site generators and you can
see they are a lot.

My criteria for choosing were:

1. Live-preview as said above
2. Having nice and high-quality themes
3. Markup language - most of them support Markdown. There are lots of light
   markup languages, but I don't want to learn a new one.
4. Text template language - it has be to simple, you are going to change the
   theme eventually
5. Implementation language - I definitely do not want to change the generator,
   but sometimes it is the only option. Or you might to want to understand how
   it works.

I have been using [hexo](https://hexo.io/) before for [another blog-like
site](https://sofiacpp.github.io/advanced-cpp/) and it is definitely a good
choice. However, it is written in JavaScript and runs in node.js, so you get
some of the node.js ecosystem problems - I spent a lot of time making scripts
for the content generation, the slides and managing the dependencies of hexo,
the theme, the other dependencies.

So this time I am going with [hugo][hugo] it is distributed as a single
executable (no more *node_modules*), it is fast and has some great themes.

## Themes

Almost everything in your blog depends on the theme that you chose for it. Be
sure to chose one that you like and works well for your content on all possible
devices. When you go to the site of any CMS or static generator, there will be
lots of themes, even hundreds of them. The list of themes is not always well
curated. Some of them include everything, some of them a too minimalist to be
useful. Sooner or later you will need to change something in the theme. Make
sure it is well written and organized, so that making changes will be easier.
I have two indicators for the quality of a theme:

- how recent is the last commit and how many commits there are in the history
- how many stars it has in GitHub. Forks too show the popularity, but many forks
  may be a symptom that many changes were necessary.

Wordpress has an edge here - it is used in a lot of commercial websites, so
there are commercial themes for it. Some of them provide support and paid
customization.

The theme I choose is [Future
Imperfect](https://themes.gohugo.io/future-imperfect/). It looks great an all
devices, (may be even a bit too stylish for me), has tons of options and is easy
to modify.

## Hosting and domain

The options I had for the hosting of the blog content were Amazon S3 and GitHub
Pages. Amazon S3 is pretty cheap, but still not free. And serving an S3 bucket
on the web requires some configuration. I have used GitHub Pages before, so
their almost zero configuration won me. The only thing I had to configure was to
make them work with a custom domain.

About the domain - it is almost impossible to find a domain that is not used or
bought, but I did it :). I have been using Amazon Route53 for company websites
before and they gave me the lowest price, so it was an easy choice.

## The setup

There is no point in replicating the documentation of hugo, GitHub Pages and
Amazon Route53, so here are the links:

1. [Hugo Quick Start][quick]
2. theme setup - [hugo-future-imperfect][theme]
3. [Hugo hosting on GitHub Pages][deploy]
4. [Using a custom domain with GitHub Pages][domain] - This seems a bit lot at
   first, but it covers all the possible usages of GitHub Pages (for an user,
   for an organization, a project) and supported types of domains.
5. Amazon Route53 UI can be confusing but for this setup you'll need to create
   only CNAME and A records and they are easy enough to set up.


# The result

So far, hugo is great, hosting and deployment is easy. The site seems fast
enough, even though the theme is not really optimized for fast loading (like
having the most important styles directly in the HTML).

The blog has took me one day to get up and running - choosing a static
generator, a theme, thinking up the domain and buying that. And it is almost
free - the only think that actually costs me something is the domain name and
that's around $12 per year. You can get a blog for free, if you don't insist on
having own domain.

As you will notice the blog does not have comments. I had configured Disqus for
comments, but loading the Disqus comments form was the slowest part of the blog.
Then I found out about [Staticman](https://staticman.net/) and really liked the
idea. So Disqus got removed and I will probably add comments with Staticman if
this blog gathers enough readers to actually need it.

[hugo]: https://gohugo.io/
[quick]: https://gohugo.io/getting-started/quick-start/
[theme]: https://github.com/jpescador/hugo-future-imperfect
[deploy]: https://gohugo.io/hosting-and-deployment/hosting-on-github/
[domain]: https://help.github.com/articles/using-a-custom-domain-with-github-pages/

