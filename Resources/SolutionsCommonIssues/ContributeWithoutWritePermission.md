# Cloning someone else's repository
You can always clone someone else's repository if you can see it on GitHub.com. This includes all public repositories and private repositories where you have been added as a collaborator. You can clone these using both Git Bash or GitHub Desktop, and you can use either of those tools to keep pulling updates made to those repositories.

However, you can only push to them if you have been added as a collaborator and have been given writing privileges. Not that you may have writing privileges but the branch you are trying to push to might be locked for edits so that only the owner can edit that branch. You can sill go through all steps of adding and committing files, but when you try to push to the cloud you will get an error.

If you have made changes in your local clone that you do not have write privileges, but you still want your edits to be included, here are some suggested solutions.

## Be added as a collaborator to the repository
If you know the owner of the repository, the by far easiest solution is to ask the owner if you can be added as a collaborator to the repository.

## If you can't be added as a collaborator, fork the repository
Your best option if you cannot be added as a collaborator but still want to submit your edits to the original repository is to create a fork. Start by forking the original repository on GitHub.com. Since you created this fork and are the owner of the fork, you will have writing privileges to it. While you now have writing privileges to your fork, your local clone still points towards the original repository. The easiest solution is to create second local clone, transfer the changes that you did in the first clone to the second clone and push from there to your clone. When you have pushed your changes to your fork, then you can create a pull request to the original repository suggesting the edits that you did.

There are complicated and technical solutions to how you can change your local clone be associated with your fork and not the original repository so you do not need to manually transfer your changes, but that is outside of the scope of this tutorial.
