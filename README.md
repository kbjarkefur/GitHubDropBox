# Tutorial on how to combine GitHub and DropBox in a Research Project

Link to the [Full DropBox integration Tutorial](Resources/Tutorial/README.md)

Sections on this page:
* [Introduction and motivation](#introduction-and-motivation)
* [Three degrees of DropBox integration](#three-degrees-of-dropbox-integration-in-a-research-project-hosted-in-github)
* [Other resources in this repository](#other-resources-in-this-repository)

#### Contributions to the tutorial is very welcomed!
We greatly appreciate any type of feedback or contribution to this repository. If you only have a quick comment/question etc. or you are not familiar with GitHub, then go to [issues](https://github.com/kbjarkefur/GitHubDropBox/issues). Otherwise, see [CONTRIBUTING](CONTRIBUTING.md) for more details.

## Introduction and motivation
GitHub was developed by computer scientists with their use case in mind. There is a lot of overlap between that use case and the use case of a quantitative research project today, and that is why GitHub is an excellent tool for researchers. However, there is one big difference. Typically, computer scientists share code that you apply to different data, but in research we typically share code and a specific data set that that code should be applied to. GitHub is great for sharing and collaborating on code as that is what it was developed for, but it is not a great place to share data. The purpose of this tutorial is how to use a syncing service like DropBox to share data at the same time you can benefit from the features of GitHub when collaborating on code. While this tutorial will refer to DropBox and GitHub it will most likely work very similarly when using alternatives to those to services.

The reason why this is not straightforward and needs a tutorial is that both DropBox and GitHub are services that syncs files, although in very different ways. For example, if one person makes an edit in a file synced both by DropBox and GitHub, then DropBox will sync that file immediately, and GitHub Desktop on any other user's computer will think that this edit was done by that user as well, and all users are then asked by their GitHub Desktop to commit and sync this edit regardless of which user actually made the edit. This will lead to a lot of conflicts in the repository. While conflicts can be solved, an even bigger issue is that two users cannot work on different branches at the same time if they work from a repository shared using DropBox, since if one user checkout a different branch, then DropBox will change the files in the folder to that branch for all users.

## Three degrees of DropBox integration in a Research Project hosted in GitHub

### No DropBox integration
The data is not shared in the project folder, it is either downloaded by the code or copied into the folder manually by each research member. This is not applicable in many research project. For example, the data needs to be available online for the code to be able to download it, or it is unfeasible to manually add it to the project folder if there are too many data files or if the data files are frequently updated. But if this solution is possible, then this is the solution that is recommended in a project hosted in GitHub.

### Half DropBox integration
The project folder is split into two folders. One with the code, and one with the data and all other files related to the project. The folder with code is shared over GitHub and the other folder is shared on DropBox. The file paths in the code points to the GitHub folder when referencing a code file and points to the DropBox folder when referencing a data file. While it is not optimal to have the split the files for one project into two separate folders, this is a solution that most researchers with programming experience are able to set up. At the end of the project, the code files can obviously be copied to the DropBox folder as well.

### Full DropBox integration
All files, including code files, are saved in the project folder on DropBox and each project member that will collaborate on the code will have a separate GitHub folder on their computer with only the code. The project members push edits to the code from the GitHub folder to GitHub.com and then those edits can be downloaded to the code in the DropBox folder directly from GitHub.com. While this solution has the benefit that all project files are in the DropBox folder, it comes to the expense that it requires a technical one-time set-up, but that is what this tutorial will help you with.

Click here for the [Full DropBox integration Tutorial](Resources/Tutorial/README.md)

## Other resources in this repository

* [Best Practices](Resources/BestPractices/README.md)
* [Solutions to Common Issues](Resources/SolutionsCommonIssues/README.md)
