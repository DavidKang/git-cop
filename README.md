# Git Cop

[![Gem Version](https://badge.fury.io/rb/git-cop.svg)](http://badge.fury.io/rb/git-cop)
[![Code Climate GPA](https://codeclimate.com/github/bkuhlmann/git-cop.svg)](https://codeclimate.com/github/bkuhlmann/git-cop)
[![Code Climate Coverage](https://codeclimate.com/github/bkuhlmann/git-cop/coverage.svg)](https://codeclimate.com/github/bkuhlmann/git-cop)
[![Gemnasium Status](https://gemnasium.com/bkuhlmann/git-cop.svg)](https://gemnasium.com/bkuhlmann/git-cop)
[![Circle CI Status](https://circleci.com/gh/bkuhlmann/git-cop.svg?style=svg)](https://circleci.com/gh/bkuhlmann/git-cop)
[![Patreon](https://img.shields.io/badge/patreon-donate-brightgreen.svg)](https://www.patreon.com/bkuhlmann)

Enforces Git rebase workflow with consistent Git commits for a clean and easy to read/debug project
history.

<!-- Tocer[start]: Auto-generated, don't remove. -->

## Table of Contents

  - [Features](#features)
  - [Requirements](#requirements)
  - [Setup](#setup)
    - [Install](#install)
    - [Configuration](#configuration)
      - [Enablement](#enablement)
      - [Severity Levels](#severity-levels)
      - [Regular Expressions](#regular-expressions)
    - [Rake](#rake)
  - [Usage](#usage)
    - [Command Line Interface (CLI)](#command-line-interface-cli)
    - [Git Hooks](#git-hooks)
    - [Continuous Integration (CI)](#continuous-integration-ci)
      - [Circle CI](#circle-ci)
      - [Travis CI](#travis-ci)
  - [Cops](#cops)
    - [Commit Author Email](#commit-author-email)
    - [Commit Author Name Capitalization](#commit-author-name-capitalization)
    - [Commit Author Name Parts](#commit-author-name-parts)
    - [Commit Body Bullet](#commit-body-bullet)
    - [Commit Body Leading Space](#commit-body-leading-space)
    - [Commit Body Line Length](#commit-body-line-length)
    - [Commit Body Phrase](#commit-body-phrase)
    - [Commit Body Presence](#commit-body-presence)
    - [Commit Subject Length](#commit-subject-length)
    - [Commit Subject Prefix](#commit-subject-prefix)
    - [Commit Subject Suffix](#commit-subject-suffix)
  - [Style Guide](#style-guide)
    - [General](#general)
    - [Commits](#commits)
    - [Branches](#branches)
    - [Tags](#tags)
    - [Rebases](#rebases)
    - [Pull Requests](#pull-requests)
    - [GitHub](#github)
  - [Tests](#tests)
  - [Versioning](#versioning)
  - [Code of Conduct](#code-of-conduct)
  - [Contributions](#contributions)
  - [License](#license)
  - [History](#history)
  - [Credits](#credits)

<!-- Tocer[finish]: Auto-generated, don't remove. -->

## Features

- Enforces a [Git Rebase Workflow](http://www.bitsnbites.eu/a-tidy-linear-git-history).
- Enforces a clean and consistent Git commit history.
- Provides a suite of cops which can be customized to your preference.

## Requirements

0. [Ruby 2.4.1](https://www.ruby-lang.org)

## Setup

### Install

For a secure install, type the following (recommended):

    gem cert --add <(curl --location --silent https://www.alchemists.io/gem-public.pem)
    gem install git-cop --trust-policy MediumSecurity

NOTE: A HighSecurity trust policy would be best but MediumSecurity enables signed gem verification
while allowing the installation of unsigned dependencies since they are beyond the scope of this
gem.

For an insecure install, type the following (not recommended):

    gem install git-cop

### Configuration

This gem can be configured via a global configuration:

    ~/.config/git-cop/configuration.yml

It can also be configured via [XDG environment variables](https://github.com/bkuhlmann/runcom#xdg)
as provided by the [Runcom](https://github.com/bkuhlmann/runcom) gem.

The default configuration is as follows:

    :commit_author_email:
      :enabled: true
      :severity: :error
    :commit_author_name_capitalization:
      :enabled: true
      :severity: :error
    :commit_author_name_parts:
      :enabled: true
      :severity: :error
      :minimum: 2
    :commit_body_bullet:
      :enabled: true
      :severity: :error
      :blacklist:
        - "\\*"
        - "•"
    :commit_body_leading_space:
      :enabled: true
      :severity: :error
    :commit_body_line_length:
      :enabled: true
      :severity: :error
      :length: 72
    :commit_body_phrase:
      :enabled: true
      :severity: :error
      :blacklist:
        - obviously
        - basically
        - simply
        - of course
        - just
        - everyone knows
        - however
        - easy
    :commit_body_presence:
      :enabled: false
      :severity: :warn
      :minimum: 1
    :commit_subject_length:
      :enabled: true
      :severity: :error
      :length: 72
    :commit_subject_prefix:
      :enabled: true
      :severity: :error
      :whitelist:
        - Fixed
        - Added
        - Updated
        - Removed
        - Refactored
    :commit_subject_suffix:
      :enabled: true
      :severity: :error
      :whitelist:
        - "\\."

Feel free to take this default configuration, modify, and save as your own custom
`configuration.yml`.

#### Enablement

By default, most cops are enabled. Accepted values are `true` or `false`. If you wish to disable a
cop, set it to `false`.

#### Severity Levels

By default, most cops are set to `error` severity. If you wish to reduce the severity level of a
cop, you can set it to `warn` instead. Here are the accepted values and what each means:

- `warn`: Will count as an issue and display a warning but will not cause the program/build to
  fail. Use this if you want to display issues as reminders or cautionary warnings.
- `error`: Will count as an issue, display error output, and cause the program/build to fail. Use
  this setting if you want to ensure bad commits are prevented.

#### Regular Expressions

Some cops support *whitelist* or *blacklist* options. These lists can consist of strings, regular
expressions, or a combination thereof. If you need help constructing complex regular expressions for
these lists, try launching an IRB session and using `Regexp.new` or `Regexp.escape` to experiment
with the types of words/phrases you want to turn into regular expressions.

### Rake

This gem provides optional Rake tasks. They can be added to your project by adding the following
requirement to the top of your `Rakefile`:

    require "git/cop/rake/setup"

Now, when running `bundle exec rake -T`, you'll see `git_cop` included in the list.

If you need a concrete example, check out the [Rakefile](Rakefile) of this project for details.

## Usage

### Command Line Interface (CLI)

From the command line, type: `git-cop --help`

    git-cop -c, [--config]        # Manage gem configuration.
    git-cop -h, [--help=COMMAND]  # Show this message or get help for a command.
    git-cop -p, [--police]        # Check feature branch for issues.
    git-cop -v, [--version]       # Show gem version.

To check if your Git commit history is clean, run: `git-cop --police`. It will exit with a failure
if at least one issue, with error severity, is detected.

This gem does not check commits on `master`. This is intentional as you would, generally, not want
to rewrite or fix commits on `master`. This gem is best used on feature branches as it automatically
detects all commits made since `master` on the feature branch.

Here is an example workflow, using gem defaults with issues detected:

    cd example
    git checkout -b test
    printf "%s\n" "Test content." > test.txt
    git add --all .
    git commit --message "This is a bogus commit message that is also terribly long and will word wrap"
    git-cop --police

    # Output:
    Running Git Cop...

    ae84620bda4c6c4fbd22f24fecee575319d25546 (Brooke Kuhlmann, 5 days ago): Added Pellentque morbi-trist sentus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam.
      ERROR: Commit Subject Length. Invalid length. Use 72 characters or less.

    97a9d4bf0cd73f0af2b31f22e0920134bb06575d (Brooke Kuhlmann, 5 days ago): Add one test file
      WARN: Commit Body Presence. Invalid body. Use a minimum of 1 line (not empty).
      ERROR: Commit Subject Prefix. Invalid prefix. Use: "Fixed", "Added", "Updated", "Removed", "Refactored".
      ERROR: Commit Subject Suffix. Invalid suffix. Use: "\.".

    2 commits inspected. 4 issues detected (1 warning, 3 errors).

### Git Hooks

This gem can be wired up as a [Git Hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
if desired. For example, you can add *post-commit* hook support by using the following code:

```
#! /usr/bin/env bash

# DESCRIPTION
# Defines Git post-commit functionality.

# SETTINGS
set -o nounset
set -o errexit
set -o pipefail
IFS=$'\n\t'

# EXECUTION
if ! command -v git-cop > /dev/null; then
   printf "%s\n" "[git]: Git Cop not found. To install, run: gem install git-cop --trust-policy MediumSecurity."
   exit 1
fi

git-cop --police --commits $(git log --pretty=format:%H -1)
```

TIP: To configure global Git Hooks, add the following to your `.gitconfig`:

```
[core]
  hooksPath = ~/.git_template/hooks
```

Now you can customize Git Hooks for all of your projects.
[Check out these examples](https://github.com/bkuhlmann/dotfiles/tree/master/home_files/.git_template/hooks).

You can also apply the above Git Hook to your local project by editing your `.git/hooks/post-commit`
file. *This is not recommend*. Use a global configuration as it'll reduce project maintenance costs
for you.

### Continuous Integration (CI)

This gem automatically configures itself for known CI build servers.

Calculation of commits is done by reviewing all commits made on the feature branch since branching
from `master`. Below are the build servers which are supported and *tested*. If you have a build
server that is not listed, please open a pull request with support.

#### Circle CI

This gem automatically detects and configures itself for [Circle CI](https://circleci.com) builds by
checking the `CIRCLECI` environment variable. No additional setup required!

#### Travis CI

This gem automatically detects and configures itself for [Travis CI](https://travis-ci.org) builds
by checking the `TRAVIS` environment variable. No additional setup required!

## Cops

The following details the various cops provided by this gem to ensure a high standard of commits for
your project.

### Commit Author Email

| Enabled | Severity | Defaults |
|---------|----------|----------|
| true    | error    | none     |

Ensures author email address exists. Git requires an author email when you use it for the first time
too. This takes it a step further to ensure the email address loosely resembles an email address.

    # Disallowed
    mudder_man

    # Allowed
    jayne@serenity.com

### Commit Author Name Capitalization

| Enabled | Severity | Defaults |
|---------|----------|----------|
| true    | error    | none     |

Ensures auther name is properly capitalized. Example:

    # Disallowed
    jayne cobb
    dr. simon tam

    # Allowed
    Jayne Cobb
    Dr. Simon Tam

### Commit Author Name Parts

| Enabled | Severity |  Defaults  |
|---------|----------|------------|
| true    | error    | minimum: 2 |

Ensures author name consists of, at least, a first and last name. Example:

    # Disallowed
    Kaylee

    # Allowed
    Kaywinnet Lee Frye

### Commit Body Bullet

| Enabled | Severity |          Defaults         |
|---------|----------|---------------------------|
| true    | error    | blacklist: `["\\*", "•"]` |

Ensures commit message bodies use a standard Markdown syntax for bullet points. Markdown supports
the following syntax for bullets:

    *
    -

It's best to use `-` for bullet point syntax as `*` are easier to read when used for *emphasis*.
This makes parsing the Markdown syntax easier when reviewing a Git commit as the syntax used for
bullet points and *emphasis* are now, distinctly, unique.

### Commit Body Leading Space

| Enabled | Severity | Defaults |
|---------|----------|----------|
| true    | error    | none     |

Ensures there is a leading space between the commit subject and body. Generally, this isn't an issue
but sometimes the Git CLI can be misued or a misconfigured Git editor will smash the subject line
and start of the body as one run-on paragraph. Example:

    # Disallowed

    Curabitur eleifend wisi iaculis ipsum.
    Pellentque morbi-trist sentus et netus et malesuada fames ac turpis egestas. Vestibulum tortor
    quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu_libero sit amet quam
    egestas semper. Aenean ultricies mi vitae est. Mauris placerat's eleifend leo. Quisque et sapien
    ullamcorper pharetra. Vestibulum erat wisi, condimentum sed, commodo vitae, orn si amt wit.

    # Allowed

    Curabitur eleifend wisi iaculis ipsum.

    Pellentque morbi-trist sentus et netus et malesuada fames ac turpis egestas. Vestibulum tortor
    quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu_libero sit amet quam
    egestas semper. Aenean ultricies mi vitae est. Mauris placerat's eleifend leo. Quisque et sapien
    ullamcorper pharetra. Vestibulum erat wisi, condimentum sed, commodo vitae, orn si amt wit.

### Commit Body Line Length

| Enabled | Severity |  Defaults  |
|---------|----------|------------|
| true    | error    | length: 72 |

Ensures each line of the commit body is no longer than 72 characters in length for consistent
readabilty and word-wrap prevention on smaller screen sizes. For further details, read Tim Pope's
original [article](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) on the
subject.

### Commit Body Phrase

| Enabled | Severity |                       Defaults                       |
|---------|----------|------------------------------------------------------|
| true    | error    | blacklist: (see configuration list, mentioned above) |

Ensures non-descriptive words/phrases are avoided in order to keep commit message bodies informative
and specific. The blacklist is case insensitive. Detection of blacklisted words/phrases is case
insensitve as well. Example:

    # Disallowed

    Obviously, the existing implementation was too simple for my tastes. Of course, this couldn't be
    allowed. Everyone knows the correct way to implement this code is to do just what I've added in
    this commit. Easy!

    # Allowed

    Necessary to fix due to a bug detected in production. The included implentation fixes the bug
    and provides the missing spec to ensure this doesn't happen again.

### Commit Body Presence

| Enabled | Severity |  Defaults  |
|---------|----------|------------|
| false   | warn     | minimum: 1 |

Ensures a minimum number of lines are present within the commit body. Lines with empty characters
(i.e. whitespace, carriage returns, etc.) are considered to be empty.

### Commit Subject Length

| Enabled | Severity |  Defaults  |
|---------|----------|------------|
| true    | error    | length: 72 |

Ensures the commit subject length is no more than 72 characters in length. This default is more
lenient than Tim Pope's
[50/72 rule](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html) as it gives one
the ability to formulate a more descriptive subject line without being too wordy or suffer being
word wrapped.

### Commit Subject Prefix

| Enabled | Severity |        Defaults        |
|---------|----------|------------------------|
| true    | error    | whitelist: (see below) |

Ensures the commit subject uses consistent prefixes that help explain *what* is being commited. The
whitelist *is* case sensitive. The default whitelist consists of the following prefixes:

- *Fixed* - Existing code that has been fixed.
- *Removed* - Code that was once added and is now removed.
- *Added* - New code that is an enhancement, feature, etc.
- *Updated* - Existing code that has been modified.
- *Refactored* - Existing code that has been cleaned up and does not change functionality.

In practice, using a prefix other than what has been detailed above to explain *what* is being
committed is never needed. This whitelist is not only short and easy to remember but also has the
added benefit of categorizing the commits for building release notes, change logs, etc. This becomes
handy when coupled with another tool, [Milestoner](https://github.com/bkuhlmann/milestoner), for
producing consistent project milestones and Git tag histories.

### Commit Subject Suffix

| Enabled | Severity |       Defaults       |
|---------|----------|----------------------|
| true    | error    | whitelist: `["\\."]` |

Ensures commit subjects are suffixed consistently. The whitelist *is* case sensitive and only allows
for periods (`.`) to ensure each commit is sentance-like when generating release notes, Git tags,
change logs, etc. This is handy when coupled with a tool, like
[Milestoner](https://github.com/bkuhlmann/milestoner), which automate project milestone releases.

## Style Guide

In addition to what is described above and automated for you, the following style guide is also
worth considering:

### General

- Use a [Git rebase workflow](http://www.bitsnbites.eu/a-tidy-linear-git-history) instead of a Git
  merge workflow.
- Use `git commit --fixup` when fixing a previous commit, addressing pull request feedback, etc.,
  and don't need to modifiy the original commit message.
- Use `git commit --squash` when fixing a previous commit, addressing pull request feedback, etc.,
  and want to combine the original commit message with the squash commit message into a single
  message.
- Use `git rebase --interactive` when cleaning up commit history, order, messages, etc. Should be
  done prior to submitting a pull request or when pull request feedback has been addressed and you
  are ready to merge to `master`.
- Use `git push --force-with-lease` instead of `git push --force` when pushing changes after an
  interactive rebasing session.
- Avoid checking in development-specific configuration files (add to `.gitignore` instead).
- Avoid checking in sensitive information (i.e. security keys, passphrases, etc).
- Avoid "WIP" (a.k.a. "Work in Progress") commits and/or pull requests. Be confident with your code
  and collegues' time. Use branches, stashes, etc. instead -- share a link to a diff if you have
  questions/concerns during development.

### Commits

- Use small, atomic commits:
  - Easier to review and provide feedback.
  - Easier to review implementation and corresponding tests.
  - Easier to document with detailed subject messages (especially when grouped together in a pull
    request).
  - Easier to reword, edit, squash, fix, or drop when interactively rebasing.
  - Easier to merge together versus tearing apart a larger commit into smaller commits.
- Use commits in a logical order:
  - Each commit should tell a story and be a logical building block to the next commit.
  - Each commit, when reviewed in order, should be able to explain *how* the feature or bug fix was
    completed and implemented properly.
- Use a commit subject that explains *what* is being commited.
- Use a commit message body that explains *why* the commit is necessary. Additional considerations:
  - If the commit has a dependency to the previous commit or is a precursor to the commit that will
    follow, make sure to explain that.
  - Include links to dependent projects, stories, etc. if available.

### Branches

- Use feature branches for new work.
- Maintain branches by rebasing upon `master` on a regular basis.

### Tags

- Use tags to denote milestones/releases:
  - Makes it easier to record milestones and capture associated release notes.
  - Makes it easier to compare differences between versions.
  - Provides a starting point for debugging production issues (if any).

### Rebases

- Avoid rebasing a shared branch. If you must do this, clear communcation should be used to warn
  those ahead of time, ensure that all of their work is checked in, and that their local branch is
  deleted first.

### Pull Requests

- Avoid authoring and reviewing your own pull request.
- Keep pull requests short and easy to review:
  - Provide a high level overview that answers *why* the pull request is necessary.
  - Provide a link to the story/task that prompted the pull request.
  - Provide screenshots/screencasts if possible.
  - Ensure all commits within the pull request are related to the purpose of the pull request.
- Review and merge pull requests quickly:
  - Maintain a consistent pace -- Review morning, noon, and night.
  - Try not to let them linger more than a day.
- Use emojis to help identify the types of comments added during the review process:
  - Generally, an emoji should prefix all feedback. Format: `<emoji> <feedback>`.
  - :tea: - Signifies you are reviewing the pull request. This is *non-blocking* and is meant to be
    informational. Useful when reading over a pull request with a large number of commits, reviewing
    complex code, requires additional testing by the reviewer, etc.
  - :information_source: - Signifies informational feedback that is *non-blocking*. Can also be used
    to let one know you are done reviewing but haven't approved yet (due to feedback that needs
    addressing), rebasing a pull request and then merging, waiting for a blocking pull request to be
    resolved, status updates to the pull request, etc.
  - :art: - Signifies an issue with code style and/or code quality. This can be *blocking* or *non-
    blocking* feedback but is feedback generally related to the style/quality of the code,
    implementation details, and/or alternate solutions worth considering.
  - :bulb: - Indicates a helpful tip or trick for improving the code. This can be *blocking* or
    *non-blocking* feedback and is left up to the author to decide (generally, it is a good idea to
    address and resolve the feedback).
  - :star: - Signifies code that is liked, favorited, remarkable, etc. This feedback is *non-
    blocking* and is always meant as positive/uplifting.
  - :white_check_mark: - Signifies approval of a pull request. The author can merge to `master` and
    delete the feature branch at this point.
- If the pull request discussion gets noisy, stop typing and switch to face-to-face chat.
- If during a code review, additional features are discovered, create stories for them and then
  return to reviewing the pull request.
- The author, not the reviewer, should merge the feature branch upon approval.
- Ensure the following criteria is met before merging your feature branch to master:
  - Ensure all `fixup!` and `squash!` commits are interactively rebased and merged.
  - Ensure your feature branch is rebased upon `master`.
  - Ensure all tests and code quality checks are passing.
  - Ensure the feature branch is deleted after being successfully merged.

### GitHub

When using GitHub, make sure to enforce a rebase workflow for all of your GitHub projects (*highly
recommended*). You can do this via your project options (i.e.
`https://github.com/<username/organization>/<project>/settings`) and editing your merge options for
pull requests as follows:

![GitHub Merge Options](doc/github-settings-options.png)

Doing this will help maintain a clean Git history.

## Tests

To test, run:

    bundle exec rake

## Versioning

Read [Semantic Versioning](http://semver.org) for details. Briefly, it means:

- Major (X.y.z) - Incremented for any backwards incompatible public API changes.
- Minor (x.Y.z) - Incremented for new, backwards compatible, public API enhancements/fixes.
- Patch (x.y.Z) - Incremented for small, backwards compatible, bug fixes.

## Code of Conduct

Please note that this project is released with a [CODE OF CONDUCT](CODE_OF_CONDUCT.md). By
participating in this project you agree to abide by its terms.

## Contributions

Read [CONTRIBUTING](CONTRIBUTING.md) for details.

## License

Copyright (c) 2017 [Alchemists](https://www.alchemists.io).
Read [LICENSE](LICENSE.md) for details.

## History

Read [CHANGES](CHANGES.md) for details.
Built with [Gemsmith](https://github.com/bkuhlmann/gemsmith).

## Credits

Developed by [Brooke Kuhlmann](https://www.alchemists.io) at
[Alchemists](https://www.alchemists.io).
