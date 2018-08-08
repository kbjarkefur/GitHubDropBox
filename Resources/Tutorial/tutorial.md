[Go back to main page](../README.md)

# A tutorial for Full DropBox integration in a project hosted on GitHub

In the full DropBox integration all the project files will be saved in the DropBox folder that will be shared with the full project team. Edits made to the code on GitHub.com can be downloaded directly to the DropBox folder. All team members that will contribute to the code should create a local clone (outside of DropBox) of the GitHub repository and do all changes in that clone and push to GitHub.com from there so those edits can later be downloaded to the DropBox folder. While it is possible to make edits to the code in the DropBox folder and upload them to GitHub.com, it is strongly discouraged as it is quite likely to create conflicts that are complicated to solve.

## Introduction to this set-up
When developing this method we prioritized easy maintenance of this solution to the expense of a technical initial set-up, but that technical step only has to be done once per project. So each project needs one team member, or take help of an external person, that sets this up once.

In order for edits to be able to download edits to the code from GitHub.com directly to the DropBox folder a second clone of the repository needs to be created in the DropBox folder. This clone is a second clone separate from the non-shared clone that each team member that collaborates on the code needs to set up outside of DropBox. It is perfectly possible to set up multiple local clones on a computer, however, that is not a feature included in GitHub Desktop and that is why we need to use Git Bash or any other command line tool.

When downloading edits to the DropBox you will also need to use Git Bash or any other command line tool, but that requires much simpler steps compare to the initial setup.

## Getting started

### Git Command Line Environment
You need to have Git Bash or any other command line tool with Git installed set up before you can continue. We recommend Git Bash as it already has Git installed, but which solution you use is up to you. To test if you have a Git Bash environment or Git environment in another command line, or to read instructions for how to set it up if you do not see [Git Command Line Setup](commandline.md).

### DropBox clone setup
How to best do this depend on whether this is a new project with an empty project folder, or if it is an already existing project being migrated to GitHub.
