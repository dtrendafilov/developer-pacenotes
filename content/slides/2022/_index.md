---
title: "Development And Product 2022"
date: 2022-03-15:10:27+03:00
type: slides
outputs: ["Reveal"]
draft: false
---
# Strategy & Tactics for Development and Product

---

# Development 2022

## Select a higher gear

- From 70 km/h in 2nd gear @ 8000 rpm
- To 100 km/h in 5th gear @ 2500 rpm

---
## How?

1. Foster culture of continuous improvement
2. Standardize and automate more procedures
3. Enhance CI systems and test coverage to catch bugs sooner
4. Improve collaboration and communication between teams


---
## Continuous improvement

- Regular meetings for discussing the how to improve the work and the team
- Define [Improvement initiatives](https://docs.google.com/document/d/19jPKjVQXGSvBUpGq7duiL-ykV4187pia2pz20_x7qPA/edit)

---
### Bi-weekly meetings

- How can we work together better?
- How to improve the development process?

---
### Improvement Initiatives

An improvement initiative has:
1. Goal with time frame
   - Improve a certain set of metrics with some value by some date.
2. Leading metrics
   - A set of actions that we believe will provide the desired improvement.
   - Leading metric is the execution of the actions.

---
### Metrics

---
#### Cycle Time

- How easy is it to get something done?
- Working days from first status "In Progress" to last status "Done"
- Does not exclude vacations, national holidays, sick days
- 85% percentile of JIRA items done in the period

Cycle time of 16, means:
- in 16 working days 85% of tasks get from "In Progress" to "Done" in less than
  16 working days. 
- 15% of tasks take more than 16 working days

---
#### Lead Time

- Time from "Created" to "Done" in working days

---
#### Time fixing bugs

- How easy it is to fix a bug?
- How much effort are investing in fixing bugs instead of adding or improving
  features
- Sum of cycle time for bugs divided by the sum of cycle time for all tasks

---
#### Time to fix for bugs

- Time to recover
- How fast are we reacting to issues?
- How fast and easy are they to fix?
- Lead time for bugs in working days
- 85% percentile of fixed bugs for the period

20 means that 85% of bugs are fixed less than a month after being reported


---
#### Change Failure Rate

- The ratio between stories DONE and bugs found for a certain period

---
#### Deployment frequency

- How often we can deploy develop? I.e. the nightly build is green

---
## Standardize more procedures

- How to do [LTS versions](https://trello.com/c/m2Ghoyzh/66-lts-and-hotfix-releases)?
- [How to do releases?](https://trello.com/c/TrdGRg69/74-release-process)
- How to make a design document?


---
## Enhance CI systems and test coverage to catch bugs sooner

- The focus of the roadmap in first half of the year
- Bring smoke builds to 15 minutes
- The hunt for the release engineer is on, goal is to have one by Easter

---
## Improve collaboration and communication between teams

- Expect a meeting about that
- One idea is to do cross team design and code reviews for anything that might
  affect more than a single team

---
## Areas

---
### Area Lead

The Area Lead is responsible for the development of a certain area of the
product.

----
##### Area Lead

- Designs new features and improvements for existing ones in the area
- Talks to customers to gather feedback and recommendations for the area
- Estimates business value and development effort in the area
- Prioritizes the development in the area based on the estimates
- Triages customers issues in the area

---
#### Triage

- noun
  - (in medical use) the assignment of degrees of urgency to wounds or illnesses
    to decide the order of treatment of a large number of patients or
    casualties.
- verb
  - decide the order of treatment of (patients or casualties).

---
### Areas

- https://docs.google.com/spreadsheets/d/1LGFnSCj4JSfpiNhjCSnBWznBAJudGFHETNmrEs3SLWA/edit#gid=0
- Currently the components in JIRA
- Will be revised in the next two weeks

---
## Goals Development 2022


- Reduce Cycle Time by 40% - from 17 to 10 working days
- Reduce time fixing bugs by 30%  - from 21% to 15%
- Reduce time to fix for bugs by 30% - from 34 to 23 working days


---
# Roadmap

---
## Product development

https://docs.google.com/document/d/1uw3pWG7LBRSmexnx2hmv3-328bC8kGq12TKZ4oZ0JqQ/edit

---
### Priotization

---
#### RICE

---
##### Reach

How many customers does the feature affect

- Specific - single stakeholder or very few stakeholders with specific use cases
- Narrow - not specific, but less than half of the stakeholders
- Wide - more than half of the stakeholders
- Everyone

---
##### Impact

How much it will affect the stakeholders

- Low - i.e. quality of life improvement
- Medium - i.e. will allow easier creation of UI, or bug / crash, minor missing
  functionality
- High - will affect how UIs are created with Gameface
- Massive - will affect significantly what is possible and how it can be
  achieved with Gameface
- Critical - we’ll lose the customer, game is unreleasable, etc 

---
##### Confidence

How confident are we that the item will have the predicted reach, impact and
will fit in the estimate

- Hope - we have little control or no idea
- Low
- Medium
- High


---
##### Estimate 

Estimate of the cost of development and support of the feature

---
### Execution

- Treat estimates as budget
  - goal is to maximize value and quality while keeping in budget
  - what is left is going to be separated and reevaluated in the roadmap
- If we can't get enough value within the estimation, we need to change it and
  reevaluate the item in the roadmap
  - we want to know that as early as possible 

---

#### Roadmap status 

- https://docs.google.com/spreadsheets/d/1XpW79tqTtcig8SrsSv1M2XszvjM93MeRgRyqx7OyWXU/edit#gid=1368821119 
- will become part of the Sprint Demo

---
##### RICE Roadmap

https://docs.google.com/spreadsheets/d/1asMhgGk1FcPokAoGrEHXk1m9wOd3qZYTibbtzzzASsA/edit#gid=0

---
##### Client Roadmap

https://docs.google.com/spreadsheets/d/1ViUuRV_snlvrZFwuV7D_BL57g1DV2W_dHGvYTQcgbFg/edit#gid=626193824

---
## Product Goals 2021

- Reduce critical issues hit by customers by 50% - from 187 to 113 -> 40%
- Reduce feature requests by customers by 50% - from 93 to 30 -> 68%
- Reduce issues hit by customers by 50% - from 188 to 184

---
## Product 2022

- Set foot in the AA developers market
- Expand market share in AAA market

---
### Product Goals

- Be stable enough to allow scaling up the number of active projects and
  customers
- Reduce the barrier to entry to allow studios with less or no frontend skills
  to work with Gameface
- Reduce time customers spend profiling and optimizing their UI

---
### Product Goals as metrics

- Reduce critical issues hit by customers by 50% 
- Reduce issues hit by customers by 50%
- Reduce evaluation time and assistance required during evaluation

---
### No Crashes

- Sanitizers
- Fuzz tests
- WPT tests

---
### Less regressions

---
#### Regressions

- something was working as expected and got broken
- something was working in a certain way and got changed

---

- Fuzz tests between `develop` and last release
- WPT tests - detect changes in layout and rendering

---
### Documentation and samples

---
#### Content

- Orient towards "job to be done"
  - How to do a map?
  - How to do Point-Of-Interest system?
  - How to integrate the input?

---
#### Creation

- use doxygen to extract the API reference
- use doxybook2 to convert that to markdown
- use hugo and doks to create the documentation
  - better (faster) search
  - much better editing experience

---
#### Leverage the web

- Showcase features that easy to do with Gaa


---
### Developer portal

- treat as part of the business
- allow customers to manage their own organizations
- improve life of the Customer Success team

---
# ?

---
## Feedback

https://forms.gle/iJASt967imb1MWkm8

