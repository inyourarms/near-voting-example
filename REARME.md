![CICD](https://github.com/inyourarms/near-voting-example/workflows/CICD/badge.svg)

# A Fully Automated Docker Deployment using GitHub Actions and Watchtower

In this guide I will explain how to create a github actions workflow that automatically tests, builds, and deploys a Docker image built from the latest source code (tags) of [NEARCore](https://github.com/nearprotocol/nearcore) repository.

## Getting started

First of all, you need to familiarize yourself with [Githab Actions](https://docs.github.com/en/actions) with which you can create any CI/CD workflows.
Also Githab Actions as well has a good API [API](https://developer.github.com/v3/actions/)

To automate a set of tasks, you need to create workflows in your GitHub repository. GitHub looks for YAML files inside of the `.github/workflows` directory.
Events like commits, the opening or closing of pull requests, schedules, or web-hooks trigger the start of a workflow. For a complete list of available events, refer to this [documentation](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).

In this guide I'll use only a [schedule](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#scheduled-events) event that allows to trigger a workflow at a scheduled time.

Workflows are composed of jobs, which run concurrently by default. You can configure jobs to depend on the success of other jobs in the same workflow.
Jobs contain a list of steps, which GitHub executes in sequence. A step can be a set of shell commands or an action, which is a pre-built, reusable step implemented either in TypeScript or inside a container. Some actions are provided by the GitHub team, while the open-source community maintains many more. [The GitHub Marketplace](https://github.com/marketplace?type=actions) keeps a catalog of known open-source actions.

GitHub Actions is free for all open-source projects, and private repositories get up to [2000 minutes per month](https://github.com/features/actions#pricing-details)(33,33 hours). For smaller projects, this means being able to take full advantage of automation from the very beginning at no extra cost. You can even use the system for free forever if you use self-hosted runners.
