![CICD](https://github.com/inyourarms/near-voting-example/workflows/CICD/badge.svg)

# A Fully Automated NEARCore Docker Deployment using GitHub Actions and Watchtower

In this guide I will explain how to create a github actions workflow that automatically tests, builds, and deploys a Docker image built from the latest source code (tags) of [NEARCore](https://github.com/nearprotocol/nearcore) repository.

## Getting started

First of all, you need to familiarize yourself with [Githab Actions](https://docs.github.com/en/actions) with which you can create any CI/CD workflows.
Also Githab Actions as well has a good API [API](https://developer.github.com/v3/actions/)

To automate a set of tasks, you need to create workflows in your GitHub repository. GitHub looks for YAML files inside of the `.github/workflows` directory.
Events like commits, the opening or closing of pull requests, schedules, or web-hooks trigger the start of a workflow. For a complete list of available events, refer to this [documentation](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).

In this guide we will use only a [schedule](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#scheduled-events) event that allows to trigger a workflow at a scheduled time.

Workflows are composed of jobs, which run concurrently by default. You can configure jobs to depend on the success of other jobs in the same workflow.
Jobs contain a list of steps, which GitHub executes in sequence. A step can be a set of shell commands or an action, which is a pre-built, reusable step implemented either in TypeScript or inside a container. Some actions are provided by the GitHub team, while the open-source community maintains many more. [The GitHub Marketplace](https://github.com/marketplace?type=actions) keeps a catalog of known open-source actions.

GitHub Actions is free for all open-source projects, and private repositories get up to [2000 minutes per month](https://github.com/features/actions#pricing-details)(33,33 hours). For smaller projects, this means being able to take full advantage of automation from the very beginning at no extra cost. You can even use the system for free forever if you use self-hosted runners.

## CI/CD Workflow

Let get started to dive deep into our CI/CD workflow.

In the repository you will see our workflow `.github/workflows/cicd.yml`

Our workflow will trigger by schedule event:
```
on:
  schedule:
    - cron: '*/90 * * * *'
```
Global environment variables:
```
env:
  DOCKER_BUILDKIT: 1 
```
In our workflow we we have one job:
```
jobs:
  build:
```
Also runs on Ubuntu latest 
```
runs-on: ubuntu-latest
```
Lets create a [strategy matrix](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstrategymatrix) to build and deploy different releases for `testnet` and `betanet`.

```
strategy:
  matrix:
    release-name: ["rc", "beta"]
```

As mentioned abowe jobs contain a list of steps, which GitHub executes in sequence.
Step 1 - **Get Github Tag** where the script downloading and saving a github tag for a given release name ("rc" or "beta")
```
echo $(curl -s https://api.github.com/repos/nearprotocol/nearcore/releases | jq -c -r --arg RELEASE_NAME "$RELEASE_NAME" 'map(select(.tag_name | contains($RELEASE_NAME)))[0].tag_name') > github-tag.txt
```
Step 2 - **Get Docker Hub Tags** where the script checks the latest tags of docker images that we have already at our [docker hub](https://hub.docker.com) repository if a github tag from the previuos step exists in the docker repo then the workflow will be cancelled if not then we have a new github tag and it's the case to build and publish a new docker image

>You have to create a secret github variable `DOCKER_IMAGE_NAME`
```
# if previous step is success
if: ${{ success() }}
        env:
          DOCKER_IMAGE_NAME: ${{ secrets.DOCKER_IMAGE_NAME }}
        run: |
          ...
```
Step 3 - **Publish GitHub Image Tag to Registry** where [elgohr/Publish-Docker-Github-Action@master](https://github.com/elgohr/Publish-Docker-Github-Action the action) is a pre-built action that publishes docker containers. It will publish our docker image with latest github tag (ex. `nearcore:1.8.0-beta.2`). 

Step 4 - **Install Rust**



