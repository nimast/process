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
On your own copy of the repository, fork the `develop` branch to create a new branch with the `feature/` prefix and a meaningful name describing the feature you are about to develop. Use this branch exclusively for any commits related to the feature being worked on. Never use it for anything else.

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
- Appliance of [SOLID principles](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design))
- Alignment with general way things are done in the app
- Alignment with framework best practises when using external frameworks and open source projects
- Isolation of dependencies (using wrappers / adapters)
- Integrity of project structure (no personal / project files)
#### Merging
When all is well and after communicating with the author of the PR around any issues that may arise, The following procedure should be followed with the hope that parts of it will be one day automated:
- Merge the feature to the develop branch.
- Deploy the develop branch to the integration environment
- Run integration tests on it (rollback the deployment if they fail)
- If the deployment succeeded, mark started stories form pivotal mentioned in the commits as deployed. 
- Notify the team that a new version is available
