# Tutorial on how to combine GitHub and DropBox

### Introduction

For good or for bad, in research we often want to use both DropBox and GitHub. Usually the DropBox folder include many folders in addition to the data folder - the folder with files relevant to GitHub. The other folders have files like budget excel sheets, concept notes, contracts with survey firms etc.

The reason this is an issue is that both DropBox and GitHub are services that syncs files, although in very different ways. If one person makes an edit in a file synced both by DropBox and GitHub, then DropBox will sync that file immediately, and GitHub Desktop on any other user's computer will think that this edit was done by that user as well, and all project team members are suggested by their GitHub Desktop to commit and sync this edit regardless of which user actually made the edit. This will lead to a lot of conflicts in the repository. While conflicts can be solved, an even bigger issue is that two users cannot work on different branches at the same time if the folder is shared using both DropBox and GitHub, since if one user change branch, then DropBox will change the folder to that branch for all users.

##### Simplest but often not most preferred solution
The simplest solution from a strict technical perspective would be to keep the data work folder separate from the DropBox folder. This tutorial will suggest something different since while it would be simple from a technical perspective, it is probably a solution that will be confusing to many project members, especially those not familiar with GitHub. So while technically simple, most project team wants all files related to the project in one folder.

##### Combining GitHub and DropBox in the same folder
This is the solution described in this tutorial. The repository folder with all data work will be in the DropBox folder, **and** for each team member using GitHub the data work folder will **also** be in a separate non-synced folder. In the non-synced folder you clone a copy of the repository using GitHub Desktop. In this clone you work with the code, make commits, and push those commits to the cloud just like normal.

In the data folder in the DropBox folder you create a second clone of the repository manually using the command line. You can not use GitHub Desktop to create a second clone on your computer. We understand that using GitHub from the command line might be intimidating for some people in our field, but that is what this tutorial will carefully and in much detail explain how to do.

If possible, it should be avoided that anyone makes edits to the files in the clone in the DropBox folder as committing edits from a clone in a shared folder is prone to conflicts. It is by all means possible, but our experience is that that work flow should be avoided and all commits to the repository should come from the non-synced clones and then be downloaded the clone in the DropBox folder. We will still explain how to do that below in case someone not familiar with the work flow makes changes in the DropBox folder and you would like to include those edits in the repository.

### Warnings

This section list warnings or drawbacks of this solution. In most cases these warnings do not matter, but please read them before implementing this solution.

* It will be possible to re-create all branches and all history of all branches of the repository using information stored in the .git folder (usually hidden) that is created when you create the clone. So anyone who has access to the DropBox folder with the clone will have access to the whole history of the repository, not just the files that is currently shown in the DropBox folder. It is not straightforward how to access this information (but perfectly possible), and in most projects all members allowed access to the DropBox folder also have access to the GitHub repository, so it usually not an issue, but keep this in mind when publishing your work.

* Binary files (images, pdf, all Microsoft office files etc.) whose history is inefficiently stored by GitHub risk making the DropBox folder very large if those files are edited frequently. This could slow down the sync, especially on a slow connection. The solution to this is to not save binary files in the repository -- more on that below.

### Requirements

This method requires that you use the command line (the default command line interface in Windows is called the _Command Prompt_ and it is called _Terminal_ on a Mac). We understand that many people in econ research may not have a lot of experience using the command line, but we will explain all steps needed in detail.

Unless you are already experienced in using the command line and have your own favorite console, we recommend that you use Git Bash instead of any of the default command line mentioned above. If you use any other command line than Git Bash you will have to install _git_ in that console. Git Bash comes with _git_ that we will need and that's why we recommend it. Each time we say Git Bash in this tutorial you could technically use any command line interface of your choice.

You can download Git Bash [here](https://git-scm.com/downloads). Follow the instructions in the installer and accept the default values of all the options. If you run in to any problems when installing Git Bash, Google the error code and you are likely to find a response. Please open up an issue to this repository if you find some instructions that you think others will benefit from and that you are willing to share. You can also fork this repository and submit a pull-request with those instructions.

To check that _git_ is properly installed in the command line inteface you intend to use is correctly set up for what we will do in this tutorial, enter `git --version` in your console and if you get an output on any of the formats you are good to go!

```
git version 2.14.2.windows.2
```
If you are using a Mac or a Linux computer you result will look different but more or less similar. If you get the answer `bash: git: command not found` your installation of _git_ was not successful.

##### Advice using Git bash
* You change folder in the command line using the command `cd`. For example, `cd "C:\Program Files"` changes the working directory to the folder _C:\Program Files_.
* Git Bash uses both relative and absolute file paths. That means if you are already in folder `"C:/Users/Researcher"` and want to do to `"C:/Users/Researcher/DropBox"` you can either type the full file path `cd "C:/Users/Researcher/DropBox"` or just the relative file path `cd "DropBox"` as you are already in the `Researcher` folder, and you only need to enter the file path relative to the folder that you are in. Relative file paths are important as Git Bash almost only use relative file paths in its output.
* Git Bash requires you to use forward slashes. So `"/Dropbox/ProjectFolder/RepositoryName"` works but `"\Dropbox\ProjectFolder\RepositoryName"` does not.
* File paths must be enclosed in quotation marks if there is a space in any of the folders or file names. It is good practice to always do so.
* To paste something in Git Bash, use `ctrl+shift` instead of `ctrl+C`. You can also right click.
* the `~` is a short hand for your user folder on your computer. As in `"C:/Users/Researcher"` in Windows or `"/Users/Researcher"` on a Mac where _Researcher_ in both cases is replaced with the user name on your computer. Test this by typing `cd ~` which change your working directory to the user folder, and afterwards type `pwd` to display your user folder.

# Initial Setup
After you have your console working and have _git_ installed on it (see the [requirements](#requirements) section above if you have note done this), you can start following these steps to set up your non-synced repository clone and your DropBox Folder Clone.

This part you only need to do once for each project. Skip to [update DropBox](#update-dropbox-clone) if you have already cloned your repository in the DropBox folder and you only want to download new changes made in or committed to the repository at GitHub.com.

### Set up GitHub Repo and DropBox folder
This section asks you to set up a GitHub repository and a DropBox folder in a completely normal way. The only important part is that you do not clone your repository to anywhere in the DropBox folder yet. Clone it using GitHub Desktop to a non-synced folder.

##### GitHub Setup - Needed to be done once per project
1. Create a new repository on GitHub.com
  * You can use an already existing repo, but this tutorial starts with a new repository.
  * In the rest of this tutorial we will assume that the repository was named _DropBoxGitHub_, but you can name it anything. Just change the name to your name where you see _DropBoxGitHub_.
1. Use a .gitignore file that ignores everything but code files. Use for example DIME's [.gitignore template](https://github.com/worldbank/DIMEwiki/blob/master/Topics/GitHub/gitignore_template.txt) that is developed to suit what researchers in economics usually needs.

##### GitHub Clone Setup - Needed to be done once per team member contributing to the code
Clone the repository to you computer using GitHub Desktop. Do **NOT** clone your repo to a folder in your DropBox folder (this is done later). Clone it to your Documents folder or any other non-synced folder.

##### DropBox Folder Setup - Needed to be done once per project
1. Create a folder in your DropBox.
1. Invite your the rest of the project team to the DropBox folder

### Create a second local clone in DropBox Folder
This should only be done once per project. The DropBox clone will be synced by DropBox to all team members sharing the DropBox folder. When the repository was cloned to a non-synced folder above, you did so using GitHub Desktop. In the next steps we will have to use the command line to create a second clone of the repo in the DropBox folder.

##### Navigate to the DropBox folder for the clone
Prepare a location for the cloned repository in the DropBox folder you created above. Note that a folder with the same name as the repository will be created where all the content of the repository will be stored. You may rename this folder once it is created.

Open Git Bash and navigate to the location in tje DropBox folder you prepared. Do this using the command _cd_ followed by the file path to where in the DropBox Folder you want to create the Data Folder. Remember that in Git Bash you must use forward slashes `/`. Like this:

```
cd "C:/Users/Researcher/Dropbox/ProjectFolder"
```

In most command line interfaces you can see where you currently is navigated. In Git Bash you can see it on the line above where you enter your command. See the example below. Remmber that `~` is a short hand to the user folder `"C:/Users/Researcher"` used by Git Bash.

```
username@computername ~/Dropbox/ProjectFolder
$
```
In the Command Prompt (the default command line in Windows) the current folder is shown on the same line that you enter your command on.

```
C:/Users/Researcher/Dropbox/ProjectFolder>
```

When you are in the location where you want to clone the repository, move on to the next step.

##### Clone the repository
Now when we are in the DropBox folder where we want to clone the repository you first need to go and copy the address to the repository. Do not use the regular URL to the repository, you need a special URL for _Cloning with HTTPS_. To find this, go to the main page of the repository on GitHub.com in the browser. Click the green button that says **Clone or Download**. Here you want to get the link for **Clone with HTTPS**. Copy the _Cloning with HTTPS_ link. If it says _Clone with SSH_ click the link where it says _Use HTTPS_. It is possible to also use SSH but we will not cover that here.

Use the code below to clone the repository into your DropBox folder, but replace the URL in the code below with your _Cloning with HTTPS_ URL. The URL in the example below works and you can test on this repo, but you will not be able to submit any edits to that repository. This will create a folder with the same name as the repository, but you can change the name of this folder once this command is done. It may take a few moments if the repository is big.
```
git clone https://github.com/kbjarkefur/DropBoxGitHub.git
```
You will be asked to enter your GitHub username and password (unless they are already cached on your computer, see end of paragraph). Each time you are using a git command where GitHub needs to know who you are it will ask you for your username and password. `git clone` needs to know who you are as if you are not already a collaborator on the repository you are cloning, GitHub must first create a fork for you. Many commands require your credentials, so to get around this there are many ways to save your username and password in git, see for example  [here](https://www.tilcode.com/push-github-without-entering-username-password-windows-git-bash/).

If the command ran successful you should get an output similar to this if it was successful:

```
Cloning into 'DropBoxGitHub'...
remote: Counting objects: 42, done.
remote: Compressing objects: 100% (31/31), done.
remote: Total 42 (delta 7), reused 38 (delta 6), pack-reused 0
Unpacking objects: 100% (42/42), done.
```

See how `git clone` created a new folder, with all the content of the repository in it. You now have a second clone on your computer in your DropBox to which you can download updates to the repository directly from the cloud (see next section) so that team members that use only DropBox and not GitHub, can still navigate the code. Remember that the recommended work flow is to never work on the code directly in the DropBox folder. Always use your other local clone, push updates to the cloud and then pull the new edits to the DropBox folder as described in the next section. This means that everyone that work on the code should be using GitHub.

## Update DropBox Clone
Make sure that you or someone in your team have already done the steps in the [initial setup](#initial-setup) before doing this step.

Each time you want to update the DropBox folder, start by navigating to the folder in Git Bash. Note that we are not navigating into the same folder as when we cloned the repository. We want to be in the folder created when we cloned the repository.
```
cd "C:/Users/Researcher/Dropbox/ProjectFolder/DropBoxGitHub"
```

When you have navigated into the repository folder, use the command `git pull`. It will copy all updates in the repository in the cloud to your DropBox folder. Make a commit to your repository in the cloud and then run this command and see how that change is now reflected in the files in your DropBox folder.
```
git pull
```

Note that `git pull` only updates the branch you are currently in. There are tools that pull all branches but they sometimes, but they sometimes do more than what you want, so unless you know them well, pull one branch at the time. And since the intended work flow is to not work on the code in the DropBox folder, it is mostly only the master branch that is needed to be up to date.  See the next section for details on branches.

##### Note on branches
If you do not see the updates that you just pulled into you DropBox Folder, you want to make sure that you are in the right branch of the repository. In Git Bash the current branch is always listed after the current working directory. See example below.

```
username@computername ~/Dropbox/ProjectFolder/DropBoxGitHub (master)
$
```

If you are not in the branch where you want to be you can switch to a different branch using this code, but replace _<branchname>_ with the name of the branch you want to switch to.

```
git checkout <branchname>
```

If you are checking out a branch other than the master branch, remember that it changes the content in the DropBox folder to everyone. So if you need to explore a branch using the DropBox clone, remember to switch back to the master branch when you are done.

Note that you cannot change branch if you have made changes to the files in DropBox without committing them. If you get an error similar to the output below, then that means that you have un-committed changes made to the files in the DropBox folder. See [here](#manual-edits-to-the-code-in-the-fropbox-folders) for suggested solutions.

```
error: Your local changes to the following files would be overwritten by checkout:
        <file path and name>
Please commit your changes or stash them before you switch branches.
Aborting
```

## Best Practices

### Store data in DropBox
Store data in the DropBox Folder and not in the the non-synced folder and not in the cloud.

## Solutions to special scenarios

This section helps you with solutions to errors using the console, and to when team members did not follow the work flow this set up require.

### Manual edits to the code in the DropBox folders
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

#### Discard all edits in the DropBox folder
This can be done if you accidentally made a few changes here, and prefer to do manually transfer the edits to the non-synced clone instead, and add them to GitHub using the intended work flow. This will also be the desired solution if the edits was a complete mistake

```
git checkout -- <file path and name>
```
Test `git status` again to make sure that there are no more unresolved edits in the DropBox folder.

#### Commit edits in the DropBox folder
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

### Cloning someone else's repository
You can always clone someone else's repository if you can see it on GitHub.com. This includes all public repositories and private repositories where you have been added as a collaborator. You can clone these using both Git Bash or GitHub Desktop, and you can use either of those tools to keep pulling updates made to those repositories.

However, you can only push to them if you have been added as a collaborator and have been given writing privileges. Not that you may have writing privileges but the branch you are trying to push to might be locked for edits so that only the owner can edit that branch. You can sill go through all steps of adding and committing files, but when you try to push to the cloud you will get an error.

If you have made changes in your local clone that you do not have write privileges, but you still want your edits to be included, here are some suggested solutions.

#### Be added as a collaborator to the repository
If you know the owner of the repository, the by far easiest solution is to ask the owner if you can be added as a collaborator to the repository.

#### If you can't be added as a collaborator, fork the repository
Your best option if you cannot be added as a collaborator but still want to submit your edits to the original repository is to create a fork. Start by forking the original repository on GitHub.com. Since you created this fork and are the owner of the fork, you will have writing privileges to it. While you now have writing privileges to your fork, your local clone still points towards the original repository. The easiest solution is to create second local clone, transfer the changes that you did in the first clone to the second clone and push from there to your clone. When you have pushed your changes to your fork, then you can create a pull request to the original repository suggesting the edits that you did.

There are complicated and technical solutions to how you can change your local clone be associated with your fork and not the original repository so you do not need to manually transfer your changes, but that is outside of the scope of this tutorial.
