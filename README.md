# xournalpp

*[![xournalpp](https://snapcraft.io/xournalpp/badge.svg)](https://snapcraft.io/xournalpp)
[![xournalpp](https://snapcraft.io/xournalpp/trending.svg?name=0)](https://snapcraft.io/xournalpp)*

Below you will find some instructions to help you contribute to this snap. The general workflow will be to submit PRs from your fork onto the "stable" branch. Once the pull request has been *submitted*, there is a GitHub action that will automatically launch a build of the snap that you can use for testing. And then once the PR has been *merged* the GitHub action will launch a build on launchpad that will be automatically uploaded to the snap store, in the candidate channel, for more extensive testing.

## How to contribute to this snap

### Initial setup
Here is the workflow for submitting a change to the stable branch, and getting it published in the snap store in the candidate channel. If this is your first time contributing to this snap, then there are a couple of things you need to do as a one-time setup to get ready to submit your first pull request:

1. [Fork the repository](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo) into your own GitHub namespace.
2. [Clone your fork](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository).
3. In addition to your fork, you need to also add this remote repository. To make things a bit more intuitive, let's rename your fork's remote too (since "origin" is hardly descriptive):

```
git remote rename origin myfork
git remote add kenvandine https://github.com/kenvandine/xournalpp.git
git fetch --all
```

### Testing your changes
To test your changes, you'll need to build your snap. You can do this with either [multipass](https://multipass.run/)
```
snapcraft
```
or with [lxd](https://snapcraft.io/docs/build-on-lxd):
```
snapcraft --use-lxd --debug
```
The `--debug` option will just drop you into the snap if a build error occurs, so you can further triage. It is completely optional and can be left off.

Once the build finishes, you will have <yoursnap>.snap in your current directory. Install it on a test system to test the changes.
```
sudo snap install <yoursnap>.snap --dangerous
```
Note you need to add the `--dangerous` flag because the snap is not being installed from the store (and signed by the store certificate, so it's an auto trusted source).

### Submitting changes in a pr

Once you're all setup for contributing, keep in mind that you want the git information to be all up-to-date. So if you haven't "fetched" all changes in a while, start with that:

```
git fetch --all -p
```

Now that your git metadata has been updated you are ready to create a bugfix branch, make your changes, and open a PR.

1. All PRs should go to the stable branch so create your branch as a copy of the stable branch:

```
git checkout -b my-bugfix-branch kenvandine/stable
```

2. Make your desired changes and push them to your fork:

```
git push myfork my-bugfix-branch
```

Once this branch has been pushed to your fork, you should update the local branch tracking so it tracks the branch pushed to your fork:

```
git branch -u myfork/my-bugfix-branch
```

3. When you feel they're ready for submitting to the main repository (stable branch), [open up a pull request](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) from your `my-bugfix-branch` to the `kenvandine/stable` branch.
4. Once you've opened the PR, it will automatically trigger the build-test action that will launch a build of the snap. You can watch the progress of the snap build from your PR (Show all checks -> Details). Once the snap build has completed, you can find the built snap (to test with) under "Artifacts".
4. Someone from the team will review the open PR and either merge it or start a discussion with you with additional changes or clarification needed.
5. Once the PR has been merged into the stable branch, then on the next git mirror sync (every 4 hours), launchpad will trigger [a build of the snap that gets published](https://launchpad.net/~desktop-snappers/+snaps) to the [snap store](https://snapcraft.io) into the *candidate* channel. After sufficient testing of the snap from the candidate channel, then the reviewer (a Collaborator of the snap in the store) will promote the snap to the stable branch in the snap store.

TODO: update the two links in step 5. above to point to actual builds and the correct snap page in the store.
