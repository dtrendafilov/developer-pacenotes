---
title: "How to review C++ code"
date: 2021-01-21T17:10:27+03:00
type: slides
outputs: ["Reveal"]
draft: true
---

---
# Test Plans

---
### Disclaimer

- Some slides describe feelings and thoughts of developers while working.
- These are my feelings and thoughts in the describe situations.

---
## Quality as early as possible

---
### Impact 

The later in the process a bug is found, the worse impact it has on the product.

---
### Cost to fix

The later in the process a bug is found, the more it costs to fix it.

---
#### A bug found by a client

Bug found _after the release_.

---
##### Client hits a bug

1. Client hits a bug and spents time to understand what is happening.
   - frustration with the product grows
2. Client opens an issue

---
##### Understand and Triage

3. The Support engineers analyze and try to reproduce the issue, ask the client
questions
    - time of our engineers
    - more frustration for the client

4. The issue gets escalated
    - discussed on the Daily SoS meeting
    - triaged by a developer or area lead
    - scheduled for a version to be fixed
    - communicated with client about timelines

---
##### Fixing

5. The issue is _getting fixed_
    - discussed on the refinement and planning meetings by the team
    - analyzed by a developer, reproduced again
    - developer feels frustrated
        - feels that fixing bugs is not cool as adding new shiny features, i.e.
          brings less value to the product
        - has to find in his head how the product works in the area of the bug.
          Or worse - understand that for the first or n-th time
   - debugged, and fixed by a developer. **Note that is the only step that
     actually adds value.**
   - fix is reviewed by the team
   - added to changelog

---
#### A bug caught during the implementation

- debugged, and fixed by a developer.

**Note that is the only step**

---
#### A bug caught by Quality Assurance

- Replace the client with QA
- Remove the support team, the triage and the changelog steps

---
# Quality Assurance vs Quality Assistance

- QA in big companies
- 10X Developer - better quality

---
---
## The zen of test plans

---
### Better sooner than later

Do as much of the test plan as early as possible.

- *The time (budget) is spent, I have 3 tests, so it should be enough.*
- Better prediction how much work is actually left.
    - *I have half the test cases covered, so I am around the middle*
- Rationality ends with the first line of code written

---
### Freedom

Following the test plan gives some structure to the implementation process.
Structure gives freedom to focus on the hard problems.

Freedom from having to think about what is the next step, *How do I know for
sure that this works?*

---
#### Freedom - no test plan

> *I need to do a linear search for the golden apple here. Is that ok for the
> performance?*

> *How many apples will one have? 20? 20000?*
> *I can't run this with so many apples easily anyway ..."*

---

It is fine.

---
#### Freedom - with test plan

> *I need to do a linear search for the golden apple here. Is that ok for the
> performance?*

The test plan says we have to support up to 5000 apples.
> *I have to do X to run the test, record the performance numbers I got and I
> will know.*

---
### Test Cases

How to create them?

---
### Existing tests

- tests from browsers
- web-platform tests

---
### New tests

- Extract tests from the standard.

> height being % of auto sized container

- edge-cases
- "insane" cases - what will not work
- worst case
- incorrect usage

---
### Interactions with other systems

- systems that are using the feature
- systems that the feature is using

---
### Risks

- what is the worst that could happen?
- how are we testing for that?

---
### Living Document

---
### Muscle

This is like doing push-ups - takes practice.

---
# Test Plan Checklist

The test plan contains answers to the following questions:

1. How do I know that the feature is working?
    - List of expected behaviors
    - What kind of test system would be the most appropriate for verifying the
      behavior
2. How does this feature affect other features in the product?
    - List of the effects of the new or changed feature
3. How does the **implementation** of this feature affects other features?
    - List of the affected features
    - List of tests that will ensure that these features are not regressed when
      used on their own or in combination with the new feature
4. What could possibly go wrong?
    - List of risks for the feature
        - multithreading and synchronization
        - lifetime of objects and resources
        - performance
        - complexity
        - cross-platforms and cross JavaScript VMs
    - How am I going to know whether a risk will materialize into a problem?
    - How am I going to fix that?


## List of tests

List of tests is a series of scenarios described in the *Given-when-then* form

Given *initial context* and *more context* when *event - feature used* then
*expected outcomes*.

---
## Resources

- https://www.atlassian.com/inside-atlassian/quality-assurance-vs-quality-assistance
- https://www.atlassian.com/inside-atlassian/qa
- https://www.atlassian.com/agile/software-development/testing
- https://www.youtube.com/watch?v=yRP29wFqu20

---
## ?

