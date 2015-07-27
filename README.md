## Interactive Rebase starten, mit den letzten 9 Commits
`git rebase -i HEAD~9`

## Einen lokalen Branch namens mein-branch pullen, der von jemand anderem mit "force" gepusht wurde
```git
git checkout mein-branch
git fetch
git reset --hard origin/mein-branch
```

## Alle Remote Branches holen und gleizeitig alle lokalen Referenzen auch Branches, die remote nicht mehr existieren, entfernen
`git fetch --all -p`
