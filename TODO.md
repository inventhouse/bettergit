To Do
=====

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
