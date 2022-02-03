# Migrate Azure Devops extension to Github Action
This is a guide on how to make your Azure Devops extensions work on github.

## Getting started
If you have limited knowledge of creating an action on github, create a new repository and follow this guide to get started. https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action  

### Typescript action template
You can use this template as a good starting point or reference when turning your extension into an action
https://github.com/actions/typescript-action

## Turn Task into Action
- Start by removing any Azure Devops specific code that is not needed and make sure that your dependencies aren't bringing in any vsts-task-lib references, as that could potentially lead into unexpected problems during runtime.
- At your terminal, install the actions toolkit core and github packages.
```
npm install @actions/core
npm install @actions/github
```
- Change your references to “vsts-task-lib/task” to "@actions/core" and then update all the related function calls to:  
core.getInput()  
core.debug()  
core.warning()  
core.SetFailed()  

### Turn task.json into action.yml
Create an action.yml file and migrate your manifest from task.json into the action.yml file.
The task.json file will most likely have fields that are not available in the action.yml file. Ignore those when converting it into action.yml.  
Action syntax can be found here: https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions  
#### Action.yml example:
```
name: 'Your name here'
description: 'Provide a description here'
author: 'Your name or organization here'
inputs:
  who-to-greet: # change this
    required: true
    description: 'input description here'
    default: 'default value if applicable'
runs:
  using: 'node12'
  main: 'dist/index.js'
```
## Setup Workflow
Test your newly created action, add a main.yml file '.github/workflows/main.yml'  
When an action is in a private repository, the action can only be used in workflows in the same repository. Public actions can be used by workflows in any repository.

#### Private action workflow example:
```
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      # To use this repository's private action,
      # you must check out the repository
      - name: Checkout
        uses: actions/checkout@v2
      - name: Hello world action step
        uses: ./ # Uses an action in the root directory
        id: hello
        with:
          who-to-greet: 'Mona the Octocat'
```

#### useful link
a website that converts Pipelines to Actions:
https://pipelinestoactions.azurewebsites.net/
