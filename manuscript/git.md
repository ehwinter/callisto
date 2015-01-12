Cleaning up branches
--------------------

#### remove local branches

while on say master, remove local branch scratch\
git branch -D scratch

#### remove branches on remote

Now remove scratch from the remote makalu:\
git push makalu :scratch

#### resynch/prune no longer useful branches

If you go to another machine scratch will not longer exist on remote
makalu so you will have to do this to remove the now deprecated remote.
This command actually resynch and prunes all non-existant remote
branches in your local repo.

git remote prune makalu \# makalu is the name of the remote, could be
origin or upstream…

Prune: get rid of useless commits
---------------------------------

If you do branch/merge some commits are no longer referenced

git prune\
git gc —prune=now

Highly mobile use case
----------------------

two repositories say origin and cache\
origin - is the reference\
cache - is the cache is another repo on another machine. private and
trashy were wip is stored. Final product will be squashed then merged
into origin.

#### Branches on local:

master - in synch with origin\
ehw-master - branches from master used as astaging area for master.\
scratch - working copy\
master-\>ehw-master-\>scratch

#### Starting out: synchronize branches and cache

git fetch origin \#Any new changes\
git fetch cache\
gitk —all\
\#on master git pull origin master\
git checkout ehw-master\
git reset —hard master \# ehw-master = master\
git checkout scratch\
git reset —hard master\
git push -f cache ehw-master \# reset remote repo too.\
git push -f cache scratch

#### when changing machines save state to scratch

git commit stuff on t234\
git checkout scratch\
git reset —soft t234 \# point HEAD to t234\
git push cache scratch \#or just scratch\
git push -f cach
