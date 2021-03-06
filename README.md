# GitHub Activity in Readme

Updates `README.md` with the recent GitHub activity of a user.

<img width="735" alt="profile-repo" src="https://raw.githubusercontent.com/DeKal/github-activity-readme/master/images/demo.png">
 
---

## Instructions

- Add the comment `<!--START_SECTION:activity-->` (entry point) within `README.md`. You can find an example [here](https://github.com/DeKal/DeKal/blob/master/README.md).


- It's the time to create a workflow file.

`.github/workflows/update-readme.yml`

```yml
name: Update README

on:
  schedule:
    - cron: '*/30 * * * *'
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    name: Update this repo's README with recent activity

    steps:
      - uses: actions/checkout@v2
      - uses: jamesgeorge007/github-activity-readme@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

The above job runs every half an hour, you can change it as you wish based on the [cron syntax](https://jasonet.co/posts/scheduled-actions/#the-cron-syntax).

Please note that only those public events that belong to the following list show up.

- `PushEvent`
- `IssueEvent`
- `IssueCommentEvent`
- `PullRequestEvent`

You can find an example [here](https://github.com/DeKal/DeKal/blob/master/.github/workflows/cron.yml).

## Instructions for private events

In case you want to show private events
- Please generate a [new personal token](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token).
- Add your token to [Secrets](https://docs.github.com/en/free-pro-team@latest/actions/reference/encrypted-secrets) in the repository which contains the README file. 
- Use `${{ secrets.MY_SECRET_GITHUB_TOKEN }}` to get private event instead of `${{ secrets.GITHUB_TOKEN }}`.

### Override defaults

Use the following `input params` to customize it for your use case:-

| Input Param | Default Value | Description |
|--------|--------|--------|
| `COMMIT_MSG` | :zap: Update README with the recent activity | Commit message used while committing to the repo |
| `MAX_LINES` | 5 | The maximum number of lines populated in your readme file |


```yml
name: Update README

on:
  schedule:
    - cron: '*/30 * * * *'
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    name: Update this repo's README with recent activity

    steps:
      - uses: actions/checkout@v2
      - uses: jamesgeorge007/github-activity-readme@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          COMMIT_MSG: 'Specify a custom commit message'
          MAX_LINES: 10
```

_Inspired by [JasonEtco/activity-box](https://github.com/JasonEtco/activity-box)_
