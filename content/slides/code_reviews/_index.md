---
title: "How to review C++ code"
date: 2018-09-05T17:10:27+03:00
type: slides
outputs: ["Reveal"]
draft: false
---
# Why and how to do code reviews

---

![XKCD](https://imgs.xkcd.com/comics/code_quality.png)

---
## Code Reviews vs Pull Requests

- Code reviews are the general practice of changes being reviewed before being
  brought into the main branch.

- Pull requests are the feature of GitHub that allows implementing Code reviews.

---
## Code Reviews vs Pull Requests

We'll use:

- Code review - for the general practice
- Pull request - for our GitHub-based implementation of the code review practice

---
## Why do code reviews?

Code reviews are a great practice. They allow for:

- ensure that codebase health does not degrade
- shared knowledge
- training reading and understanding code
- more readable and understandable code
- less smelly code
- less bugs
- ensuring code confirms to best practices and coding standards


---
## Why do code reviews?

![Code Reviews Microsoft](https://i0.wp.com/www.michaelagreiler.com/wp-content/uploads/2019/03/Code-Review-at-Microsoft-Graphics.png?w=793&ssl=1)

---
### Why do code reviews?

![Code Reviews Google](https://i2.wp.com/www.michaelagreiler.com/wp-content/uploads/2019/07/Reasons-for-code-reviews-1.png?w=512&ssl=1)

---
### Ensure that codebase health does not degrade

Often codebases health degrade through small decreases over time, especially
when a team is under significant time constraints and they feel that they have
to take shortcuts in order to accomplish their goals.

---
### Ensure that codebase health does not degrade

The reviewer has responsibility over the code they are reviewing. They want to
ensure that the codebase stays consistent, maintainable, and all of the other
things.

---
### Shared knowledge

Since at least two more developers will review the changes, this ensures that
more people are aware of the changes in the code base.

---
### Reading and understanding code

- Code is much more often read than written.
- Code is written once.
- After that code is either changed or read and understood over and over again.

---
### Reading and understanding code

Reading and understanding code is a core skill of every software developer.

---
### Fixing a bug during the code review:

1. Here is this bug.
2. Fix
3. Review fix

---
### Fixing a bug in production

1. Client updates and finds a bug
2. Client reports the bug
3. Customer Success validates the bug
4. Area owner triages the bug
5. Bug is refined and planned
6. Fix
7. Review fix

---
### Extra cost

Four extra steps, involving at least 4 more people and not to mention the lost
trust of the client.

---
### What is the worst bug that you know of?

- [History's worst software bugs](https://www.wired.com/2005/11/historys-worst-software-bugs/)
- [Wikipedia List of bugs with significant consequences](https://en.wikipedia.org/wiki/List_of_software_bugs)


---

    int connections = GetConnections();
    if (connections = 2) {
    }

{{< note >}}
Could not find the article, but this cost something like 2 000 000 for a UNIX
distributor in the 1980s. That is 7 500 000 in 2023.
{{< /note >}}

---
## So how to do them?

There are lots of advice on how to conduct code reviews. It focuses on:

1. how to prepare yourself and the code for the review
2. how to write the review comments
3. improving effectiveness of code reviews
4. enforcing coding standards

---
## Creating a pull request

1. Create a pull request for each meaningful change
    - unrelated small improvements can get a in-place review
2. Make sure that the commits in the pull requests are
    - meaningful and atomic
    - properly tagged for the ChangeLog
3. Keep generated files in a separate commit in the pull request

---
## Creating a pull request

4. Add the JIRA task in the pull request title or summary
    - this will add link from JIRA to the pull request as well
5. Link the design document

---
## Posting a code review

> Do review your changes before asking others to review them

1. Did you do all of the above?
2. Try to re-read and re-understand the code, without using the knowledge in
   your head gathering while writing it.
3. Check for smells.
4. Check for coding style violations.
 
---
### Code author mindset
 
The issues found during the review are issues with the code and implementation.
Not with your personality.
 
---
### The IKEA effect

Two groups should price the value of IKEA furniture. One group got already
assembled furniture, the other group had to assemble them first. The results
showed that the second group was willing to pay 63 % more than the first group.
 
---
### Code author takeaway

> Reflect on the review, take something as a learning and you will do better the
> next time.

---
## Writing review comments

1. Non judgemental
2. Not personal

---
## Review Comments

All of us (and the reviewer and the author of the code) have to keep in mind
is:

> The code review is about the code produced by the developer at a certain
> moment in time.

---
### The reviewer

- You are reviewing the code, not the person.
- Refer to the code and author's behavior, not the author's traits

---
#### Prefer _I_ over _You_

> "I don't see the test for case X".

vs

> "You've missed the test for case X".

---
#### Ask questions

Avoid `shoulding` on people.

> "This should be named X"

vs 

> "How about X for a name?"

---
#### OIR for feedback

1. Observation

Describe your observations in an objective and neutral way. Refer to the
behavior if you have to talk about the author. Using an I-message is often
useful here.

---
#### OIR for feedback

2. Impact

Explain the impact that the observation has on you. Use I-messages.

---
#### OIR for feedback

3. Request

Use an I-message to express your wish or proposal.

---
#### OIR for feedback

1. Observation

> This method is over 100 lines long.

---
#### OIR for feedback

2. Impact

> This makes it hard for me to understand it.

---
#### OIR for feedback

3. Request

> I suggest extracting this and that into seperate methods.


---
### Discussion

1. The code review is a discussion.
2. The author keeps ownership over the changes.
    > "X told me to do Y"
3. Pull-requests are terrible place for discussions.
    - when a non-trivial discussion needs to happen use real-time communication

---
### Code reviews checklist


---
#### Preparation

1. Understand what the code is supposed to do on a high-level and then drill
   down.
2. Give yourself enough time - start at 500 LoC / hour and adjust to
   your speed and knowledge of the area.

---
#### The review

1. Understand what the code actually does. Mark any differences between the
   intended effects and the actual ones.
2. Are the changes covered by tests?
3. Do they follow DRY, KISS?
4. Does the commit message correctly describe the intent of the changes?
5. Are there any seemingly unrelated changes?
6. Does the code confirm to the coding standards?


---
### What to look for?

1. The code is well-designed.
2. The functionality is good for the users of the code.
3. Any UI changes are sensible and look good.
4. Any parallel programming is done safely.
5. The code isn’t more complex than it needs to be.
6. The developer isn’t implementing things they might need in the future but
   don’t know they need now.
 
---
### What to look for?

8. Code has appropriate unit tests.
9. Tests are well-designed.
10. The developer used clear names for everything.
11. Comments are clear and useful, and mostly explain why instead of what.
12. Code is appropriately documented
13. The code conforms to our style guides.

---
#### Unrelated changes

Seemingly unrelated changes are very smelly, because they should have been
either part of another pull request or there is some non-obvious flow that
actually makes them _related_ changes.


---
#### Extra-care topics

1. Timing issues
2. Multi-threaded code
3. Reentrancy

Think through all possible scenarios.


---
#### Coherent Labs Specific

1. Do the changes conform to the design document?
2. Which is off - the design document or the changes?
3. Artifacts for public changes
    - documentation
    - packaging
    - samples
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
##### Treat hard to understand code as wrong.

A given piece of code should be understandable and its correctness *verifiable*
in a minutes.

No matter whether the reviewer is an expert in the subsystem under review or
not. Once having a high-level grasp of the subsystem and the project, everyone
should be able to understand a small piece of code in minutes.

---
##### Lots of code changed

- it is no fun to make a review of thousands of lines of code
- the more code changed, the higher chance of bugs

---

Microsoft have a study, that shows that the most often changed files during
the development of a new Windows version, had the most bugs after the release.

---
##### Naming

Beware of names without any meaning that have significant lifetime.

---

    int Where(const std::vector<int>& v, int i)
    {
        auto it = std::find(v.begin(), v.end(), i);
        return std::distance(v.begin(), it);
    }

---

- `Where` ... what?
- `i` ... ??? - needle?
- `v` ... ??? - heystack, container?
- `it` - ok, lives for two lines

---
##### 4. Lack of logs

- Any error state should be properly log with meaningful information
- Major events in the program should be logged

---
##### 5. Magic Values

Lack of a properly named constant usually means:

> I need something here ... 13, 64, ... no ... 42 seems about right!

---
#### C++ specific code smells

So what to look (*sniff*) for in a C++ code review:

---
##### Pointers and references

Pointers are not the root of all evil, but do actually tend to cause problems.

---
###### Pointers?

Whenever a plain pointer is used, be sure that:

1. The pointer is not `nullptr`.
2. The pointed-to object will outlive the pointer
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
##### Container choice

Use a `vector`.

- double think and explain why using something else
- double think and explain why using `deque`
- double think and explain why using `*Deprecated` maps

---
###### Unless you have a compelling reasons like:


- it will hold a large number of elements, will change a lot, and the most
  common operation is going to be find an element with some property having
  a specific value
- elements must not be relocated

---
##### Container choice

The container invalidation is something that can be easily overlooked when
writing the code and what is worse - it will appear to work correctly in most
cases.

{{< frag c="Until the container relocates its storage ..." >}}

---
##### So, for each container check:

1. Is that the right type of container for the usage?
2. Is the container modified while being iterated?
3. Is there an iterator or a pointer inside the container that will out-live
   possible relocating operation for that container?

---
##### Container iteration

Container modification might not be obvious while doing iteration.

    auto& eventListeners = ModifyEventListeners(eventName);
    for (auto& listener: eventListeners)
    {
        dispatchEvent(listener, event);
    }

---
##### Container iteration

1. What if `dispatchEvent` gets to code that registers a new listener?
1. What if `dispatchEvent` gets to code that unregisters a listener?


---
##### Casts

Prefer `static_cast` and constructor casts whenever possible, since the compiler
will not let you shoot yourself in the foot for most cases.

The exception here is cast from `void*` to something else - make sure that the
type is correct.

---
##### Casts

*C-style* and `reinterpet_cast` are more dangerous, the compiler is much more
permissive with them.

---
##### Casts

1. The cast used is the safest one possible.
2. The types are correct.
3. The level of indirection is not casted away, i.e. casting from `T**` to `Y*`.
4. `const_cast` is usually a *design* smell, consider whether that is really the
   way to go

---
##### Assumptions that are not asserted or checked

This a bit a general category, so here are the most frequent examples:

1. Accessing array element that is not there. Out-of-bounds access or using
   `.front()`, `.back()` on an empty container.
2. Iterator returned by `find` is not the *end* iterator.
3. Index returned by `string::find` is not `npos`.

---

    v.erase(std::remove_if(v.begin(), v.end(), [](auto& e) {
        return e.IsNotUsed();
    });


---
##### Operations with strings

There are three major sources of issues when working with strings:

1. Null termination.
2. `char` and `wchar_t`
2. Encoding
4. Indices for substrings

---
###### Null termination.

- All the APIs working with strings have special cases whether and when the
  terminating null is written to buffer and whether it is counted in the
  returned length of the string.

---
###### Null termination.

- Some of the APIs return a null terminated buffer, some don't. It is very
  easy to use a non-terminated buffer and cause a bug.

Double check the documentation and the usage!

---
###### `char` and `wchar_t`

`char*` (UTF-8) and `wchar_t*` (UTF-16 or UTF32, depending on the platform)
do not mix well. Using anything but a unicode conversion library will most
likely result in bugs when non-ascii symbols are present.

---
###### Encoding

Check that the code assumes the correct encoding of the string, otherwise it
will still be garbage.


---
###### Indices

The `std::basic_string` API is full of taking indices and lengths, so it very
easy to make off by one error.

---
##### Smart pointers

Smart pointers are there to resolve the problems of the plain ones, but still
they have their own set of caveats.

---
##### Smart pointers
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
##### `auto` and `auto&`

`auto` is great, but it makes it very easy to create a copy of an object. Check
that:

1. `auto&` is used wherever possible.
2. `auto&` is not used for something that gets destroyed, i.e.

        auto& top = stack.top();
        stack.pop();

---
##### Objects with padding

It is very tempting to use functions like `memcmp` and `HashMemory` to create
comparison and hash functions for objects that are used in a hash map.

However they will use the uninitialized memory that is present in any padding
inside the object.

---
##### Objects with padding

1. Check that there is no padding inside objects used with `memcmp` and
   `HashMemory`.
2. Check that there are `static_assert`s that will enforce that. Keep in mind
   that some compilers will move the padding around when there is inheritance in
   the mix.

Consider adding the padding explicitly in the object to make it visible:

    char __padding[N] = { 0 };
    static_assert(No-Extra-Padding, "Extra padding detected");


---
##### Float precision and NAN

- Don't use NAN.

When compiled with *fast-math* it leads to undefined behavior.

---
##### Floating point comparison

    `some_float == 0`

Due to floating point representation this is almost always not what you need.
Use epsilon based comparisons for floating point numbers.

---
##### Error checking for OS and external APIs

Any OS or external API call can fail, so there must be a check for failures and
appropriate logging.

---
##### Hiding or reusing a variable

Hiding a

- function parameter
- member variable
- variable from a outer scope

is really confusing and makes the code harder to read and often wrong.

---
##### `using namespace` in headers

---
#### Coherent Labs specific code smells

---
##### Temp allocator

We are using a temp allocator that has significant performance gains. However,
we must be sure that each temp allocation is guarded by a `TempAllocatorScope`
and does not out-live that.

---
##### Temp allocator

Also look for cases, where the normal allocator is used, while the *temp
allocator* will suffice.

---
##### Entry-points for public APIs

The memory allocators rely on specific thread local variables to be set. So each
entry point of our products must be guarded with entry point from the
`COHTML_ENTRY_POINT` family.

---
##### Types used as task parameters

We do have a great system to prevent you from shooting yourself easily.

> Make sure that any added types are using the correct semantics.


---
##### Container and iterator invalidations

We iterate containers and invoke user code, C++ and JavaScript all over the
place.

---

    for (auto& listener : m_ListenersToNotify)
    {
        Notify(listener);
    }
    m_ListenersToNotify.clear();

---

    decltype(m_ListenersToNotify) toNotify;
    m_ListenersToNotify.swap(toNotify);
    for (auto& listener : toNotify)
    {
        Notify(listener);
    }

---

    TempAllocatorScope tempScope;
    TmpVector<Listener> toNotify(m_ListenersToNotify.begin(),
                                 m_ListenersToNotify.end());
    for (auto& listener : toNotify)
    {
        Notify(listener);
    }

---

    // Make m_ListenersToNotify LazyCopyVector

    // ensure that the iteration keeps a reference
    auto toNotify = m_ListenersToNotify;

    for (auto& listener : toNotify)
    {
        Notify(listener);
    }

---
##### Missing `TraceMembers` 

Every type that is exposed to the scripting and has member that is exposed
to the scripting **must** have `TraceMembers` override and trace all such
members there.

---

    decltype(m_ListenersToNotify) toNotify;
    m_ListenersToNotify.swap(toNotify);
    for (auto& listener : toNotify)
    {
        Notify(listener); // GC happens here?
    }

---

    // Trace both containers in TraceMembers

    COHERENT_DEBUG_ASSERT(m_CurrentToNotify.empty());
    m_ListenersToNotify.swap(m_CurrentToNotify);
    for (auto& listener : m_CurrentToNotify)
    {
        Notify(listener); // GC happens here?
    }
    m_CurrentToNotify.clear();

---



---
##### Timing issues in tests

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

    void DataBinder::RunEvaluatorsForProperty() {
        auto kt = modelData.PropertyEvaluators.find(propWithIndex);
        if (kt != modelData.PropertyEvaluators.end()) {
            auto& evaluators = *kt->second.ArrayPtr;
            for (size_t i = 0; i < evaluators.size(); ++i) {
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
- `kt` - ???
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

- hash of uninitialized memory

---

    using BindingEvaluatorArray = DomDeque<DomSharedPtr<BindingEvaluator>>;

---

- Why a `deque`?

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

- Entry point?

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

- `0x7ffffff` ?

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

- `string::data` was not null terminated - until C++'11

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
    }

---

- Create -> Where is the release?

---

    engine.trigger('Start');

---

- Is there anyone listening?

---

    auto handlers = EventMultiMap.equal_range(name);
    std::foreach(handlers.first, handlers.second, [this](auto& handler) {
        handler->Invoke(this);
    });

---

- iterator invalidation - multimap is harder to invalidate but still `handler`
  is user code, so it can register or unregister a handler
- If we have `"a"` key in the multimap, `itA` the iterator to `"a"`, the
  `handlers` range will be `[itA, end)` and `itA++` will be `end`. If the `"a"`
  handler inserts `"b"` handler, then `handlers` is still `[itA, end)`, but
  `itA++` will point to `"b"`

---

    struct EventRecord
    {
        Node* TargetNode;
    };
    
---

- How are we sure that the pointer will be valid?


---
## Takeaway

1. Prepare yourself to understand the code
2. If you are not convinced something is correct in a minute, poke it until you
   are confident that it is either correct or that it is broken.
3. Keep your eyes peeled for the smells.

---
# ?

---
# Resources

- https://phauer.com/2018/code-review-guidelines/
- https://www.slideshare.net/AprilWensel/compassionate-yet-candid-code-reviews
- https://google.github.io/eng-practices/review/developer/
- https://google.github.io/eng-practices/review/reviewer/
- https://chromium.googlesource.com/chromium/src/+/master/docs/cr_respect.md
- https://chromium.googlesource.com/chromium/src/+/refs/heads/main/docs/cl_respect.md
- https://www.michaelagreiler.com/code-reviews-at-microsoft-how-to-code-review-at-a-large-software-company/
- https://www.michaelagreiler.com/code-reviews-at-google/
- https://www.researchgate.net/publication/276162927_An_empirical_study_of_the_impact_of_modern_code_review_practices_on_software_quality

---

Do not forget to give feedback!
