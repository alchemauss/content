---
date: "2020-10-13T17:38:25+07:00"
title: Style Guide to a Semantic Commit Message
description: How to write semantic commit message and summarize your changes in under 50 characters
tags: [tutorial, git]
thumbnail: https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fcodereviewvideos.com%2Fblog%2Fwp-content%2Fuploads%2F2015%2F06%2Fgit-goodness.gif&f=1&nofb=1
---

This will be a quick and short post, so feel free to spare a minute to read how I summarize changes within my code, and what style guide I'm referring to.

```
<type>(<scope>): <subject>

[optional body]

[optional footer]
```

Generally, this should work for both commit messages and PR/MR titles.

```
feat: add syntax highlighting
├──┘  └──────────┬──────────┘
│                │
│                └── Summary in present tense and imperative form
│
└────── Type: chore | docs | feat | fix | refactor | style | test
```

I used to write them like title, with a capitalized letter at the beginning. But, as I find out more about other repositories, most of them uses all lowercase characters. If you need a strict guidance in your repository for your team or anyone contributing, you can set up [commitlint](https://github.com/conventional-changelog/commitlint). Unfortunately, it only works with Node projects, so you'll need to have a `package.json`. You can take a look at my [`t.sapper`](https://github.com/ignatiusmb/t.sapper) template for an example on how it looks.

```
feat: (new feature for the user, not for a build script)
fix: (bug fix for the user, not a fix for build script)
refactor: (refactoring production code; renaming a variable, etc.)
docs: (changes to the documentation)
chore: (updating grunt tasks etc; no production code change)
style: (formatting, missing semi colons, etc; no production code change)
test: (adding missing tests, refactoring tests; no production code change)
```

This could also work for pull requests (PR) and merge requests (MR). Look up my references for more great examples!

It needs to be said, there are no right or wrong ways to write commit messages. Just like currencies or most things in this world, as long as the people agrees and accepts it as their currency, it will have its own value. The same goes for your commits, as long as you understand what the title means and what it changes, you can follow any style guides you want.

---
Reference(s):

- <https://gist.github.com/joshbuchea/6f47e86d2510bce28f8e7f42ae84c716>
- <https://www.conventionalcommits.org/en/v1.0.0/#summary>
- <https://seesparkbox.com/foundry/semantic_commit_messages>
- <http://karma-runner.github.io/1.0/dev/git-commit-msg.html>
