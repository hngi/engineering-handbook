### Table of Content
***

1. [Introduction](#introduction)
2. [Automagic Formatting](#automagic-formatting)
    1. [Basic Linting Rules](#basic-linting-rules)
    2. [Basic Prettier Rules](#basic-prettier-rules)
3. [File Naming](#file-naming)
4. [Branch Naming](#branch-naming)
5. [Commit Message](#commit-message)
    1. [Message Header](#message-header)
    2. [Message Body](#message-body)
    3. [Message Footer](#message-footer)
    4. [Message Example](#message-example)
6. [Pull Request](#pull-request)
    1. [PR Title](#pr-title)
    2. [PR Description Template](#pr-description-template)
7. [Jira Issue](#jira-issue)
    1. [Story Issue](#story)
    2. [Bug Issue](#bug)
    3. [Task Issue](#task)
8. [Repo Readme](#repo-readme)

### Introduction
***
The following are rules guiding our coding process. Common linting and prettier standards are listed below, and they are based on our work with TypeScript, which is our development language. Projects should extend these linting and prettier standards within their codebase as may be needed.

### Automagic Formatting
***
Our projects should have the following packages installed within `devDependencies`:

> use `eslint` for non typescript codebase

- `tslint` >= 5.12.1
- `prettier` >= 1.16.4
- `husky` >= 1.3.1
- `lint-staged` >= 8.1.4

The following script commands should be added within the project package's `scripts`

```json
{
  "prettify": "prettier --write",
  "lint": "tslint -p tsconfig.json -c tslint.json"
}
```

The following pre-commit hooks configuration should also be specified within the project package

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "ignore": [],
    "linters": {
      "*.{ts,js,json,css,md}": [
        "yarn prettify",
        "yarn lint",
        "git add"
      ]
    }
  }
}
```

We specify basic linting and prettier standards that all projects within Engineering must extend so that linting and prettier issues are automagically resolved if engineers inadvertently attempt to commit non-compliant work.

#### Basic Linting Rules
***

```json
  {
    "defaultSeverity": "error",
    "extends": ["tslint:recommended"],
    "jsRules": {
      "no-unused-expression": true
    },
    "rules": {
      "arrow-parens": false,
      "array-type": [ false ],
      "curly": false,
      "eofline": true,
      "indent": [ true, "spaces", 2 ],
      "interface-name": [ true ],
      "max-classes-per-file": [ true, 1 ],
      "max-line-length": [
        true,
        {
          "limit": 150,
          "ignore-pattern": "^import |^export {(.*?)}|class [a-zA-Z]+ implements |//"
        }
      ],
      "member-access": [ false ],
      "member-ordering": [ false ],
      "no-console": [ true, "log", "info", "error", "warn" ],
      "no-empty": true,
      "no-empty-interface": true,
      "no-unused-expression": false,
      "object-literal-key-quotes": [true, "as-needed"],
      "object-literal-sort-keys": false,
      "one-line": [ true, "check-catch", "check-finally", "check-else", "check-open-brace" ]    ,
      "one-variable-per-declaration": [ false ],
      "ordered-imports": [ false ],
      "semicolon": [ true, "always" ],
      "variable-name": [ true, "ban-keywords", "check-format", "allow-leading-underscore" ]
    },
    "rulesDirectory": []
  }

```

#### Basic Prettier Rules
***

```json
{
  "bracketSpacing": true,
  "endOfLine": "lf",
  "semi": true,
  "trailingComma": "all",
  "tabWidth": 2,
  "useTabs": false
}

```

### File Naming
***
We favour the `PascalCase` convention. Files created should be named using the following format:

```
{Filetitle}{Filesubtype}{Filetype}.{file extension}
```

`Filetitle` - Indicates what the file is about. Examples include `permission-type`, `employee`, `permission`, `role`, etc.

`Filesubtype` - The subtype of the file. The file may be an interface of a repository. In this case, the subtype is `repository`. A file can have more than one subtype, but they must all be separated by the period [.].

`Filetype` - Indicates the type of the file. These could be `controller`, `dto` `service`, `repository`, `interface`, etc. A file should have only one type.

`Ffile extension` - Denotes the extension of the file, eg, `js`, `ts`, `json`, etc.

For example, an ExampleFile controller would be broken down thus:

```
file title: ExampleFile
file type: Controller
file extension: ts
```

Giving us `ExampleFileController.ts`.
NOTE that in the example above, there's no file subtype.

If a file has subtypes, all subtypes should be listed in their respective positions with the file type listed last just before the file extension. For example, an interface for ExampleFile service should be named with the following parts:

```
file title: ExampleFile
file subtype: Service
file type: Interface
file extension: ts
```

Giving us `ExampleFileServiceInterface.ts`.

### Branch Naming
***
Branches created should be named using the following format:

> Replace JIRA_ISSUE_ID with ID from the project management tool you are using eg. Clubhouse. Also integrate your project management tool to github is that is possible

```
{issue type}/{Jira issue ID}-{issue summary}
```

`issue type` - Indicates the context of the branch and should be one of:

- story
- bug
- task

`Jira Issue ID` - The ID of the Jira issue associated with the commit

`issue summary` - Short 2-3 words summary about what the branch contains

NOTE: The `issue type` is the same even for subtasks belonging to any of the issue types.

**Example**

```
story/DT-1048-resources-rest-endpoints
```

### Commit Message
***
A commit message consists of a **header**, a **body** and a **footer**, separated by a blank line.

Any line of the commit message cannot be longer than **100 characters!** This allows the message to be easier to read on github as well as in various git tools.
```
<issue ID>-<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```
These rules are adopted from [the AngularJS commit convention](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/).

#### Message Header
***
The message header is a single line that contains succinct description of the change containing an **issue ID**, a **type**, an optional **scope** and a **subject**.

`issue ID` - this is to provide the advantage of immediately seeing the related Jira issue at the very top of the commit message. This becomes really useful when one does `git log --oneline` which doesn't show as far down to the footer of the commit message.

#####`<type>`
This describes the kind of change that this commit is providing. It also applies to subtasks belonging to any of the Jira issue types..
* story (story)
* fix (bug fix)
* docs (documentation)
* style (formatting, missing semi colons, …)
* refactor
* test (when adding missing tests)
* task (maintain)

#####`<scope>`
Scope can be anything specifying place of the commit change. For example **events**, **kafka**, **userModel**, **authorization**, **authentication**, **loginPage**, etc...

#####`<subject>`
This is a very short description of the change.
* use imperative, present tense: “change” not “changed” nor “changes”
* don't capitalize first letter
* no dot (.) at the end

#### Message Body
***
* just as in `subject` use imperative, present tense: “change” not “changed” nor “changes”
* includes motivation for the change and contrasts with previous behavior

http://365git.tumblr.com/post/3308646748/writing-git-commit-messages

http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

#### Message Footer
***
Finished, fixed or delivered issue types should be listed on a separate line in the footer prefixed with "Finishes", "Fixes" , or "Delivers" keyword like this:

```
[(Finishes|Fixes|Delivers) JIRA_ISSUE_ID]
```

A reminder would be to think of completion this way:
- We **Finish** tasks, so for a `task` issue type, the footer should read:

        Finishes JIRA_ISSUE_ID

- We **Fix** bugs, so the footer for a `bug` issue type would be:

        Fixes JIRA_ISSUE_ID

- We **Deliver** stories, so the footer for a `story` issue type would be:

        Delivers JIRA_ISSUE_ID

If the commit doesn't mark completion, that is, this isn't the last commit for the issue, simply put the `JIRA_ISSUE_ID` on the footer like this:

```
[JIRA_ISSUE_ID]
```

#### Message Example
***
```
DT-1048-story(kafka): implement exactly once delivery

- ensure every event published to kafka is delivered exactly once
- implement error handling for failed delivery

[Delivers DT-1048]
```

### Pull Request
***

> NOTE: Every pull request should have at least two approvals before it can be merged.

Please copy both [PR and ISSUE]() template to your codebase

#### PR Title
***
The PR title should be named using the following format:

```
[JIRA_ISSUE_ID] [Story description]
```

#### PR Description Template
***

The description of the PR should contain the following headings and corresponding content in Markdown format.

```md
#### Description
#### Type of change
#### How Has This Been Tested?
#### Checklist:
#### Pivotal Tracker
``

#### Story
***

A Story issue should contain the following information

**Title:** one line describing the story

**Description:** a detailed description of the story. The description should include acceptance criteria

Example
```
Title:
As a Director of Success (DoS), I should be able to initiate a "Share with Fellows" action for engagements with "Completed" needs assessments to Fellows who are not externally placed.

Description:
* As a DoS, I should be able to select and share engagements with "Completed" Needs Assessment status
* After sharing an engagement, it shows up in the open engagement view
```

**Note:** Every story title should include the word ‘should’ as opposed to ‘can’. e.g. It’s unclear whether the story “User can delete comment” is a feature or a bug. “User should be able to delete comment” or “User should not be able to delete comment” are much clearer: the former is a feature, the latter a bug.

#### Bug
***
A Bug issue should contain the following information:

**Title:** A short description of the bug.

**Description:** What is currently happening? What should be happening?

**Steps To Reproduce:** Outline the steps to reproduce/show the bug.

**Resources:** Include screenshots and other assets to help explain/show the bug.

Example
```
Title:
Unwanted spaces should not appear between widgets.

Description:
The number of widgets showing per row is inconsistent.
When users view their dashboard, each row should contain 2 widgets per row.

Steps To Reproduce:
- Login to Skilltree dashboard.
- Some rows display unwanted spaces in between the widgets.

Resources:
Attach a screenshot of the error caused by the bug if applicable.
```

#### Task
***
A Task issue should include the following information:

**Title:** A short description of what needs to be done.

**Description:** Why is it needed? Does it help the team go faster or is it a dependency that could cause problems in the codebase if it’s not done?

**Acceptance Criteria:** These should include conditions that must be met for the chore to be accepted.

Example

```

Title:
Setup CI for user management service

Description:
Stable CI setup will make it easy to integrate our code while ensuring that all defined tests are passing per integration.

Acceptance Criteria:
Working CI integration with circle CI.
```

### Repo Readme
***
There's a lot we can do to encourage code reuse, sharing and onboarding of new team members. For instance, Every github repo should fill in the short description and website link at the top. Also, a github repository needs a proper Readme to best help out our teammates.

The belief here is that a repo should have a few defining traits to make it easy for anyone to understand it's purpose, set it up, run it's tests, and work towards contributing to it.

Each Repo's readme should follow this basic structure:

```
## Repo Name [codeclimate badge] [testing badge]

1 sentence tag line

# Description

3-5 sentences describing the code and what it's purpose is

## Documentation

List of endpoints exposed by the service

## Setup

Step by step instructions on how to get the code setup locally. This may include:

### Dependencies

List of libraries, tools, etc needed (e.g. yarn, node.js, python, etc)

### Getting Started

List of steps to get started (e.g. clone repo, submodule, .env file, etc)

### Run The Service

List of steps to run the service (e.g. docker commands)

### Microservices

List out the microservices if any that this repo uses

## Testing

Step by step instructions on how to run the tests so that the developer can be sure they've set up the code correctly

## Contribute

Any instructions needed to help others contribute to this repository

## Deployment

Step by step instructions so that the developer can understand how code gets updated
```
