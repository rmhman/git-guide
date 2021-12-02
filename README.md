# git-guide
quick git guide
# Intro to Git with (mostly) CLI

## Table of contents

* **[Setup Git Config](#setup-git-config)**
    * **[OSX example](#osx-example)**
    * **[Linux example](#linux-example)**
    * **[Windows example](#windows-example)**
* **[Clone Repo Locally](#clone-repo-locally)**
* **[Working with Branches](#working-with-branches)**

## Setup SSH Config

Add entry to `~/.ssh/config` where IdentityFile points to a generated RSA private key. For example:

```
Host github.com
    User git
    HostName github.com
    IdentityFile ~/.ssh/key
```


## Setup Git Config

Update `~/.gitconfig`.

### OSX example:

```
[credential]
        helper = store
[user]
        email = 
        name = 
[merge]
        tool = extMerge
[mergetool "extMerge"]
        cmd = extMerge "$BASE" "$LOCAL" "$REMOTE" "$MERGED"
        trustExitCode = false
[diff]
        external = extDiff
[alias]
        dt = diff
        mt = mergetool
```

For OSX, create some scripts to open the diff/merge tool (referenced above in the .gitconfig file).
This example uses **P4 Merge**, but other tools would have similar setups.

* /usr/local/bin/extMerge

```
#!/bin/sh
/Applications/p4merge.app/Contents/MacOS/p4merge $*
```

* /usr/local/bin/extDiff

```
#!/bin/sh
[ $# -eq 7 ] && /usr/local/bin/extMerge "$2" "$5"
```

### Linux example

```
[user]
        email = 
        name = 
[merge]
        keepBackup = false;
        tool = p4merge
[mergetool]
        prompt = false
[mergetool "p4merge"]
        cmd = p4merge "$BASE" "$LOCAL" "$REMOTE" "$MERGED"
        keepTemporaries = false
        trustExitCode = false
        keepBackup = false
[diff]
        tool = p4merge
[difftool]
        prompt = false
[difftool "p4merge"]
        cmd = p4merge "$LOCAL" "$REMOTE"
        keepTemporaries = false
        trustExitCode = false
        keepBackup = false
[alias]
        dt = difftool
        mt = mergetool
```

Linux doesn't need any special start scripts.

### Windows Example

TODO: :expressionless:


## Clone Repo Locally

* `cd ~/some/dir/for/git/repos`
* `git clone <secretToken>@github.com:<group-name>/<repo-name>`
* `cd <repo-name>`

## Working with Branches

* Show local branches
```
git branch
```
* Show local and remote branches
```
git branch -a
```
* Creating a new branch and switch to it immediately
```
git checkout -b <branch-name>
```
* Push branch to remote and have your local branch track changes to remote branch
```
git push -u origin <branch-name>
```
* Delete a branch (NOTE: Must switch to another branch first)
```
git branch -d <branch-name>
```
* Forcefully delete a branch (NOTE: Must switch to another branch first)
```
git branch -D <branch-name>
```
* Delete a remote branch (NOTE: Must delete local branch first. Also notice the colon in front of the branch name.)
```
git push origin :<branch-name>
```

### Tips

* Creating a branch is very inexpensive, so branch early and often, and push to Git. You can always delete/cleanup branches later.
* Always have a **master** and **develop** branch to start, then create feature branches from your **develop** branch.
* Always perform work under a feature branch (isolate your changes), then when ready, code review and merge into **develop** branch
* Commit or stash your changes to your current branch before switching to another branch, otherwise those changes will follow you to the next branch.
* Perform `git pull` often so that your local git repo is aware of changes across all branches.
* When looking to merge changes from a **source** branch to a **target** branch, always do the following:
    * commit/stash any outstanding changes in your **source** (current) branch `git commit -m 'meaningful commit message' .`
    * switch to the **target** branch `git checkout <target-branch>`, and perform a `git pull`
    * switch back to the **source** branch `git checkout <source-branch>`, and perform a merge `git merge <target-branch>`
    * resolve any merge conflicts, then push updates to remote branch `git push`
    * switch back to the **target** branch `git checkout <target-branch>`, and perform a merge `git merge <source-branch>`
    * push merged updates to remote branch `git push`
* Always try to resolve merge conflicts in your feature branch or other non-shared branches.
Trying to resolve merge conflicts in shared branches (master, develop, etc.) can be tricky and lead to lots of headaches if not done properly.

## Diff

**TODO**

* git diff
* git diff --cached
* Using a visual diff/merge tool: ~/.gitconfig

## Merges

**TODO**

* git checkout <to_branch>
* git pull
* git merge <from_branch>

## Extras

* [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/)
* [Mac tab completion](http://stackoverflow.com/questions/12399002/how-to-configure-git-bash-command-line-completion)

## Advanced Topics

**TODO**

* Stashing Changes
* Tagging Changes
* Merge Conflicts
* Visual Diff and Merge
