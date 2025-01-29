# Concourse Resource for Gitea checks (build status)

Push build status from [Concourse](https://concourse-ci.org) to [Gitea](https://github.com/go-gitea/gitea).

## Installing

Add the resource type to your pipeline:

```yaml
resource_types:
  - name: gitea-checks
    type: docker-image
    source:
      repository: rubenv/concourse-gitea-checks-resource
```

## Source Configuration

- `host`: _Required._ URL to the Gitea server.
- `token`: _Required._ API token.
- `organization`: _Required._ Organization name.
- `project`: _Required._ Project name.

## Behavior

### `check`: Not implemented

Not implemented.

### `in`: Not implemented

Not implemented.

### `out`: Create release.

Updates a commit check status.

#### Parameters

- `commit_from`: _Required._ Path to a file containing the SHA of the git commit.
- `context`: _Required._ Name of the integration (should be the same for every status update, to update an existing status).
- `description`: _Optional._ Optional text label.
- `state`: _Required._ Commit state can be "pending", "success", "error" and "failure".
- `target_url`: _Optional._ Defaults to the URL of the build.

## Example

### Out

Define the resource:

```yaml
resources:
  - name: myproject-checks
    type: gitea-checks
    source:
      host: https://...
      token: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
      organization: myorg
      project: myproject
```

Add to job:

```yaml
jobs:
  # ...
  plan:
    - get: myrepo
    - put: myproject-checks
      params:
        commit_from: myrepo/.git/ref
        context: Concourse
        state: success
```
