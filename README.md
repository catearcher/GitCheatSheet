## Step 0:
Read this: http://rakeroutes.com/blog/deliberate-git/

> But Patrick, this article is so long ...

**Read it! Read it and memorize it!** All of this will be on your final exam!

> Okay ... :(

Also, before doing anything else, run these commands:
```git
git config --global pull.ff only
git config --global url."https://".insteadOf "git://"
```

The first one prevents git from automatically creating merge commits when pulling changes from the remote. We don't want those merge commits because thay can completely mess up your history as soon as you try rebasing your branch, leading to lots of duplicate commits, and making rebasing completely impossible.

The second one prevents `npm install` from getting stuck on outdated dependencies that still use `git://` URLs for downloading their non-npm dependencies. Those URLs can slow down `npm install` considerably, and by forcing git to automatically replace them with https URLs you will prevent yourself from running into hard-to-debug issues when checking out old repos and installing their dependencies.

## Start interactive rebase, using the 9 most recent commits.
`git rebase -i HEAD~9`

*When to use*:
- If you need to remove a commit from history
- If you want to combine two commits into one
- If you want to change the order of two or more commits

## Pull a branch named "some-branch" that somebody else force-pushed, without any conflicts.
```git
git checkout some-branch
git fetch
git reset --hard origin/some-branch
```

Or simply use the `git forcepull` alias (see section [Useful Aliases](#useful-aliases)) after checking out the branch.

*When to use*: If you have a branch in your local repo that you have previously pushed, but which was changed in the meantime by somebody else who then force-pushed. Especially useful on live servers.

## Fetch all remote branches and at the same time delete all local references to branches that do not exist anymore on the remote server.
`git fetch --all -p`

Or simply use the `git fap` alias (see section [Useful Aliases](#useful-aliases)) which does the same thing, plus some additional cleanup.

*When to use*: All the time.

## Useful Aliases

Git aliases are awesome! Don't want to remember exact commands like `git fetch --all -p`? Simply add an alias to your global `.gitconfig` file: `fap = git fetch --all -p`. Now you can just write `git fap`.

I have collected and written some lot of aliases that I use almost every day. Just add all of the following to your **global** `.gitconfig` File:
```git
[alias]
  # List all aliases
  aliases = !git config -l | grep alias | cut -c 7-

  # Prettier version of git log
  lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%ar)%C(bold blue)<%an>%Creset' --abbrev-commit

  # Like the lg command (see above), but also include a diff for each commit
  ld = lg -p

  # Another version of git log, showing which files were actually modified in each commit
  ll = log --pretty='format:%Cred%h%C(yellow)%d %Creset%s%C(bold blue) [%cn]' --decorate --numstat

  # Switch to the previous branch (if you call `git toggle` again, git switches back to the first branch)
  toggle = checkout -

  # Fetch all branches from remote and clean up your local references
  fap = !git fetch --all -p && git prune && :

  # Use this if you want to replace your local branch with the latest version of the remote branch
  # (e.g. if someone else did a force-push and you simply want to continue on top of their work).
  # Usage: `git forcepull`
  forcepull = !git fetch && branchname=`git rev-parse --symbolic-full-name --abbrev-ref HEAD` && git reset --hard origin/$branchname && :

  # Reset and remove untracked files and folders from the working tree
  boom=!git reset --hard && git clean -df && :

  # Undo some commits and put the changes from those commits onto the stage.
  # Example: `git rewind HEAD~3` to undo the last 3 commits.
  rewind = reset --soft

  # Commit changes and write a commit message.
  # Example: `git cm "This is my commit summary"`
  cm = commit -a -m

  # Fast-forward an existing tag to the current commit, both locally and on the remote.
  # Example: `git fftag v1.0` will delete the remote "v1.0" tag, overwrite the local "v1.0" tag with a new one and then push the new tag to the remote.
  fftag = !1>/dev/null && git push origin :refs/tags/$1 && git tag -fa $1 && git push origin --tags && :

  # Shows an overview of file changes of the current branch compared to main
  changes = diff --stat main...HEAD

  # Some short aliases for simple commands that are used frequently
  re = reset --hard HEAD
  co = checkout
  cp = cherry-pick
  done = !git branch --merged | egrep --invert-match '(main|master|\\* )'

  # Deletes all local branches that have been merged into the current branch
  dropdone = !git done | xargs -n 1 git branch -d
```
