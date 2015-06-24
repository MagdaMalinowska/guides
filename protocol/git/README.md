Git Protocol
============

A guide for programming within version control.

- [Maintain a Repo](#maintain-a-repo)
- [Write a Feature](#write-a-feature)
- [Master Commits](#master-commits)
- [Review Code](#review-code)
- [Merge](#merge)
    + [Rebase Interactively](#rebase-interactively)
    + [Reshare Branch - Force Push Your Branch](#reshare-branch---force-push-your-branch)
    + [Merge on Staging / QA](#merge-on-staging--qa)
    + [Merge on Master](#merge-on-master)
    + [Delete Your Branch](#delete-your-branch)

Maintain a Repo
---------------

* Avoid including files in source control that are specific to your
  development machine or process.
* Delete local and remote feature branches after merging.
* Perform work in a feature branch.
* Rebase frequently to incorporate upstream changes.
* Use a pull request for code reviews.
* Avoid commits on master except empty commits which trigger a deploy.


Write a Feature
---------------

Create a local feature branch based off master.

- command line:

        git checkout master
        git pull
        git checkout -b <branch-name>

- SourceTree:

    + select and checkout `master` branch
    + pull (with rebase) latest changes from `origin/master`
    + click "Branch" button

Rebase frequently to incorporate upstream changes.

- command line:

        git fetch origin
        git rebase origin/branch-name
        git rebase origin/master

- SourceTree:

    + checkout `branch-name`
    + pull (with rebase) latest changes from `origin/branch-name`
    + right-click on `master` branch and select "Rebase current changes onto master"

Resolve conflicts. When feature is complete and tests pass, stage the changes.

- command line:

        git add --all

- SourceTree:

    + go to "Working Copy"
    + right-click on files to stage and select "Add to index"
    + alternatively, drag-and-drop files from "working tree" to "staged index"

When you've staged the changes, commit them.

- command line:

        git status
        git commit --verbose

- SourceTree:

    + click "Commit" button

Write a [good commit message]. Example format:

    Present-tense summary under 50 characters

    * More information about commit (under 72 characters).
    * More information about commit (under 72 characters).

    http://project.management-system.com/ticket/123

Prefer [atomic commits]:

* Commit each fix or task as a separate change.
* Only commit when a block of work is complete.
* Commit each layout change separately.
* Joint commit for layout file, code behind file, and additional resources.

Benefits

* Easy to roll back without affecting other changes.
* Easy to make other changes on the fly.
* Easy to merge features to other branches.

If you've created more than one commit, use a rebase to squash them into
cohesive commits with good messages:

- command line:

        git rebase -i origin/master

- SourceTree:

    + first, make sure you are on your `branch-name`
    + select "all branches" view and look for last commit on `origin/master` branch
    + right-click that commit and select "Rebase children of \<sha\> interactively..."
    + for more information see detailed article on [Interactive rebase in SourceTree](http://blogs.atlassian.com/2014/06/interactive-rebase-sourcetree/)

Share your branch.

- command line:

        git push origin <branch-name>

- SourceTree:

    + push `branch-name` to `origin/branch-name`

Submit a [Bitbucket pull request] or a [GitHub pull request] depending on project.

Ask for a code review in the project's chat room.

[good commit message]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
[atomic commits]: http://www.freshconsulting.com/atomic-commits/
[Bitbucket pull request]: https://www.atlassian.com/git/tutorials/making-a-pull-request/
[GitHub pull request]: https://help.github.com/articles/using-pull-requests/

Master Commits
--------------

Master commits should be avoided. The only reason one should make a commit on master, and only one, is to trigger a deploy with Codeship.

    git commit --allow-empty -m 'Deploy.'
    git push

Review Code
-----------

A team member other than the author reviews the pull request. They follow
[Code Review](/code-review) guidelines to avoid
miscommunication.

They make comments and ask questions directly on lines of code in the
Bitbucket / GitHub web interface or in the project's chat room.

For changes which they can make themselves, they check out the branch.

    git checkout <branch-name>
    git diff master..HEAD

They make small changes right in the branch, test the feature on their machine,
run tests, commit, and push.

When satisfied, they either approve the pull request (Bitbucket), or comment on the pull request `Ready to merge.` (Github).

Merge
-----

### Rebase Interactively

Squash commits like "Fix whitespace" into one or a
small number of valuable commit(s). Edit commit messages to reveal intent. Run
tests.

    git fetch origin
    git rebase -i origin/master

### Reshare Branch - Force Push Your Branch

This allows Bitbucket / GitHub to automatically close
your pull request and mark it as merged when your commit(s) are pushed to master.
It also makes it possible to [find the pull request] that brought in your changes.

- command line:

        git push --force origin <branch-name>

- SourceTree:
    
    + first, enable "push with force" from SourceTree settings
    + push with force `branch-name` to `origin/branch-name`

### Merge on Staging / QA

Changes to `staging` and `qa` branches come through merge commits. We do not rebase this branches on `master`.

- command line:

        git checkout staging
        git pull
        git merge <branch-name>
        git push

- SourceTree:

    + select `staging` branch
    + pull (with rebase) latest changes from `origin/staging`
    + right-click on `branch-name` branch and select "Merge branch-name changes into staging"
    + push changes to `origin/staging`

### Merge on Master

View a list of new commits. View changed files.

    git log origin/master..<branch-name>
    git diff --stat origin/master

Use "Merge pull request" button from Bitbucket / GitHub web interface for merging a pull request.

### Delete Your Branch

Delete your remote feature branch.

    git push origin --delete <branch-name>

Delete your local feature branch.

    git branch --delete <branch-name>

[find the pull request]: http://stackoverflow.com/a/17819027
