# Rclone Github Action

Run [rclone](https://rclone.org) to sync files and directories from different cloud storage providers.

## Usage

```yaml
---
name: "🔄 Rclone"

on:
  push:
    branches: [main]
    # Paths which will tigger workflow
    #paths:
    #  - "static/**"
  workflow_dispatch: {}

jobs:
  sync:
    #if: github.repository == 'org-or-user/repository-name'
    runs-on: ubuntu-latest
    concurrency:
      group: ${{ github.workflow }}-${{ github.ref }}
      cancel-in-progress: true
    env:
      # Source path (some/path/to/source)
      local_path: ""
      # Remote path (remote:some/remote/path
      remote_path: ""
    steps:
      - name: "⤵️ Check out code from GitHub"
        uses: actions/checkout@v3
      - name: "⏫ Run rclone"
        uses: digital-clouds/rclone-action@v1.0.0
        with:
          # The RCLONE_CONFIG secret must be set to set up for rclone (required)
          config: ${{ secrets.RCLONE_CONFIG }}
          # Pass any argumets supported by rclone (required)
          args: "sync ${{ env.local_path }} ${{ env.remote_path }}"
          # Verbose debugging and logging or carry on, but do quit on errors (optional)
          debug: false
```

> - `config` can be omitted if [CLI arguments](https://rclone.org/flags/#backend-flags) or [environment variables](https://rclone.org/docs/#environment-variables) are supplied.
>   - Can be encrypted if [`RCLONE_CONFIG_PASS`](https://rclone.org/docs/#configuration-encryption) secret is set.
> - `args` pass any argumets supported by `rclone`.
> - `config-file` set custom location for `rclone` configuration file.
> - `self-update` if true will force update `rclone` to the latest version.
> - `debug` verbose debugging and logging or carry on, but do quit on errors.
