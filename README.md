# Prevent-Public-Repos Probot App

A GitHub Probot App that monitors and prevents Public Repositories from being created in an organization.


## Features
- Can convert newly created Public Repos to Private
- Can also be enabled for repos that switch visibility from private to public
- Will create an issue in the repo explaining the action
- Monitor only mode will not change the repo visibility but will still create an issue
- Can set configuration parameters by using YAML file set in a specific repo for the entire org
- Can exclude certain repos
- Can set a list of users/groups to cc on every issue created

## How it Works

By default when a new repository is created with Public visibility, an Issue will be created in the repository warning that it is Public to the internet [Monitor-Only mode is enabled].

A `.github/prevent-public-repos.yml` file is recommended to override the [default settings](./lib/defaults.js) created in Repository `org-settings`. This repository will contain global settings for the organization.

```yml
# Configuration for Prevent-Public-Repos

# Turn on Monitor Mode. In this mode the repo visibility is not modified and only an Issue is created
monitorOnly: true

# Enables detection of repos that change visibility from private to public (not just newly created ones)
enablePrivateToPublic: false

# Issue Title when repo is privatized
privatizedIssueTitle: '[CRITICAL] Public Repositories are Disabled for this Org'

# Issue Body when repo is privatized
 privatizedIssueBody: 'NOTE: Public Repos are disabled for this organization! Repository was automatically converted to a Private Repo. Please contact an admin to override.'

# Issue Title when monitor mode is enabled
monitorIssueTitle: '[CRITICAL] Public Repository Created'

# Issue Body when monitor mode is enable
monitorIssueBody: 'Please note that this repository is publicly visible to the internet!'

# Users/Groups that should be cc'ed on the issue. Should be users/groups separated by a space.
ccList: '@issc29'

# Repos to  exclude in detection. Should be a List of Strings.
excludeRepos: ['repo1', 'repo2']
```

When setting up this Probot App you can also set a number of Environment Variables

## Setup

See [docs/deploy.md](docs/deploy.md) if you would like to run your own instance of this app.

Possible Environment Variables:
- FILE_NAME [default: '.github/prevent-public-repos.yml'] - Sets the location/file name of the config yml file
- ORG_WIDE_REPO_NAME [default: 'org-settings'] - Set the repo where to find the config yml file
