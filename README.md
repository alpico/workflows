# workflows

A collection of Github workflows we are using.

All workflows are in `.github/workflows` (because they have to be for them to work).

## Workflows

### in_review_on_pr

Moves all issues that will be closed by a PR to in review when the PR is opened (and also when edited in case new issues which will be closed are added).

Example caller:

```yaml
name: Move linked issues to "In Review"

on:
  pull_request:
    types: [opened, edited, ready_for_review]

jobs:
  MoveLinkedIssues:
    uses: alpico/workflows/.github/workflows/in_review_on_pr.yml@main
    with:
      organization: "alpico"
      project: "Fast-Deploy"
    secrets:
      gh_token: ${{ secrets.GH_TOKEN }}
```

### Draft Pull Requests

If the pull request is still a draft, then nothing happens.
Once the pull request is marked as ready, the job is run.
