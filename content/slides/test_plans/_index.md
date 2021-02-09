---
title: "The Zen of Test Plans"
date: 2021-01-21T17:10:27+03:00
type: slides
outputs: ["Reveal"]
draft: true

---
# Test Plans

---
### Disclaimer

- Some slides describe my thoughts while developing a feature.
- Your mileage my vary.

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
    - timelines communicated with client

---
##### Fixing

5. The issue is  _getting fixed_
    - discussed on the refinement
    - discussed on the planning meeting
    - _both_ of these take the whole team

---
##### Fixing

6. The issue is  about to  _getting fixed_
    - analyzed by a developer, reproduced again
    - developer feels frustrated
        - feels that fixing bugs is not cool as adding new shiny features, i.e.
          brings less value to the product
        - has to remember how the product works in the area of the bug.  Or
          worse - understand that for the first or n-th time
   - debugged, and fixed by a developer. **Note that is the only step that
     actually adds value.**
   - fix is reviewed by the team
   - test plan is reviewed

---
##### Fixed and releases

7. Added to changelog

---
#### A bug caught during the implementation

- debugged, and fixed by the developer.

**Note that is the only step**

---
#### A bug caught by Quality Assurance

- Replace the client with QA
- Replace the support team with QA/developer for the different steps
- Remove the changelog step

- We removed a single step - not having to add something to the changelog.
- We have reduced the frustration of the customers from "it doesn't work" to
  "when it will work".

---
## Quality as early as possible

We want to move quality as early as possible in the process of developing the
product.

- You can see this somewhere as "move quality left", since we tend to move
  things from left to right as they go in the process (on the board)

---
## The process

1. Design
    - Design Document
2. Implement
    - Test Plan
3. Review
    - Code Review
    - Demo
4. _Quality Assurance / Clients_

---
## The plan

1. Improve the Test Plans so that we get better quality at 2 and 3, instead of
   4.
2. Improve the design documents to get better quality at 1.

---
## Quality Assurance in successfull companies

> "At Google it's the product teams that own quality, not testers. Every
> developer is expected to do their own testing. The job of the tester is to
> make sure they have the automation infrastructure and enabling processes that
> support this self reliance. Testers enable developers to test," 

- James Whitaker, Engineering Directr

---
# Quality Assistance

- coined by Atlassian

> Engineers define how to test their own features and test them. Test engineers
> help with the definition of the test plan.

---
## 10X Developer

Better quality can easily get a developer a 2X multiplier
- A feature developed for a week - 5 days
- 3 bugs, each taking 3 or more days  of company(!) time to get fixed = 10 more
  days

Suddenly a feature trippled its cost, not counting the negative impacts.


---
## The zen of test plans

---
### Better sooner than later

Do as much of the test plan as early as possible.

- You know what is that you want to develop
- You will not be surprised that "we don't have a way to test my feature" when
  it is _ready_.
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
#### No test plan

> I need to do a linear search for the golden apple here. Is that ok for the
> performance?

---
#### No test plan

> How many apples will one have? 20? 20000?

> I can't run this with so many apples easily anyway ..."

---
####

It is fine.

---

![This is fine meme](https://thenib.com/wp-content/uploads/2019/08/this-is-not-fine-005-015ff5-1.png "Everything is on fire")

---
#### Having a plan

> I need to do a linear search for the golden apple here. Is that ok for the
> performance?

---
#### Having a plan

The test plan says we have to support up to 5000 apples.

> I have to do X to run the test, record the performance numbers I got and I
> will know.

---
### Test Cases

How to create them?

---
### Existing tests

- Take tests from browsers
- Take tests from web-platform tests

---
### New tests

---
#### Tests from the standard

> height being % of auto sized container

https://www.w3.org/TR/css-sizing-3/#cyclic-percentage-contribution


---
#### Genering test case generation techniques

- cause and effect
- state transitioning
- error guessing

---
#### Test case generation

- Consider them as different points of view on the problem and the solution.
- They work on any level - user story, design document, implementation


---
##### Cause and effect

Given X and Y happen, so should Z.

> Given ... when ... then ... .

---
##### State transitioning

In state X, given the event Y, go to state Z.

- obvious for state machine like - XHRs, WebSockets, etc.
- great for multi step algorithms - from HTML + CSS -> button the screen


---
##### Error guessing

Use prior experience to guess where the bug might be.

> What could possibly go wrong?

---
#### Tests from the user story

- What will work?
- What will not work?
- Edge cases?


---
#### Tests from the code

- expected cases
- edge-cases
- worst case
- _incorrect_ usage
- error guessing - aka "code smells"

---
##### Edge cases

- _I need the parent of the element..._
- Does it _always_ have a parent?

- _I need the document ..._
- Will there _always_ be a document?


---
##### What will not work

- _This won't support height % from autosized container_

- But still the behavior must be defined and documented.
- have a test for it.

---
##### Worst case

- _I have to support 5000 apples._

- Have a test with 5000 apples

---
##### Incorrect usage

- _`url('background.pn` is not valid CSS, we don't need to support it_

---
###### Still a usage

- This the style that we need to parse while the client is typing the url in the
  inspector
- it doesn't have to be valid, but it should not crash.

---
##### Error guessing

- empty or `nullptr` strings
- non-`null` terminated strings
- iterating and changing a collection
- lifetime
- multithreading
- ...

---
#### Tests

- edge-cases
- insane cases
- worst case
- _incorrect_ usage
- error guessing - aka "code smells"


---
### Interactions with other systems

- systems that are using the feature
- systems that the feature is using


---
### Interactions with other systems

- integration in Unreal Engine 4
- integration in Unity3D - C# API?
- WebAnimations API with style resolving and layout?

---
### Risks

- what is the worst that could happen?
- how are we testing for that?

---
### Risks

- start with 3 risks
- how can we mitigate them?

---
### Living Document

The test plan is a living document - as I do the implementation I see things:

- interactions with other features and systems
- edge-cases in the implementation
- risks for the implementation

---
### Muscle

Creating test plans and cases is like everything - it takes practice.

Lets say you want to do push-ups.
1. Start with 10, even not full ones. 
2. It will get easier day by day and one day you'll do easily 100.

You can't be at 2. without 1.


---
## Sample Story 

> Users can show real-time rendered content from the game inside the UI.

- the content can be updated by the game, so the user can tell the UI to update
- same content might be visible in different views

---
### Tests from the design document

---
### Sample _design document_

- Real-time content is rendered to render textures - i.e. images.
- The ways to show an image in cohtml are `<img>` and `background-image`
- User images are _special_ responses for resource requests, that have id and a
  texture
- User images will not be cached, since they are resources of the client, and he
  needs to have control over their lifetime
- the user images are reference counted and shared between elements and views
- the user images are rendered just as a regular texture


---
### The tests

- Given an image that is _user-image_, it is visible
- Given an image that is _user-image_ and the user updates it, it is updated.

- Image can be `<img>` or `background-image` 

---
### The tests

> User images are _special_ responses for resource requests, that have id and a
  texture

---
#### Resource requests 

Even for special resource requests:

- `load` and `error` should work
- aborting resource requests
- asynchronous response on any thread
- the element that needed the user-image is no longer alive?

---
#### Id

- what is a valid id? 
- what if the user duplicates an id?

---
#### Texture

- Lifetime of the texture? Who owns and determines its lifetime?
- What if the texture is bigger / smaller than the HTML element?, different
  aspect ratio?
- Mip levels?

---
#### Rendering

> the user images are rendered just as a regular texture 

- the content color space must be RGB with 32-bit color, pre-multiplied alpha

---
#### Lifetime

- What happens if the HTML element is no longer visible?
    - removed from the DOM
    - destroyed
- Threading and rendering resources

---
### Risks

1. Leaking user-images, life-time management with engines with automatic
   life-time management for resources.
2. Sharing user-images between views and elements
3. Drawing an texture while it is still being rendered.

---
# Test Plan Checklist

The test plan contains answers to the following questions:

---
## What I am creating?

1. How do I know that the feature is working?
    - List of expected behaviors
    - What kind of test system would be the most appropriate for verifying the
      behavior
2. How does this feature affect other features in the product?
    - List of the effects of the new or changed feature

---
## How will I do it?

3. How does the **implementation** of this feature affects other features?
    - List of the affected features
    - List of tests that will ensure that these features are not regressed when
      used on their own or in combination with the new feature

---
## How will I do it?

4. What could possibly go wrong?
    - List of risks for the feature
        - multithreading and synchronization
        - lifetime of objects and resources
        - performance
        - complexity
        - cross-platforms and cross JavaScript VMs
    - How am I going to know whether a risk will materialize into a problem?
    - How am I going to fix that?

---
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

