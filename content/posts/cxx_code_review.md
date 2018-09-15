---
title: "How to review C++ code"
date: 2018-09-05T17:10:27+03:00
type: post
draft: true
---

## Conducting code reviews

Code reviews are a great practice. They allow for:

- shared knowledge
- ensuring code confirms to best practices and the house coding standards
- less smelly code
- less bugs

There are lots of advice on how to conduct code reviews, but it is too general.

Most of it, focuses on how to write the review comments, i.e.

- non judgemental
- not personal

The general idea that all of us (and the reviewer and the author of the reviewed
code) have to keep in mind is:

- The code review is about the code produced by a developer at a certain moment
  in time.

The reviewer:

- You are reviewing the code, not the person. Avoid using *you* in the comments,
  it is easily replaceable by *we*.

The person that wrote the code:

- The issues found during the review are issues of the code, not your own. You
  will do better next time.

Besides that, all the advice is a bit too generic to be useful. 

https://www.youtube.com/watch?v=t6L8b4tUmeE

### Conducting code reviews

1. Understand what the code is supposed to do on high level and then drill down.
2. Understand what the code actually does. Mark any differences between the
   intended effects and the actual ones.
3. Does the commit message correctly describe the changes?
4. 500 LOC / Hour
5. docs for public changes
6. Timing issues
7. Multi-threaded code

### Code smells

[Code smell](https://en.wikipedia.org/wiki/Code_smell) is any characteristic in
the code that possibly indicates a deeper problem.

#### General code smells

1. Treat hard to understand code as wrong. A given piece of code should be
   understandable and its correctness verifiable in a couple of minutes.

#### C++ specific code smells

So what to look for in a C++ code review:

##### 1. Pointers and references

Pointers are not the root of all evil, but do actually tend to cause problems.
Whenever a plain pointer is used, be sure that:

1. The pointer is not `nullptr`.
2. The pointed-to object will out live the pointer
3. The pointed-to object will not move/relocate somewhere else in the memory

This topic covers two of the most *C++* kind of issues -  ownership and
lifetime.

##### 2. Container choice

Generally, container choice in C++ is easy - go for `vector`. Unless you have a
compelling reasons like:

- it will hold a large number of elements and the most common operation is going
  to be find an element
- element must not be relocated

The container invalidation is the something that can be easily overlooked when
writing the code and what is worse - it will appear to work correctly for most
test cases. Until the container relocates is storage ...

So, for each container check:

1. Is that the right type of container for its use?
2. Is the container modified while being iterated?
3. Is there an iterator or a pointer inside the container that will out-live
   possible relocating operation for that container?

##### 3. Casts

Prefer `static_cast` and constructor casts whenever possible, since the compiler
will not let you shoot yourself in the foot for most cases. The exception here
is cast from `void*` to something else - make sure that the type is correct.

*C-style* and `reinterpet-cast` are more dangerous, the compiler is much more
permissive with them.

1. The cast used is the safeest one possible.
2. The types are correct.
3. The level of indirection is not casted away, i.e. casting from `T**` to `Y*`.

##### 4. Assumptions that are not asserted or checked

This a bit a general category, so here are the most frequent examples:

1. Accessing array element that is not there. Out-of-bounds access or using
   `.front()`, `.back()` on an empty container.
2. Iterator returned by `find` is not the *end* iterator.

    {{< highlight cpp >}}
v.erase(std::remove_if(v.begin(), v.end(), [](auto& e) {
    return e.IsNotUsed();
}, v.end());
    {{</ highlight >}}

1. Something

##### 5. Operations with strings

There are three major sources of issues when working with strings:

1. Null termination.

    - All the API working with strings have special cases whether and when the
      terminating null is written to buffer and whether it is counted in the
      returned length of the string.  
    - Some of the API return a null terminated buffer, some don't. It is very
      easy to log a non-terminated buffer and cause a crash.

    Double check the documentation and the usage!

2. `char*` (UTF-8) and `wchar_t*` (UTF-16 or UTF32, depending on the platform)
   do not mix well. Using anything but unicode conversion library will most
   likely result in bugs when non-ascii symbols are present.

3. The `std::basic_string` API is full of taking indices, so it very easy to
   make off by one error when using it.

##### 6. Smart pointers

Smart pointers are there to resolve the problems of the plain ones, but still
they have their own set of caveats.

Most shared pointers use reference counting, so the following must be checked:

1. Cyclic references
2. *The Create Rule* - some objects are created with one reference in them, so
   they must be released explicitly when they are no longer used. When such
   objects are stored in a smart pointer, this reference must be *adopted*,
   otherwise it will leak.
4. Two shared pointers to the same object. That is not an issue with *intrusive
   pointer*

##### 7. `auto` and `auto&`

`auto` is great, but it makes it very easy to create a copy of an object. Check
that:

1. `auto&` is used wherever possible.
2. `auto&` is not used for something that gets destroyed, i.e.

        auto& top = stack.top();
        stack.pop();

##### 8. Objects with padding

It is very tempting to use functions like `memcmp` and `HashMemory` to create
comparators and hash functions for objects that are used in a hash map. However
they will use the uninitialized memory that is present in any padding inside the
object.

1. Check that there is no padding inside objects used with `memcmp` and
   `HashMemory`.
2. Check that there are `static_assert`s that will enforce that. Keep in mind
   that some compilers will move the padding around when there is inheritance in
   the mix.

##### 9. Float precision
##### 10. Error checking for external APIs

#### Coherent Labs specific code smells

##### 1. Temp allocator

We are using a temp allocator that has significant performance gains. However,
we must be sure that each temp allocation is guarded by a `TempAllocatorScope`
and does not out-live that.

Also look for cases, where the normal allocator is used, while the *temp
allocator* will suffice.

##### 2. Entry-points for public APIs

The memory allocators rely on specific thread local variables to be set. So each
entry point of our products must be guarded with entry point like
`COHERENT_UIGT_ENTRY_POINT`.

##### 3. Timing issues in tests

Tests tend to depend on time for loading resources and similar. Every *sleep*
and *wait* in tests should be treated as extremely smelly. Most cases can be
rewritten to use events or polling in worst case to check that something has
finished loading.
