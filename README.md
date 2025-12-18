# Exemplary documentation repo

For its feature counterpart, see: [`novusnota/test`](https://github.com/novusnota/test)

Important files added: `.github/workflows/docs-release-gate.yml` — the reusable CI workflow, which should be referenced in feature repos in order to start checking the existence of relevant documentation PRs when they are needed.

In order to use the workflow, the feature repo in question should:

1. Create `.github/PULL_REQUEST_TEMPLATE.md`, see the [example one in `novusnota/test`](https://github.com/novusnota/test/blob/main/.github/PULL_REQUEST_TEMPLATE).

1. Consider creating a label "needs-ton-docs" (see `label_name` variable) — if the description is lacking, the `docs-release-gate.yml` looks for this label to determine whether authors consider documentation be required for a given PR in a feature repo.

1. Create `.github/workflows/ton-docs.yml`, see the [example one in `novusnota/test`](https://github.com/novusnota/test/blob/main/.github/workflows/ton-docs.yml). Adjust the variables as you see fit.

   For example, in `ton-blockchain/ton` the workflow might look like this:

   ```yml
   name: "TON Docs"
   on:
     pull_request_target:
       types: [opened, edited, labeled, unlabeled]
   permissions: read-all
   jobs:
     docs-release-gate:
       uses: ton-org/docs/.github/workflows/docs-release-gate.yml@main
       with:
         require_same_author: true # defaults to false
         from_branch: "testnet" # defaults to "*", which means any branch
         into_branch: "master" # defaults to main
   ```

   Notice that I've assumed `ton-org/docs` already has the reusable workflow from this repository, which is not the case until this PR is merged: ...
