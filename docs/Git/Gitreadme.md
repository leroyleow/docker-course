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

