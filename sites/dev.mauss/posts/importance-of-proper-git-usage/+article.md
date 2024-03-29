---
date: "2020-04-28T17:38:25+07:00"
title: Importance of Proper Git Usage
description: A good commit can come a long way in the future and it can significantly reduce debugging time with descriptive body
tags: [ppl2020, git, proper]
---

[![Commit messages steadily declining in quality](https://imgs.xkcd.com/comics/git_commit.png)](https://xkcd.com/1296/)

This is probably by far my favorite picture that describes git commit messages in a nutshell. It sums up most of our experience using and seeing git commits by ourselves or with our team.

It's pretty common to stumble upon these kinds of commit messages in the wild or with your team, especially if they're not accustomed to using git with a team or other developers, or just git itself.

## Prerequisites

Before reading, I expect you to at least know what Git is and its common terminology such as `pull`, `push`, `commit`, `merge`, etc. I won't explain the basics here since you can learn that by yourself.

## Why you should know and use this

Documentation for anything in your life is in my opinion one of the most crucial things to (at least) do, and if you do it right, it could be your best strong suit. Git commit, is one of your most powerful and convenient tool to document your codebase.

Did at least one of these crossed your mind at some point?

- Writing code without documentation is enough by just writing a clean and readable code with descriptive variables
- Git commit messages are a hassle and just a trivial thing you need to do every time you want to push changes to your repository
- Git is just a way to save and synchronize your code like what cloud services do and messages committed with it is insignificant

What happens when there's a spaghetti code with minimal descriptive messages between commits, where would we know where to look first or where to learn. I can't barely remember what I wrote half a year ago if it weren't for the commits and documentation I made ahead of time. I'm sure we have better things to do to aside from remembering what we wrote for eternity.

Of course, other than to help us remember what we wrote, it also helps other developers and maintainers to find and fix the same issue we use to have. We could even revert to a working state in the past if it all breaks down, isn't that cool!

## How a good commit message is supposed to be

Perhaps the explanation beforehand was a little bit too vague, here's a solid and concrete example of what I meant. I present to you in my honest opinion by far the best commit message

[![Best git commit message](./best-git-commit.png)](https://github.com/alphagov/govuk-puppet/commit/63b36f93bf75a848e2125008aa1e880c5861cf46)

This commit message could have been easily written as `fix bug`, `add whitespace`, or `change comments`. Instead, [Dan Carley](https://github.com/dcarley), author of this commit, took his time to explain and document his commit for the benefit of the people working with him.

### Why this is IMHO the best commit message

- It explains the reason behind the change with explanation of **what** and **why** it changed

```
I introduced some tests in a feature branch to match the contents of
`/etc/nginx/router_routes.conf`. They worked fine when run with `bundle exec
rake spec` or `bundle exec rspec modules/router/spec`. But when run as
`bundle exec rake` each should block failed with:

    ArgumentError:
      invalid byte sequence in US-ASCII
```

- It is searchable by the error using `git log` and explains how it happens

```
    ArgumentError:
      invalid byte sequence in US-ASCII
```

```
That particular template appears to be the only file in our codebase with an
identified encoding of `utf-8`. All others are `us-ascii`:
    ...
Attempting to convert that file back to US-ASCII identified the offending
character as something that looked like a whitespace:
    ...
```

- It tells you its story

```
I eventually found that removing the `.with_content(//)` matchers made the
errors go away. That there weren't any weird characters in the spec file. And
that it could be reproduced by requiring Puppet in the same interpreter
```

It goes into detail about what the problem is, the process of debugging and fixing it, and the last line reminds us that there's another human being behind every commit. We can feel Dan's frustration and his satisfaction after finally solving it.

```
Now the tests work! One hour of my life I won't get back..
```

### Commit messages matter

It takes practice and full consciousness to write good and descriptive messages. This might be an extreme example for just a whitespace and I wouldn't expect all commits should be like this Especially not like [this C commit message](https://github.com/mpv-player/mpv/commit/1e70e82baa9193f6f027338b0fab0f5078971fbe)

## How do we properly write our commits

Your Version Control System (VCS) could display your git commits if you give it a body or description

![GitHub git commit](./github-git-commit.png)

See example above where the top commit has 3 dots, it could be expanded as shown below

![GitHub commit description](./github-git-commit-description.png)

And when we click on it, it'll show its description by default rather than just one line of message

![GitHub commit expanded](./github-git-commit-expanded.png)

> A diff will tell you what changed, but only the commit message can properly tell you why.
> -Chris

Here's a guided list of what you should remember when writing your commit messages

1. Separate subject from body with a blank line

    You can add a body as description by separating the first line with a blank space and the rest would be the body

2. Limit the subject line to 50 characters

    A title should be able to give a rough overview of what the story wants to tell

3. Capitalize the subject line

    A title should start with a capitalized letter, pretty self-explanatory here

4. Do not end the subject line with a period

    A title should now end with a period

5. Use the imperative mood in the subject line

    Here's a sentence to help remember this rule
    - This commit will **your subject here**

    For example, with imperative forms:
    - This commit will *refactor feature X*
    - This commit will *add tests for home*
    - This commit will *remove deprecated packages*
    - This commit will *fix bug in worker file*

    It will not work with non-imperative forms:
    - This commit will *fixed bug not printing*
    - This commit will *added feature Y*

    Imperative forms is only important for subject, so you can write anything you want in the body

6. Wrap the body at 72 characters

    Git doesn't wrap texts automatically so you need to keep in mind its right margin, and wrap your text manually. The recommendation is to do this at 72 characters, so that Git has plenty of room to indent text while still keeping everything under 80 characters overall.

7. Use the body to explain what and why vs. how

    I think this has been sufficiently explained in the second part of this post by Dan's commit

### VS Code for editing your commit messages

There's a lot of text editors that could integrate seamlessly with Git, but the one I use is VS Code and it's chosen when Git is installed

You could add/stage your files using VS Code's integrated source control or with the classic `git add` and then just run

```
git commit
```

It will then open the commit file editor and you can write your subject and body there

![VS Code commit editor](./vscode-git-commit.png)

## Keep the default branch clean

Writing good commits is one thing, cluttering your default (usually production) branch is another thing, and good commits isn't enough for your repo's maintainability.

![Cluttered commits](./cluttered-merge-request-commits.png)

Imagine pushing all of this directly into the default branch with all the conflicts and issues, pipeline failing, and so on. With this Merge Request (MR) opened, all of this cluttered commits can be squashed and it will just display its title as one commit, which is `[PBI-2] Menambahkan fitur untuk menampilkan informasi lbh`

By applying a strict and consistent git flow, you could limit the errors occurring in production build and prevent any issues and conflicts before they're merged to your default branch. Here are some of the advantages:

1. Default branch can be kept clean with only working and fully functional codes
2. All changes that will affect the default branch would be shown and they can be reviewed by your team before merging them
3. All of that changes will trigger the configured pipeline and we can see if it fails before we merge it so that we can fix it first

Here are some tips on how to strictly and properly apply this technique, note that this will work optimally if applied to a developer team and not individuals:

1. Start by setting your default branch and set them to protected

    This will deny all commits and immediate push to the branch and require any changes to be made through a MR

    ![GitLab protected branches](./gitlab-protected-branches.png)

2. Set up your pipeline jobs properly with linter, tests, code analysis, coverages, and so on
3. Set the MR settings to *delete source after merge*, *squash commits by default*, *pipelines must succeed*, and *all discussions must be resolved*

    ![GitLab merge options and check](./merge-request-settings.png)

4. (Ignore if you're working individually) Set your MR approvals to at least 2 people

    This will make sure that your code gets reviewed by at least 2 person and you can only merge when they approve of it. You can also add certain roles to approve of your commit so that at least one of DevOps Engineer/Software Developer/Security Analyst will review your code before it goes to production.

    ![GitLab merge request approvals](./merge-request-approvals.png)

> Write good commits and keep your repository maintainable, the future maintainer that thanks you may be yourself!

---

Reference(s):

- <https://dhwthompson.com/2019/my-favourite-git-commit>
- <https://chris.beams.io/posts/git-commit/>
