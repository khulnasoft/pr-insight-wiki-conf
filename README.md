# pr-insight-wiki-conf
Community version of [PR-Insight Wiki configuration file](https://pr-insight-docs.khulnasoft.com/usage-guide/configuration_options/#wiki-configuration-file)

# Why?
- [PR-Insight Wiki configuration file](https://pr-insight-docs.khulnasoft.com/usage-guide/configuration_options/#wiki-configuration-file) feature is currently only available in the paid version ([PR-Insight Pro](https://www.khulnasoft.com/pricing/))
- This Composite Action provides functionality similar to the PR-Insight Wiki Configuration feature as Open Source
- Provides functionality to make it easier to set and manage common `extra_instructions`

# How does it work
Note: current support is `extra_instructions` config only

1. Load `.pr_insight.toml` configuration from Wiki page
1. Merge `common_instructions` and the env `extra_instructions` with the Wiki config value
1. Set each extra_instructions to environment variables

# How to Use
- Example of [.pr_insight.toml](https://github.com/khulnasoft/pr-insight-wiki-conf/wiki/.pr_insight.toml)
- See details [GitHub Actions](https://github.com/khulnasoft/pr-insight-wiki-conf/actions)
```yaml
on:
  pull_request:
    types: [opened, reopened, ready_for_review]
  issue_comment:
jobs:
  pr_insight_job:
    if: ${{ github.event.sender.type != 'Bot' }}
    runs-on: ubuntu-latest
    permissions:
      issues: write
      pull-requests: write
      contents: write
    steps:
      - name: PR-Insight Wiki Configuration
        uses: khulnasoft/pr-insight-wiki-conf@main
        env:
          common_instructions: >-
            - Answer in Bengali.
          pr_reviewer.extra_instructions: >-
            - Additional second priority point: focus on the need for test code additions or changes to the application code changes.
      - name: PR Insight action step
        id: prinsight
        uses: khulnasoft/pr-insight@main
        env:
          OPENAI_KEY: ${{ secrets.OPENAI_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
