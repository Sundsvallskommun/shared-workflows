# Shared workflows for Sundsvalls Kommun

Central repository to store all of the workflows used in Sundsvalls Kommuns other repositories. Eases the roll out of updates and changes to workflows to all affected repositories.

## Usage

To make use of any of these workflows in your repository you need to set up things in your repository.

Create a workflow file (e.g. `.github/workflows/codeql.yml` see [Creating a Workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file)) for the workflow you want to add to your repository and see the descriptions below on what you need for that workflow.

## Common Workflows

These workflows does not rely on a specific technology and can be used by all services regardless.

#### Dependabot Auto Reviewer

Automatically approves and merges dependabot pull requests that have passed all required status checks. This should be used with caution. Using this workflow without properly setup checks and tests in your application **will** eventually lead to broken applications.

If a pull request is opened by anyone else but Dependabot the reviewer will skip trying to auto-merge the PR.

```
name: "Call Dependabot reviewer"

on:
  pull_request_target:

permissions:
  pull-requests: write
  contents: write

jobs:
  shared-workflows:
    uses: Sundsvallskommun/shared-workflows/.github/workflows/common-dependabot-reviewer.yml@main
    secrets: inherit
```

## Java specific workflows

The following workflows are only compatible to be run in repositories containing a project with java 21 and maven.

### Maven CI

Just as you should do before you push your code this workflow runs `mvn -B verify`. Useful to make sure code actually compiles and test passes. Important to use as a required check if you want to implement the dependabot autoreviewer.

```

name: "Call Java CI with Maven"

on:
  workflow_dispatch:
  pull_request:
    types: [opened, synchronize, reopened]
  push:
    branches:
      - main
  schedule:
    # At 03:00 on Sunday (Please note: GitHub actions schedule is in UTC time).
    - cron: "0 3 * * 0"

jobs:
  shared-workflows:
    uses: Sundsvallskommun/shared-workflows/.github/workflows/java-maven-ci.yml@main

```

##

### CodeQL

Used to scan the code in the repository for find potential vulnerabilities.

```
name: "Call CodeQL"

on:
  workflow_dispatch:
  pull_request:
    types: [opened, synchronize, reopened]
  push:
    branches:
      - main

jobs:
  shared-workflows:
    uses: Sundsvallskommun/shared-workflows/.github/workflows/java-codeql.yml@main
    permissions:
      actions: read
      contents: read
      security-events: write

```

##

### Maven Release

Used to release a new version of the project. This workflow will create a new tag and push it to the repository.

```
name: "Call Maven Release"

on:
  workflow_dispatch:

jobs:
  shared-workflows:
        uses: Sundsvallskommun/shared-workflows/.github/workflows/java-maven-publish.yml@main
    	secrets: inherit
          
```
## Contributions

Contributions are welcome! See the [Contributor's Guide](CONTRIBUTING.md).

##

Copyright (c) 2024 Sundsvalls kommun
