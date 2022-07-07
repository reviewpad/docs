One of the key advantages of using Reviewpad is that you can access a set of built-in functions and actions that allow for flexible specifications and actions.

# Functions
______________

Reviewpad functions allow to query data from a `pull request` or `organization` in order to act on it.

The functions are organized into 4 categories:
- **[Pull Request](#pull-request)** - Functions to query pull request data.
- **[Organization](#organization)** - Functions to query organization data.
- **[User](#user)** - Functions to query user's data.
- **[Utilities](#utilities)** - Functions to help act on the queried data.
- **[Engine](#engine)** - Functions used to work with `reviewpad.yml` file.

## Pull Request
______________

Set of functions to get pull request details.

## &nbsp; assignees
______________

**Description**:

Returns the list of GitHub user login that are assigned to the pull request.

**Parameters**:

*none*

**Return value**:

`[]string`

The list of GitHub user login that are assigned to the pull request.

**Examples**:

```yml
$assignees()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: assignedToTechLead
    kind: patch
    description: Verifies if pull request was assigned only to a specific tech lead
    spec: $assignees() == ["john"]
```


## &nbsp; author
______________

**Description**:

Retrieves the pull request author GitHub login.

**Parameters**:

*none*

**Return value**:

`string`

The GitHub login of the pull request author.

**Examples**:

```yml
$author()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isAuthoredByTechLead
    kind: patch
    description: Verifies if authored by tech lead
    spec: $author() == "john"
```


## &nbsp; base
______________

**Description**:

Returns the name of the branch the pull request should be pulled into.

**Parameters**:

*none*

**Return value**:

`string`

The name of the branch the pull request should be pulled into.

**Examples**:

```yml
$base()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: shouldNotifyTechLead
    kind: patch
    description: Verifies if pull request is going to be pulled into "features" branch
    spec: $base() == "features"
```

## &nbsp; changed :zap:

| :zap: Professional Edition (*) |
| ------------------------------ |

_(*) It requires Reviewpad `edition` to be set to `professional` on `reviewpad.yml`_
```yml
edition: professional
```
______________


**Description**:

This built-in receives two regexes with named capturing groups denoted by the `@X` pattern.
These regexes are matched against the set of files in the patch.
The built-in checks if the value set of the first regex is contained in the second one.

**Parameters**:

| variable          | type   | description                        |
| ----------------- | ------ | ---------------------------------- |
| `preFilePattern`  | string | antecedent file pattern expression |
| `postFilePattern` | string | consequent file pattern expression |

**Return value**:

`boolean`

Returns `true` if the value set of the first regex is contained in the second one, `false` otherwise.

**Examples**:

```yml
$changed("@1.go", "@1.md")
```

This spec will return `true` if for each file in the patch with the extension `.go`
there is also a file in the patch with the same name with the extension `.md`.

A `revy.yml` example:

```yml
rules:
  - name: does-not-modify-tests
    kind: patch
    description: Does not modify test files
    spec: '!$changed("src/@1.java", "test/@1.java")'
```

## &nbsp; commitCount
______________

**Description**:

Returns the total number of commits made into the pull request.

**Parameters**:

*none*

**Return value**:

`int`

The total number of commits in the pull request.

**Examples**:

```yml
$commitCount()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: hasTooManyCommits
    kind: patch
    description: Verifies if it has than 3 commits
    spec: $commitCount() > 3
```

## &nbsp; commits
______________

**Description**:

Returns the list of commit messages of the pull request.

**Parameters**:

*none*

**Return value**:

`[]string`

The list of commit messages of the pull request.

**Examples**:

```yml
$commits()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: skipCIOnCommitMention
    kind: patch
    description: Verifies if any of the commit messages of the pull request contains "skip-ci"
    spec: $elemContains("skip-ci", $commits())
```


## &nbsp; createdAt
______________

**Description**:

Returns the time the pull request was created at.

**Parameters**:

*none*

**Return value**:

`int64`

The number of seconds elapsed since January 1, 1970 UTC.

**Examples**:

```yml
$createdAt()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: wasCreatedOnApril
    kind: patch
    description: Verifies if the pull request was created on the April 14th of 2011 at 16:00:49
    spec: $createdAt() == "2011-04-14T16:00:49Z
```


## &nbsp; description
______________

**Description**:

Returns the description of the pull request.

**Parameters**:

*none*

**Return value**:

`string`

The description of the pull request.

**Examples**:

```yml
$description()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: hasDescription
    kind: patch
    description: Verifies if the pull request description is "Testing description"
    spec: $description() == "Testing description"
```

## &nbsp; fileCount
______________

**Description**:

Returns the total number of files changed in the patch.

**Parameters**:

*none*

**Return value**:

`int`

The total number of files changed in the patch.

**Examples**:

```yml
$filesCount()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: changesTooManyFiles
    kind: patch
    description: Verifies if it has than 3 files
    spec: $fileCount() > 3
```


## &nbsp; hasAnnotation :zap:


| :zap: Professional Edition (*) |
| ------------------------------ |

_(*) It requires Reviewpad `edition` to be set to `professional` on `reviewpad.yml`_
```yml
edition: professional
```
______________


**Description**:

Verifies if the patch contains a symbol with the provided annotation, returning `true` or `false` as appropriate.

**Parameters**:

| variable     | type   | description                         |
| ------------ | ------ | ----------------------------------- |
| `annotation` | string | annotation to look for in the patch |

**Return value**:

`boolean`

Returns `true` if the patch changes a symbol with the provided annotation, `false` otherwise.

**Examples**:

```yml
$hasAnnotation("critical")
```

A `reviewpad.yml` example:

```yml
rules:
  - name: changesCritical
    kind: patch
    description: Verifies if changes critical code
    spec: $hasAnnotation("critical")
```

## &nbsp; hasCodePattern
______________

**Description**:

Verifies if the patch matches the provided code pattern, returning `true` or `false` as appropriate.

The code pattern needs to be a compilable regular expression.

**Parameters**:

| variable       | type   | description                        |
| -------------- | ------ | ---------------------------------- |
| `queryPattern` | string | query pattern to look for on patch |

**Return value**:

`boolean`

Returns `true` if the patch matches the code query, `false` otherwise.

**Examples**:

```yml
$hasCodePattern("placeBet\(.*\)")
```

A `reviewpad.yml` example:

```yml
rules:
  - name: usesPlaceBet
    kind: patch
    description: Verifies if uses placeBet
    spec: $hasCodePattern("placeBet\(.*\)")
```


## &nbsp; hasFileExtensions
______________

**Description**:

Determines whether all the extensions of the changed files on the patch are included on the provided list of file extensions, returning `true` or `false` as appropriate.

Each extension provided on the list needs to be a [glob](https://en.wikipedia.org/wiki/Glob_(programming)).

**Parameters**:

| variable     | type     | description                 |
| ------------ | -------- | --------------------------- |
| `extensions` | []string | list of all file extensions |

**Return value**:

`boolean`

Returns `true` if all file extensions in the patch are included in the list, `false` otherwise.

**Examples**:

```yml
$hasFileExtensions([".test.ts"])
```

A `reviewpad.yml` example:

```yml
rules:
  - name: changesAreOnlyTests
    kind: patch
    description: Verifies if changes are only on test files
    spec: $hasFileExtensions([".test.ts"])
```


## &nbsp; hasFileName
______________

**Description**:

Determines whether the provided filename is among the files on patch, returning `true` or `false` as appropriate.

**Parameters**:

| variable   | type   | description                                        |
| ---------- | ------ | -------------------------------------------------- |
| `filename` | string | filename to look for in the patch. case sensitive. |

**Return value**:

`boolean`

Returns `true` if the patch has a file with the provided filename, `false` otherwise.

The provided filename and the filename on the patch need to be exactly the same in order to get a positive result.

**Examples**:

```yml
$hasFileName("placeBet.js")
```

A `reviewpad.yml` example:

```yml
rules:
  - name: changesPlaceBet
    kind: patch
    description: Verifies if changes place bet file
    spec: $hasFileName("placeBet.js")
```


## &nbsp; hasFilePattern
______________

**Description**:

Determines whether the provided file pattern matches any of the files in the patch, returning `true` or `false` as appropriate.

The file pattern needs to be a [glob](https://en.wikipedia.org/wiki/Glob_(programming)).

**Parameters**:

| variable      | type   | description                            |
| ------------- | ------ | -------------------------------------- |
| `filePattern` | string | file pattern glob to look for on patch |

**Return value**:

`boolean`

Returns `true` if any of the files on patch matches the provided file pattern, `false` otherwise.

**Examples**:

```yml
$hasFilePattern("src/transactions/**")
```

A `reviewpad.yml` example:

```yml
rules:
  - name: changesTransactions
    kind: patch
    description: Verifies if changes transactions
    spec: $hasFilePattern("src/transactions/**")
```


## &nbsp; hasLinearHistory
______________

**Description**:

Checks if a pull request has a linear history.

A linear history is simply a Git history in which all commits come after one another, i.e., you will not find any merges of branches with independent commit histories.

**Parameters**:

*none*

**Return value**:

`boolean`

Returns `true` if it has a linear history. `false` otherwise.

**Examples**:

```yml
$hasLinearHistory()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: hasLinearHistory
    kind: patch
    description: Verifies if the pull request has a linear history
    spec: $hasLinearHistory()
```


## &nbsp; hasLinkedIssues
______________

**Description**:

Checks if a pull request has associated issues that might be closed by it.

**Parameters**:

*none*

**Return value**:

`boolean`

Returns `true` if it has linked issues. `false` otherwise.

**Examples**:

```yml
$hasLinkedIssues()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: hasLinkedIssues
    kind: patch
    description: Verifies if the pull request has linked issues
    spec: $hasLinkedIssues()
```


## &nbsp; head
______________

**Description**:

Returns the name of the branch where the pull request changes are implemented.

**Parameters**:

*none*

**Return value**:

`string`

The name of the branch where the pull request changes are implemented.

**Examples**:

```yml
$head()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: changesImplementedInDevelopmentBranch
    kind: patch
    description: Verifies if pull request changes are implemented in the "development" branch
    spec: $head() == "development"
```

## &nbsp; isDraft
______________

**Description**:

Verifies whether the pull request is `Draft`, returning `true` or `false` as appropriate.

To know more about [GitHub Draft pull request](https://github.blog/2019-02-14-introducing-draft-pull-requests/).

**Parameters**:

*none*

**Return value**:

`boolean`

A boolean which is `true` if the pull request is `Draft`, `false` otherwise.

**Examples**:

```yml
$isDraft()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isDraft
    kind: patch
    description: Verifies if is Draft
    spec: $isDraft()
```


## &nbsp; labels
______________

**Description**:

Returns the list of labels of the pull request.

**Parameters**:

*none*

**Return value**:

`[]string`

The list of labels of the pull request.

**Examples**:

```yml
$labels()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: onlyHasTestLabelAssigned
    kind: patch
    description: Verifies if the pull request only has "test" label assigned
    spec: $labels() == ["test"]
```


## &nbsp; milestone
______________

**Description**:

Returns the milestone title associated to the pull request.

**Parameters**:

*none*

**Return value**:

`string`

The milestone title associated to the pull request.

**Examples**:

```yml
$milestone()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isPartOfBugFixesMilestone
    kind: patch
    description: Verifies if the pull request is associated with the bug fixes milestone
    spec: $milestone() == "Bug fixes"
```


## &nbsp; reviewers
______________

**Description**:

Returns the list of GitHub user login or team slug that were requested to review the pull request.

**Parameters**:

*none*

**Return value**:

`[]string`

The list of GitHub user login or team slug that were requested to review the pull request.

**Examples**:

```yml
$reviewers()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: hasRequestedReviewers
    kind: patch
    description: Verifies if the pull request has reviewers
    spec: $reviewers() != []
```


## &nbsp; size
______________

**Description**:

Returns the total amount of changed lines in the patch.

Any added or removed line is considered a change. For instance, the following patch will have a `size` of 2 (one line removed and one line added.)

```diff
function helloWorld() {
-   return "Hello"
+   return "Hello World"
}
```

**Parameters**:

*none*

**Return value**:

`int`

The sum of all changed lines in the patch.

**Examples**:

```yml
$size()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isBigChange
    kind: patch
    description: Verifies if change is big
    spec: $size() > 100
```

## &nbsp; title
______________

**Description**:

Returns the title of the pull request.

**Parameters**:

*none*

**Return value**:

`string`

The title of the pull request.

**Examples**:

```yml
$title()
```

A `reviewpad.yml` example:

```yml
rules:
  - name: hasTitle
    kind: patch
    description: Verifies if the pull request title is "Test custom builtins"
    spec: $title() == "Test custom builtins"
```

## Organization
______________

Set of functions to get organization details.


## &nbsp; organization
______________

**Description**:

Lists all the members of the organization that owns the pull request.

If the authenticated user is an owner of the organization, this will return both concealed and public members, otherwise it will only return public members.

**Parameters**:

*none*

**Return value**:

`[]string`

The list of all members of the organization to where the pull request running against.

**Examples**:

```yml
$organization()
````

A `reviewpad.yml` example:

```yml
rules:
  - name: isAuthorFromOrganization
    kind: patch
    description: Verifies if author belongs to organization
    spec: $isElementOf($author(), $organization())
```


## &nbsp; team
______________

**Description**:

Returns the members of a team and child teams.

To list members in a team, the team must be visible to the authenticated user.

| :warning: Requires a GitHub token :warning: |
| ------------------------------------------- |

By default a GitHub action does not have permission to access organization members.

Because of that, in order for the function `team` to work we need to provide a GitHub token to the Reviewpad action.

[Please follow this link to know more](https://docs.reviewpad.com/docs/install-github-action-with-github-token).

**Parameters**:

| variable   | type   | description                          |
| ---------- | ------ | ------------------------------------ |
| `teamSlug` | string | The slug of the team name on GitHub. |

**Return value**:

`[]string`

Returns the list of all team and child teams members GitHub login.

**Examples**:

```yml
$team("devops")
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isAuthorByDevops
    description: Verifies if author belongs to devops team
    kind: patch
    spec: $isElementOf($author(), $team("devops"))
```

## User

Set of functions to get user details.


## &nbsp; totalCreatedPullRequests
______________

**Description**:

Returns the total number of pull requests created by the provided GitHub user login.

**Parameters**:

| variable    | type   | description           |
| ----------- | ------ | --------------------- |
| `userLogin` | string | the GitHub user login |

**Return value**:

`int`

The total number of created pull requests created by GitHub user login.

**Examples**:

```yml
$totalCreatedPullRequests($author())
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isJunior
    kind: patch
    description: Verifies if author is junior
    spec: $totalCreatedPullRequests($author()) < 3
```

## Utilities
______________

Set of functions to help handle the queried data.


## &nbsp; append
______________

**Description**:

Appends elements to the end of a slice and returns the updated slice.

**Parameters**:

| variable   | type     | description                                  |
| ---------- | -------- | -------------------------------------------- |
| `slice`    | []string | slice that will have elements appended to it |
| `elements` | []string | elements to be added to the end of the slice |

**Return value**:

`[]string`

Returns a new slice by appending the slices passed to it.

**Examples**:

```yml
$append(["a", "b"], ["c"])    # ["a", "b", "c"]
```

A `reviewpad.yml` example:

```yml
groups:
  - name: frontendAndBackendDevs
    description: Frontend and backend developers
    kind: developers
    spec: $append($team("frontend"), $team("backend"))

rules:
  - name: authoredByWebDeveloper
    kind: patch
    description: Authored by web developers
    spec: $isElementOf($author(), $group("frontendAndBackendDevs"))
```


## &nbsp; contains
______________

**Description**:

Determines whether a text includes a certain sentence, returning `true` or `false` as appropriate.

**Parameters**:

| variable         | type   | description                |
| ---------------- | ------ | -------------------------- |
| `text`           | string | The text to search in      |
| `searchSentence` | string | The sentence to search for |

**Return value**:

`boolean`

Returns `true` if searchSentence is found within the text, `false` otherwise.

**Examples**:

```yml
$contains("Testing string contains", "string contains")     #true
$contains("Testing string contains", "test")                #false
```

A `reviewpad.yml` example:

```yml
rules:
  - name: hasCustomKeywordInTitle
    kind: patch
    description: Verifies if the pull request title has "custom" keyword
    spec: $contains($title(), "custom")
```


## &nbsp; isElementOf
______________

**Description**:

Determines whether a list includes a certain value among its entries, returning `true` or `false` as appropriate.

**Parameters**:

| variable        | type      | description                |
| --------------- | --------- | -------------------------- |
| `searchElement` | literal   | The value to search for    |
| `list`          | []literal | The list to search in      |

**Return value**:

`boolean`

Returns `true` if searchElement is found within the list, `false` otherwise.

**Examples**:

```yml
$isElementOf("john", ["maria", "john"])  # true
$isElementOf(3, [1, 2])                  # false
```

A `reviewpad.yml` example:

```yml
rules:
  - name: authoredByJunior
    description: Verifies if author is junior
    kind: patch
    spec: $isElementOf($author(), $group("junior"))
```

## &nbsp; startsWith
______________

**Description**:

Determines whether a text starts with a certain sentence, returning `true` or `false` as appropriate.

**Parameters**:

| variable         | type   | description                |
| ---------------- | ------ | -------------------------- |
| `text`           | string | The text to search in      |
| `prefix`         | string | The prefix                 |

**Return value**:

`boolean`

Returns `true` if `prefix` is a prefix of `text`, `false` otherwise.

**Examples**:

```yml
$startsWith("Testing string contains", "Test")     #true
$startsWith("Testing string contains", "string contains")      #false
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isDevBranch
    kind: patch
    description: Verifies if the head branch of the pull requests starts with dev
    spec: $startsWith($head(), "dev/")
```

## Engine
______________

Set of functions used to handle `reviewpad.yml` file.

This functions should be used to access and handle data declared into `reviewpad.yml`, e.g. `$group` to get a defined group.


## &nbsp; group
______________

**Description**:

Lists all members that belong to the provided group. This group needs to be defined in the same `reviewpad.yml` file.

`group` is a way to refer to a defined set of users in a short way.

**Parameters**:

| variable    | type   | description                            |
| ----------- | ------ | -------------------------------------- |
| `groupName` | string | the group name to list the member from |

**Return value**:

`[]string`

Returns all members from the group.

**Examples**:

```yml
$group("techLeads")
```

A `reviewpad.yml` example:

```yml
groups:
  - name: techLeads
    description: Group with all tech leads
    kind: developers
    spec: '["john", "maria", "arthur"]'

rules:
  - name: isAuthorByTechLead
    description: Verifies if author is a tech lead
    kind: patch
    spec: $isElementOf($author(), $group("techLeads"))
```

## &nbsp; rule
______________

**Description**:

Evaluates a rule. This rule needs to be defined in the same `reviewpad.yml` file.

**Parameters**:

| variable   | type   | description   |
| ---------- | ------ | ------------- |
| `ruleName` | string | the rule name |

**Return value**:

`bool`

Returns the evaluation of the rule.

**Examples**:

```yml
$rule("isSmall")
```

A `reviewpad.yml` example:

```yml
rules:
  - name: isSmall
    description: Small pull request
    kind: patch
    spec: '$size() > 10'

  - name: isNotSmall
    description: Not a small pull request
    kind: patch
    spec: '!$rule(isSmall)'
```

# Actions
______________

## &nbsp; addLabel
______________

**Description**:

Adds a label to the pull request.

It will check if there is a label with this id in the `labels` section. 
If such label exists, it will use name of the label; if that property is not present, then it uses the key as the name.
If such label does not exist, it will use the input string and potentially create a label with that name.

**Parameters**:

| variable | type   | description       |
| -------- | ------ | ----------------- |
| `name`   | string | name of the label |

**Return value**:

None.

**Examples**:

```yml
$addLabel("bug")
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: label-small-pull-request
    description: Label small pull request
    if:
      - rule: isSmall
    then:
      - $addLabel("small")
```


## &nbsp; assignRandomReviewer
______________

**Description**:

Assigns a random user of the GitHub organization as the reviewer.
This action will always pick a user different than the author of the pull request.

However, if the pull request already has a reviewer, nothing happens. This is to prevent
adding a reviewer each time the pull request is updated.

When there's no reviewers to assign to, an error is returned.

**Parameters**:

*none*

**Return value**:

*none*

**Examples**:

```yml
$assignRandomReviewer()
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: assign-random-reviewer
    description: Assign random reviewer
    if:
      - rule: tautology
    then:
      - $assignRandomReviewer()
```


## &nbsp; assignReviewer
______________

**Description**:

Assigns a defined amount of reviewers to the pull request from the provided list of reviewers.

When there are not enough reviewers to assign to, a warning is returned.

If a reviewer from the defined list has performed a review, their review will be re-requested.

**Parameters**:

| variable           | type     | description                                                       |
| ------------------ | -------- | ----------------------------------------------------------------- |
| `reviewers`        | []string | list of GitHub logins to select from                              |
| `total` (optional) | int      | total of reviewers to assign. by default assigns to all reviewers |

**Return value**:

*none*

**Examples**:

```yml
$assignReviewer(["john", "marie", "peter"], 2)
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: review-code-from-new-joiners
    description: Assign senior reviewers to PRs from new joiners
    if:
      - rule: authoredByJunior
    then:
      - $assignReviewer($group("seniors"), 2)
```

## &nbsp; assignTeamReviewer
______________

**Description**:

Assigns a list of teams to review the pull request.

**Parameters**:

| variable                | type     | description                                                       |
| ----------------------- | -------- | ----------------------------------------------------------------- |
| `teamReviewers`         | []string | list of GitHub team slugs that will be requested to review        |

**Return value**:

*none*

**Examples**:

```yml
$assignTeamReviewer(["core", "support"])
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: review-core-code
    description: Assign review to core team when changes are made in critical code
    if:
      - rule: changesCritical
    then:
      - $assignTeamReviewer(["core"])
```

## &nbsp; close
______________

**Description**:

Closes a pull request.

**Parameters**:

*none*

**Return value**:

*none*

**Examples**:

```yml
$close()
```

A `revy.yml` example:

```yml
workflows:
  - name: close-pull-request
    description: Close pull request
    if:
      - rule: stalePullRequest
    then:
      - $close()
```


## &nbsp; comment
______________

**Description**:

Comments a pull request.

Note that this comment will always be added whenever this action is executed.

**Parameters**:

| variable  | type   | description         |
| --------- | ------ | ------------------- |
| `comment` | string | body of the comment |

**Return value**:

*none*

**Examples**:

```yml
$comment("This pull request has git conflicts. Please resolve them.")
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: conflict
    description: Ask to resolve conflict
    if:
      - rule: hasConflicts
    then:
      - $comment("This pull request has git conflicts. Please resolve them.")
```


## &nbsp; commentOnce
______________

**Description**:

Comments a pull request once.

If the comment is already present, then the action does nothing.

**Parameters**:

| variable           | type   | description                            |
| ------------------ | ------ | -------------------------------------- |
| `comment`          | string | body of the comment                    |

**Return value**:

*none*

**Examples**:

```yml
$commentOnce("This is your first contribution! Thank you!")
```

A `revy.yml` example:

```yml
workflows:
  - name: comment-pull-request
    description: Comment pull request
    if:
      - rule: firstContribution
    then:
      - $commentOnce("This is your first contribution! Thank you!")
```


## &nbsp; error :zap:

| :zap: Professional Edition (*) |
| ------------------------------ |

_(*) It requires Reviewpad `edition` to be set to `professional` on `reviewpad.yml`_
```yml
edition: professional
```
______________


**Description**:

Add a message to the errors section of the report.

**Parameters**:

| variable  | type   | description                       |
| --------- | ------ | --------------------------------- |
| `comment` | string | comment to be added in the errors |

**Examples**:

```yml
$error("Please do not touch these files.")
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: error-large-pull-requests
    description: Error about large pull requests
    if:
      - rule: isLarge
    then:
      - $error("This pull request was considered too large.")
```

## &nbsp; info :zap:

| :zap: Professional Edition (*) |
| ------------------------------ |

_(*) It requires Reviewpad `edition` to be set to `professional` on `reviewpad.yml`_
```yml
edition: professional
```
______________


**Description**:

Add a message to the info section of the report.

**Parameters**:

| variable  | type   | description                             |
| --------- | ------ | --------------------------------------- |
| `comment` | string | comment to be added in the informations |

**Examples**:

```yml
$info("Please do not forget to add the tests.")
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: info-large-pull-requests
    description: Info about large pull requests
    if:
      - rule: isLarge
    then:
      - $info("This pull request was considered too large.")
```

## &nbsp; merge
______________

**Description**:

Merge a pull request with a specific merge method.

By default, if no parameter is provided, it will perform a standard git merge.

| :warning: Requires GitHub token :warning: |
| ----------------------------------------- |

By default a GitHub action does not have permission to access organization members.

Because of that, in order for the function `team` to work we need to provide a GitHub token to the Reviewpad action.

[Please follow this link to know more](https://docs.reviewpad.com/docs/install-github-action-with-github-token).

**Parameters**:

| variable | type   | description                            |
| -------- | ------ | -------------------------------------- |
| `method` | string | merge method (merge, rebase or squash) |

**Return value**:

*none*

**Examples**:

```yml
$merge()
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: auto-merge-small-pull-request
    description: Auto-merge small pull request
    if:
      - rule: isSmall
    then:
      - $merge()
```


## &nbsp; removeLabel
______________

**Description**:

Removes a label applied to a pull request.

If the label is not applied to the pull request then nothing happens.

It will check if there is a label with this id in the `labels` section. 

If such label exists, it will use name of the label; if that property is not present, then it uses the key as the name.

If such label does not exist, returns an error.

**Parameters**:

| variable | type   | description       |
| -------- | ------ | ----------------- |
| `name`   | string | name of the label |

**Return value**:

Error if the label does not exist in the repository.

**Examples**:

```yml
$removeLabel("bug")
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: remove-small-label-in-pull-request
    description: Remove small label applied to pull request
    if:
      - rule: isNotSmall
    then:
      - $removeLabel("small")
```

## &nbsp; warn :zap:


| :zap: Professional Edition (*) |
| ------------------------------ |

_(*) It requires Reviewpad `edition` to be set to `professional` on `reviewpad.yml`_
```yml
edition: professional
```
______________


**Description**:

Add a message to the warnings section of the report.

**Parameters**:

| variable  | type   | description                         |
| --------- | ------ | ----------------------------------- |
| `comment` | string | comment to be added in the warnings |

**Examples**:

```yml
$warn("Please do not forget to add the tests.")
```

A `reviewpad.yml` example:

```yml
workflows:
  - name: warn-large-pull-requests
    description: Warn about large pull requests
    if:
      - rule: isLarge
    then:
      - $warn("This pull request was considered too large.")
```