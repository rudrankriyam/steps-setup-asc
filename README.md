# Setup asc CLI

Official Bitrise Step for installing and optionally running [`asc`](https://github.com/rudrankriyam/App-Store-Connect-CLI).

## Step ID

- `setup-asc`

## What it does

- `mode=install`: installs `asc` from GitHub Releases and exports:
  - `ASC_CLI_PATH`
  - `ASC_CLI_VERSION`
- `mode=run`: installs `asc`, optionally exports `ASC_*` auth environment variables, runs a command, and exports:
  - `ASC_COMMAND_EXIT_CODE`

## Basic usage

```yaml
workflows:
  primary:
    steps:
    - setup-asc:
        inputs:
        - mode: install
        - version: latest
    - script:
        inputs:
        - content: |-
            #!/usr/bin/env bash
            set -euo pipefail
            "${ASC_CLI_PATH}" --help
```

Install + run in one step:

```yaml
workflows:
  primary:
    steps:
    - setup-asc:
        inputs:
        - mode: run
        - version: latest
        - command: asc --help
```

## Local validation

```bash
stepman audit --step-yml ./step.yml
bitrise run audit-this-step
bitrise run test-install
bitrise run test-run-help
```

