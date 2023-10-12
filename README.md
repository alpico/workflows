# workflows

A collection of Github workflows we are using.

All workflows are in `.github/workflows` (because they have to be for them to work).

## Workflows

### in_review_on_pr

Moves all issues that will be closed by a PR to in review when:

- the PR is opened
- the PR is edited (in case new issues which will be closed are added)
- a review is requested
- the PR is marked as ready for review (does not run on draft PRs)

### in_progress_on_change_requested

Moves all issues that will be closed by a PR to in progress when changes are requested through a review.

## Developing

Here are some insights I gathered while developing which could be helpful to you.

### Reusing code

`in_progress_on_change_requested` and `in_review_on_pr` share a lot of code.
Since most of it is GraphQL, it is not very intuitive so changes made in one
can easily be forgotten in another.

Using another nested workflow in this step doesn't work because you can't share
the environment of the calling workflow as an input to the called workflow.
This means that a variable such as `${{ env.ORGANIZATION }}` cannot be used as an
argument rendering most tasks useless.

You can, however, create [composite actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)
which reside in `.github/actions/`.
These actions automatically inherit the entire environment from the calling
workflow.

Composite actions are not indicated to be a separate tasks like workflows are and
instead the steps performed by them are simply shown as steps performed directly
in the workflow - perfect for our purposes.

### GraphQL

Making any changes to GitHub usually means you will need to use the
[GraphQL API](https://docs.github.com/en/graphql).
The best way to get anything done with this is to click yourself through
the [Explorer](https://docs.github.com/en/graphql/overview/explorer).

You cannot save any kind of data in variables or make a join or any of
that fancy stuff.
GraphQL will always only produce how data relates to each other.
If you want to do something with this data, you need to feed the resulting
JSON into [`jq`](https://jqlang.github.io/jq/)
(try using [jqplay](https://jqplay.org/)) and select the data you need and write it into a variable.
View any of the actions in `.github/actions/` for examples.

### Starter Workflows

These workflows always have an intended trigger in mind.
To speed up setting up such a workflow, we can create templates for
these workflows called [Starter Workflows](https://docs.github.com/en/actions/using-workflows/creating-starter-workflows-for-your-organization).

These need to go into our [.github repository](https://github.com/alpico/.github/tree/main/workflow-templates).
Users can then easily select one of these when creating a new workflow in a repo.
