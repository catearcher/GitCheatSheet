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

I have collected and written some lot of aliases that I use almost every day. Just add the contents of [this repo's .gitaliases file](.gitaliases) to your **global** `.gitconfig` File!
