# How to Rollback a Git Commit

[Source](https://www.theserverside.com/tutorial/How-to-git-revert-a-commit-A-simple-undo-changes-example)

## Methods
There are two ways to rollback a commit:
1. `git revert` or
2. `git reset`

The former only rollback a commit specifically. The latter rollback *all* the commits up-to the specific commit. For now, it is better to just use revert as it is easier to learn and manage.

[how-to-undo-almost-anything-with-git](https://github.blog/2015-06-08-how-to-undo-almost-anything-with-git/)

## Example
We will add five files to the repo, one at a time.
```bash
touch alpha.html
git add . && git commit -m "1st git commit: 1 file"
touch beta.html
git add . && git commit -m "2nd git commit: 2 files"
touch charlie.html
git add . && git commit -m "3rd git commit: 3 files"
touch delta.html
git add . && git commit -m "4th git commit: 4 files"
touch edison.html
git add . && git commit -m "5th git commit: 5 files"
```

Get the log of commit history
```bash
$ git reflog
b4dc558 (HEAD -> main) HEAD@{0}: commit: 7th git commit: 7 files
8cb0b0a HEAD@{1}: commit: 6th git commit: 6 files
c12ef27 HEAD@{2}: commit (amend): 5th git commit: 5 files
e006507 HEAD@{3}: commit: 5th git commit: 5 files
38d8fed HEAD@{4}: commit: 4th git commit: 4 files
f2f0b6b HEAD@{5}: commit: 3rd git commit: 3 files
90e8a9b HEAD@{6}: commit: 2nd git commit: 2 files
55891a6 HEAD@{7}: commit: 1st git commit: 1 file
bb08da3 (origin/main, origin/HEAD) HEAD@{8}: clone: from github.com:EdwardL08/rollback_example.git
```

Push the commits to git repository.
```bash
git push
```

Revert the commit of 8cb0b0a
```bash
git revert 8cb0b0a
```


## BASH Reference
- `touch` [Doc](https://man7.org/linux/man-pages/man1/touch.1.html) - Change file timestamps `touch [option]... file`
- `&&` is the condition AND [BASH Cheatsheet](https://devhints.io/bash)

## Appendix

### Identity

[Doc](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you start creating:

```BASH
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

Again, you need to do this only once if you pass the `--global` option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or email address for specific projects, you can run the command without the `--global` option when you’re in that project.

To check on the current settings
```bash
git config --global --edit
```

### VI Editing Commands

[An introduction to the vi editor](https://www.redhat.com/sysadmin/introduction-vi-editor) <br>
[Basic vi Commands](https://www.cs.colostate.edu/helpdocs/vi.html)

vi <filename> — Open or edit a file.<br>
i — Switch to Insert mode.<br>
Esc — Switch to Command mode.<br>
:w — Save and continue editing.<br>
:wq or ZZ — Save and quit/exit vi.<br>
:q! — Quit vi and do not save changes.<br>
yy — Yank (copy) a line of text.<br>
p — Paste a line of yanked text below the current line.<br>
o — Open a new line under the current line.<br>
O — Open a new line above the current line.<br>
A — Append to the end of the line.<br>
a — Append after the cursor’s current position.<br>
I — Insert text at the beginning of the current line.<br>
b — Go to the beginning of the word.<br>
e — Go to the end of the word.<br>
x — Delete a single character.<br>
dd — Delete an entire line.<br>
Xdd — Delete X number of lines.<br>
Xyy — Yank X number of lines.<br>
G — Go to the last line in a file.<br>
XG — Go to line X in a file.<br>
gg — Go to the first line in a file.<br>
:num — Display the current line’s line number.<br>
h — Move left one character.<br>
j — Move down one line.<br>
k — Move up one line.<br>
l — Move right one character.<br>
