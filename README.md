# git-integration

Are you working with continuous integration, with git and with development branches?
If you are rebasing and using the history rewrite features of Git, if you can not be sure that you committed what is needed for the commits to pass the continuous integration tests, then you are in need of local continuous integration management.

`git-integration` may fulfill your need.

## Principle

`git-integration` is a tool designed to work on your own computer and check that all the commits that you are about to push are passing successfully the integration tests.

`git-integration` checks out one by one every new commit that have been added in your development branch in another working directory and performs the continuous integration tests on them. This way, you are sure that the individual commits pass continuous integration independently of the changes and files which exist only in your working directory. Of course, you can run it after having reworked your branch's history.

## Setup

To be able to use `git-integration`, it must be installed in your path.

Now, you need to define the command that should be run to perform the integration tests and the base branch from which your development branches are stemming. For instance, for a makefile based project where development branches stem from `master`:

	$ git config integration.cmd 'make check'
	$ git config integration.origin master

Please note that the integration test command is supposed to be run from the root directory of the project just after a clean checkout.

## Usage

When you want to check that your development branch is clean, just type:

	$ git integration

This command uses a clone of your repository in a `.privatebuild` directory located in the root directory of your working copy and launches the tests on the sequence of commits of `integration.origin..HEAD`. The script is smart enough to not run the test on a tree which has already passed in the past (thanks to the magic of Git).

If all the commits successfully passed the tests, a `Integration OK` message is displayed, meaning that the branch is ready for a push. Otherwise, the script stops on the culprit commit.
