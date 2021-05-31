# git-workshop

### Table of Contents
[0. Prerequisites](https://github.com/mstanczy/git-workshop#0-prerequisites)

[1. Working Directory, Index and Commits](https://github.com/mstanczy/git-workshop#1-working-directory-index-and-commits)

[2. Branches](https://github.com/mstanczy/git-workshop#2-branches)

[3. Merges and Conflicts](https://github.com/mstanczy/git-workshop#3-merges-and-conflicts)

[4. Remote Repository](https://github.com/mstanczy/git-workshop#4-remote-repository)
 
-----
### 0. Prerequisites

#### 0.1 Install git

##### On Mac:  [http://git-scm.com/download/mac](http://git-scm.com/download/mac)
##### On Linux: http://git-scm.com/download/linux
##### On Windows: [http://git-scm.com/download/win](http://git-scm.com/download/win)

During installation on Windows:
 - Select your favorite text editor as default 
 - leave **Let Git decide** option selected for the initial branch name 
 - leave other default options intact

Open Git Bash and verify:
```
git version
```
On Windows in addition to git please also install WinMerge (free software).

------

#### 0.2  Basic Config

##### Username + Email
Present yourself to git
```
git config --global  user.name  my_username
git config --global  user.email  my@email.com
```
#####  Default  text  editor
Choose your default editor (for example nano or notepad++)
```
git config --global  core.editor nano
```

##### Verify the config
```
git config user.name

git config user.email

git config core.editor
```
------

#### 0.3 Gitlab account

Create an account on our Cisco Gitlab (VPN required) if you don't have it yet:

https://gitlab-sjc.cisco.com

-----
### 1. Working Directory, Index and Commits

 1. Create a new local folder called 'git_workshop'
```
mkdir git_workshop
```
 2. Initialize the git repository in the new folder
```
cd git_workshop
git init
```
4. Display all files, including hidden ones. Notice there's .git folder created which includes all git objects, trees and refs. Display all files in .git folder:
``` 
ls -la
ls -l .git
```

5.  Create a new file "test_file.txt" with a single line of text: "This is line 1"
6.  The new file is now in the Working Directory but it is not tracked by git.  "git status" helps determine which files are tracked by git:
```
git status
```
7. Add the file to the Index (Staging Area):
```
git add test_file.txt
```

8. Issue "git status" to verify. From now on git will track changes to this file.
```
git status
```
9. Edit the file by adding a new line of text: "This is line 2".
10. Issue "git status" and notice the file is now listed twice, in fact git recognized there's two versions of the same file - the new file staged for commit (in the Index) and the modified (dirty) version of the same file which we're still working on (it's in Working Directory)
``` 
git status
```

11. Issue "git diff" to compare the current version of the file in Working Directory with the version that exists in the Index
```
git diff
```

12. Create a new file "test_file2.txt" with some text in it. **Don't add this file to Index yet.**
13. Issue "git status" and notice that "test_file2.txt" is listed as untracked - changes to this file are not tracked by git
 ``` 
git status
```
14. Commit changes to the main (default) branch
```
git commit -m "test_file added"
```
15. Issue 'git status' and notice that modified version of test_file.txt and untracked file test_file2.txt are still displayed in Working Directory (not staged for commits)
 ``` 
git status
```

16. Issue "git log" to see the existing commits in the main branch. The HEAD points to the latest commit.
 ``` 
git log
```
17. Add both the modified test_file.txt and a new test_file2.txt to the Index
 ``` 
git add .
```

18. Issue "git" status and notice both files are staged for commit and will be added to the local repo upon the next commit.

 ``` 
git status
```

19. Issue "git diff" and notice there's no output produced... why?
 ``` 
git diff
```

20. Issue ```git diff --cached``` and observe the difference

```
git diff --cached
```


21. You realized that test_file2.txt is added by mistake and shouldn't be committed. Let's unstage it using ```git reset [file]``` command.
 ``` 
git reset test_file2.txt
```
 Confirm the file is no longer listed in Index
 ```
 git status
 ```
20. Commit changes with below commit message:
```
git commit -am  "added line2 in test_file"
```
21. Issue 'git log' and notice that HEAD now points to the new commit
 ```
 git log
 ```

22. Let's see what is the diff between the last two commits.  Issue ```git diff <sha_nr1> <sha_nr2>``` where each parameter includes the first 6 digits of the hash of the last 2 commits. 

```
git diff <sha_nr1> <sha_nr2>
```

23.  Change the order of hashes: 
```
"git diff <sha_nr2> <sha_nr1>"
```
What changed?  Why?

24. Instead of referring to SHA IDs let's refer to HEAD and its parent commit (HEAD^):

```
git diff HEAD^ HEAD
```

Notice that we're still comparing the same two commits as before.


25. clean up the working directory
 ```
git clean -f
 ```

-----

#### Optional tasks:
26. create .gitignore file and list /static folder there:
```
echo "/static" >> .gitignore
cat .gitignore
```
27.  commit changes
``` 
git add .gitignore
git commit -m "added .gitignore with /static"
git status
```
28. create 2 new folders named 'static' and static2,  add a new file into each folder 
```
mkdir static static2
echo "some text" >> static/newfile.txt
echo "some text" >> static2/newfile.txt
ls -l static static2
```
29.  display ```git status``` and notice only static2 is listed as a folder that can be added to Index and then committed.

30.  Clean up by removing the recently added folders from working directory
```
rm -rf static static2
```


-----
### 2. Branches

0. Clone the test repository into your local folder:
```
git clone https://gitlab-sjc.cisco.com/mstanczy/git_workshop
```

Verify the below file and folder are present in your local folder:
```
drwxr-xr-x@ 9 mstanczy1  staff 288 May 25 18:17 files
-rw-r--r--  1 mstanczy1  staff  4626 May 26 14:49 jumbotron.html
```

1. Configure difftool:

On Mac/Linux:
```
git config diff.tool opendiff
git config --global difftool.prompt false
```
On Windows:
```
git config diff.tool winmerge
git config --global difftool.prompt false
```

2. add a tag to the most recent commit:
 ```
 git tag last_good_commit
 ```
 
Check if the tag got attached to the commit
```
git log
```

3. Open jumbotron.html in your favorite text editor. Open the same file also in your favorite browser.
4. Update line 80 of the code - replace '*btn-default*' with '*btn-primary*'.
5. Save the file and refresh the page in the browser. Notice the color of the button in Section 1 is different.
6. display diffs in a slightly different format 
```
git diff --word-diff
```
Changes between the commited and staged versions of the file are presented.

7. commit the changes
```
git commit -am "changed button in section 1"
 ```
Confirm the new commit was added  
```
git log
```

8. Update line 85 - replace '*btn-default*' with '*btn-info*'
9. Save the file and refresh the page in the browser. Notice the color of the button in Section 2 is different.
10. display diffs using preconfigured difftool
```
git difftool
```
Close the FileMerge / WinMerge window to get back the prompt in the Terminal.

11. commit the changes
```
git commit -am "changed button in section 2"
```
 Confirm the new commit was added
 ```
 git log
 ```
 
12. Update line 90 - replace '*btn-default*' with '*btn-warning*'

13. Save the file and refresh the page in the browser. Notice the color of the button in Section 3 is different.
14.  commit the changes and confirm the new commit was added
```
git commit -am "changed button in section 3"
git log
```

15. Display diffs using "gitk"
```
gitk
```

Close the gitk window to get back the prompt in the Terminal.

16. You realized that you were committing to the main branch instead of creating a separate branch. Rewind HEAD to the "last_good_commit"
```
git reset last_good_commit
```

17. display 'git log' and notice the last 3 commits are no longer in the history
```
git log
```
18. display ```git status``` and notice that changes to 'jumbotron.html' are now in the Working Directory
```
git status
```
19. display ```git diff``` and verify all 3 changes are included
```
git diff
```
20. Create a new branch and switch to it automatically. 
```
git checkout -b buttons
```

Display 'git log' and notice that both branches are pointing to the same commit.
```
git log
```
Issue ```git status``` to confirm our current active branch is "buttons"
```
git status
```


21. Display diffs and check which changes are currently in the working directory
```
git diff
```
 If we stage changes in a standard way they will be all added into a single commit. Let's say we want to split those changes into 3 commits, the same way it was done before. For this purpose we can selectively stage changes per hunk. This operation is called *staging patches*.

```
git add -p
```

We want to split the hunk into smaller pieces hence the first option will be "split"
```
s
```

Then we accept the new proposed hunk for staging (for Section 1) 
```
y
 ```
We then decline the next 2 proposed hunks : 
```
n
n
```

17. display diffs between the last commit and the Index: 
```
git diff --cached
```

18.  We should see only changes to Section 1 button (changed to *btn-primary*)
19. commit changes to the button in Section 1
```
git commit -m "changed button in section 1"
```
20. display the list of commits - our HEAD is now pointing to the most recent commit in "buttons" branch; the master branch is still pointing to the 'last_good_commit'
```
git log
```

21. continue with selective staging per hunk, this time we want to stage changes for Section 2 button
```
git add -p
 s
 y
 n
 ```

22. commit changes to the button in Section 2: 
```
git commit -m "changed button in section 2"
```
23. Stage and commit the remaining changes
```
 git commit -am "changed button in section 3"
 ```
24. display the history of commits. The 'buttons' branch is now 3 commits ahead of 'master'. 
``` 
git log
```
25. Display the history of commits in a more user friendly way
```
git log --oneline --graph --all
```
26. Configure a new alias for a simple reference to this view
```
git config --global alias.history “log --oneline --graph --all” 
```
From now on you can use the new alias instead the long command:
```
git history
```
26. Let's say we want to view the version of the code after one of the previous commits. Let's try to go back to the commit "changed button in section 1" - this is two commits before HEAD
```
git checkout HEAD~2  
```
This way of referencing commits allows us to navigate back in the history of commits.

Notice that git warned you that you're moving into "detached HEAD" state.

27. Issue "git log" and "git status". 
```
git log
git status
```
Notice that HEAD points to the commit you referred to but the subsequent commits and the pointer to the "buttons" branch are not displayed anymore. That's why it's called detached HEAD state. The changes you might apply at this stage will not go into "buttons" branch.

Issue "git history" to view all branches, notice that "buttons" branch still points to the most recent commit:

```
git history
```

28. Refresh the browser tab - notice that buttons for Section 2 and Section 3 went back to the original color.

29. Let's try make some experimental changes. Edit line 97 and replace "2016" with "2021". 
30. Commit changes with the below message:
```
git commit -am "updated footer to 2021"
```

31. display the list of commits and notice that HEAD points to the new commit but still the "buttons" branch isn't displayed.
```
git log
```

32. Go back to the original HEAD ("buttons" branch in our case):
```
git checkout - 
```
or
``` 
git checkout buttons
```

Before completing the operations git emits a warning about the extra commit that is not connected to any of the branches. We can still create a new branch for that single commit, for easier reference in the future, without switching to that branch. If we don't do it, this commit will remain in git database but no branch will be pointing to it. Let's create a new branch called "footer" and point it to the extra commit we just created:

```
Git branch footer <sha>   
```
where sha refers to the SHA ID for the commit called "updated footer to 2021"

33. Notice that our active branch is now "buttons". Visualize the status of all branches:
```
git history
```
or
```
gitk --all
```

34. Refresh the browser tab and confirm all buttons have updated colors.
35. Let's see how git helps restore deleted files. Remove *bootstrap.css* file from the */files* folder and confirm the file is no longer there:
```
rm files/bootstrap.css

ls files/bootstrap.css
```
36. Refresh the browser tab and notice the page formatting is now broken.
37. Issue ```git status``` and notice that git detected changes in the tracked files.
```
git status
```
38. Let's commit those changes:
```
git commit -am "deleted bootstrap.css"
```
39. Let's add a dummy commit just to move HEAD forward, to simulate a scenario where some other changes have been committed to the branch as well:
```
git commit --allow-empty –m “dummy commit”
git history
```
40. Our webpage is broken and we want to investigate what changes were made to the bootstrap.css file in the past. Let's display the git log for that specific file only:
```
git log -- files/bootstrap.css
```
41. Now we know which commit is responsible for this change. Let's call it "bad commit". Git allows to go back and see what our page looked like **before that bad commit**. We can simply checkout the specific commit - this will put us in "detached HEAD" state.
How do we know which commit we should refer to?  We can display "git log" and find the SHA ID of the commit added before the "bad commit" OR we can use a trick and refer to the "parent commit of the bad commit":
```
git checkout <bad commit>^
```
42. Refresh the browser tab. The page should look OK. At this point your local version of the project includes the deleted file. You can start a new branch from here if that fits your needs but we decide to go back to the active branch and later on simply revert the bad commit.
```
git checkout -
```

43. Let's restore the deleted file. This is possible thanks to the way git tracks all changes in its database. We can use ```git revert``` command which effectively creates a new commit with reverted changes (you will be prompted to provide the commit message)
```
git revert <bad commit>
```  
44. Check the history of commits and refresh the browser tab to confirm the formatting is now correct.
```
git history
```
-----
### 3. Merges and Conflicts

Let's merge some of our changes to the main branch.

1. switch to the target (master) branch:   
```
git checkout master
```  
Refresh the browser page, the buttons should go back to their original state.

2. in *jumbotron.html* update the line 56. Change "Sign in!" into "Login".
3. Commit changes
```
 git commit -am "Login text"
 ```

4. View the status of branches. Notice that there's one new commit on the main branch.
```
git history
```

5. merge "buttons" branch into master:  
```
git merge buttons
```

Since the target branch has it's own commit added after the "buttons" branch was created git will apply a **merge commit** and you will be prompted to provide the commit message (git uses the tool you configured as core.editor) 
If "master" branch didn't have it's own commit then git would have applied the fast forward merge (with no merge commit required).

6. display the updated graph of commits
```
git history
```

7.  Git automatically keeps the state of the repository from before each risky operation (e.g. merge) in the commit tagged as **ORIG_HEAD**. By default, the  .orig file is created with the original conflicts (as a backup). Where is this information stored? As everyting else in git it's in one of the files that constitute git database. In this case the SHA1 ID of the commit that corresponds to ORIG_HEAD is stored in .git/ORIG_HEAD text file: 

```
ls -l .git
cat .git/ORIG_HEAD
```

Let's see if we're able to revert the merge commit at this point.

> "*git reset --hard [commit]*" allows to rewind HEAD and Working Directory to a specific commit. In our case, we'll refer to the
> ORIG_HEAD commit, which is our safeguard.

```
git reset --hard ORIG_HEAD
```
7.  display the list of commits and notice that our recent merge was reverted back and we're again in the state where "buttons" and "master" branches are separated.
```
git history
```

8. Let's now try a different operation - rebase. 
```
git rebase buttons
```
9. display ```git history``` and notice that commits from buttons branch were rewinded and then re-applied on the master branch instead of creating a merge commit. 
```
git history
``` 

10. Let's try create some conflict. Update line 97 and change '2016' into '2020'. Save the updated file and commit changes into "master" 
```
git commit -am "updated footer to 2020"
```
11. Now, let's try merge the 'footer' branch into 'master'
```
 git merge footer
 ```
 
 Notice git is reporting a conflict that couldn't be resolved automatically and will have to be fixed manually.

12. issue "git status" to see which file has a conflict. 


> An ongoing merge that results in conflicts can be cancelled with "*git merge --abort*". In our case we will proceed with resolving the conflict instead of aborting the merge.

14. issue "git diff" to see which lines are conflicting
```
git diff
```
 As expected, when trying to merge 'master' and 'footer' branches we hit a scenario where the same line differs between two branches and git cannot decide which version is correct. 
 
HEAD commit (on **master** branch) includes '2020' whereas the most recent commit on **footer** branch includes '2021'. 

```
<<<<<<< HEAD
        <p>© 2020 Company, Inc.</p>
=======
        <p>© 2021 Company, Inc.</p>
>>>>>>> footer
```

You as the project admin have to make a call and remove the bad line as well as separators added by git. You decided that "© 2020 Company, Inc." is the correct one.

14. Edit the jumbotron.html file and remove line 97, 99, 100 ad 101:
```
97  <<<<<<< HEAD
98         <p>© 2020 Company, Inc.</p>
99  =======
100        <p>© 2021 Company, Inc.</p>
101 >>>>>>> footer
```
15. save the changes and issue commit 
```
commit -am "resolved conflict in the footer"
```

16. display the history of commits and notice that the branches have been merged and HEAD now points to the merge commit.
```
git history
```

17. branch 'footer' is now obsolete and we can delete it. Effectively, we will just remove a pointer without affecting any commits.

```
git branch -d footer
```
Display ```git history``` and notice all the commits are still there, just the "footer" branch isn't listed anymore.


-----
### 4. Remote Repository

*Origin* is the default name of the remote repo when you do *git clone*, the same way as  *master* is the default name of the main branch

1. display the currently known remote repos
```
 git remote -v
```
2. Configure a new remote repo on your gitlab account
```
git remote add newrepo https://gitlab-sjc.cisco.com/<username>/newrepo
```

3. display the list of remote repos
```
 git remote -v
```

4. The newly configured remote repository is not linked to any local repository yet. In order to be able to push local changes to the remote repo such association (tracking branch) needs to be created. 
The local branch will be tracking the remote branch on the remote server. The tracking branch is also referred to as upstream branch - that's why we use -u option (upstream) in git command:
```
git push newrepo -u master
```
Branch 'master' is now set up to track remote branch 'master' from 'newrepo'.

Go to your gitlab account and notice that a new project has been created automatically.

5. display the list of branches (local and remote):
```
git branch -vv
```

6. display "git log" or "git history" and notice that the new remote branch is now listed in the graph.
```
git history
```

7. Refresh the gitlab page. Verify if new files were added to the newrepo project and all commits are present.

8. Let's simulate a scenario where another developer commits changes to the same branch on the remote repo.
**On the gitlab account** edit the *jumbotron.html* file (pay attention to do it on the remote repo).

 
In line 65 change "Hello, world!" into "Hello, Programmability Makers!".  
Add a commit message "*Hello message updated on remote branch*". Commit changes.

9. Display the list of commits in gitlab project

10. In the terminal issue "git log" and notice the local repo doesn't have the latest changes.
```
git log
```

11. Issue "git pull" to download the latest version of the code from the remote branch.
```
git pull
```

12.  Notice that the changes were automatically merged into your working directory. Confirm the local branch has the new commit and the local version of jumbotron page was updated
```
git log
```

13. Make another edit on the remote branch (on gitlab account). Edit line 65 and change "Hello, Programmability Makers!" into "Welcome".  Add a commit message "*Updated welcome message*". Commit changes.

14. This time instead of pulling changes we will use **fetch** command.
```
git fetch
```

15. Notice this time we downloaded the objects and refs from the remote repo to a local branch but didn't merge those changes into the working directory.
```
git history
```

16. Since we fetched the updated version from the remote repo we can review the diffs before merging them into the local branch. 
*HEAD* points to the most recent commit on the currently active local branch and we can use this pointer instead of the commit SHA ID.
*newrepo/master* refers to the most recent commit on the remote *master* branch on *newrepo* repository.
```
git diff HEAD newrepo/master
```
18. Once we're comfortable with all changes we can incorporate them into the local branch and working directory:  
```
git merge 
```
If any conflict occurs during this merge we will be able to resolve it locally and when we're done we can push the updates to the remote repo.
