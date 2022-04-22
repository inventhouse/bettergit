To Do
=====

git-dropb
---------
FIXME: currently hardcodes `master` as the default branch, adapt logic from rebase-default

If alias is not specified, prompt `[Y/n]` to drop the current branch (maybe even killb should too...)

git-lop
-------
Git log from pasteboard - select a hash, and get that log without pasteing for quick access and re-use of command history

git-logjump
-----------
A `git log --oneline` format that adds a Jira-style "project" abbreviation to the hash combined with a shortcut service to open that log

### how to make it work
- log oneline command could made a short symlink in a known location back to the git directory and add that prefix to log hashes
- service splits prefix from hash and uses the symlink to open a detailed log in the right repo

git-rbm
-------
- Rebase from DEFAULT_BRANCH or main or master
- Some various ways this can be done:
```
Tim McCormack [he or they]  4 months ago
The best I have so far is this. There's gotta be a better way...
git remote show $(git remote | head -n1) | grep "HEAD branch:" | sed 's/.*: //'


Michael Terry:ghost:  4 months ago
https://stackoverflow.com/questions/28666357/git-how-to-get-default-branch suggests:
git symbolic-ref refs/remotes/origin/HEAD
(edited)
Stack OverflowStack Overflow
git - how to get default branch?
My team alternates between usage of dev and master as default branch for several repos and I would like to write a script that checks for the default branch when entering a directory. When pull re...
:nice:
1

Tim McCormack [he or they]  4 months ago
git checkout $(git symbolic-ref refs/remotes/origin/HEAD | sed 's|^refs/remotes/[^/]\+/||')

Tim McCormack [he or they]  4 months ago
gross but it works

Tim McCormack [he or they]  4 months ago
Oh hmm, still need to get "origin"...

Tim McCormack [he or they]  4 months ago
git symbolic-ref "refs/remotes/$(git remote | head -n1)/HEAD" | sed 's|^refs/remotes/[^/]\+/||' yay (edited) 

Tim McCormack [he or they]  4 months ago
(Actually longer than the first thing I had, but this is language-neutral; "branch" is not reliable if your system isn't in English...) (edited) 

Michael Terry:ghost:  4 months ago
git ls-remote --symref git:pointer-to-your-repo HEAD
^ also from that SO page - looks like it doesn't require a checkout?

Tim McCormack [he or they]  4 months ago
Whoops, didn't need the checkout, that was a copy error.

Ned Batchelder  4 months ago
I use: git checkout $(git branch -a | sed -n -E -e '/remotes.origin.ma(in|ster)/s@remotes/origin/@@p')
```

git-save
--------
- DONE: Env var or git config for "alert pattern" to flag `BJH` or other lines that shouldn't be committed
    - Show filename on matched lines somehow
    - Maybe should distinguish between staged changes and unstaged in matched lines
    - Maybe should be/allow a script to enable complex things by other tools
- Drop `-m` support?
- Ability to add specific files at the prompt
- DONE: Enable long-form message commits
    - DONE: If no message is supplied, do standard commit instead of canceling
    - DONE: Describe in help and README

---
