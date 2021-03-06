# Tutorial on how to combine GitHub and DropBox

#### Contributions to the tutorial is very welcomed!
We greatly appreciate any type of feedback or contribution to this repository. Either open an [issue](https://github.com/kbjarkefur/GitHubDropBox/issues) if you have a comment/suggestion/bug you want to draw our attention to, or create a fork and submit a pull request if you want to edit language, improve how something is explained, or add to the common issue solutions or best practices in the [Resources](Resources) folder. See [CONTRIBUTING](CONTRIBUTING.md) for more details.

### Introduction
For good or for bad, in research we often want to use both DropBox and GitHub. Usually the DropBox folder include many folders in addition to the data folder - the folder with files relevant to GitHub. The other folders have files like budget excel sheets, concept notes, contracts with survey firms etc. This tutorial is tested on DropBox but should work just as well on Box, OneDrive or any other syncing service.

The reason this is an issue is that both DropBox and GitHub are services that syncs files, although in very different ways. If one person makes an edit in a file synced both by DropBox and GitHub, then DropBox will sync that file immediately, and GitHub Desktop on any other user's computer will think that this edit was done by that user as well, and all users are then asked by their GitHub Desktop to commit and sync this edit regardless of which user actually made the edit. This will lead to a lot of conflicts in the repository. While conflicts can be solved, an even bigger issue is that two users cannot work on different branches at the same time if they work from a repository shared using DropBox, since if one user change branch, then DropBox will change the folder to that branch for all users.

##### Technically simplest but often not a preferred solution
The simplest solution from a strict technical perspective would be to keep the data work repository separated from the DropBox folder. This is usually not an acceptable solution to most research project teams as there will be researchers on the project that does not know GitHub and needs to be able to access the latest version of the data work files.

##### The solution recommended in this tutorial
The set-up in this tutorial requires everyone who contributes to the code using GitHub to set up a clone of the repository in a local folder on their computer that is **not** the DropBox folder. One person in the team then follows the instructions in this tutorial and sets up a second local clone in the DropBox folder. DropBox will then sync this clone to everyone who is on that DropBox folder.

Then team members make commits to their non-synced clone and push them to GitHub.com just like in any other GitHub collaboration. Then, at any time of your choice, you can download any new changes in the repository in GitHub.com to the DropBox clone. Each time new changes are downloaded to the DropBox clone, DropBox share them with everyone in the DropBox folder.

The only catch is that GitHub Desktop can only work with one local clone of each repository at the time, so you will have to use the command line to manage the DropBox clone. We know that not everyone is used to working with the command line but providing easy to follow instructions to that is exactly what this tutorial is all about. We have boiled it down so in addition to navigate folders using the `cd` command, you only need to use one command line command `git clone` when setting up your second clone, and one command line command `git pull` when you download new edits to the repository. And all of this will be explained in great detail.

##### Other Solutions
Git offers several other ways to do this. We have seen tutorials describing ways to push directly to the DropBox clone from the other clone, use webhooks to automatize some steps, etc., but we recommend the method described in this tutorial as it is the least complicated method that is still relatively easy to set up using the command line.

### Warnings

This section list warnings or drawbacks of this solution. In most cases these warnings do not matter, but please read them before implementing this solution.

* It will be possible to re-create all branches and all history of all branches of the repository using information stored in the .git folder (usually hidden) that is created when you create the clone. So anyone who has access to the DropBox folder with the clone will have access to the whole history of the repository, not just the files that is currently shown in the DropBox folder. It is not straightforward how to access this information (but perfectly possible), and in most projects all members allowed access to the DropBox folder also have access to the GitHub repository, so it usually not an issue, but keep this in mind when publishing your work.

* Binary files (images, pdf, all Microsoft office files etc.) whose history is inefficiently stored by GitHub risk making the DropBox folder very large if those files are edited frequently. This could slow down the sync, especially on a slow connection. The solution to this is to not save binary files in the repository -- more on that [here](Resources/BestPractices/BinaryFiles.md).

### Requirements

This method requires that you use the command line (the default command line interface in Windows is called the _Command Prompt_ and it is called _Terminal_ on a Mac). We understand that many people in econ research may not have a lot of experience using the command line, but we will explain all steps needed in detail.

Unless you are already experienced in using the command line and have your own favorite console, we recommend that you use Git Bash instead of any of the default command line mentioned above. If you use any other command line than Git Bash you will have to install _git_ on that console. Git Bash comes with _git_ that we will need and that's why we recommend it. Each time we say Git Bash in this tutorial you could technically use any command line interface of your choice.

You can download Git Bash [here](https://git-scm.com/downloads). Follow the instructions in the installer and accept the default values of all the options. If you run in to any problems when installing Git Bash, Google the error code and you are likely to find a response. Please open up an issue to this repository if you find some instructions that you think others will benefit from and that you are willing to share and we might add it to the [Resources](Resources) folder. You may also fork this repository and submit a pull-request with those addition to the Resources folder yourself.

To check that _git_ is properly installed in the command line interface you intend to use is correctly set up for what you will do in this tutorial, enter `git --version` in your console and if you get an output on any of the formats you are good to go!

```
git version 2.14.2.windows.2
```
If you are using a Mac or a Linux computer you result will look different but more or less similar. If you get the answer `bash: git: command not found`, or something like that, your installation of _git_ was not successful.

##### Advice using Git bash
* You change folder in the command line using the command `cd`. For example, `cd "C:/Program Files"` changes the working directory to the folder _C:/Program Files_.
* Git Bash uses both relative and absolute file paths. That means if you are already in folder _C:/Users/Researcher_ and want to go to _C:/Users/Researcher/DropBox_ you can either type the full file path `cd "C:/Users/Researcher/DropBox"` or just the relative file path `cd "DropBox"` as you are already in the _C:/Users/ResearcherResearcher_ folder, and you only need to enter the file path relative to the folder that you are in. Relative file paths are important as Git Bash almost only use relative file paths in its output.
* Git Bash requires you to use forward slashes. So `cd "/Dropbox/ProjectFolder/RepositoryName"` works but `cd "\Dropbox\ProjectFolder\RepositoryName"` does not.
* File paths must be enclosed in quotation marks if there is a space in any of the folders or file names. It is good practice to always do so.
* To paste something in Git Bash, use `shift+insert` instead of `ctrl+v`. You can also right click and select paste.
* The `~` (tilde) is a short hand for your user folder on your computer. As in _C:/Users/Researcher_ in Windows or _/Users/Researcher_ on a Mac where _Researcher_ in both cases is replaced with the user name you are logged in as on your computer. Test this by typing `cd ~` which change your working directory to the user folder, and afterwards type `pwd` to display your user folder.

# Initial Set-Up
After you have your console working and have _git_ installed on it (see the [requirements](#requirements) section above if you have note done this), you can start following these steps to set up your non-synced repository clone and your DropBox Folder Clone.

This part you only need to do once for each project. Skip to [update DropBox](#update-dropbox-clone) if you have already cloned your repository in the DropBox folder and you only want to download new changes made in or committed to the repository at GitHub.com.

### Create GitHub Repo and DropBox folder
Start by creating a GitHub repository and a DropBox folder for your project. You may use a GitHub repository and a DropBox folder that you already have, but the content of the repository may not already be in the DropBox folder. In this tutorial the repository will be called _DropBoxGitHub_ and the DropBox project folder will be called _ProjectFolder_ and its local location is _C:/Users/Researcher/Dropbox/ProjectFolder_. But you can call your repository or project folder anything you want, and your DropBox fodler might be in a different location.

We strongly recommend you to use a .gitignore file that ignores everything but code files. Use for example World Bank DIME's [.gitignore template](https://github.com/worldbank/DIMEwiki/blob/master/Topics/GitHub/gitignore_template.txt) that is developed to suit what researchers in economics usually needs.

# Create the clones

### Create a Non-synced clone
This should be done by each team member that will contribute to the code. Clone the repository to your computer using GitHub Desktop. Do **NOT** clone the repo to a folder in your DropBox folder (a second clone will be created in the DropBox folder next). Clone the repository to, for example, _C:/Users/Researcher/Documents/GitHub_.

### Create a Synced Clone in the DropBox folder
The DropBox clone will be synced by DropBox to all team members sharing the DropBox folder so this should only one project team member needs to do this. GitHub Desktop can not be used to create a second clone in a different location. Therefore, in the next steps we will have to use the command line to create a second clone of the repo in the DropBox folder.

##### Navigate to the DropBox folder for the clone
Start by preparing a location for the cloned repository in the DropBox folder you created above. Note that a folder with the same name as the repository will be created in the location that you choose, and all the content of the repository will be cloned into that folder. You may rename this folder once it is created.

Open Git Bash and navigate to the location in the DropBox folder you prepared. Do this using the command _cd_ followed by the file path to where in the DropBox Folder you want to create the Data Folder. Remember that in Git Bash you must use forward slashes _/_. Like this:

```
cd "C:/Users/Researcher/Dropbox/ProjectFolder"
```

In most command line interfaces you can see where you currently is navigated. In Git Bash you can see it on the line above where you enter your command. See the example below. Remember that _~_ is a short hand to the user folder (in this case _C:/Users/Researcher_) used by Git Bash.

```
username@computername ~/Dropbox/ProjectFolder
$
```
In the Command Prompt (the default command line in Windows) the current folder is shown on the same line that you enter your command on.

```
C:/Users/Researcher/Dropbox/ProjectFolder>
```

When you are in the location where you want to clone the repository, move on to the next step.

##### Clone the repository to DropBox
Now when you have navigated to the location in the DropBox folder where you want to clone the repository you first need the _Clone with HTTPS_ URL for this repository. You find this URL on GitHub.com and note this is not the same as the regular URL to the repository. To get the _Clone with HTTPS_ URL, go to the main page of the repository on GitHub.com in the browser. For this repository that would be [this page](https://github.com/kbjarkefur/GitHubDropBox). Click the green button that says **Clone or Download**. Here you want to get the link for **Clone with HTTPS**. Copy the _Clone with HTTPS_ link. If it says _Clone with SSH_ click the link where it says _Use HTTPS_ and it will change to the one we need here. It is perfectly possible to also use SSH but how to do that is not covered in this tutorial.

Use the code below to clone the repository into your DropBox folder, but replace the URL in the code below with your _Cloning with HTTPS_ URL. The URL in the example below works (it is this repo) and feel free to test with it. The access settings on this repository prevents you to make any changes to the repository, so feel free to experiment however you like. The code below will create a folder with the same name as the repository, but you can change the name of this folder once this command is done. It may take a few moments if the repository is big or has a long edit history.
```
git clone https://github.com/kbjarkefur/DropBoxGitHub.git
```
If it is a private repository you are cloning, you will be asked to enter your GitHub username and password unless they are already chached on your computer (either done manually or done by GitHub Desktop). Many _git_ commands require your credentials, so to get around the requirement to add your credentials each time see for example [here](https://www.tilcode.com/push-github-without-entering-username-password-windows-git-bash/).

If the command ran successful you should get an output similar to this if it was successful:

```
Cloning into 'DropBoxGitHub'...
remote: Counting objects: 42, done.
remote: Compressing objects: 100% (31/31), done.
remote: Total 42 (delta 7), reused 38 (delta 6), pack-reused 0
Unpacking objects: 100% (42/42), done.
```

See on your computer how `git clone` created a new folder, with all the content of the repository in it. You now have a second clone on your computer in your DropBox to which you can download updates to the repository directly from the cloud (see next section) so that team members that use only DropBox and not GitHub, can still access the code. Remember that the recommended work flow is to not work on the code directly in the DropBox folder. Always work in the clone in a non-synced folder, push updates to the cloud from there and then pull the new edits to the DropBox folder as described in the next section. This means that everyone that work on the code should be using GitHub. It is possible to include edits made directly in the DropBox folder. This is explained [here](Resources/SolutionsCommonIssues/EditsInDropBoxClone.md) but it is not a recommended work flow as it is prone to conflicts and errors.

# Update the DropBox Clone
Make sure that you or someone in your team have already done the steps in the [initial setup](#initial-setup) section before doing this step. Note that it does not matter who did the set-up initially, anyone with access to the clone in the DropBox folder and git installed can do the steps described here.

Each time you want to update the DropBox folder, start by navigating to the folder in Git Bash. Note that we are not navigating into the same folder as when we cloned the repository. We want to be in the folder created when we cloned the repository, i.e. _Dropbox/ProjectFolder/DropBoxGitHub_ instead of _Dropbox/ProjectFolder_.
```
cd "C:/Users/Researcher/Dropbox/ProjectFolder/DropBoxGitHub"
```
When you have navigated into the repository folder, use the command `git pull`. It will copy all updates in the repository in the cloud to your DropBox folder. There will be nothing to update if you have not made any edits to the repository in the cloud since you first created this clone.
```
git pull
```
This is all you have to do in order to sync any edits to the repository on GitHub.com to your clone in the DropBox folder. Once you have pulled the new edits to your DropBox folder, DropBox will then sync these new edits, just like it does with any type of file, to everyone else's DropBox. Note that `git pull` follows the ignore rules for this repository, and will not download any files ignored by gitignore in this repository.

Note that `git pull` only updates the branch you are currently in. There are tools that pull all branches but they sometimes do more than what you want, so unless you know them well, pull one branch at the time. And since the intended work flow is to not work on the code in the DropBox folder, it is mostly only the master branch that is needed to be up to date. See the next section for details on branches.

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

**Warning:** Note that if you change branch, git will change the files in the folder so that they reflect the files in that branch. DropBox will interpret this as if the files have changed and will then sync those changes to everyone. So if you change branch in the DropBox clone you change the branch in everyone's DropBox folder, and the repository does not change back to the master branch by itself, this has to be done manually. This will be very confusing to anyone accessing the files using DropBox, so it is probably best to avoid changing branches in the DropBox clone unless necessary.

Note that you cannot change branch if you have made changes to the files in DropBox without committing them. If you get an error similar to the output below, then that means that you have un-committed changes made to the files in the DropBox folder. See [here](Resources/SolutionsCommonIssues/EditsInDropBoxClone.md) for suggested solutions.

```
error: Your local changes to the following files would be overwritten by checkout:
        <file path and name>
Please commit your changes or stash them before you switch branches.
Aborting
```

# Other resources in this repository

* [Best Practices](Resources/BestPractices/README.md)
* [Solutions to Common Issues](Resources/SolutionsCommonIssues/README.md)
