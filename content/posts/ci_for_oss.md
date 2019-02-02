---
title: "Creating C++ projects"
date: 2018-10-20T19:58:28+03:00
draft: true
---

Most software developers like to start new projects (finishing them is another
matter). These projects can be for some recreational programming, learning
something, experimenting or deliberate practice.

While I am not into recreational programming - I do like to experiment and
create things just to understand better how they work.


Having these projects under source control is universally accepted practice and
there lots of options for hosting these projects in public and private
repositories.

However most of these projects are not covered by the good practices we have for
professional projects - ranging from coding standards, style checker, tests to
Continous Integration (CI) and Continous Delivery (CD).

I think there two main reasons for most projects to not have them. The first is
that adding all that tooling to a pristine new project is tedious. The second -
probably that it may involve costs that we don't want to pay.

In a series of posts, we'll cover the following topics:

- Source Control
- Build System
  - genie vs cmake
- Project Generation
  - cookiecutter
- Coding standards and style
  - clang format
  - git hooks
- Continous Integration
  - appveyor vs travis ci
  - azure devops
- Testing
    - googletest as a submodule vs subtree
- Continous Delivery

## Source Control

As discussed - source control is universally accepted practice and there lots of
options for hosting your projects in public or private repositories, with *Git*
or *Mercurial* as version control systems. GitHub, GitLab, BitBucket are
probably the most used, have similar features and generally will not make an
mistake with any of them. Azure DevOps is a *newer* (or the name is new)
contender on that front.

There are a few files that are recommended for every project that is hosted
somewhere:

- README
- LICENSE
- .gitignore (assuming you are using Git for version control)

## Project generation

Project generation is quite popular in the web and nodejs communities starting
from yeoman to every major framework having its own starter app, that you simply
*clone*, or a project generation tool.

Cookiecutter is a generic generation tool - it takes a folder as a template and
generates a copy of that folder with all the special variables replaced.

Over the course of this series we'll be creating a cookiecutter template for C++
projects that will allows us to have a build system, code style checker, tests,
CI and CD.

So for the first post of this series, we'll be discussing of how to create a
project with .gitignore, README and LICENSE.

## Build System

Unfortunately for C++ projects that aim to be cross-platform, some kind of build
generation system is required. It can be CMake, premake or genie.  While CMake
is quite popular, I personally prefer the later two as they allow a full
programming language - Lua - for project generation and generated projects are
more like what someone will manually create.

