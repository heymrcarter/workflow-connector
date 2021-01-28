# Workflow Connector

A GitHub action trigger secondary workflows

## Usage

```yml
  - uses: heymrcarter/workflow-connector@v1
    with:
      token: ${{ secrets.REPO_PAT }}
      event-type: my-event
      repository: # optional, defaults to the current repo
      event-payload:
        sha: ${{ github.sha }}
        ref: ${{ github.ref }}
```
- A [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token) with `repo` scope is required to trigger the desired workflow.

- You can add up to 10 key-value pairs to the event payload

## Example Connector

Suppose you have two Actions workflows, build and deploy.

In the first workflow, build, add Workflow Connector as the last step:

```yml
  - uses: actions/checkout@v1
  ...
  - uses: heymrcarter/workflow-connector@v1
    with:
      token: ${{ secrets.REPO_PAT }}
      event-type: deploy
      client-payload:
        sha: ${{ github.sha }}
```

Assuming all of the previous steps execute successfully, Workflow Connector will trigger the deploy workflow via the `deploy` [`repository_dispatch`](https://developer.github.com/v3/repos/#create-a-repository-dispatch-event) event (as configured by `event-type`).

To run the deploy workflow when the `deploy` event is dispatched add it as the trigger:

```yml
name: deploy
on:
  repository_dispatch:
    types: [deploy]

...
```

## License

[MIT](LICENSE)
