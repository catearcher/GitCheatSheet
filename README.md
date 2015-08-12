## Step 0:
Read this: http://rakeroutes.com/blog/deliberate-git/

> But Patrick, this article is so long ...

**Read it! Read it and memorize it!** All of this will be on your final exam!

> Okay ... :(

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

*When to use*: If you have a branch in your local repo that you have previously pushed, but which was changed in the meantime by somebody else who then force-pushed. Especially useful on live servers.

## Fetch all remote branches and at the same time delete all local references to branches that do not exist anymore on the remote server.
`git fetch --all -p`

*When to use*: All the time.

## Useful Aliases

Git aliases are awesome! Don't want to remember ecxact commands like `git fetch --all -p`? Simply add an alias to your global `.gitconfig` file: `fap = git fetch --all -p`. Now you can just write `git fap`.

I have collected and written some lot of aliases that I use almost every day. Just add all of the following to your **global** `.gitconfig` File:
```git
[alias]
  # List all aliases
	list = !git config -l | grep alias | cut -c 7-

	# Prettier version of git log
	lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr)%C(bold blue)<%an>%Creset' --abbrev-commit

	# Another version of git log, showing which files were actually modified in each commit
	ll = log --pretty=format:%C(yellow)%h%Cred%d\\ %Creset%s%C(green)\\ [%cn]--decorate --numstat

  # Switch to the previous branch (if you call `git toggle` again, git switches back to the first branch)
	toggle = checkout -

	# Fetch all branches from remote and clean up your local references
	fap = !git fetch --all -p && git prune && :

	# Use this if you want to pull a branch that was force pushed by somebody else.
	# Usage: `git forcefetch name-of-the-branch`
	forcefetch = !git checkout $1 && git fetch && git reset --hard origin/$1 && :

  # Remove untracked files and folders from the working tree
	boom = clean -df

	# Undo some commits and put the changes from those commits onto the stage.
	# Example: `git rewind HEAD~3` to undo the last 3 commits.
	rewind = reset --soft

	# Commit changes and write a commit message.
	# Example: `git cm "This is my commit summary"`
	cm = commit -a -m

	## Some short aliases for simple commands that are used frequently
	re = reset --hard HEAD
	co = checkout
	cp = cherry-pick
```
