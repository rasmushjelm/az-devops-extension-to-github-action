# Migrate Azure Devops extension to Github Action
This is a guide on how to make your Azure Devops extensions work on github.

## Getting started
If you have limited knowledge of creating an action on github, create a new repository and follow this guide to get started. https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action  

### Typescript action template
You can use this template as a good starting point or reference when turning your extension into an action
https://github.com/actions/typescript-action

## Turn Task into Action
- Start by removing any Azure Devops specific code that is not needed and make sure that your dependencies aren't bringing in any vsts-task-lib references, as that could potentially lead into unexpected problems during runtime.
- Change your references to “vsts-task-lib/task” to "@actions/core" and then update all the related function calls to:  
core.getInput()  
core.debug()  
core.warning()  
core.SetFailed()  

### turn task.json into action.yml

https://pipelinestoactions.azurewebsites.net/
