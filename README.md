# Zcash Developers Hub

This is the source repository for the Zcash Developers Hub website: https://zcash.github.io/developers/

For now, this repo hosts the script used to generate ECC's development dependency graph, and a workflow that generates and hosts it.

## Setup

This project uses `poetry` for dependency management: https://python-poetry.org/

The scripts provided by this project require two environment variables:

- `GITHUB_TOKEN`: a GitHub API token with permission to read the necessary repositories.
- `ZENHUB_TOKEN`: a ZenHub API token.

The DAG script depends upon graphql APIs for github which can be generated
using the following example script:

```bash
#!/usr/bin/env bash

poetry run python3 -m sgqlc.introspection \
  --exclude-deprecated \
  --exclude-description \
  -H "Authorization: bearer $GITHUB_TOKEN" \
  https://api.github.com/graphql \
  github_schema.json

poetry run sgqlc-codegen schema github_schema.json github_schema.py
```

## Generating DAGs

The `zcash-issue-dag.py` script supports several configuration options,
also supplied as environment variables:

- `DAG_VIEW=[core|wallet|zf]`: The DAG to render (default: `core`).
- `SHOW_MILESTONES=[true|false]`: Whether or not to render GitHub milestones as boxes (default: `false`).
- `INCLUDE_FINISHED=[true|false]`: Whether or not to include closed issues with no open blockers (default: `false`).

Here's an example script for easily running the DAG generator:

```bash
#!/usr/bin/env bash

DAG_VIEW=core \
SHOW_MILESTONES=false \
GITHUB_TOKEN=<INSERT> \
ZENHUB_TOKEN=<INSERT> \
poetry run python ./zcash-issue-dag.py
```


