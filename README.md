Better Git
==========
_Tools and improvements for better git workflows_

Install these tools by cloning the repo and adding it to your shell's PATH; for example add the following to your `~/.bash_profile`
```bash
export PATH="$PATH:$HOME/bettergit"
```

Git will automatically pick them up as new commands, which also means they can be used seamlessly with [allgit] to work with many repositories.  All the tools will print a brief usage message when called with `-h` or `--help` (for now they don't have proper `man` pages so `git help` _won't_ work 😞)

[allgit]: https://github.com/inventhouse/allgit

**[git alias](#git-alias):** Set, unset, view, and share git aliases.

**[git save](#git-save):** Streamlines the consider-commit process; if called as `git send` it also pushes after committing.

**[BranchTools](BranchTools.md):** A collection of commands to make working with branches better, especially ones with short "alias" local names, and longer best-practice remote names.
- `git newb`: Create a new branch with a long remote name and short local name
- `git getb`: Get a remote branch and makes a short-named local one
- `git setupb`: Set up an existing local branch with a long-named remote one

- `git listb`: List branches with different local vs. remote names, also always lists the current branch
- `git logb`: Print commit logs back to the merge-point with the parent branch

- `git dropb`: Delete a local branch, but _not_ the remote one
- `git killb`: Delete a branch _and_ the corresponding remote one

---

git alias
---------
_The missing command to make aliases easy to use_

Git aliases are really useful for making `git` work the way _you_ want, but arcane to set up or view, not to mention share.

- `git alias`: Show existing aliases.

- `git alias A 'C'`: Map alias `A` to command `C`.

- `git alias -u A`: Unset alias `A`.

- `git alias -s`: Print aliases as a shell script that can be run on a new machine to set them up there.  Note that the script will use standard `git` commands and won't require `git alias`.

Here are a few that I use a lot:
```
git alias co 'checkout'
git alias cp 'cherry-pick'
git alias lo 'log -n 10 --oneline'
git alias pra 'pull -r --autostash'
git alias rbm 'rebase origin/master'
git alias ss 'status -s'
```

Git aliases can even run non-git commands with the shell escape `!`
```
$ git alias hi '! echo Hello'
Setting hi = '! echo Hello'
$ git hi
Hello
```


git save
--------
_Streamline the consider-commit flow_

"Commit early and often" they say, but when I really tried to live that, I found the workflow cumbersome and often seemingly not worth the effort.  So I wrote `git save` to commit (and optionally push) quick changes, well, _quickly_, while still checking and controlling what would go in.

### It goes like this:
__Consider:__
- Optionally, if you know you only want to commit specific files, go ahead and `git add` them.
- Usually, though, just start with `git save` or `git send`.
- With no arguments, it will start by printing short status...
- ...followed by a message saying whether it will add modified files (similar to commit's `-a`), or only commit staged changes.
- It will check the diff for a pattern (by default `NOCOMMIT`) to catch debugging lines and other things and alert before the commit.
- Then prompt for a short commit message.  If no message is given, `git` will open the default editor for a long-form commit message.
- If there are changes you don't want to commit, `^C` will cancel.

__Commit:__
- If there are some staged changes, but you also want to add modified files you can "retroactively" give it a `-a` at the beginning of your commit message (it also mentions this in the message about what it will do)
- Type your message or let `git` open your editor for a nice long one - make it good, not just 'WIP', please! - and hit `Enter`
- It will then add modified files, if desired, and make the commit.  If invoked as `git save`, that's all, and you can pull/push when you're ready.

__Push:__
- If invoked as `git send`, after committing...
- ...it will automatically pull with rebase and autostash...
- ...and push!


### Advantages:
- One command with no arguments for most of this workflow
- Efficient re-use of command history
- Doesn't spawn a whole editor for a brief message, but still allows that choice after considering your changes
- Shows and tells you what it's going to do
- Automates the boring parts without taking away control

### Limitations:
- Depending on your shell, the message prompt may not have line editing (e.g. arrow keys may show control codes instead of moving the cursor)
- Not able to add specific files to commit within the tool (nor specific hunks)
- The pattern check examines the whole diff, as it runs before "knowing for sure" which files will be committed
- The pattern check does not display filenames for the matched lines
- It's a hacky shell script

---
