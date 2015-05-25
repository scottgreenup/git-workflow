# The Humblebee Git Work Flow

  1. Update your local copy of the remote
  ```
  git fetch --all
  ```

t Work Flow              
## 1. Pull down a remote you want to work with.
 
  You can use HTTPS
 ```
 git clone https://github.com/torvalds/linux.git
 ```
 or you can use SSH
 ```
 git clone git@github.com:torvalds/linux.git
 ```
## 2. Update your local copy of the remote                      
 ```                                                          
 git fetch --all                                              
 ```
## 3. Checkout the branch you want to branch from
If you want to checkout `origin/existing-branch`, do this:
```
git checkout existing-branch
```
A couple ways to see the branches are:
```
git branch --all
```
```
git log --graph --abbrev-commit --decorate --date=relative --oneline
```
If you want better git log representations, check out this link: http://stackoverflow.com/questions/1057564/pretty-git-branch-graphs

For example if you want to work from the master branch after pulling, it will be something like `origin/feature-a`
```
git checkout feature-a
```

## 4. Branch from this branch
Create a new branch:
```
git checkout -b "sgre-my-feature"
```
I use `sgre` as the start of branch names as my name is Scott Greenup. I'd recommend it to more easily find your branches.Or use the standard with the git remote you are working with.

### 5. Work, Add, Commit, [Push], Repeat
You are now in your own branch!
 1. Edit some files or do some work
 2. Add the files to git for staging
 ```
 git add file1
 git add folder1/file2
 ```
 3. Commit these files to your branch
 ```
 git commit
 ```
 It should open up in Vim or some editor, the first line is a brief subject of what your commiting, the lines 3 onwards are a more verbose description
 
 4. [Push]ing is optional, if you have done a lot of work or are leaving the computer for an extended period, it is recommended.
 ```
 git push origin your-branch-name
 ```
 With our example before, it would be:
 ```
 git push origin "sgre-my-feature"
 ```
 5. Go back to 1.

### 6. You have finished your feature and want to merge
If you follow these steps, you'll be safe. Some of these steps are uneccessary, they are explained in the step.
 1. Update your local copy to match the remote
 ```
 git fetch --all
 ```
 2. Fast-forward the branch you branched from to the origin version.
 ```
 git checkout "parent-branch-that-you-branched-from"
 git merge --ff-only "origin/parent-branch-that-you-branched-from"
 git checkout "your-working-branch"
 ```
 For our example from before:
 ```
 git checkout "master"
 git merge --ff-only "origin/master"
 git checkout "sgre-my-feature"
 ```
 Note: You may not have to fast forward if origin/parent-branch and parent-branch are the same commit, which would also
 mean you don't have to rebase.
 3. Time to rebase our branch.
 
 NOTE: This step is not recommended if you have pushed your branch to origin or if you have someone branching off of your branch. If you've pushed to origin,you can rebase then push with force. If someone has branched off your branch they need to rebase onto your stuff after you've pushed with force.

 ```
 git checkout "your-working-branch"
 git rebase "parent-branch-that-you-branched-from"
 ```
 If there are conflicts. You have to open the file that is conflicted. Edit the section that looks like
 ```
 <<<<<<<<<
 // stuff from commit a
 =========
 // stuff from commit b
 >>>>>>>>>>
 ```
 Then git add it, and run `git rebase --continue`. Or ask someone to help.
 
 4. Checkout and merge to parent branch, then push
 ```
 git checkout "parent-branch-that-you-branched-from"
 git merge --no-ff "your-working-branch"
 git push origin "parent-branch-that-you-branched-from"
 ```
 
 5. Go back to "2. Update your local copy of the remote"
 
