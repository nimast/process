# Learni Development Process

The intention of this document is to describe in details the end-to-end process required in order to develop a single feature in any of the Learni sub systems.

## General go-tos 

We use scrum boards in the form of projects in [Pivotal Tracker](https://www.pivotaltracker.com/). Each sub system will have a pivotal project of its own.

## Developer setup
Follow the instructions on the sub system repository's page in order to setup your computer. If its a new repository, use the instructions of a repository that uses the same language.
### Contribution repository setup
Fork the repository to your own github account

## Developing a feature
### Picking a task
Pick up the top task from the backlog of the related project in pivotal tracker. In order to be developed, the story must have:

- A Link to the specification (design) document
- A Link / reference to the required design assets in Dropbox
- A Link to the HLD of the feature
 
If the first story doesn't contain any of these, go forward to the next one and inform the development manager. When you find a suitable feature, press the Start button on pivotal tracker.

### Getting ready to develop
On your own copy of the repository, Branch out of the `develop` branch to create a new branch with the `feature/` prefix and a meaningful name describing the feature you are about to develop. Use this branch exclusively for any commits related to the feature being worked on. Never use it for anything else.

### Developing
Write any necessary code needed for the feature you are about to develop following the project's styleguide, the design specification and the HLD. Author unit tests wherever appropriate. We don't have coverage goals, just make sure the error prone areas of the feature have robust tests. Work in small, reasonable chunks and commit freely to the feature branch you're working on, using meaningful commit messages.

### Discussing changes
Need to break an interface / change a core component? Put in on the table for discussion with fellow developers. You can use a Pull Request to do that as well, details below.

### Finishing up
- Make sure your branch is up-to-date by synchronising your fork with the original repository followed by merging the `develop` branch with the feature branch you're working on.
- Create a pull request. The default branch on the target repository should be `develop`, but even if it isn't, this is the only branch that is suitable as a target of a pull request.
- Mark the story in pivotal as finished
- Repeat...

## Maintaining a repository 
This section is describing the common actions needed to be done by the repository's maintainer. 
### Reviewing and discussing pull request
In the order in which they arrive, review pull requests. 
#### General structure
Ensure that the Pull Request:
- Has a meaningful name
- Contains only commits that (by name) are related to the feature being developed
- Contains at least one commit referencing the `story id` of the feature in Pivotal Tracker
 
#### Code Review
Go over the files that have changed using the `Files Changed` tab on GitHub. For each change, asses: 
- Code style
- Appliance of [SOLID principles](http://en.wikipedia.org/wiki/SOLID_\(object-oriented_design\))
- Alignment with general way things are done in the project
- Alignment with framework best practises when using external frameworks and open source projects
- Isolation of dependencies (using wrappers / adapters)
- Integrity of project structure (no personal / project files)
- Relevancy to the feature being developed

#### Merging
When all is well and after communicating with the author of the PR around any issues that may arise, The following procedure should be followed with the hope that parts of it will be one day automated:
- Merge the feature to the develop branch.
- Deploy the develop branch to the integration environment
- Run integration tests on it (rollback the deployment if they fail)
- If the deployment succeeded, mark started stories form pivotal mentioned in the commits as deployed. 
- Notify the team that a new version is available

## Parallel development
Our git workflow setup is optimized to not stand in your way when you are the only developer currently working on a repository, while maintaining our process and enabling parallel work when it is needed. The following section describes the additional actions that might be required when multiple developers are working on the same master repository.
### Terminology
Throughout this document and here on after the following terminology is applied:
- `master repository` the master copy of a repository, the one under the `learni` organization account.
- `fork` a copy of the repository under a developer's account.
- `branch out` creating a new branch from the branch we 'branch out' from.

### Fixing bugs in a release while developing other features
This is when someone is working on bug fixes in an upcoming release, and another one is developing features that shouldn't be included in that release. In this case, they can't work simultaneously on the `develop` branch. 
- The person working on the release will branch out of `develop` to a branch with the `release/` prefix and the version number as the branch name on the master repository.
- The person working on the release, on his copy of the repository, will fork this branch to create PRs to the release branch on the master repository.
- The person working on the features will PR to `develop` as usual.
- When the release is done, just like in [gitflow](https://www.atlassian.com/git/workflows#!workflow-gitflow), the release branch will be merged to `master` **and** to `develop` on the master repository.

### Working on breaking changes in parallel
This is when there's a big breaking change (like a major API change or a major update in a library) that needs to be worked on in parallel by more than one developer. 
- The first one to reach this 'breaking' feature will create a branch of the `develop` branch on the master repository with the `feature/` prefix and a self explanatory name as the branch name.
- All developers working on this breaking feature will branch out of this branch on their forks and PR back to it.
- Once the feature is done, just like in gitflow, it is merged back to the `develop` branch on the master repository.

### Applying a hotfix
This is when we need to make a slight change to the production code after progress has already been made on the `develop` branch.
- If this is a small change that can be made by one person, he will branch out of `master` on his synced fork to create a new branch with the `hotfix/` prefix and a meaningful name. He will then make the change and PR this branch with `master` on the master repository as the target of the PR.
- If this is a larger change needs to be worked on by multiple developers, the hotfix branch should be created on the master repository, with all developers working on the change PRing to that branch. 
- After all work is done, just like in gitflow, we merge the hotfix branch to `master` **and** to `develop`.

##Github and SourceTree

###Branch name tips
develop branch: The last working development version(pull from this one).
master branch: The production version. (don’t touch :) )
New branches names(use underscores):
1) feature/feature_name
2) hotfix/fix_name
3) release/

###New project
In bookmarks window click the add repo button.
Source Path/ URL: take this from the Github web page.
Change the name of this repo to upstream.
Right click remotes and add your own repo(if you don’t have one, create it using Github webpage).

###New local branch from remote repo
Right click the remote repo’s branch and click checkout.

###New local branch from existing local branch
Check out the branch you want to branch from (default: develop).
Right click the branch and select new branch.
The new branch should have a meaningful name and start with hotfix/, feature/.

###Update local develop branch
Check out the branch you want to pull to (default: develop).
Right click upstream.develop branch, click pull.

###Commit, push and pull request
*Before committing make sure all UT work.
Commit your changes, make sure to write a meaningful comment.
Right click your local branch and click push to YourRemoteRepo(Not upstream!), use the exact same name, folder as your local branch.(f.e: local: hotfix/FlashCrashFix, MyRepo: hotfix/FlashCrashFix)
Right click your remote branch and click pull request.
This opens github in a browser, fill in a pull request comment and approve.

###Extra commits on the same branch
As long as your pull request is pending, every time you push new commits, they will be added to the pull request.
Note that the admin won’t be notified of this commit, you should write in the comment @username if you want that user to be notified of your new commits.
If your pull request is closed, you will have to create a new one.
You might want to update your branch, first update your local develop branch, then merge your develop branch and your working branch.

###Stash
Use this if you want to backup uncommitted changes.
Click the stash button, and choose a name. All the pending changes will reset, and saved in a stash.
You can then get these changes back, by right clicking the stash(left bottom corner) and choosing apply stash.
*You can have multiple stashes.
**You can apply a stash on different branch than the one it was made on.

###Fetch
Use button to simply refresh your source tree ui.
