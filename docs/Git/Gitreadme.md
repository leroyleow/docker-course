# Git

## Another way to init remote repo
1. Each repo has a url [e.g Github/ Bitbucket / GitLabs]
2. Once code created, type "git remote add <alias: origin> <url>
3. Type "git push <alias: origin> <branchname: master>

## Git Cheatsheet
![Git Cheatsheet](/docs/imgs/Git%20cheat%20sheet%20light%20(FINAL).jpg)

![Git Cheatsheet2](/docs/imgs/git02.jpg)

The following command pull from origin alias, which refers to a conneciton. 
git pull <alias:origin> <branch:master>

In the branch, if want to get specific commit, type "git cherry-pick <git hash>"

Following command to reset back 1 step while keep the changes
```
$ git reset --soft HEAD~1
```

Following command to reset back 1 step and discard changes
```
$ git reset --hard HEAD~1
```

## Fork
### What is Fork?
In Git, a fork is a copy of a repository. It allows you to create a new repository based on the code of an existing repository, but with the ability to make changes without affecting the original repository. This is commonly used for open-source projects, where developers can fork the main repository, make changes, and then submit a pull request to have their changes merged into the main repository

### How do PR work in Fork ?
When a developer forks a repository, they create a copy of the original repository that they can make changes to. The developer can then make changes and commit them to their forked repository.

Once the developer has made the desired changes and commits, they can then open a pull request. A pull request is a request to the maintainers of the original repository to review the changes made in the fork and merge them into the main repository.

The maintainers of the original repository will review the changes and determine if they should be merged. If the changes are accepted, they will be merged into the main repository. If there are issues with the changes, the maintainers may request that the developer make additional changes before merging.

Once the pull request is accepted and merged, the changes made in the fork will be part of the main repository.

It's a common practice in open source projects, where contributors fork the main repository, make changes, and then submit a pull request to have their changes merged into the main repository.

![git-fork](/docs/imgs/git01.JPG)

## Stash

Git stash is a command in Git that allows you to temporarily save changes that you have made to your working directory but do not want to commit yet. The changes are saved in a "stash" and can be later reapplied to the working directory or a different branch. This is useful when you need to switch to a different branch to work on something else but do not want to commit your current changes and lose them.

Here is an example of using the git stash command:

You are working on a feature branch called feature1 and have made some changes to your working directory.

You want to switch to the master branch to work on a different task, but you do not want to commit the changes you made to the feature1 branch.

Run the command git stash to save the changes in a stash.

```
$ git stash
Saved working directory and index state WIP on feature1: abcdefg
```
HEAD is now at abcdefg
Now you can switch to the master branch using git checkout master

Once you are done working on the master branch, you can switch back to the feature1 branch and reapply the changes using the command git stash apply

```
$ git stash apply
On branch feature1
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   file1.txt
        modified:   file2.txt
```
Note: If you want to remove the stash after you have applied or if you want to list, apply or drop multiple stashes, you can use git stash list, git stash apply and git stash drop commands respectively.

![Git Stash](/docs/imgs/git04.jpg)