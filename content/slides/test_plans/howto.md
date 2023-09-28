# Test Plan How To

Version 1.1 - 23.03.2021.

The test plan contains answers to the following questions:

## How do I know that the feature is working?

Start with the User Story.

- What are the user visible features that will or may change?
- Is it platform dependent?

Is there a design document?

> List the expected behaviors

## List of tests

Start with each behavior.

Ask yourself - What is this? What it can be? How this can be done?

Given *initial context* and *more context* when *event - feature used* then
*expected outcomes*.

1. Can you use tests from other sources?
   - from the Web Standard
   - from the Web Platform Tests or existing browsers
   - from skia
2. Can you re-use some of our tests?
3. What are the things that will not work? (i.e. negative tests)
   - something may not work, but still we have to handle it gracefully
4. What are the boundary cases?

> List the tests is a series of scenarios described in the *Given-when-then*
> form

Here is a moment (not the only one!) where you can ask the QA team for help.

### What kind of test system would be the most appropriate for each test?

> List the system that you are going to use for the tests.

In case there is no appropriate automated test system for a certain test -
attach the assets and describe any steps for the manual tests.

## How does this feature affect other features in the product?

Most features depend on some other features, but covering all the possible
combinations might be hard.

> List the features that affect the new or changed feature.

## How does the **implementation** of this feature affects other systems?

### What systems are you going to use in the implementation?

Will you be using the system as intended? The moment you need to change
something in the system - it is not being used as intended.

If a system is used as intended - assume it works correctly.

> List the systems that will be changed
> List the tests that will ensure that these systems are not regressed when
  used on their own or in combination with the new feature

## What could possibly go wrong?

Risk clue database:

- Web features
  - combinations with other features
  - edge cases - most are described in the standard, but it may be in the
    section about another feature that interacts with the current one
  - performance
  - breaking changes
- Rendering features
  - All the backends X Unreal Engine 4 backend X Unity3D X C# backend
  - AA
  - Scaling
  - performance
  - packaging of shaders and backends
  - breaking changes
- General
    - multithreading and synchronization
    - lifetime of objects and resources
    - complexity
    - cross-platforms and cross JavaScript VMs
    - platform changes - packaging and scripts
    - third-party libraries - runtime dependencies, packaging and scripts
    - Cohtml Application is used on all platforms
- Bugs
  - crash in near / similar cases

> List the risks for the feature.  For each risk answer "How am I going to know
> whether a risk will materialize into a problem? How am I going to fix that?"


