# Belyfted Git/Github Branching Standards & Conventions

## Quick Legend

<table>
  <thead>
    <tr>
      <th>Instance</th>
      <th>Branch</th>
      <th>Description, Instructions, Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Stable(Production)</td>
      <td>main</td>
      <td>Accepts merges from Development & Hotfixes</td>
    </tr>
    <tr>
      <td>Working(Staging)</td>
      <td>development</td>
      <td>Accepts merges from Features/Issues and Hotfixes</td>
    </tr>
    <tr>
      <td>Features/Issues</td>
      <td>feature/*</td>
      <td>Always branch off HEAD of Working</td>
    </tr>
    <tr>
      <td>Hotfix</td>
      <td>hotfix/*</td>
      <td>Always branch off Stable</td>
    </tr>
  </tbody>
</table>

## Main Branches

The main repository will always hold two evergreen branches:

* `master`
* `development`

The main branch should be considered `origin/main` and will be the main branch where the source code of `HEAD` always reflects a state with the latest delivered development changes for the next release. As a developer, you will be branching from `development`.

Consider `origin/main` to always represent the latest code deployed to production. During day to day development, the `main` branch will not be interacted with.

When the source code in the `development` branch is stable and has been deployed, all of the changes will be merged into `main` and tagged with a release number. *How this is done in detail will be discussed later.*

## Supporting Branches

Supporting branches are used to aid parallel development between team members, ease tracking of features, and to assist in quickly fixing live production problems. Unlike the main branches, these branches always have a limited life time, since they will be removed eventually.

The different types of branches we may use are:

* Feature branches
* Bug branches
* Hotfix branches

Each of these branches have a specific purpose and are bound to strict rules as to which branches may be their originating branch and which branches must be their merge targets. Each branch and its usage is explained below.

### Feature Branches

Feature branches are used when developing a new feature or enhancement which has the potential of a development lifespan longer than a single deployment. When starting development, the deployment in which this feature will be released may not be known. No matter when the feature branch will be finished, it will always be merged back into the developement branch.

`<feature name>` represents the feature to which it will be tracked.

* Must branch from: `development`
* Must merge back into by a P.R: `development`
* Branch naming convention: `feature/<feature-name>` e.g `feature/wallet`

#### Working with a feature branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A feature branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```
$ git checkout -b feature/wallet development                 // creates a local branch for the new feature
$ git push origin feature/wallet                        // makes the new feature remotely available
```

Periodically, changes made to `development` (if any) should be merged back into your feature branch.

```
$ git merge development                                  // merges changes from development into feature branch
```

When development on the feature is complete in your local machine, Don't directly push yet rather kindly do the following

STEP 1 - Commit your changes
```
$ git add .                               // stash all your changes
$ git commit -m "feat(wallet) Completed wallet system"  // Commit your changes
```

STEP 2 - Ensure you are upto date with the development branch. This step is important to see if there have been commits since the feature was branched
```
$ git checkout development                               // change to the development branch  
$ git status                              // Ensure you are upto date with the development branch 
$ git pull origin development                               // get the latest commits from the development branch if the development branch is ahead of your branch by some commits
$ git checkout feature/wallet                               // change back to the feature/wallet branch 
$ git merge development                      // makes sure to merge the development branch into your feature branch
```

STEP 3 - Push your commit
```
$ git push origin feature/wallet                            // push merge changes
```
STEP 4
Then create a P.R(Pull Request) to merge the feature e.g feature/wallet into the development branch. The P.R will be analysed by Ernest and will decide if it can be merged to the development branch or not.

### Bug Branches

Bug branches differ from feature branches only semantically. Bug branches will be created when there is a bug on the live site that should be fixed and merged into the next deployment. For that reason, a bug branch typically will not last longer than one deployment cycle. Additionally, bug branches are used to explicitly track the difference between bug development and feature development. No matter when the bug branch will be finished, it will always be merged back into `development`.

Although likelihood will be less, during the lifespan of the bug development.

`<bug-name>` represents the bug name to which Project Management will be tracked. 

* Must branch from: `development`
* Must merge back into: `development`
* Branch naming convention: `bug/bug-name`

#### Working with a bug branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A bug branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```
$ git checkout -b bug/login-bypass development                     // creates a local branch for the new bug
$ git push origin bug/login-bypass                            // makes the new bug remotely available
```

Periodically, changes made to `development` (if any) should be merged back into your bug branch.

```
$ git merge development                                  // merges changes from development into bug branch
```

When development on the bug is complete in your local machine, Don't directly push yet rather kindly do the following

STEP 1 - Commit your changes
```
$ git add .                               // stash all your changes
$ git commit -m "fix(login) Login bypass fixed"  // Commit your changes
```

STEP 2 - Ensure you are upto date with the development branch. This step is important to see if there have been commits since the bug was branched
```
$ git checkout development                               // change to the development branch  
$ git status                              // Ensure you are upto date with the development branch 
$ git pull origin development                               // get the latest commits from the development branch if the development branch is ahead of your branch by some commits
$ git checkout bug/login-bypass                               // change back to the bug/login-bypass branch 
$ git merge development                      // makes sure to merge the development branch into your bug branch
```
STEP 3 - Push your commit
```
$ git push origin bug/login-bypass                            // push merge changes
```
STEP 4
Then create a P.R(Pull Request) to merge the bug e.g bug/login-bypass into the development branch. The P.R will be analysed by Ernest and will decide if it can be merged to the development branch or not.


### Hotfix Branches

A hotfix branch comes from the need to act immediately upon an undesired state of a live production version. Additionally, because of the urgency, a hotfix is not required to be be pushed during a scheduled deployment. Due to these requirements, a hotfix branch is always branched from a tagged `main` branch. This is done for two reasons:

* Development on the `main` branch can continue while the hotfix is being addressed.
* A tagged `main` branch still represents what is in production. At the point in time where a hotfix is needed, there could have been multiple commits to `main` which would then no longer represent production.

`<hotfix name>` represents the hotfix name to which Project Management will be tracked. 

* Must branch from: tagged `main`
* Must merge back into: `main`
* Branch naming convention: `hotfix/funds-transfer-failure`

#### Working with a hotfix branch

If the branch does not exist yet (check with the Lead), create the branch locally and then push to GitHub. A hotfix branch should always be 'publicly' available. That is, development should never exist in just one developer's local branch.

```
$ git checkout -b hotfix/funds-transfer-failure                 // creates a local branch for the new hotfix
$ git push origin hotfix/funds-transfer-failure                         // makes the new hotfix remotely available
```

When development on the hotfix is complete in your local machine, Don't directly push yet rather kindly do the following

STEP 1 - Commit your changes
```
$ git add .                               // stash all your changes
$ git commit -m "fix(transfer) Funds Transfer failure fixed"  // Commit your changes
```

STEP 2 - Ensure you are upto date with the main or development branch. This step is important to see if there have been commits since the bug was branched
```
$ git checkout main                               // change to the main branch since your branched out from here  
$ git status                              // Ensure you are upto date with the main branch 
$ git pull origin main                               // get the latest commits from the main branch if the main branch is ahead of your branch by some commits
$ git checkout hotfix/funds-transfer-failure                               // change back to the hotfix/funds-transfer-failure branch 
$ git merge main                      // makes sure to merge the main branch into your hotfix branch
```
STEP 3 - Push your commit
```
$ git push origin hotfix/funds-transfer-failure                            // push merge changes
```

STEP 4
Then create a P.R(Pull Request) to merge the hotfix e.g hotfix/funds-transfer-failure into the main branch. The P.R will be analysed by Ernest and will decide if it can be merged to the main branch or not.


### COMMIT MESSAGE CONVENTIONS
When you collaborate with others or want to have tidy commit messages on your own, you can follow Git commit message conventions. There are a handful of conventions.

* `feat`: A new feature
* `fix`: A bug fix
* `docs`: A documentation change
* `style`: A code style change, doesn't change implementation details
* `refactor`: A code change that neither fixes a bug or adds a feature
* `perf`: A code change that improves performance
* `test`: When testing your code
* `chore`: Changes to the build process or auxiliary tools and libraries

They follow this syntax: `<type>(<scope>): <subject>`

An example taken from the command line could be:
`git commit -m "feat(wallet) add filter feature"`

That's how you can keep a tidy commit history for yourself but also for the team.


### NOTE
When working with Belyfted, commiting or pushing changes into the `main` or `development` branch is highly `PROHIBITED`

Instead of working directly with the `main`(often called production) or `development`(staging) branch , you will be working with any of the supporting branches. You will be branching issues out of the development branch and merging them back into the same remote branch.

Once a group of issues have been solved, that develop branch will be merged into the main (or production) branch, usually denoting a version change in the app
