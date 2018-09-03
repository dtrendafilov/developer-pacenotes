---
title: "How to review C++ code"
date: 2018-09-05T17:10:27+03:00
type: post
draft: true
---

## Conducting code reviews

Code reviews are a great practice. They allow for:

- shared knowledge
- ensuring code confirms to best practices and coding standards
- less bugs

There are lots of advice on how to conduct code reviews, but it is too general.

Most of it, focuses on how to write the review comments, i.e.

- non judgemental
- not personal

The general idea that all of us (and the reviewer and the author of the reviewed
code) have to keep in mind is:
- The code review is about the code produced by a developer at a cerain moment
  in time.

The reviewer:
- You are reviewing the code, not the person. Avoid using 'you' in the comments.


The person:
- The code issues found during the review are issues of the code, not yourself.
  Next time, you will do better.

Besides that, all the advice is a bit too generic to be useful. 

### Conducting code reviews

- Understand what the code is supposed to do?
- Understand what the code does?
- 500 LOC / Hour
- Does the commit message correctly describe the changes?

## C++ code reviews

So what to look in a C++ code review:

- Ownership
- Lifetime
- casts
- container choice
- iterator invalidations
- docs for public changes
- pointer deref
- array access
- assumptions that are not asserted/verified
- null termination

