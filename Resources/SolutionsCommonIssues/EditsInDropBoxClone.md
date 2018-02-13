# Manual edits to the code in the DropBox folders
While the intended work flow is that edits to the code should only be done in the non-synced folder and pushed to the DropBox folder through the GitHub cloud, it is easy to imagine that it could happen some time. To test whether there has been any edits been done in the DropBox folder to the code, you can type `git status` in Git Bash.

If there are no edits made to the code you will get an output similar to the output below.  The important part is that it is saying _**nothing to commit**_.

```
On branch master
Your branch is up-to-date with 'origin/master'.

nothing to commit, working tree clean
```

If changes were made you will get an output similar to below. The important part is _**Changes not staged for commit**_ but the rest of the output is also helpful indicating which files are edited and suggests two solutions.

```
On branch master
Your branch is up-to-date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   <file path and name>

no changes added to commit (use "git add" and/or "git commit -a")
```
Which solution you want depends on what you want to do and is described in the following sections.

## Discard all edits in the DropBox folder
This can be done if you accidentally made a few changes here, and prefer to do manually transfer the edits to the non-synced clone instead, and add them to GitHub using the intended work flow. This will also be the desired solution if the edits was a complete mistake

```
git checkout -- <file path and name>
```
Test `git status` again to make sure that there are no more unresolved edits in the DropBox folder.

## Commit edits in the DropBox folder
This should be done if you want to keep the edits done, and it is unfeasible or impractical to manually transfer the edits to the non-synced folder and commit them from there, which is the preferable solution, see above.

To commit an edit from the console to the repository in the cloud, you should start by telling Git Bash you want to commit these changes. You do that with the following code that should be repeated for each file you want to commit. You can use wild cards in file names to include multiple files, i.e. write `file*.do` to include both `file1.do` and `file2.do` from that folder. Note that you should only add the files you want to include in the same commit here. The GitHub Desktop equivalence to this step is to tick/untick files in the list of changed files. IF you want to split up the edits in multiple commits, then do the next step before adding more files.

```
git add <file path and name>
```
The next step is to commit the changes. It will take all files currently selected with `git add` but not yet committed. You always have to add a commit message, so update the string in the code below before using this. This is the same message you write in GitHub Desktop. Not that the last step is required to send anything to the cloud.

```
git commit -m "Commit message"
```

So far nothing have been sent to the repository in the cloud. To do so you have to `git push` your commits. You can _push_ multiple commits at the same time, but it is perfectly fine to push one commit at the time. To push the commits you have made, simply use the code below:

```
git push
```

If it was successful you should see an output similar to

```
Delta compression using up to 4 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 728 bytes | 728.00 KiB/s, done.
Total 7 (delta 3), reused 0 (delta 0)
remote: Resolving deltas: 100% (3/3), completed with 3 local objects.
To https://github.com/kbjarkefur/DropBoxGitHub.git
   62276bf..2329d3a  master -> master
```

Test `git status` again to make sure that there are no more unresolved edits in the DropBox folder.
