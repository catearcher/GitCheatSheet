## Start interactive rebase, using the 9 most recent commits.
`git rebase -i HEAD~9`

## Pull a branch that somebody else force-pushed, named "some-branch", which you have previously pulled.
```git
git checkout some-branch
git fetch
git reset --hard origin/some-branch
```

## Fetch all remote branches and at the same time delete all local references to branches that do not exist anymore on the remote server.
`git fetch --all -p`
