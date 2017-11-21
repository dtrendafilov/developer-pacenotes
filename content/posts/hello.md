---
title: "Hello"
date: 2017-11-20T17:54:55+02:00
draft: false
type: post
---
# Hello

This is a smoke post to check that everything is OK.

<!--more-->

{{< highlight html >}}
<section id="main">
  <div>
    <h1 id="title">{{ .Title }}</h1>
    {{ range .Data.Pages }}
      {{ .Render "summary"}}
    {{ end }}
  </div>
</section>
{{< /highlight >}}
