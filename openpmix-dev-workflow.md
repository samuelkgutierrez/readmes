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

## Resolving Conflicts (Sweeping)
git checkout feature
git rebase -Xours origin/master # Favor master's content
git rebase --continue

https://stackoverflow.com/questions/63611460/git-pull-rebase-resolve-conflicts-by-keeping-local-changes

## Testing
```
# Build and install OpenPMIx
./configure --prefix=/home/samuel/local/openpmix/gds-shmem && make -j && make install

# Build Open MPI
./configure --prefix=/home/samuel/local/openpmix/ompi --with-pmix=/home/samuel/local/openpmix/gds-shmem --disable-man-pages && make -j && make install

PATH=/home/samuel/local/openpmix/ompi/bin:/home/samuel/local/openpmix/gds-shmem/bin:$PATH
export LD_LIBRARY_PATH=/home/samuel/local/openpmix/ompi/lib:$LD_LIBRARY_PATH
```

## Debug
```
export PMIX_MCA_gds_base_verbose=1
```
