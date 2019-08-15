## Create New Repo in GitHub and Push Code

### Description:

I have some code in my local IDE. Then I'd like to create a repository in GitHub and push my local code to my new repo.



### Create new repo in GitHub

Click "create new repository" and fill out the blanks.

**Note: DO NOT check "Initialize this repository with a README"**. Otherwise conflict may happen when doing your first push.

![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LhhGUjjrx95GyAyz88l%2F-LmH7QqHm0CTiVGmzTRT%2F-LmH8AqW36rx3c_Hb3xo%2FScreen%20Shot%202019-08-14%20at%204.59.11%20PM.png?alt=media&token=8a564a1e-9847-453f-91ef-5ff0ab0cb49c)



![img](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LhhGUjjrx95GyAyz88l%2F-LmH7QqHm0CTiVGmzTRT%2F-LmH81ahiqvBujFFYPWr%2FScreen%20Shot%202019-08-14%20at%204.54.05%20PM.png?alt=media&token=5c5af098-dece-4343-8e05-dfb8de8c8f08)



### Push the first code to new repo (No README file in your new repo)

#### 1. If this code is your locally stand-alone project (is not cloned/linked from any other repos)

```
$ cd <path to your project>
$ git init
# Step1: go to your local project, and init it as a new local git repo


$ git remote add origin <repo link - https or ssh>
# Step2: link local git repo to remote repo


$ git add .
$ git commit -m"<your mesage>"
# Step3: add all your local files and commit the current existing code as the first commit


$ git push -u origin master
# Step4: push current existing code to remote master branch
```

For example:

```
weiqianwang$ cd /Users/weiqianwang/IdeaProjects/LeetCode
weiqianwang$ git init
Initialized empty Git repository in /Users/weiqianwang/IdeaProjects/LeetCode/.git/
weiqianwang$ git remote add origin https://github.com/VeromcaWang/LeetCodeSolutions.git
weiqianwang$ git add .
weiqianwang$ git commit -m"First push to my new repo created from scratch"
weiqianwang$ git push -u origin master
```



#### 2. If this code is cloned from another repo

**(1) If we want to keep the history of this code: Mirroring**

```
$ cd ~
$ git clone --bare https://github.com/exampleuser/old-repository.git
# Step1: go to anywhere and create a bare clone of the repository.
# It will create a "old-repository.git" folder at where you are.


$ cd old-repository.git
$ git push --mirror https://github.com/exampleuser/new-repository.git
# Step2: mirror-push to the new repository.
# Then you can clone the code from the new repo to your local and modify it.


$ cd ..
$ rm -rf old-repository.git
# Step3: remove the temporary local repository you created in step 1.‌
```

**(2) If we do not want to keep the history:**

Easiest way is to delete the .git folder of this project. Then handle the code like a locally stand-alone project. --> in section 1.



### What if you checked "Initialize with README" when you create your new repo?

Clone this repo to your local first. Then push new code to it.