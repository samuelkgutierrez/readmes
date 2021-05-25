OpenPMIx Development Workflow
======================================================

## Setup

```
git clone git@github.com:samuelkgutierrez/openpmix.git
# Setup upstream to keep fork up-to-date
git remote add upstream git@github.com:openpmix/openpmix.git
```

## Keeping fork's master Up-to-Date

```
git co master
git fetch upstream
git pull upstream master
git push
```

## Keeping Branch Up-to-Date With master
```
#
git checkout master
git pull origin master
# Rebase
git checkout feature
git rebase master
git push --force origin feature
