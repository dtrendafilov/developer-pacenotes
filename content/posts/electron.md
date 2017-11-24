---
title: "Electron is bad, but Electron apps are here to stay"
date: 2017-11-20T19:00:00+02:00
type: post
draft: false
---

Ever since Electron has been created a lot has been discussed about the
practicality of using [it for powering desktop applications][apps].
Electron-based apps tend to have high CPU usage and even higher memory usage,
easily taking gigabytes of RAM.They reduce battery life and generally make
your PC feel like an electric typewriter.

[apps]: https://news.ycombinator.com/item?id=14245183

<!--more-->

Electron is a magical combination of node.js and Chromium. It allows you to
have a web application that has access to the OS and effectively have a
desktop app. Both CPU and memory usage are caused by Chromium. It is no
surprise - Chromium is designed to show every website out there in the most
robust and secure way, while providing tons of extra features - from spell
checking to Safe Browsing and extensions. Performance comes somewhere after
that.

Here is a screenshot of the memory and CPU usage for [Visual Studio
Code](https://code.visualstudio.com/), while editing this blog post.

{{< figure src="/img/vscode.jpg" title="Visual Studio Code with 9 processes and 555 MB for a single file and markdown preview" >}}

There has been a [call for going back to native][native] applications. But
they are not perfect either - they are not portable, sometimes even between
different versions of the same OS. They do provide the native look and feel
of the OS that you are using, but that, in my opinion, is not a great
feature. As someone who is using Windows, Mac OS X and Linux on regular
basis, often literally all-tabbing between them, I much more prefer to have
the same experience on all of them - same buttons, same shortcuts. Mentally
switching between the OSes 10 times an hour will significantly reduce my
productivity. Native apps always require a chunky download and install, just
even to give the application a try. Compare this to trying out a web
application - click a link in your already present and always open browser.
The web has always had the immensely lower barrier to entry. That is why so
many new applications start their life as web apps. And updates - when did
you had to restart your browser to see the new version of an web app? Unless
I need to change the underlying Electron version, I can deploy newer and
better versions of my application every minute. And no one will ever have to
restart my app.

[native]: https://medium.com/@caspervonb/electron-is-cancer-b066108e6c32

A lot of effort has been invested by popular Electron applications to reduce
their resource usage. Slack, for example, [cut down tabs][tabs] for unused
teams and migrated from [`<webview>` to `BrowserView`][browser]. It took me
some time to grasp how actually the [`BrowserView`][browserview] is helping
them. Here is a simpler explanation.

[tabs]: https://slack.engineering/reducing-slacks-memory-footprint-4480fec7e8eb
[browser]: https://slack.engineering/growing-pains-migrating-slacks-desktop-app-to-browserview-2759690d9c7b
[browserView]: https://blog.figma.com/introducing-browserview-for-electron-7b40b4b493d5

First you need to know a little bit how Chromium works. It has a
multi-process architecture (link to our blog). In short - it creates a single
process for every web page, iframe or plugin present on a webpage. There are
two additional processes - one that controls the all the processes and
communicates with the OS, and a GPU process - the one that draws everything.
Electron shows web content inside webview, so it creates an additional process
BrowserView shows the web content directly, with no extra process, but you
have to size and move the window without the layout powers of CSS.

So going away from `<webview>` to a `BrowserView` for Slack meant that they
got rid of one process of all. And they had to change a lot of their code in
order to do that.

Meanwhile, other industries such as gaming, embedded and automotive have
realized the need for a fast and resource efficient web renderer. We, at
[Coherent Labs](https://coherent-labs.com), have created
[LensVR](https://lensreality.com), a lean and mean web renderer that can
[render html at 1000 frames per second][fps1000].

[fps1000]: https://lensreality.com/lensvrrenderingpart1/

{{< figure src="/img/editor.jpg" title="The Coherent Editor written in TypeScript and HTML and running inside Coherent GT" >}}

So just as the terrible browsers circa 2000-2005 did not stop the web from
becoming the ubiquitous platform, the Electron of today won't stop web-based
applications from running on the desktop. We have to ditch Chromium for a
better rendering engine. So what do you think? Would you code your next
desktop app in a leaner and meaner Electron or go back to native? I do
believe that web apps on the desktop are here to stay, so we will be porting
a couple of Electron applications to Lens in next weeks. Which one should we
start?
