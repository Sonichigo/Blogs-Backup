---
title: "How to write a custom GitHub Action"
seoTitle: "How to write a custom GitHub Action"
seoDescription: "Custom actions provide a reusable solution, streamlining tasks across repositories and allowing me to encapsulate complex build or deployment logic."
datePublished: Fri Jan 12 2024 10:14:58 GMT+0000 (Coordinated Universal Time)
cuid: clrahfawo000008jz0jo2blyp
slug: how-to-write-a-custom-github-action
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705054654192/439a5e8a-4b62-4218-8488-43252884ddc3.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1705054676805/5a638262-5d46-41dc-a336-1506d3c1007f.png
tags: docker, github, javascript, github-actions, github-actions-1

---

GitHub Action is a powerful CI/CD feature, you get once you have an account in GitHub.

It enables you to define all the steps you like to happen on a set of specified [triggers](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows). These actions encompass a variety of tasks, providing a flexible framework for automating processes related to your software development workflow. Some examples of these actions include triggering builds, running tests, deploying applications, and more. Essentially, GitHub Actions allows you to orchestrate a sequence of operations in response to specific events or changes in your repository.

## **Why Would You Write Custom GitHub Action?**

Custom actions provide a reusable solution, streamlining tasks across repositories and allowing me to encapsulate complex build or deployment logic. This approach grants us control over versioning, maintainability, and security, ensuring precise management of automation processes and compliance standards.

Also, these actions significantly contribute to efficient workflow automation, offering adaptability and tailored solutions to unique organizational needs while fostering code organization and better maintenance.

Let's dive into various ways of writing a custom GitHub Action and provide a practical example of each.

## How to Write a Custom GitHub Action?

[![Diagram that displays the three types of GitHub Actions; Docker, JavaScript, and composite run steps actions.](https://learn.microsoft.com/en-us/training/github/create-custom-github-actions/media/action-types.png align="left")](https://learn.microsoft.com/en-us/training/modules/create-custom-github-actions/create-custom-github-action)

There are three ways to write a GitHub Action.

1. **Docker actions: -** If you're Docker enthusiast, just like me, you'll find this one very interesting since anything that can be containerized can also be shipped as a GitHub Action. But they are slower than JS actions, because the job has to build and retrieve the container.
    
2. **JavaScript actions: -** JavaScript actions can run directly on the runner machine and separate the action code from the environment that's used to run the action. Because of this, the action code is simplified and can execute faster than actions within a Docker container.
    

**Composite run steps actions: -** Last but not least, my very favourite one, steps actions allow you to reuse actions by using shell scripts, multiple automated tasks can easily be turned into an action and reuse them for different workflows.

## **Common Definition: Metadata and syntax**

Whether we are creating actions using JS, Shell or Docker, one thing of very importance is `action.yml`, this file is to assess which inputs, outputs, description, runs, and other configuration information is required by the action, and it should pe present at the root of your project.

| Parameter | Description | Required |
| --- | --- | --- |
| Name | The name of your action. Helps visually identify the action in a job. | yes |
| Description | A summary of what your action does. | yes |
| Inputs | Input parameters enable you to specify data that the action expects to use during runtime. These parameters become environment variables in the runner. | optional |
| Outputs | Output parameters enable you to specify data that subsequent actions can use later in the workflow after the action that defines these outputs has run. | optional |
| Runs | The command to run when the action executes. | yes |
| Branding | Colour and Feather icon to use to create a badge to personalize and distinguish your action in GitHub Marketplace. | optional |

`Action.yml` file has three main components; rest all are optional. This definition tells the [GitHub Runner](https://github.com/actions/runner) how to run your Action, acting as the entry point; without it, the logic defined in the repository will not be executed since the runner doesn't know how.

## **Writing Your First GitHub Action Using Docker**

Containerized applications have gained a lot of attention in a last decade and for good reasons. They allow you to write your application in the any programming language of your choice, put it in its own "container," and ship it everywhere.

Now let us jump to code part, where we will develop an actual GitHub Action.

### Requirement: Comment the Changes of PR

Here we'll write a Js app that does the job by using `@actions/core`, `@actions/github` libraries.

```javascript
const core = require('@actions/core');
const github = require('@actions/github');

async function run() {
  try {
    const owner = core.getInput('owner', { required: true });
    const repo = core.getInput('repo', { required: true });
    const pr_number = core.getInput('pr_number', { required: true });
    const token = core.getInput('token', { required: true });

    const octokit = new github.getOctokit(token);

    const { data: changedFiles } = await octokit.rest.pulls.listFiles({
      owner,
      repo,
      pull_number: pr_number,
    });

    let diffData = {
      addition: 0,
      deletions: 0,
      changes: 0
    };

    diffData = changedFiles.reduce((acc, file) => {
      acc.additions += file.additions;
      acc.deletions += file.deletions;
      acc.changes += file.changes;
      return acc;
    }, diffData);

    await octokit.rest.issues.createComment({
      owner,
      repo,
      issue_number: pr_number,
      body: `
        Pull request #${pr_number} has be updated with: \n
        - ${diffData.changes} changes \n
        - ${diffData.additions} additions \n
        - ${diffData.deletions} deletions
      `
    });

    for (const file of changedFiles) {
      const fileExtention = file.filename.split('.').pop();
      let label = '';
      switch(fileExtention) {
        case 'md':
          label = 'markdown';
          break;
        case 'js':
          label = 'javascript';
          break;
        case 'yml':
          label = 'yaml';
          break;
        case 'yaml':
          label = 'yaml';
          break;
        default:
          label = 'noextension';
      }
      await octokit.rest.issues.addLabels({
        owner,
        repo,
        issue_number: pr_number,
        labels: [label]
      });
    }
  } catch (error) {
    core.setFailed(error.message);
  }
}

run();
```

As you can see, there is no complexity to the code, it'll receive the required inputs, and comment it accordingly on the PR.

### **Containerize the Application**

A `Dockerfile` is the key that allows us to run the app inside an isolated environment, anywhere, anytime, and on any operating system.

```dockerfile
FROM node:18

COPY package*.json ./

RUN npm install

COPY . .

CMD [ "node", "index.js" ]
```

## **Define the GitHub Action**

We have only written our logic up until now, but it cannot be used in a GitHub Action without a well & proper defined `action.yml` and that's where the next step comes in.

```yaml
name: 'pr-comment'
description: 'Adds PR changes as comment'
inputs:
  owner:
    description: 'The owner of the repository (user or org)'
    required: true
  repo:
    description: 'The repository name'
    required: true
  pr_number:
    description: 'The number of the pull request'
    required: true
  token:
    description: 'The token to use to access the GitHub API'
    required: true
    default: ${{ github.token }}
runs:
  using: 'docker'
  image: 'Dockerfile'
```

> `github.token`: - A token to authenticate on behalf of the GitHub App installed on your repository. This is **functionally equivalent** to the GITHUB\_TOKEN secret.

### **Publish to GitHub Marketplace**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1705047871798/cca3ba69-d659-4c88-b487-357bbf397cd6.png align="center")

Upon such GitHub release, you and others will be able to use the GitHub Action using the following YAML-formatted workflow placed under the `.github/workflows/` directory of the repository.