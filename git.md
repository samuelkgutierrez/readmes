Git Things
============

## Fix pull request with a single commit
```
git rebase -i HEAD~X # X is number of commits to merge
# Fixup
git push --force -u origin
```

## Fetch Pull Request Locally
```
git fetch origin pull/ID/head:BRANCH_NAME
```
where ID is the pull request ID.
