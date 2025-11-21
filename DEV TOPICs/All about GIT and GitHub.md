[https://github.com/arslanbilal/git-cheat-sheet#readme](https://github.com/arslanbilal/git-cheat-sheet#readme)

## practice more time to time 
https://learngitbranching.js.org/ 

commit is information about the difference between changes from point 1 to point 2

git commit 

#### commands to create new branch and to move to it 

git branch branch_name

git checkout branch_name 

git checkout -b branch_name  ---> do this directly 

### Merging 

git merge 
this creates a special commit that has two unique parents 
git merge branch_name --> we merge the specified branch into the current branch.

git merge   only accepts one argument which is the name of branch we want to merge into our current branch. 

![[Pasted image 20251007213659.png]]

## Rebase 
it is a different technique to combining work between branches , used to make a nice linear sequence of commits. The commit log / history of the repository will be a lot cleaner if only rebasing is allowed.

git rebase main  <active branch is bugFix> 
git rebase <b1> <b2> accepts two argument b1 is the base b2 is the one who you want to change the base means we move b2 to b1 in simple terms.

using this the copy of commits point to main branch commit 
the commits of bugFix will shift to main branch appearing the features to be developed in sequence it's like we are saying to change the base to main for active branch bugFix

Head -> always point to the most recent commit which is reflected in working tree. Most git commands which make changes to working tree will start from HEAD.
Normally Head points to a branch, as we commit the status of branch changes which is visible through HEAD.  

observe a commit using
<git checkout commit_name >
irrespective of its branch. 

Moving upwards one commit a time with ^ 
Moving upwards a number of times with ~<num>

appending ^ to a ref name like --> main^ help us to switch to first parent .
and git checkout main^^ to grandparent of main. 

technically git checkout main^ helps us to just move up in commit tree . 

# primary ways to undo the changes 
git reset HEAD~1  <cmd means reset HEAD by step 1 > --> move the branch backwards as if the commit had never been made. works for local branches , some issues with remote branch that others are using .
git revert HEAD just <revert the current commit >--> this creates new commit below the commit we wanted to reverse. This commit introduce changes that exactly reverse our previous commit. 

## Moving workflow here and there 

git cherry-pick <commit1> <commit2> <commit3> 
copying series of commits below your current location (HEAD). 
we are introducing the changes of commit1 and so on in our active commit and moving down . 
###### what if we don't know the commit 
--> here comes interactive rebasing , its best way to review a series of commit that we are about to rebase .

It is worth mentioning that in the real git interactive rebase you can do many more things like squashing (combining) commits, amending commit messages, and even editing the commits themselves.
here we will only consider about reordering , and choosing which commit to include.

when we we copy one commit to another place we just moves the changes to that place great way.. 
for e.g. consider there is bug into my project i tried to resolve using come debug code and finally the bug is fix . so i want to say that changes in last 2 commits of BugFix branch will contains the code for bugFix only not any debug statement which when copied into the main branch keep our code clean. 

###### git commit --amend
to add my changes in previous commit itself . 

### GIT TAGS 
they permanently mark certain commits as milestones ... for these tags can't be "check out" and perform actions
git tag v1 <commit_name> v1 is the tag name -- version 1

###### git describe <ref> 
output is somewhat -- <tag>-<numCommits>-g<hash>