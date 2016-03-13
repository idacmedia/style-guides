# Git strategy

The branching model used is based on [git flow](http://jeffkreeftmeijer.com/2010/why-arent-you-using-git-flow/).

Each project should have:

## master branch

The HEAD of the Master branch should always be stable code. Except for trivial projects no code should be committed 
directly to the master branch, meaning code in the master branch:

- has been peer-reviewed
- has been tested, and tests pass
- has been approved by stakeholders

In other words, it is production ready.


## develop branch

The develop branch is where new features are merged prior to a release. Code in the develop branch:

- has been peer-reviewed
- has been tested


## Topic branches

Work should take place on topic branches with a naming scheme like:

### feature/...

A feature branch for implementing a new feature, such as a particular user story.

### hotfix/...

A branch for implementing a bug fix. If the fix should be applied to the master branch the branch should be merged with
both master and develop.

### refactor/...

Although refactoring is better done incrementally and as part of feature branches, if there is no better place you can
use a refactor branch.


## Workflow

A typical daily git workflow could look like this:

```shell session
$ git fetch # get latest work from origin
$ git checkout develop
$ git pull
$ git checkout feature/my-feature
$ git rebase develop # rewrite my work on top of new commits from origin/develop
$ # do work...
```

Rebasing is recommended while working on feature branches to avoid creating unnecessary merge commits, and to make
merges back to the develop branch cleaner.


After doing some work:

```shell session
$ git add -p # recommend always doing a patch commit so you can review your own work before committing
$ git add -i # use interactive shell to add/update any new files, don't just add everything
$ git commit -m "Add feature to implement spaghetti monster widgets"
$ git push -u origin feature/my-feature # or just git push if the branch already exists upstream
```

Then go and create a pull request for your branch online.


Warning: Don not continue work on a topic branch that has already been merged. If you rebase you will probably need to
resolve the same merge conflicts over and over. Instead delete a branch once it has been merged and start with a new
branch.


## Git config and aliases

Set up git aliases to save your fingers! This can be set up in your `~/.gitconfig` file under the `[alias]` section.
Some common shortcuts are:

co
: checkout

b
: branch

cm
: commit -m

s
: status


Set up a global `.gitignore` with common ignore patterns for your IDE and system. These don't belong in a project 
`.gitignore` file.

