# Auto-update Custom Fields in Linked Asana Tasks

## What does this do?

This updates a custom field in Asana called Status when the following happen:

- PR merged (or any other push to main, for that matter)
- PR opened or reopened

## Development of this Github Action

Use Github Codespaces to develop this in-browser. There's an existing codespace already set up.

Save your changes and commit the build artifacts:

```
nvm use && npm install &&npm run build && npm run package && git add -A && git commit && git push origin main
```

Upon pushing, you'll trigger an automatic release

## Using this Github Action from another repo

_Make sure to allow Github Actions from the respective repo you want this Github Action to operate on._

Include a Github Workflow file in the respective repo:

.github/workflows/auto-update-asana-custom-field.yml

```
name: Asana Status

on:
  pull_request:
    types: [opened, reopened]
  push:
    branches:
      - "main"

jobs:
  auto-merge-main-into-open-pull-requests:
    name: Update
    runs-on: "ubuntu-latest"

    steps:
      - name: Checkout repo
        uses: actions/checkout@v3
      - name: Update Asana Status
        uses: "sprucehealth/auto-update-asana-custom-field-action@latest"
        with:
          mainBranchName: main
          asanaToken: ${{ secrets.ASANA_TOKEN }}
          statusFieldName: "Status"
          statusFieldValueWhenDraftPRIsOpened: "📖 In Code Review"
          statusFieldValueWhenPRReadyForReviewIsOpened: "✏️ In Development"
          statusFieldValueForMergedCommitToMain: "ᛦ Merged"
```
