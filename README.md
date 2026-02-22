# Setup asc CLI (Bitrise Step)

[![CI](https://github.com/rudrankriyam/steps-setup-asc/actions/workflows/ci.yml/badge.svg)](https://github.com/rudrankriyam/steps-setup-asc/actions/workflows/ci.yml)
[![Bitrise Step](https://img.shields.io/badge/Bitrise-Step-6A5ACD)](https://www.bitrise.io/integrations/steps/setup-asc)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](./LICENSE)

Official Bitrise Step for installing and running
[`asc`](https://github.com/rudrankriyam/App-Store-Connect-CLI), the App Store
Connect CLI.

## Step ID

- `setup-asc`

## Why use this step

- Install `asc` from GitHub Releases (`latest` or pinned versions)
- Verify release checksum before installing
- Run `asc` commands in the same step when needed
- Export common `ASC_*` auth variables for CI execution
- Works in Linux and macOS Bitrise environments

## Modes

- `mode=install`
  - installs `asc` only
  - exports `ASC_CLI_PATH` and `ASC_CLI_VERSION`
- `mode=run`
  - installs `asc`, optionally exports auth/runtime `ASC_*` environment
    variables, and runs the provided command
  - exports `ASC_COMMAND_EXIT_CODE`

## Quick start

Install only:

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

Install and run:

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

Run with App Store Connect auth:

```yaml
workflows:
  primary:
    steps:
      - setup-asc:
          inputs:
            - mode: run
            - version: latest
            - key_id: $ASC_KEY_ID
            - issuer_id: $ASC_ISSUER_ID
            - private_key_b64: $ASC_PRIVATE_KEY_B64
            - bypass_keychain: "yes"
            - command: asc apps list --output json
```

## Security notes

- Store credentials in Bitrise Secret Env Vars
- Sensitive inputs are marked as `is_sensitive` in `step.yml`
- Prefer `private_key_path` or `private_key_b64` over inline private key content

## Useful links

- asc CLI repository: https://github.com/rudrankriyam/App-Store-Connect-CLI
- asc docs: https://asccli.sh/
- Bitrise Step docs: https://docs.bitrise.io/en/steps-and-workflows/introduction-to-steps.html

## Local validation

```bash
stepman audit --step-yml ./step.yml
bitrise run audit-this-step
bitrise run test-install
bitrise run test-run-help
```

