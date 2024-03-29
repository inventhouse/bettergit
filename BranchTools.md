BranchTools
==========
_Quicker and easier branch workflow_

Working on feature or bugfix branches is very common when collaborating on code in git repositories, but adhering to best-practice naming conventions like 'users/my_name/JIRA-123-my-feature' can make working with branches tedious.  The workflow gets worse if the change involves multiple repositories, and _much_ worse if we have to juggle several branches.

In fact, git lets us use a different local vs. remote name for branches, but this feature can be arcane to use; these scripts aim to make working with well-named shared branches easier and integrate with [allgit] to scale across repositories.

[allgit]: https://github.com/inventhouse/allgit

To describe these commands and how to use them, let's call the local, shorter, name an _alias_ for the longer remote name, and let's say that the remote name is made up of a _prefix_ and a _slug_.


Setup
-----
Assuming bettergit is set up as described in [README.md](README.md), there are two more things to do, the first tells git to use the remote, "upstream", branch for push:

`$ git config --global push.default upstream`

The second sets the default branch prefix to match our shared branch naming convention:

`$ git config --global bettergit.branchprefix 'users/abc'`

(This can also be overridden by setting it on particular repositories, without using `--global`, of course)


Basics and Workflow
-------------------
| Commands | Notes |
|----------|-------|
| _(start implementing a feature)_ ||
| `$ git newb JIRA-123-my-feature [alias]` | Creates a local branch `mf` (or the alias we choose) and immediately pushs it (unchanged!) to `users/abc/JIRA-123-my-feature`
| _(Commit and push and merge normally)_ ||
| `$ git killb mf` | Check out master and delete local and remote branches

In addition to `git newb` and `git killb`, there are a few more commands:

- `$ git listb` will show all the branches that have a different local vs. remote name (including local-only branches); it also lists the current branch even if that isn't aliased.
- `$ git setupb JIRA-234-add-foobar [foo]` will push up the existing local branch `foo` (or the current branch), so we can separate local branch creation from pushing.
- `$ git getb users/jkl/JIRA-112-make-a-thing [alias]` will check out a local branch `mat` (or whatever alias we want) for an existing remote branch (Note that the remote branch does _not_ need to use the same prefix or conventions)
- `$ git dropb foo` will delete the local branch `foo`, but _not_ the remote branch

All of the commands print help when run with `-h`.

Scaling with Allgit
-------------------
| Commands | Notes |
|----------|-------|
| _(start implementing a feature)_ ||
| `$ allgit -m - newb JIRA-123-my-feature` | Checks out a local branch `mf` and immediately pushs it (unchanged!) to `users/abc/JIRA-123-my-feature` in all repositories with local modifications (`-m`)
| `$ allgit -b mf - commit -am "WIP my feature"` | Work with the branch (`-b`) using the local alias `mf`
| `$ allgit -b mf - push` | Commit and push normally
| _(create PRs for branches and merge them)_ | _([hub](https://hub.github.com) can help with that for GitHub repos)_ |
| `$ allgit -b mf - killb` | Check out master and delete local and remote branches (`killb` gets the branch name from allgit)

As always with allgit, this workflow is the same no matter how many repositories we have or how many we are changing; the branches will be consistent, and we won't need repeat any commands.


_(( Advanced: describe variables, config, and variable-prefix support ))_


Goals
-----
- short "alias" local names, longer descriptive remote names
    - workflow should be just as easy regardless of how many repos are involved
    - easy as possible to create aliased branches
    - easy to find working branches
    - easy to clean up working branches
    - easy to alias-checkout existing remote branches


To Do
-----
- investigate making these proper git subcommands

- DONE: git aliases for seamless integration
- newb/getb auto-alias names should be much shorter
    - DONE: cleverly pull initials from the slug?
    - `$ echo "DISCO-123-do-the-thing" | sed -e 's:.*/::g' -e 's/[A-Z][A-Z]*-[0-9][0-9]*//' -e 's/^\([A-Za-z]\)[^_.-]*/\1/' -e 's/[_.-]\([A-Za-z]\)[^_.-]*/\1/g'`
    - `$ echo "DISCO-1223-DoTheThing" | sed -e 's/[A-Z]*-[0-9]*//' | tr -d 'a-z0-9_.-' | tr 'A-Z' 'a-z'`
    - easy-or-automatic to avoid/deal with alias conflicts


- explain the subtleties of ALLGIT_BRANCH and BranchFlow
- detail the defaults and parsing behavior of the various commands (when ALLGIT_BRANCH is the alias vs. the branch, etc) - or maybe need to detail that in the -h?

- `squashbranch [-c|-m message] [-p?]` - squash, re-commit with original message + squash-hashes or message + squash-hashes, force-push (pre-commit check in here somewhere?) - or maybe `squashpush` does squashbranch + extras  (Does git support precommit hook? yes, also look into https://pre-commit.com)

- PUNT: use `hub` - Make PRs by constructing and opening github URLs:

    > remote: Create a pull request for 'bjh/thing' on GitHub by visiting:
    > remote:      https://github.com/inventhouse/Test/pull/new/bjh/thing

    - should only do this for GH-remoted repos?  What if there's a mix?  Does a GH-specific script belong in allgit?

- need an allgit utils module or something?


### Doneyard

- DONE: $ALLGIT_BRANCH - mechanism for helpers that expect to be on the desired branch so they can enforce that?

- DONE: `newb` - create a local branch with short name & upstream with longer composed name
    - DONE: need `setupb SLUG` for composing upstream name after-the-fact - need to guard for already tracking?
- DONE: `killb` - clean up local and upstream branches
    - DONE: need a good way to delete just local branch but with $ALLGIT_BRANCH - `dropb`?
- DONE: `getb` - check out remote branch as alias

- DONE: how to accomodate workflows that use different branch prefixes for different types, like 'bugfix/', 'feature/'?
    - DONE: perhaps setupb should not prefix if slug contains '/'?
    - DONE: in that case, should alias extract the last part? - yes, if getb would

- DONE: easy to list "alias" branches
    - DONE: `listb` - lists branches that don't match their upstream (including local branches)
    - PUNT: allgit -a/--alias-branches? - run in repos with local branches that don't "match" upstream - needs to be a "pre-filter" in allgit though
    - DONE: should it include "pure local" branches? - probably
    - DONE: always include current branch

---
