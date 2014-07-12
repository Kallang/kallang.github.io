---
layout: post
title:  "Change GIT Commit Message"
date:   2014-07-12 14:31:00
categories: git
---
Threre are many scenarios that we need to modify the git commit messages. For the commits that are not pushed we can change the last commit message using:

    git commit --amend

To change several commit messages all in once we can use `rebase`, and it can even squash several commits into a single one.

    git rebase -i HEAD~3


For the commits that have been pushed, we should not change the commit message any more, as other collaborators may have already fetched your commits. If you change the pushed commit that have been fetched by others, it may create lots of problems for other collaborators. So, do NOT do it.

But if you insist on modifying commits that have be pushed to the git server, you can do it using the ways above, and when you want to push, use the following command. The `--force` parameter should be used cautiously.

    git push --force