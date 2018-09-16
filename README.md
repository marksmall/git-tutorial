# GIT

- [How is GIT used](#how-is-git-used)
- [Merge conflicts](#merge-conflicts)
- [View changes in commit](#view-changes-in-commit)
- [When was code changed](#when-was-code-changed)
- [Temporarily stash changes](#temporarily-stash-changes)
- [Interactive rebasing](#interactive-rebasing)
- [Debugging code](#debugging-code)
- [Cherry picking](#cherry-picking)
- [GIT config](#git-config)
- [References](#references)

## How is GIT used

GIT is a **Distributed version control system**. Okay, that's a bit of a mouthful, what does it actually mean. In essense, there is no one master copy, every copy has the same validity, contributors can `pull` code from any other known `remote` copy. In practice however, users have preferred the [Star Network](https://en.wikipedia.org/wiki/Star_network). You have one **master** copy and all contributors `clone/push/pull` from it.

## Merge conflicts

We've all dealt with our fair share of **merge conflicts**, the tool I like to use is meld. One big point to note, merge conflict resolution doesn't mean the code is right, it's very easy during merge conflict resolution to choose the wrong code to merge, especially if your unfamiliar with it.

## View changes in commit

It may be that you need to view changes made in a prior commit, in this case you use `git show <commit hash>`. This is very useful in understanding how the code was developed. This can be invaluable when debugging code.

## When was code changed

Sometimes you want to know when or why some code was changed, you can use the worst named feature `git blame <filename>`. This lists each line of code with the hash of the commit, the line was edited in. I find this most useful to see what code looked like prior to a line being edited.

## Temporarily stash changes

Say your in the middle of developing some code that's not ready to commit, but you've got a rush job to do on another branch. You can't just check-out that branch until the current one is clean. You can though, commit the code and amend it later, or, your can `git stash` your changes, to come back to later (or apply to a different branch). A nice feature of stashing is you can stash only part of your uncomitted code. Say your developing and have introduced a bug, but can't figure out where. You can `stage` some of the code and stash the rest, if the bug still exists, you know it's in the `stagged` or `stashed` code.

## Interactive rebasing

**Rebasing** is one of the most powerful and dangerous features of GIT. This is probably the main reason most people stay away from it. **Rebasing** can be very powerful, if used properly.

### Implications

Rebasing, if used improperly can seriously impact on your repository, the biggest being you can lose code in the **master** copy. This can however be resolved, if another contributor still has an up-to-date local copy of the **master** branch. The most common problem is rebasing a branch after others have `pulled` from it, then their copy no longer matches and there will be problems merging. Another very common scenario is losing **Code Review** comments, as the comments are aligned to a **commit hash**, and the hash will be different after the `rebase`.

None of this means you shouldn't use rebasing, just be aware of the implications, this is what scares people off.

### Why rebase

Rebasing is basically you, editing existing commits, so why would you? The aim is to have an easy to understood commit history, but reason may be, you are working on a feature branch and realise you've made many commits. Some could be `squashed` together into one, as they are tightly coupled. Or, it could you edited some code, then started on another aspect of the feature. After committing some code for it, you realize a bug in a previous commit (one before the last). You could just fix it and add a new commit, but wouldn't it be neater to add it to the previous commit, as if there never was a bug? Well, this is why `rebasing` exists.

### How to rebase

First thing to know about `rebasing` is you can back out right up until the last conflict has been resolved, the downside, you'll not know which is the last. If your really paranoid, you can always checkout a new copy of your the branch before rebasing into it as the ultimate way of backing out.

## Debugging code

`git bisect`

## Cherry picking

Sometimes merging and rebasing are just a pain, or you've been developing one thing, got stuck and solved another problem, but forgot to start a new feature branch. This is where **Cherry-picking** commits can help. Simply put, you take a commit from another branch and apply it to the current one.

## GIT config

```bash
[user]
        name = Mark Small
        email = mark.small@astrosat.space

[color]
        diff = auto
        status = auto
        branch = auto
        interactive = auto

[core]
        excludesfile = /home/msmall/.gitignore
        autocrlf = input
        editor = emacs
[alias]
        st = status
        co = checkout
        br = branch
        mg = merge
        ns = log --name-status
        lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%dCreset %s %Cgreen(%cr)%Creset' --abbrev-commit --date=relative
        pop = stash pop
        ci = commit
        ch = !sh -c \"git log --reverse ..annotations\" | grep \"^commit\" | awk \"{print$2}\"
        us = reset HEAD --
        list = stash list
        in = log --oneline master..origin/master
        out = log --oneline origin/master..master
        pull = pull --rebase
        add = add -A
[bash]
        showDirtyState = true
[diff]
[init]
[push]
        default = current
[merge]
        tool = meld
```

## References

- [GIT Tutorial Code](https://github.com/marksmall/git-tutorial)
