# Git workflow for contributing to open source projects

## Motivation

This tutorial explains a workflow for contributing your code to other people's repositories. If you are new to this and have never contributed to an open source repo, this might be helpful.

## Workflow

### Forking the repository

The first step is to fork the repository you want to contribute to. Forking a repository is basically creating a new repository that is an exact copy of the original one, and is linked to it.

To fork a repository, click the _Fork_ button in the upper right corner of the repository's page.

### Cloning the repository

This step is stright forward. Clone the repo you just forked to your local machine. Be sure to clone the forked repository, not the original one.

### Configuring remotes

By default, Git already added a remote called _origin_ when you cloned the repository. You can see it by running `git remote` command. This remote points to your forked repository.

You will need to add one more remote that points back to the original repository. You can do that by running `git remote add <remote_name> <url>`. The convention is to call this remote _upstream_, but you can use any name you want.

Now, if you run `git remote` again, you should see two remotes: _origin_ and _upstream_ (assuming that you named the second origin _upstream_).

### Creating a branch, pushing and opening a pull request

You are ready to start making changes to the code. Create and checkout to a new branch by running `git checkout -b <branch_name>` command.

Once you've made the changes to the code, commit the changes and push them to github by running `git push origin <branch_name>`. _Origin_ in the command denotes that you are pushing changes to the forked repository.

Now go to Github and open a pull request from your branch to the original repository.

### Cleanup and maintenance

Once the maintainers merge your pull request (hopefully), you can delete your working branch by running `git branch -d <branch_name>`, then run `git push origin main` to push the deletion of the branch to your remote repository.

To keep your fork in sync with the original repository, use `git pull upstream main` command.
