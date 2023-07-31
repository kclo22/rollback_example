# How to Rollback a Git Commit

[Source](https://www.theserverside.com/tutorial/How-to-git-revert-a-commit-A-simple-undo-changes-example)

## Methods
There are two ways to rollback a commit:
1. `git revert` or
2. `git reset`

The former only rollback a commit specifically. The latter rollback *all* the commits up-to the specific commit.

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

Reset back to commit of f2f0b6b
```bash
# Option 1
git reset --hard f2f0b6b
# This will delete all the files from the commit proceeding f2f0b6b

# Option 2
git reset f2f0b6b
# This will go back to commit f2f0b6b but proceeding files will not be deleted
```

[Resetting remote to a certain commit](https://stackoverflow.com/questions/5816688/resetting-remote-to-a-certain-commit) Stackoverflow.
> Assuming that your branch is called master both here and remotely, and that your remote is called origin you could do:
```zsh
git reset --hard <commit-hash>
git push -f origin master
```


## BASH Reference
- `touch` [Doc](https://man7.org/linux/man-pages/man1/touch.1.html) - Change file timestamps `touch [option]... file`
- `&&` is the condition **AND** [BASH Cheatsheet](https://devhints.io/bash)

## Appendix


<details>
<summary>Git Identity - Set Global User Details</summary>

### Git Identity - Set Global User Details

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
</details>

<details><summary>git fetch vs git pull</summary>

### git fetch vs git pull

- [What is the difference between 'git pull' and 'git fetch'?](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch) stackoverflow.

> In the simplest terms, `git pull` does a `git fetch` followed by a `git merge`.

> `git fetch` updates your remote-tracking branches under `refs/remotes/<remote>/`. This operation is safe to run at any time since it never changes any of your local branches under `refs/heads`.

> `git pull` brings a local branch up-to-date with its remote version, while also updating your other remote-tracking branches.

- [git-fetch](https://git-scm.com/docs/git-fetch) documentation - Download objects and refs from another repository.
- [git-pull](https://git-scm.com/docs/git-pull) documentation - `git pull` runs `git fetch` with the given parameters and then depending on configuration options or command line flags, will call either `git rebase` or `git merge` to reconcile diverging branches.

</details>

<details><summary>How to check if a local repo is up to date?</summary>

### How to check if a local repo is up to date?

- [How to check if a local repo is up to date?](https://stackoverflow.com/questions/7938723/git-how-to-check-if-a-local-repo-is-up-to-date) stackoverflow.

First use `git remote update`, to bring your remote refs up to date. Then you can do one of several things, such as:

<details><summary>git remote details</summary>

- [git remote](https://git-scm.com/docs/git-remote) documentation - Manage the set of repositories ("remotes") whose branches you track.
	- [update](https://git-scm.com/docs/git-remote#Documentation/git-remote.txt-emupdateem) - Fetch updates for remotes or remote groups in the repository as defined by `remotes.\<group\>`.
</details>
<br>

1. `git status -uno` will tell you whether the branch you are tracking is ahead, behind or has diverged. If it says nothing, the local and remote are the same. 

<details><summary>git status details</summary>

- [git status](https://git-scm.com/docs/git-status) documentation - Show the working tree status.
	- [u\[\<mode\>\]](https://git-scm.com/docs/git-status#Documentation/git-status.txt--ultmodegt) - Show untracked files.
		- The mode parameter is used to specify the handling of untracked files. It is optional: it defaults to **all**, and if specified, it must be stuck to the option (e.g. `-uno`, but not `-u no`).
	- The possible options are:
		- **no** - Show no untracked files.
		- **normal** - Shows untracked files and directories.
		- **all** - Also shows individual files in untracked directories.

</details>
<br>

Sample result:

```text
On branch DEV

Your branch is behind 'origin/DEV' by 7 commits, and can be fast-forwarded.

(use "git pull" to update your local branch)
```

</details>

<details><summary>Divergent Branches</summary>

### Divergent Branches

#### Summary

- `fast-forward only` is recommended as default config on divergent branches
- If error encountered after, try either 
	1. merge `git pull --rebase=false` or 
	2. rebase `git pull --rebase=true`

<details><summary>Diagrams</summary>

### Diagrams

**Situation A**
```
			  D1---E1---F Feature
			 /         
    A---B---C---D---E Main
```

The *Rebase* Option
```
		        D1"*---E1"*---F"* Feature
		       /			           
    A---B---C^---D^---E^

" Feature
^ Main
* Merge Commit
```

The *Merge* Option
```
	      D1---E1---F---G* Feature
	     /		   /         
    A---B---C------D------E Main

* Merge Commit
```


**Situation B**
```
	      D---E---F Feature
	     /         
    A---B---C Main
```

The *Fast Forward* Option
```
	      D---E---F Main
	     /			           
    A---B---C
```
</details>

<details><summary>Scenario</summary>

### Scenario

**Info**<br>
- Command `git --version`
- Computer A & B running git version 2.39.2 (Apple Git-143)

1. Remote Repositary has one file alpha.html
2. Computer A and B downloads the repsitary locally.
3. Computer B created two files (beta.html & charlie.html), added, commited and pushed to Repositary.

Computer B's reflog:
```zsh
edwardlo@MacEd:~/git_rebase_text|main ⇒  git reflog
7417f19 (HEAD -> main, origin/main, origin/HEAD) HEAD@{0}: commit: 3rd git commit: 3 files
6103166 HEAD@{1}: commit: 2nd git commit: 2 files
a73ad43 HEAD@{2}: reset: moving to a73ad43
287fcdc HEAD@{3}: commit: 3rd git commit: 3 files
f704baf HEAD@{4}: commit: 2nd git commit: 2 files
a73ad43 HEAD@{5}: reset: moving to a73ad43
6bda313 HEAD@{6}: reset: moving to 6bda313
6bda313 HEAD@{7}: commit: 3rd git commit: 3 files
2a4d506 HEAD@{8}: commit: 2nd git commit: 2 files
a73ad43 HEAD@{9}: clone: from github.com:EdwardL08/git_rebase_text.git
(END)
```

4. Computer A created one file (delta.html), added, commited but not pushed.
5. Computer A pulls from Remote Repositary but get:
```zsh
edwardlo@Edwards-MBP:~/git_rebase_text|main ⇒  git pull
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```
- [git-config](https://git-scm.com/docs/git-config#_variables) documentation. Get and set repository or global options.
	- [pull.rebase true](https://git-scm.com/docs/git-config#Documentation/git-config.txt-pullrebase) - When true, rebase branches on top of the fetched branch, instead of merging the default branch from the default remote when "git pull" is run.
	- [pull.rebase false](https://git-scm.com/docs/git-config#Documentation/git-config.txt-pullrebase) - When false, merging the default branch from the default remote when "git pull" is run.
	- [pull.ff](https://git-scm.com/docs/git-config#Documentation/git-config.txt-pullff) - By default, Git does not create an extra merge commit when merging a commit that is a descendant of the current commit. Instead, the tip of the current branch is fast-forwarded.


6. [Stackoverflow](https://stackoverflow.com/questions/71768999/how-to-merge-when-you-get-error-hint-you-have-divergent-branches-and-need-to-s/71774640#71774640) recommends defaulting to `fast-forward only`. If I try `fast-forward only`, I know this will not work because this is not a Situation B (above). Hence, I expect an error:
```zsh
edwardlo@Edwards-MBP:~/git_rebase_text|main ⇒  git config pull.ff only
edwardlo@Edwards-MBP:~/git_rebase_text|main ⇒  git pull
fatal: Not possible to fast-forward, aborting.
```

7. In this case, I know the difference in the two branches does not affect each other. Hence either a `merge` or `rebase` will work. However, a different command is needed since I have set the config to use `fast-forward only`.

- [git pull --rebase](https://git-scm.com/docs/git-pull#Documentation/git-pull.txt--r) documentation.
	- `git pull --rebase[=false|true|...]`
	- `git pull --rebase` - merge the upstream branch into the current branch. (See The Merge Option Diagram Above)
	- `git pull --rebase=true` - rebase the current branch on top of the upstream branch after fetching. (See The Rebase Option Diagram Above)


```zsh
# Computer A
# merge
edwardlo@Edwards-MBP:~/git_rebase_text|main ⇒  git pull --rebase
Successfully rebased and updated refs/heads/main.
```

Computer A's reflog:
```zsh
edwardlo@Edwards-MBP:~/git_rebase_text|main ⇒  git reflog
516655f (HEAD -> main) HEAD@{0}: pull --rebase (finish): returning to refs/heads/main
516655f (HEAD -> main) HEAD@{1}: pull --rebase (pick): 4th git commit: 4 files
7417f19 (origin/main) HEAD@{2}: pull --rebase (start): checkout 7417f19aa8abb43aecedc6a8baa61257b5a18090
dad9be5 HEAD@{3}: commit: 4th git commit: 4 files
a73ad43 HEAD@{4}: commit (initial): 1st git commit: 1 file
(END)
```
- Starting at commit id `7417f19`, this is the commit on remote. (It is the lastest commit in Computer B's reflog above). Git pulls `7417f19` commit, then pulls another commit `516655f` to get one forward local commit `dad9be5`.
- Interestingly, Git checks out the main branch first then attach the feature branch's commit.


- We reset the local repository to test the `rebase=true` command: `git reset --hard dad9be5`

```zsh
# Computer A
# rebase
edwardlo@Edwards-MBP:~/git_rebase_text|main ⇒  git pull --rebase=true
Successfully rebased and updated refs/heads/main.
```

Computer A's reflog:
```zsh
edwardlo@Edwards-MBP:~/git_rebase_text|main ⇒  git reflog
1a743c9 (HEAD -> main) HEAD@{0}: pull --rebase=true (finish): returning to refs/heads/main
1a743c9 (HEAD -> main) HEAD@{1}: pull --rebase=true (pick): 4th git commit: 4 files
7417f19 (origin/main) HEAD@{2}: pull --rebase=true (start): checkout 7417f19aa8abb43aecedc6a8baa61257b5a18090
dad9be5 HEAD@{3}: reset: moving to dad9be5
516655f HEAD@{4}: pull --rebase (finish): returning to refs/heads/main
516655f HEAD@{5}: pull --rebase (pick): 4th git commit: 4 files
7417f19 (origin/main) HEAD@{6}: pull --rebase (start): checkout 7417f19aa8abb43aecedc6a8baa61257b5a18090
dad9be5 HEAD@{7}: commit: 4th git commit: 4 files
a73ad43 HEAD@{8}: commit (initial): 1st git commit: 1 file
(END)
```
- Starting at `dad9be5 HEAD@{3}: reset: moving to dad9be5` because we did a hard reset in order to be able to use `rebase=true` command.
- Computer A local branch is considered as the Feature branch, the Remote Repository is the main branch. Hence, Git will first bring the Remote Repository commits in first and then attach the Feature branch commits. That's why commit id `1a743c9` finishes with the comment `4th git commit: 4 files`.

</details>
<br>
</details>

<details><summary>Branch & Merge</summary>

### Branch & Merge

#### Summary

1. `git branch <branchname>` - Branch Creation
2. `git checkout <branchname>` - Switch to Branch
3. `git merge <branchname>` - Merge `<branchname>` into Current Branch

<details><summary>Intro</summary>

## Intro

- [Git Branch](https://www.atlassian.com/git/tutorials/using-branches) tutorial.

When you want to add a new feature or fix a bug—no matter how big or how small—you spawn a new branch to encapsulate your changes. This makes it harder for unstable code to get merged into the main code base, and it gives you the chance to clean up your future's history before merging it into the main branch.

```zsh
		  B1 Little Feature
		 /         
    A---B---C---D Main
    		 \
    		  C1---D1---E1 Big Feature

```

The diagram above visualizes a repository with two isolated lines of development, one for a little feature, and one for a longer-running feature. By developing them in branches, it’s not only possible to work on both of them in parallel, but it also keeps the `main` branch free from questionable code.
</details>

<details><summary>Commands</summary>

## Commands

<details><summary>git branch - 1st Form - Listing Existing Branches</summary>

### git branch - 1st Form - Listing Existing Branches

- [git branch](https://git-scm.com/docs/git-branch#_synopsis) documentation.
```
git branch [--color[=<when>] | --no-color] [--show-current]
	[-v [--abbrev=<n> | --no-abbrev]]
	[--column[=<options>] | --no-column] [--sort=<key>]
	[--merged [<commit>]] [--no-merged [<commit>]]
	[--contains [<commit>]] [--no-contains [<commit>]]
	[--points-at <object>] [--format=<format>]
	[(-r | --remotes) | (-a | --all)]
	[--list] [<pattern>...]
```
- If `--list` is given, or if there are no non-option arguments, existing branches are listed; the current branch will be highlighted in green and marked with an asterisk.
</details>

<details><summary>git branch - 2nd Form - Branch Creation</summary>

### git branch - 2nd Form - Branch Creation

- [git branch](https://git-scm.com/docs/git-branch#_synopsis) documentation.
```
git branch [--track[=(direct|inherit)] | --no-track] [-f]
	[--recurse-submodules] <branchname> [<start-point>]
```
- The command’s second form creates a new branch head named `<branchname>` which points to the current HEAD, or `<start-point>` if given.

</details>

<details><summary>git branch -d - Delete Branch</summary>

### git branch -d - Delete Branch

- [git branch](https://git-scm.com/docs/git-branch#_synopsis) documentation.
```
git branch (-d | -D) [-r] <branchname>...
```
- With a `-d` or `-D` option, `<branchname>` will be deleted. You may specify more than one branch for deletion. If the branch currently has a reflog then the reflog will also be deleted.
- Use `-r` together with `-d` to delete remote-tracking branches.

#### OPTION

`-d`<br>
`--delete`<br>
Delete a branch. The branch must be fully merged in its upstream branch, or in `HEAD` if no upstream was set with `--track` or `--set-upstream-to`.
- [Command Options](https://www.atlassian.com/git/tutorials/using-branches) 
> This is a “safe” operation in that Git prevents you from deleting the branch if it has unmerged changes.

`-D`<br>
Shortcut for `--delete --force`.
- [Command Options](https://www.atlassian.com/git/tutorials/using-branches) > Force delete the specified branch, even if it has unmerged changes. This is the command to use if you want to permanently throw away all of the commits associated with a particular line of development.

</details>

<details><summary>git branch -m - Rename Branch</summary>

### git branch -m - Rename Branch

- [git branch](https://git-scm.com/docs/git-branch#_synopsis) documentation.
```
git branch (-m | -M) [<oldbranch>] <newbranch>
```
- With a `-m` or `-M` option, \<oldbranch> will be renamed to \<newbranch>.

#### OPTION

`-m`<br>
`--move`<br>
Move/rename a branch, together with its config and reflog.

`-M`<br>
Shortcut for `--move --force`.

</details>

<details><summary>git checkout - Switch branches (including main) or restore working tree files</summary>

### git checkout - Switch branches (including main) or restore working tree files

- [git checkout](https://git-scm.com/docs/git-checkout) documentation.
```
git checkout [-q] [-f] [-m] [<branch>]
```
- To prepare for working on `<branch>`, switch to it by updating the index and the files in the working tree, and by pointing `HEAD` at the branch.

#### OPTION

`-q`<br>
`--quiet`<br>
Quiet, suppress feedback messages.

`-f`<br>
`--force`<br>
When switching branches, proceed even if the index or the working tree differs from HEAD, and even if there are untracked files in the way. This is used to throw away local changes and any untracked files or directories that are in the way.

`-m`<br>
`--merge`<br>
When switching branches, if you have local modifications to one or more files that are different between the current branch and the branch to which you are switching, the command refuses to switch branches in order to preserve your modifications in context. However, with this option, a three-way merge between the current branch, your working tree contents, and the new branch is done, and you will be on the new branch.

</details>

<details><summary>git merge - Join two or more development histories together</summary>

### git merge - Join two or more development histories together

- [git merge](https://git-scm.com/docs/git-merge) documentation.
- Incorporates changes from the named commits (since the time their histories diverged from the current branch) into the current branch. This command is used by **git pull** to incorporate changes from another repository and can be used by hand to merge changes from one branch into another.

Assume the following history exists and the current branch is "`master`":
```
	  A---B---C topic
	 /
D---E---F---G master
```
Then "`git merge topic`" will replay the changes made on the `topic` branch since it diverged from `master` (i.e., `E`) until its current commit (`C`) on top of `master`, and record the result in a new commit along with the names of the two parent commits and a log message from the user describing the changes. Before the operation, `ORIG_HEAD` is set to the tip of the current branch (`C`).

Example [Merge Workflow](https://www.atlassian.com/git/tutorials/using-branches/git-merge).
```zsh
# Start a new feature
git checkout -b new-feature main
# Edit some files
git add <file>
git commit -m "Start a feature"
# Edit some files
git add <file>
git commit -m "Finish a feature"
# Develop the main branch
git checkout main
# Edit some files
git add <file>
git commit -m "Make some super-stable changes to main"
# Merge in the new-feature branch
git merge new-feature
git branch -d new-feature
```

</details>

</details>
<br>
</details>

<details><summary>VI Editing - Basic vi commands</summary>

### VI Editing - Basic vi commands

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

</details>
