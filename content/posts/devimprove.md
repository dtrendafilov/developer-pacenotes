# Goal of the developer discussions

- improve the development process across and cross teams

## Topics

- How do we discuss such topics and make decisions?
  - not a by a consensus, but all valid concerns should be noted and addressed
  - Decision is made when all valid concerns have been addressed
    - _"It takes some extra effort, but is doable"_  might be a valid way to
      address a concern
- How do we do changelogs?
- How to keep develop green?
- How to do features cross teams?

### How do we do changelogs?

- Current scheme - rely on tags inside commit messages
- Most often than not these are missing

#### Ideas for solution

- `pre-merge` and `pre-push` hook?
  - forbid push unless at least one commit has a tag
  - add [Internal] tag for non-public changes
- pull request template?
- keep an explicit changelog file?

## Commit messages

Each commit message must meet the following criteria

1. The text of the commit must be in English containing only wide known
   abbreviations.
2. One commit contains a subject (also known as commit title) and may contain a
   body.
   - the subject is a short summary of the changes in 50 characters or less
   - the body is more detailed explanation wrapped to 72 characters per line.
   - there must be an empty line between the subject and the body
3. The commit summary must be clear and short, showing the intent of the changes
made by the commit author. The body can contain more details about the changes.
4. The subject must be in *imperative mood*.
5. Include the JIRA story key in the body of the commit.
6. Annotate with tags any publicly visible changes

> The empty line between the subject and body and the character restrictions 
> are not arbitrary. GitHub is actually suggesting to follow them and will
> truncate your subject in most of its views if it does not conform to them.

### Commit Subject

The subject of the commit is a short summary of the intent of the changes.

- Limit the subject to 50 characters
- Capitalize the subject line (start with a capital letter)
- Do not end the subject line with a period
- Use *imperative mood* in the subject line. [Here][imperative] is why.

[imperative]: https://chris.beams.io/posts/git-commit/#imperative

#### Imperative Mood

*Imperative mood* means "spoken on writer as if giving a command or
instruction"

Examples:

- `Fix bug X` instead of `Fixed bug X`
- `Add feature Y`

The rule to follow is:

> If applied, this commit will *your-subject-line-here*

### Commit Body

The commit body explains the what and why (the *intent*) of the changes, not the
how.

- Separate the body with an empty line from the subject
- Wrap the body to 72 characters

If something must be explained in more detail, do this in the code under the
type of comments outside and inside of the function(s).

### Commit Notes in branches

When working in a feature branch it is important to keep the description of each
commit complete on its own and not dependent on the rest of the commits in the
branch. When browsing the source in GitHub and looking at the last commit that
has changed a file or a folder; looking at the history of a single file or
blaming a line in a file, all the tools show single commits and the rest of the
context of the branch is lost.

### Commit Notes and submodules

When updating a submodule, explain why you are updating it - what is the effect
of the updated submodule. You may use the commit notes from the commits in the
submodule that you are updating to.

Compare the info you get from two commits:
- "Update submodule"  
- "Add feature X in submodule Y"

### Commit Notes and JIRA

Including the JIRA story key in the commit links the JIRA stories with the
commits and pull-requests that are related to them. This way one can find the
commits that implement a give story or the story that a certain commit
implements.

1. Include the JIRA story key in the body of the commit.
2. Include the JIRA story key in the pull request title as well.

### Commit Notes and the ChangeLog

Use the special commit tags in the body to annotate any commits that have impact
on the users. Impact on the users have any commits that:

- add features
- fix bugs
- change API or behavior 

Special commit tags:

* ```[Release] 1.2.3.4```
* ```[Feature] One line description of the cool feature```
* ```[Enhancement] Improved feature X```
* ```[Fix] A bad bug got fixed```
* ```[API] Changed API from ... to ...```

#### Updating the changelog

If you have not tagged your commit message, you might be asked to edit the
changelog. You can use credentials and instructions provided in the email for
that.

In case the developer that has not properly tagged his commit is out of office,
the ones that have shipped the review have to update the changelog.


### More

More tips can be found in the [here](http://chris.beams.io/posts/git-commit/).
