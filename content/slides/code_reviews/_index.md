---
title: "How to review C++ code"
date: 2018-09-05T17:10:27+03:00
type: post
outputs: ["Reveal"]
draft: true
---
# How to review C++ code

---

![XKCD](https://imgs.xkcd.com/comics/code_quality.png)

---
## Why do code reviews?

Code reviews are a great practice. They allow for:

- shared knowledge
- ensuring code confirms to best practices and the coding standards
- less smelly code
- less bugs - cost of a bug grows with the time to find it

---

## What is the most expensive bug that you know of?

[History's worst software bugs](https://www.wired.com/2005/11/historys-worst-software-bugs/)

---

    int connections = GetConnections();
    if (connections = 2) {
    }

{{< note >}}
Could not find the article, but this cost something like 2 mln $ for a UNIX
distributor.
{{</note >}}

---
## So how to do them?

There are lots of advice on how to conduct code reviews, but it is too general.
It focuses on:

- how to prepare for the review
- how to write the review comments
- enforcing coding standards
- improving test coverage

---
## Writing review comments

1. Not personal
2. Non judgemental

---
## Review Comments

All of us (and the reviewer and the author of the code) have to keep in mind
is:

> The code review is about the code produced by a developer at a certain moment
> in time.

---
### The reviewer

- You are reviewing the code, not the person.

> "That is will not work when ..." vs "You have missed the case when ..."

> Avoid using *you* in the comments, it is easily replaceable by *we*.

---
### The author of the code:

- The issues found during the review are issues with the code and
  implementation. Not with your personality.

> Do better the *next time*.

---
### Code reviews checklist


---
#### Preration

1. Understand what the code is supposed to do on a high-level and then drill
   down.
2. Give yourself enough time - start at 500 LOC / hour and adjust to your speed
   and knowledge of the area.

---
#### The review

1. Does the commit message correctly describe the intent of the changes?
2. Understand what the code actually does. Mark any differences between the
   intended effects and the actual ones.
3. Does the code confirm to the coding standards?
4. Do they follow DRY, KISS?
5. Are the changes covered by tests?
6. Are there any seemingly unrelated changes?

---
#### Extra-care topics

1. Timing issues
2. Multi-threaded code

Think through all possible scenarios.

TODO: Extend?


---
#### Coherent Labs Specific

1. Do the changes conform to the initial design document?
3. Artifacts for public changes
    - documentation
    - packaging
    - samples
    - launcher
4. Are new headers added to project files? Are the project filters correct?

---
### Code smells

[Code smell](https://en.wikipedia.org/wiki/Code_smell) is any characteristic in
the code that possibly indicates a deeper problem.

- it might be not a bug, it might be a design problem
- it is *never* a feature

---
#### General code smells

---
##### 1. Treat hard to understand code as wrong.

A given piece of code should be understandable and its correctness *verifiable*
in a couple of minutes.

No matter whether the reviewer is an expert in the subsystem under review or
not. Everyone should be able to understand a small piece of code in minutes,
once having a high-level grasp of the subsystem and the project.

---
##### 2. Lots of code changed

- it is no fun to make a review of 1000 LOC
- the more code changed, the higher chance of bugs

---

Microsoft have a study, that shows that the most often changed files during
development of a new Windows, had the most bugs after the release.

---
##### 3. Naming

Beware of names without any meaning that span a lot of code.

---
    int Where(const std::vector<int>& v, int i)
    {
        auto it = std::find(v.begin(), v.end(), i);
        return std::distance(v.begin(), i);
    }

---

- `Where` ... what?
- `i` ... ??? - needle?
- `v` ... ??? - almost ok, given its type.
- `it` - ok, lives for

---
##### 4. Lack of logs

- Any error state should be properly log with meaningful information
- Major events in the program should be logged

---
##### 5. Magic Values

Lack of a properly named constant usually means:

> I need something here ... 13, 64, ... no ... 42 seems about right!

Lets keep the magic in the air and out of the code!

---
#### C++ specific code smells

So what to look (*sniff*) for in a C++ code review:

---
##### 1. Pointers and references

Pointers are not the root of all evil, but do actually tend to cause problems.

---
###### Pointers?

Whenever a plain pointer is used, be sure that:

1. The pointer is not `nullptr`.
2. The pointed-to object will out live the pointer
3. The pointed-to object will not move/relocate somewhere else in the memory

{{< note >}}
This topic covers two of the most *C++*-ish kind of issues -  ownership and
lifetime.
{{</note >}}

---
###### Pointers?

1. No naked `new` and `delete` - these are almost always not necessary. Require
   extra care for the proper management of the objects allocated this way.

---

> I am smart and use smart pointers, so I am safe :)

---

Smart pointers can be `nullptr` as well.

---
##### 2. Container choice

Use a `vector`.

- double think and explain why using `queue`
- double think and explain why using `*Deprecated` maps

---
###### Unless you have a compelling reasons like:


- it will hold a large number of elements, will change a lot,  and the most
  common operation is going to be find an element with some property having
  a specific value
- elements must not be relocated

---
##### 2. Container choice

The container invalidation is something that can be easily overlooked when
writing the code and what is worse - it will appear to work correctly in most
cases.

{{< frag c="Until the container relocates is storage ..." >}}

---
##### So, for each container check:

1. Is that the right type of container for the usage?
2. Is the container modified while being iterated?
3. Is there an iterator or a pointer inside the container that will out-live
   possible relocating operation for that container?

---
##### 3. Casts

Prefer `static_cast` and constructor casts whenever possible, since the compiler
will not let you shoot yourself in the foot for most cases.

The exception here is cast from `void*` to something else - make sure that the
type is correct.

---
##### 3. Casts

*C-style* and `reinterpet-cast` are more dangerous, the compiler is much more
permissive with them.

---
##### 3. Casts

1. The cast used is the safeest one possible.
2. The types are correct.
3. The level of indirection is not casted away, i.e. casting from `T**` to `Y*`.
4. `const_cast` is usually a *design* smell, consider whether that is really the
   way to go

---
##### 4. Assumptions that are not asserted or checked

This a bit a general category, so here are the most frequent examples:

1. Accessing array element that is not there. Out-of-bounds access or using
   `.front()`, `.back()` on an empty container.
2. Iterator returned by `find` is not the *end* iterator.
3. Index returned by `string::find` is not `npos`.

---

    v.erase(std::remove_if(v.begin(), v.end(), [](auto& e) {
        return e.IsNotUsed();
    }, v.end());


---
##### 5. Operations with strings

There are three major sources of issues when working with strings:

1. Null termination.
2. `char` and `wchar_t`
2. Encoding
4. Indices for substrings

---
###### 1. Null termination.

- All the APIs working with strings have special cases whether and when the
  terminating null is written to buffer and whether it is counted in the
  returned length of the string.

---
###### 1. Null termination.

- Some of the APIs return a null terminated buffer, some don't. It is very
  easy to use a non-terminated buffer and cause a bug.

Double check the documentation and the usage!

---
###### 2. `char` and `wchar_t`

`char*` (UTF-8) and `wchar_t*` (UTF-16 or UTF32, depending on the platform)
do not mix well. Using anything but unicode conversion library will most
likely result in bugs when non-ascii symbols are present.

---
###### 3. Encoding

Check that the code assumes the correct encoding of the string, otherwise it
will still be garbage.


---
###### 4. Encoding

The `std::basic_string` API is full of taking indices, so it very easy to make
off by one error.

---
##### 6. Smart pointers

Smart pointers are there to resolve the problems of the plain ones, but still
they have their own set of caveats.

---
##### 6. Smart pointers
Most shared pointers use reference counting, so the following must be checked:

1. Cyclic references

2. [*The Create Rule*][create_rule] - some objects are created with one
   reference in them, so they must be released explicitly when they are no
   longer used. When such objects are stored in a smart pointer, this reference
   must be *adopted*, otherwise it will leak.

[create_rule]: https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFMemoryMgmt/Concepts/Ownership.html

4. Two shared pointers to the same object. That is not an issue with *intrusive
   pointer*

---
##### 7. `auto` and `auto&`

`auto` is great, but it makes it very easy to create a copy of an object. Check
that:

1. `auto&` is used wherever possible.
2. `auto&` is not used for something that gets destroyed, i.e.

        auto& top = stack.top();
        stack.pop();

---
##### 8. Objects with padding

It is very tempting to use functions like `memcmp` and `HashMemory` to create
comparators and hash functions for objects that are used in a hash map.

However they will use the uninitialized memory that is present in any padding
inside the object.

---
##### 8. Objects with padding

1. Check that there is no padding inside objects used with `memcmp` and
   `HashMemory`.
2. Check that there are `static_assert`s that will enforce that. Keep in mind
   that some compilers will move the padding around when there is inheritance in
   the mix.

Consider adding the padding explictly in the object to make it visible:

    char __padding[N] = { 0 };


---
##### 9. Float precision and NAN

- Don't use NAN.

When compiled with *fast-math* it leads to undefined behaviour.

---
##### 10. Error checking for OS and external APIs

Any OS or external API call can fail, so there must be a check for failures and
appropriate logging.

---
##### 11. Hiding or reusing a variable

Hiding a

- function parameter
- member variable
- variable from a outer scope

is really confusing and makes the code harder to read and often wrong.

---
##### 12. `using namespace` in headers

---
#### Coherent Labs specific code smells

---
##### 1. Temp allocator

We are using a temp allocator that has significant performance gains. However,
we must be sure that each temp allocation is guarded by a `TempAllocatorScope`
and does not out-live that.

---
##### 1. Temp allocator

Also look for cases, where the normal allocator is used, while the *temp
allocator* will suffice.

---
##### 2. Entry-points for public APIs

The memory allocators rely on specific thread local variables to be set. So each
entry point of our products must be guarded with entry point like
`COHERENT_UIGT_ENTRY_POINT`.

---
##### 3. Types used as task parameters

We do have a great system to prevent you from shooting yourself easility.

> Make sure that any added types are using the correct semantics.

---
##### 4. Timing issues in tests

Tests tend to depend on time for loading resources and similar. Every *sleep*
and *wait* in tests should be treated as extremely smelly. Most cases can be
rewritten to use events or polling in worst case to check that something has
finished loading.

---
## Quiz time!

---

    ViewBinder::~ViewBinder() {
        ScriptingScope scope(m_View->GetScripting());
    }

---

1. `m_View` is a pointer -> lifetime? (The `ViewBinder` is being destroyed ... )
2. The `ViewBinder` lives in the `View` and has a pointer to it :/

---

- NAN
- hash
- temp allocator
- JSString to WTF String
- wchar_t to char
- &vector<char>[0]
- hiding a variable
- changing a variable meaning
- start/ end in range requests
- JSStringCreate
- GC with no adopt


