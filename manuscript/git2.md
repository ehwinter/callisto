# Git Basics

### How to create a new fresh repository

[[url:https://www.assembla.com/wiki/show/fotw/Git-creating_a_new_repsitory|How to create a new repository]]
[[url:https://www.assembla.com/spaces/fotw/wiki/Git_Mobile_Case_-_throwaway_branches|Git mobile case - throwaway branches]]

### Cleaning up: branches, remotes, pruning, etc

[[url:https://www.assembla.com/spaces/fotw/wiki/Git_-_Cleaning_up|Git Clean Up - cleanliness is next to Godliness]]

#### Handy Commands

```
see what origin is set to:
git config remote.origin.url 

set a new one:
git config remote.origin.url thegituser@makalu:git/cloud/walkull
```

#### Typical Flow

Here is a typical example of using Git with multiple branches.


### Git Flow

```
"git flow description":http://nvie.com/posts/a-successful-git-branching-model/
master - sacrosanct major release, split from a release branch
v1.0, vX - release branch
develop - primary development branch
```

h5. typical flow

```
git checkout -b feature/something
multiple commits
git rebase develop # keep abreast of changes
# don't git merge into feature/something rebase.
#ready for merge
git checkout develop
git merge --no-ff feature/something
# nice clean branch history
```

### Create/Apply a Patch

If you can not push to a github repo or clone/pull request create a patch:
```
Assumes spree (parent) and tax-render (branch off parent)
From: Spree branch commit 5fd4937d359f3db56cc52cf43bbf430ef18d2891
$ git checkout tax-render
Switched to branch 'tax-render'
$ git format-patch spree --stdout > spree2tax-render_branch.patch #creates patch
$ git checkout spree
Switched to branch 'spree'
$ git apply --stat spree2tax-render_branch.patch #see the deltas, do nothing
$ git apply --check spree2tax-render_branch.patch # test application
$ git am --signoff < spree2tax-render_branch.patch  # do the application of the patch.
Applying: ajax tax render fix
```

### Create local branches (to track remote branches)

This would be used if you do not already have local branches set up (e.g. if you created a new repository), or if you never had a local copy of a particular "tracking branch" (i.e. a local branch that tracks the remote branch).
 
* git checkout -b master -f remotes/origin/master
 
* git checkout -b shipping -f remotes/origin/shipping
 
* git checkout -b communication -f remotes/origin/communication

### Create Local Branches

Create a local branch (splits it from master)

* git checkout -b vcodes

* change and commit in the new vcodes

* (may need a git clean)

* git merge -s resolve --squash vcodes

## Refreshing local branches

In the case above you did not have a local tracking branch. If you already have a repository with a tracking branch, say master, but that branch has stale code then you will want to update that stale branch to be in synch with the remote.

* git checkout -f master  (ignores/overwrites local files, and switches to that (local) tracking branch.  Still stale)

* git reset --hard (optional, but might be useful in the event that you have files not checked in that cause a conflict with files about to be pulled in.)

* git pull origin master (get's the latest code from remote. you are not stale anymore.)

* git reset --hard  (gets rid of any local files that are not a part of the commit)

* git pull  (fastforwards the current local branch w/ the latest remote tags and moves remotes/origin/master up.)

Here they are again for cutting/pasting:

git checkout -f master 

git reset  --hard

git pull origin master

git reset  --hard

git pull


## svn revert - undoing a change in a single file.

If you have an uncommitted change (its only in your working copy) that you wish to revert (in SVN terms) to the copy in your latest commit, do the following:

git checkout filename

This will checkout the file from HEAD, overwriting your change. This command is also used to checkout branches, and you could happen to have a file with the same name as a branch. All is not lost, you will simply need to type:

git checkout -- filename

## Make a change and commit to the local repository.

If following commands directly from the above, checkout -f to your working branch, perhaps shipping. Note this would be on some branch other than master most likely. 
 
* edit file.txt
 
* git add file.txt
 
* git status (just to check that you did not forget anything)
 
* git commit

### get new files on this branch that someone else may have made

 * git fetch

* if there are merge conflicts repeat the edit/add/status/commit process from above.

* git push (optional, in case you want to make your code visible externally)

## Merge in code from the most up to date version of master

* git merge -s resolve remotes/origin/master 

* if there are merge conflicts repeat the edit/add/status/commit process from above.

* test to valididate

### push current version to git repository

* git push origin communication


## Checkout (get)

Assuming you don't mind losing files not yet checked in try this sequence.  Creates a new (local) branch called my-local-branch-name. and pulls in all the files needed for the remote version of dirty vendor. The "f" is force so that you don't get merge issues (since you are merging code into your existing "working tree" (file files you see and edit)

* git checkout -b my-local-branch-name -f remotes/origin/dirty-vendor


* git checkout -b dirty-vendor -f remotes/origin/dirty-vendor (this is preferred to the above command)


## Push - pushing your code to the remote repository


Use this to push your local committed code on branch "communication"  up to the remote git communication (i.e. remotes/origin/communication

* git push origin communication

If instead you called your local branch comm, you would want to indicate that Comm (locally) should be pushed to communication at remote

* git push origin comm:communication

## Clean and other useful things

Unmanaged files can cause conflicts.  To clean your local files (your local tree).  

* git clean -f

Graphically view the commit history.

* gitk

Clean up old unused commits (check it...)

* git reflog expire --all --expire=now --dry-run --verbose


### Set the appropriate files to ignore (on local machine)

Add files to _.git/info/exludes_

* *~
* log/*
* .project
* .loadpath

### Handy commands from: schacon@gmail.com

#### books/sites

* Git Internals
* Pro Git proGit.org

#### incremental changes

git add -p (add chunks)

#### viewing _whole_ tree

```
gitk --all
git log --graph --all
```

#### remote servers/repositories

```

clone creates origin from where you pulled it.
git remote add orign git@github.... (add remote server having name origin)
git push origin master (push master branch to the origin remote server)
```


#### fetch!

git fetch (pull will try to merge it, fetch just gets it. better than pull)


git blame -C kdkd.rb
git bisect start (binary search for an error)
git bisect bad (binary search for an error)
git bisect reset



# from err the blog

Setup
-----

```
git clone <repo>
  clone the repository specified by <repo>; this is similar to "checkout" in
  some other version control systems such as Subversion and CVS
```

Add colors to your ~/.gitconfig file:

```

  [color]
    ui = auto
  [color "branch"]
    current = yellow reverse
    local = yellow
    remote = green
  [color "diff"]
    meta = yellow bold
    frag = magenta bold
    old = red bold
    new = green bold
  [color "status"]
    added = yellow
    changed = green
    untracked = cyan
```

Highlight whitespace in diffs

```
  [color]
    ui = true
  [color "diff"]
    whitespace = red reverse
  [core]
    whitespace=fix,-indent-with-non-tab,trailing-space,cr-at-eol
```

Add aliases to your ~/.gitconfig file:

```
  [alias]
    st = status
    ci = commit
    br = branch
    co = checkout
    df = diff
    lg = log -p
```


Configuration
-------------

```
git config -e [--global]
  edit the .git/config [or ~/.gitconfig] file in your $EDITOR

git config --global user.name 'John Doe'
git config --global user.email johndoe@example.com
  sets your name and email for commit messages

git config branch.autosetupmerge true
  tells git-branch and git-checkout to setup new branches so that git-pull(1)
  will appropriately merge from that remote branch.  Recommended.  Without this,
  you will have to add --track to your branch command or manually merge remote
  tracking branches with "fetch" and then "merge".

git config core.autocrlf true
  This setting tells git to convert the newlines to the system’s standard
  when checking out files, and to LF newlines when committing in
```

You can add "--global" after "git config" to any of these commands to make it
apply to all git repos (writes to ~/.gitconfig).


Info
----

```
git diff
  show a diff of the changes made since your last commit
  to diff one file: "git diff -- <filename>"
  to show a diff between staging area and HEAD: `git diff --cached`

git status
  show files added to the staging area, files with changes, and untracked files

git log
  show recent commits, most recent on top. Useful options:
  --color       with color
  --graph       with an ASCII-art commit graph on the left
  --decorate    with branch and tag names on appropriate commits
  --stat        with stats (files changed, insertions, and deletions)
  -p            with full diffs
  --author=foo  only by a certain author
  --after="MMM DD YYYY" ex. ("Jun 20 2008") only commits after a certain date
  --before="MMM DD YYYY" only commits that occur before a certain date
  --merge       only the commits involved in the current merge conflicts

git log <range>..<range>
  show commits between the specified range. Useful for seeing changes from
  remotes:
  git log HEAD..origin/master # after git remote update

git show <rev>
  show the changeset (diff) of a commit specified by <rev>, which can be any
  SHA1 commit ID, branch name, or tag (shows the last commit (HEAD) by default)

git blame <file>
  show who authored each line in <file>

git blame <file> <rev>
  show who authored each line in <file> as of <rev> (allows blame to go back in
  time)

git gui blame
  really nice GUI interface to git blame

git whatchanged <file>
  show only the commits which affected <file> listing the most recent first
```


Adding / Deleting
-----------------

```
git add <file1> <file2> ...
  add <file1>, <file2>, etc... to the project

git add <dir>
  add all files under directory <dir> to the project, including subdirectories

git add .
  add all files under the current directory to the project

git rm <file1> <file2> ...
  remove <file1>, <file2>, etc... from the project

git rm $(git ls-files --deleted)
  remove all deleted files from the project

git rm --cached <file1> <file2> ...
  commits absence of <file1>, <file2>, etc... from the project
```


Staging
-------

```
git add <file1> <file2> ...
git stage <file1> <file2> ...
  add changes in <file1>, <file2> ... to the staging area (to be included in
  the next commit

git add -p
git stage --patch
  interactively walk through the current changes (hunks) in the working
  tree, and decide which changes to add to the staging area.

git add -i
git stage --interactive
  interactively add files/changes to the staging area. For a simpler
  mode (no menu), try `git add --patch` (above)
```


Committing
----------

```
git commit <file1> <file2> ... [-m <msg>]
  commit <file1>, <file2>, etc..., optionally using commit message <msg>,
  otherwise opening your editor to let you type a commit message

git commit -a
  commit all files changed since your last commit
  (does not include new (untracked) files)

git commit -v
  commit verbosely, i.e. includes the diff of the contents being committed in
  the commit message screen

git commit --amend
  edit the commit message of the most recent commit

git commit --amend <file1> <file2> ...
  redo previous commit, including changes made to <file1>, <file2>, etc...
```


Branching
---------

```
git branch
  list all local branches

git branch -r
  list all remote branches

git branch -a
  list all local and remote branches

git branch <branch>
  create a new branch named <branch>, referencing the same point in history as
  the current branch

git branch <branch> <start-point>
  create a new branch named <branch>, referencing <start-point>, which may be
  specified any way you like, including using a branch name or a tag name

git branch --track <branch> <remote-branch>
  create a tracking branch. Will push/pull changes to/from another repository.
  Example: git branch --track experimental origin/experimental

git branch -d <branch>
  delete the branch <branch>; if the branch you are deleting points to a commit
  which is not reachable from the current branch, this command will fail with a
  warning.

git branch -r -d <remote-branch>
  delete a remote-tracking branch.
  Example: git branch -r -d wycats/master

git branch -D <branch>
  even if the branch points to a commit not reachable from the current branch,
  you may know that that commit is still reachable from some other branch or
  tag. In that case it is safe to use this command to force git to delete the
  branch.

git checkout <branch>
  make the current branch <branch>, updating the working directory to reflect
  the version referenced by <branch>

git checkout -b <new> <start-point>
  create a new branch <new> referencing <start-point>, and check it out.

git push <repository> :<branch>
  removes a branch from a remote repository.
  Example: git push origin :old_branch_to_be_deleted
```


Merging
-------

```

git merge <branch>
  merge branch <branch> into the current branch; this command is idempotent and
  can be run as many times as needed to keep the current branch up-to-date with
  changes in <branch>

git merge <branch> --no-commit
  merge branch <branch> into the current branch, but do not autocommit the
  result; allows you to make further tweaks

git merge <branch> -s ours
  merge branch <branch> into the current branch, but drops any changes in
  <branch>, using the current tree as the new tree
```


Cherry-Picking
--------------

```
git cherry-pick [--edit] [-n] [-m parent-number] [-s] [-x] <commit>
  selectively merge a single commit from another local branch
  Example: git cherry-pick 7300a6130d9447e18a931e898b64eefedea19544
```


Squashing
---------
```

WARNING: "git rebase" changes history. Be careful. Google it.

git rebase --interactive HEAD~10
  (then change all but the first "pick" to "squash")
  squash the last 10 commits into one big commit
```


Conflicts
---------

```
git mergetool
  work through conflicted files by opening them in your mergetool (opendiff,
  kdiff3, etc.) and choosing left/right chunks. The merged result is staged for
  commit.

For binary files or if mergetool won't do, resolve the conflict(s) manually and
then do:

  git add <file1> [<file2> ...]

Once all conflicts are resolved and staged, commit the pending merge with:

  git commit
```


Sharing
-------

```
git fetch <remote>
  update the remote-tracking branches for <remote> (defaults to "origin").
  Does not initiate a merge into the current branch (see "git pull" below).

git pull
  fetch changes from the server, and merge them into the current branch.
  Note: .git/config must have a [branch "some_name"] section for the current
  branch, to know which remote-tracking branch to merge into the current
  branch.  Git 1.5.3 and above adds this automatically.

git push
  update the server with your commits across all branches that are *COMMON*
  between your local copy and the server.  Local branches that were never pushed
  to the server in the first place are not shared.

git push origin <branch>
  update the server with your commits made to <branch> since your last push.
  This is always *required* for new branches that you wish to share.  After the
  first explicit push, "git push" by itself is sufficient.
```


Reverting
---------

```
git revert <rev>
  reverse commit specified by <rev> and commit the result.  This does *not* do
  the same thing as similarly named commands in other VCS's such as "svn revert"
  or "bzr revert", see below

git checkout <file>
  re-checkout <file>, overwriting any local changes

git checkout .
  re-checkout all files, overwriting any local changes.  This is most similar to
  "svn revert" if you're used to Subversion commands
```


Fix mistakes / Undo
-------------------

```
git reset --hard
  abandon everything since your last commit; this command can be DANGEROUS.  If
  merging has resulted in conflicts and you'd like to just forget about the
  merge, this command will do that.

git reset --hard ORIG_HEAD
  undo your most recent *successful* merge *and* any changes that occurred
  after.  Useful for forgetting about the merge you just did.  If there are
  conflicts (the merge was not successful), use "git reset --hard" (above)
  instead.

git reset --soft HEAD^
  forgot something in your last commit? That's easy to fix. Undo your last
  commit, but keep the changes in the staging area for editing.

git commit --amend
  redo previous commit, including changes you've staged in the meantime.
  Also used to edit commit message of previous commit.
```


Plumbing
--------

test <sha1-A> = $(git merge-base <sha1-A> <sha1-B>)
  determine if merging sha1-B into sha1-A is achievable as a fast forward;
  non-zero exit status is false.


Stashing
--------

```
git stash save <optional-name>
  save your local modifications to a new stash (so you can for example
  "git svn rebase" or "git pull")

git stash apply
  restore the changes recorded in the stash on top of the current working tree
  state

git stash pop
  restore the changes from the most recent stash, and remove it from the stack
  of stashed changes

git stash list
  list all current stashes

git stash show <stash-name> -p
  show the contents of a stash - accepts all diff args

git stash clear
  delete current stashes
```


Remotes
-------

```
git remote add <remote> <remote_URL>
  adds a remote repository to your git config.  Can be then fetched locally.
  Example:
    git remote add coreteam git://github.com/wycats/merb-plugins.git
    git fetch coreteam

git push <remote> :refs/heads/<branch>
  delete a branch in a remote repository

git push <remote> <remote>:refs/heads/<remote_branch>
  create a branch on a remote repository
  Example: git push origin origin:refs/heads/new_feature_name

git push <repository> +<remote>:<new_remote>
  create a branch on a remote repository based on +<remote>
  Example: git push origin +master:my_branch

git remote prune <remote>
  prune deleted remote-tracking branches from "git branch -r" listing
```


Submodules
----------

```
git submodule add <remote_repository> <path/to/submodule>
  add the given repository at the given path. The addition will be part of the
  next commit.

git submodule update [--init]
  Update the registered submodules (clone missing submodules, and checkout
  the commit specified by the super-repo). --init is needed the first time.

git submodule foreach <command>
  Executes the given command within each checked out submodule.

Remove submodules

   1. Delete the relevant line from the .gitmodules file.
   2. Delete the relevant section from .git/config.
   3. Run git rm --cached path_to_submodule (no trailing slash).
   4. Commit and delete the now untracked submodule files. 
```


Git Instaweb
------------

git instaweb --httpd=webrick [--start | --stop | --restart]


Environment Variables
---------------------

```
GIT_AUTHOR_NAME, GIT_COMMITTER_NAME
  Your full name to be recorded in any newly created commits.  Overrides
  user.name in .git/config

GIT_AUTHOR_EMAIL, GIT_COMMITTER_EMAIL
  Your email address to be recorded in any newly created commits.  Overrides
  user.email in .git/config
```

