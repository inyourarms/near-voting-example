![CICD](https://github.com/inyourarms/near-voting-example/workflows/CICD/badge.svg)

# A Fully Automated Docker Deployment using GitHub Action and Watchtower

In this guide I will explain how to create a github actions workflow to automate builds and test the latest source code (tags) of [NEARCore](https://github.com/nearprotocol/nearcore) repository.

## Getting started

First of all, you need to familiarize yourself with [Githab Actions](https://docs.github.com/en/actions) with which you can create any CI/CD workflows.
Also Githab Actions as well has a good API [API](https://developer.github.com/v3/actions/)

To automate a set of tasks, you need to create workflows in your GitHub repository. GitHub looks for YAML files inside of the `.github/workflows` directory.
Events like commits, the opening or closing of pull requests, schedules, or web-hooks trigger the start of a workflow. For a complete list of available events, refer to this [documentation](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).

In this guide I'll use only a [schedule](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#scheduled-events) event that allows to trigger a workflow at a scheduled time.
