Git Protocol
============

A guide for programming within version control.

Maintain a Repo
---------------

* Avoid including files in source control that are specific to your
  development machine or process.
* Delete local and remote feature branches after merging.
* Perform work in a feature branch.
* Rebase frequently to incorporate upstream changes.
* Use a pull request for code reviews.
* Avoid commits on master except empty commits which trigger a deploy


Write a Feature
---------------

Create a local feature branch based off master.

    git checkout master
    git pull
    git checkout -b <branch-name>

Rebase frequently to incorporate upstream changes.

    git fetch origin
    git rebase origin/master

Resolve conflicts. When feature is complete and tests pass, stage the changes.

    git add --all

When you've staged the changes, commit them.

    git status
    git commit --verbose

Write a [good commit message]. Example format:

    Present-tense summary under 50 characters

    * More information about commit (under 72 characters).
    * More information about commit (under 72 characters).

    http://project.management-system.com/ticket/123

If you've created more than one commit, use a rebase to squash them into
cohesive commits with good messages:

    git rebase -i origin/master

Share your branch.

    git push origin <branch-name>

Submit a [Bitbucket pull request] or a [GitHub pull request] depending on project.

Ask for a code review in the project's chat room.

[good commit message]: http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
[Bitbucket pull request]: https://www.atlassian.com/git/tutorials/making-a-pull-request/
[GitHub pull request]: https://help.github.com/articles/using-pull-requests/

Master commits
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
Bitbucket/GitHub web interface or in the project's chat room.

For changes which they can make themselves, they check out the branch.

    git checkout <branch-name>
    git diff master..HEAD

They make small changes right in the branch, test the feature on their machine,
run tests, commit, and push.

When satisfied, they comment on the pull request `Ready to merge.`

Merge
-----

Rebase interactively. Squash commits like "Fix whitespace" into one or a
small number of valuable commit(s). Edit commit messages to reveal intent. Run
tests.

    git fetch origin
    git rebase -i origin/master

Force push your branch. This allows Bitbucket/GitHub to automatically close
your pull request and mark it as merged when your commit(s) are pushed to master.
It also makes it possible to [find the pull request] that brought in your changes.

    git push --force origin <branch-name>

View a list of new commits. View changed files. Merge branch into master.

    git log origin/master..<branch-name>
    git diff --stat origin/master
    git checkout master
    git merge <branch-name> --ff-only
    git push

Use "Merge pull request" button from Bitbucket/Github web interface for merging a pull request.

Delete your remote feature branch.

    git push origin --delete <branch-name>

Delete your local feature branch.

    git branch --delete <branch-name>

[find the pull request]: http://stackoverflow.com/a/17819027
