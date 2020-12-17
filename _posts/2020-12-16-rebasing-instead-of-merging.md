---
layout: post
title: "Rebasing instead of merging"
categories: tech
---

One of the first things I do when I set up a new development environment is to ensure that I have rebasing pulls enabled by default in Git. This is enabled across all repositories by adding the following to my global `.gitconfig` by running `git config --global pull.rebase true`:
```
[pull]
    rebase = true
```
## Why I rebase when I pull?

Rebasing when pulling from a Git repo creates a cleaner history than the default which is to merge when pulling from a Git repo. Merging is useful when the work is conceptually distinct and having a single merge commit helps specify a clear point when different work and code was joined at a single place. This is rarely, if ever, the case when working on a single branch. Merge commits contain no relevant information when they are a commit to `master` with the message `Merge branch 'master' of https://github.com/oconnor0/blog`. This creates clutter and makes the history harder to navigate as instead of a linear sequence of commits in a branch, we have graph of commits, many of which are the result of unnecessary merges. This can make it harder to track down which patches and commits made it to a branch.

## Doesn't rebasing cause problems?

Git allows rewriting the history of a repository to change what is stored in it. This can be used to change a remote branch and as such is potentially dangerous. Git disallows, by default, pushes that would change the history of a remote repository and requires an explicit override in a force push (of which `git push --force-with-lease` is the recommended version). In general, force pushing to protected branches should probably be disallowed, by tooling if possible.

However, rebasing on a pull does not rewrite any shared history but reinterprets whatever local commits you have as having been applied to the new head of the branch.

For example, let us say we have a branch with commits A, B, C:
```
A
|
B
|
C
```
You take that branch and begin working on it, creating commits 1, 2, 3:
```
A
|
B
|
C
|
1
|
2
|
3
```
Simultaneously other developers add commits D, E, F to the remote branch:
```
A
|
B
|
C
|
D
|
E
|
F
```
When you attempt to push your changes, you are unable to as HEAD has changed. So you run `git pull` (without having set `pull.rebase = true` in your `.gitconfig`) and the following tree is created, with X as the new merge commit:
```
A
|
B
|
C
|\
1 D
| |
2 E
| |
3 F
|/
X
```
If you had run a rebasing pull, the branch would look like the following linearized history:
```
A
|
B
|
C
|
D
|
E
|
F
|
1
|
2
|
3
```
Notice how your local changes are moved to the end of the history. This better reflects the sequence of commits made to the shared repository over time. In both cases (merge and rebase) conflicts between files changed in both branches have to be resolved.

## Rebasing in development branches

I have also found rebasing when working in a development branch to be a powerful tool. If I am unable to merge a pull request because the destination branch now conflicts, I can either merge the destination branch into mine (creating more merge commits) or I can rebase my branch on the new head of the destination branch, helping to linearize the history.

For example, let us say that master has commits A, B, C on it, and that you create a new feature branch and add commits 1, 2, 3:
```
A
|
B
|
C
 \
  1
  |
  2
  |
  3
```
Meanwhile, master gets commits D, E, F that conflict with your work:
```
A
|
B
|
C
|\
D 1
| |
E 2
| |
F 3
```
You could merge master into your development branch, creating a new merge commit X and then merging back into master with Y:
```
A
|
B
|
C
|\
D 1
| |
E 2
| |
F 3
|\|
| X
|/
Y
```
Or you could rebase your development branch onto master and then merge into master with Y:
```
A
|
B
|
C
|
D
|
E
|
F
|\
| 1
| |
| 2
| |
| 3
|/
Y
```
This creates a history with fewer merge commits and is easier to understand.

This does change the history of your development branch and if it has already been pushed to the remote repository, these updates will need to be force pushed. In a development branch that no one else is working on, you reduce the chances of breaking anything as no one else has a copy of your branch that would be in a conflicting state. Using force with lease verifies there aren't newer commits than what you had locally. This means you are rewriting what you expect not what someone else inserted.

In all, I find rebasing a useful tool for helping manage the complexity of large source repos and think it's worth adopting.
