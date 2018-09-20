---
title: "How to review C++ code"
date: 2018-09-05T17:10:27+03:00
type: post
outputs: ["Reveal"]
draft: false
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
{{< /note >}}

---
## So how to do them?

There are lots of advice on how to conduct code reviews, but it is too general.
It focuses on:

- how to prepare for the review
- how to write the review comments
- enforcing coding standards
- improving test coverage

---
## Posting a code review

1. Post a separate review for each meaningful change
    - unrelated small improvements can get a in-place review
2. Remove generated files from the review
    - unless you have changed the code that generates them

---
## Posting a code review

3. Explain how you have tested the changes
4. Add any notes that will help the reviewer
    - link to design document
    - link to JIRA task

---
## Posting a code review

> Do review your changes before asking others to review them

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
the development of a new Windows version, had the most bugs after the release.

---
##### 3. Naming

Beware of names without any meaning that span a lot of code.

---
    int Where(const std::vector<int>& v, int i)
    {
        auto it = std::find(v.begin(), v.end(), i);
        return std::distance(v.begin(), it);
    }

---

- `Where` ... what?
- `i` ... ??? - needle?
- `v` ... ??? - almost ok, given its type.
- `it` - ok, lives for two lines

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
{{< /note >}}

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

If you happen to recollect that bug, give time the others to think before
answering.

---

    ViewBinder::~ViewBinder() {
        ScriptingScope scope(m_View->GetScripting());
    }

---

1. `m_View` is a pointer -> lifetime? (The `ViewBinder` is being destroyed ... )
2. The `ViewBinder` lives in the `View` and has a pointer to it :/

---

    void DataBinder::RunEvaluatorsForProperty()
    {
        auto kt = modelData.PropertyEvaluators.find(propWithIndex);
        if (kt != modelData.PropertyEvaluators.end())
        {
            auto& evaluators = *kt->second.ArrayPtr;
            for (size_t i = 0; i < evaluators.size(); ++i)
            {
                const auto& evaluator = evaluators[i];
                auto node = evaluator->DOMNode.lock();
                if (!node || !node->IsElement())
                {
                    continue;
                }
                UpdatePropertyInDOM(AsElementSafe(node.get()));
            }
        }
    }

---

- container relocation
- double work - checking for `AsElementSafe`

---

    Ref<WebKitCSSMatrix> WebKitCSSMatrix::skewX(double angle) const
    {
        if (std::isnan(angle))
            angle = 0;

        auto matrix = create(m_matrix);
        matrix->m_matrix.skewX(angle);
        return matrix;
    }

---

- NAN

---

    std::size_t csl::hash<ShadowShape>::operator()(const ShadowShape& s) const
    {
        return farmhash::Hash(reinterpret_cast<const char*>(&s),
                              sizeof(ShadowShape));
    }

---

- hash of unitialized memory

---

    using BindingEvaluatorArray = DomDeque<DomSharedPtr<BindingEvaluator>>;

---

- Why a deque?

---

    case CommandType::FillText:
    {
        TmpVector<TextRunProps> runs;
        TmpVector<TypefacePtr> typefaces;
        runs.reserve(batch.Commands.size());
        // ...
    }

---

- `TempAllocatorScope`

---

    WTF::String JSStringToUTF8WTFString(JSContextRef jsContext,
                                        JSStringRef jsStr)
    {
        const size_t utf8Max = JSStringGetMaximumUTF8CStringSize(jsStr);
        LChar* strData = nullptr;
        auto str = WTF::String::createUninitialized(utf8Max, strData);
        JSStringGetUTF8CString(jsStr, (char*)strData, utf8Max);

        return str;
    }

---

- null termination?
- cast

---

    void UISystemImpl::RegisterGamepad(unsigned id, const char* info,
                                       unsigned axesCount,
                                       unsigned buttonsCount)
    {
        m_GamepadProvider->RegisterGamepad(id, info, axesCount,
                                           buttonsCount);
    }
---

- Entry points, needs `COHERENT_UIGT_ENTRY_POINT`.

---

    WTF::String JSStringToUTF8WTFString(JSContextRef jsContext,
                                        JSStringRef jsStr)
    {
        const size_t utf8Max = JSStringGetMaximumUTF8CStringSize(jsStr);
        LChar* strData = nullptr;
        auto str = WTF::String::createUninitialized(utf8Max, strData);

        auto length = JSStringGetUTF8CString(jsStr, (char*)strData,
                                             utf8Max);
        str.remove(length - 1, (int)0x7ffffff);

        return str;
    }

---

- `0x7ffffff` ... what???

---

    JSRetainPtr<JSStringRef> eventName(JSValueToStringCopy(context,
                                       arguments[0],
                                       exception));

    Coherent::String name;
    auto chars = JSStringGetCharactersPtr(eventName.get());
    name.assign(chars, chars + JSStringGetLength(eventName.get()));

---

- broke the encoding
- The create rule, leaked a ref to the string - use `AdoptPtr`

---

    CohString returnString("return ");
    returnString += evaluationString;
    JSValueRef exception = nullptr;
    JSObjectRef jsFunc = nullptr;
    if (scopeParams.isEmpty())
    {
        jsFunc = JSObjectMakeFunction(jsContext, nullptr, 0, nullptr,
            MakeJSStringPtr(returnString.data()).get(),
            nullptr, 0, &exception);
    }

---

- `string::data` is not null terminated

---
    virtual void InitFileReader(AAssetManager* assetManager,
                                const char* fn) override
    {
        AAsset* m_Asset = AAssetManager_open(assetManager,
                                             NormalizePath(fn).c_str(),
                                             AASSET_MODE_BUFFER);
        if (m_Asset)
        {
            AAsset_seek(m_Asset, 0, SEEK_SET);
            m_FileSize = AAsset_getRemainingLength(m_Asset);
        }
    }

---

- `m_Asset` is local and hides the member variable

---

    virtual unsigned Read(unsigned start,
                          unsigned char* buffer,
                          unsigned end) override
    {
        end = std::min(end, unsigned(m_FileSize));

        assert(start >= 0);
        assert((end >= 0));
        assert((end >= start));
        // ...
    }

---

- `end` changes meaning from requested end to possible end
- first two asserts don't test anything, `unsigned` >= 0

---

	void DataBindExpression::EvaluateIf(bool value, Element* parent,
										TmpVector<NodePtr>& deadNodes)
	{
		TempAllocatorScope scope;
		if (!value)
		{
			deadNodes.push_back(parent->RemoveChild(children[index]));
		}
	}

---

- `deadNodes` can grow in the current `TempAllocatorScope`, so when the function
  returns - the vector will be invalid

---

    if (propValue.ConvertTo(truthy))
    {
        truthy = evaluator.HasNegation ? !truthy : truthy;
        ScriptVector<NodePtr> deadNodes;
        // Data bind if has only 1 set of evaluators.
        evaluator.Children.resize(1);
        expression->EvaluateIf(truthy, element, deadNodes,
                               evaluator.Children[0].Evaluators);
        EvaluateGroup(evaluator.Children[0], evaluator,
                      propValue, binder);

        ReleaseDeadNodes(deadNodes, binder);
    }

---

- could use a `TmpVector`

---

    WTF::String JSStringToUTF8WTFString(JSContextRef jsContext,
                                        JSValueRef jsValue)
    {
        JSStringRef jsStr = JSValueToStringCopy(jsContext,
                                                jsValue, nullptr);
        return JSStringToUTF8WTFString(jsContext, jsStr);
    }

---

- The *create* rule - leak of a reference
- no error handling

---

    bool ScriptValue::GetLength(ScriptingEngine engine, int& len)
    {
        auto context = ToJSCContext(engine);
        auto js = ToJSC(m_Handle);
        auto propName = JSStringCreateWithUTF8CString("length");
        auto obj = JSValueToObject(context, js, nullptr);
        if (obj && JSObjectHasProperty(context, obj, propName))
        {
            // ...
        }

---

- Create -> Where is the release?

---

    engine.trigger('Start');

---

- Is there anyone listening?

---
## Takeaway

1. Prepare yourself to understand the code
2. If you are not convinced something is correct in a minute, poke it until you
   prove that it is correct or that it is broken.
3. Keep your eyes peeled for the smells.

---
# ?

---

Do not forget to give feedback!
