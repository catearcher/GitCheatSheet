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
