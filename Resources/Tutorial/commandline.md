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
