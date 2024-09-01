

W - Workflow
E - Events (Condition that triggers the workflow)
T - Trigger
J - Jobs (are made up of shell script or action)
S - Steps
R - Runners

To define workflows to run on GitHub Actions runners based on events, create a YAML file inside the .github/workflows directory of the repository. 

For our example, we define a .github/workflows/first-actions.yaml file.

# Events 

A PR, push a branch, an issue, can all be termed as events, which trigger the pipeline

Events trigger the workflow

# Workflow name

The first thing we should do in our workflow file is define the workflow name, but if you omit the name field in your workflow file, GitHub Actions will use the path of the workflow file relative to the root of the repository as the default name. This default name is usually the path of the file, including the directory and file name.

In our example, the name is set with the following line:

    name: My First GitHub Actions

If we were to omit the name, in the configuration file, this would default to .github/workflows/first-actions.yaml.

# Triggers

Then, we define the triggers. This is achieved by setting different options for the 'ON' keyword. 
This is done here:

    on: [push]


Another eg where we want to trigger the pipeline if there is a push or pull_request, any of them targeting the main branch and any changes on the path terraform/**, we can write trigger in this way.

on:
 push:
   branches:
   - main
   paths:
   - terraform/**
 pull_request:
   branches:
   - main
   paths:
   - terraform/**


This is the trigger event. An event is a activity that triggers a workflow run. For eg, when someone creates a PR, opens a issue, or pushes a commit to a repo.


# Jobs — consist of shell script or Actions

We use the keyword 'jobs' to define our single job with the keyword jobs. We set the type of machine to run the job on with runs-on and the default shell and working-directory with defaults.

A job is composed of a sequence of steps. Each step is a task that runs in its own process in the GitHub Actions runner environment.

NOTE - We can create multiple JOBS as well in one file, which means we can execute multiple pipelines in one file. For our purpose, we have created only one job. In jenkins we had used Docker as agent, and then use the image (ubuntu).

If your workflow has multiple jobs, each job will require runner.

For github actions, we can mention use ubuntu-latest image, and inside that, set up a python env. Our functionality has to be verfied on python 3.8, and 3.9 both

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: [3.8, 3.9]

Before mentioning the steps which a job will do, we have just mentioned the env whivh github should use, and it should have python version 3.8, 3.9. Then we write steps (which are stages in Jenkins). Like we have Build, Test, Deploy in Jenkins, similar thing we can do in Github as well by segregating our steps .

# Steps

Note - Steps run in sequential order, however jobs can run in parallel as they have independent runners. Defulat behvious is jobs run in parallel

First step we will do is checkout our code. We can find all code for this from github actions

   steps:
    - uses: actions/checkout@v3

This is a plugin, so it will remain similar for all. It means we are using version 3 of checkout plugin.

NOTE - We have to define which version of the plugin that we want to use.

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}

Github actions has a MARKETPLACE OF PLUGINS, and we can make use of these plugins. In jenkins , we have to go manage plugins, and install them. In github actions, plugins are installed by default. All we need is, whichever plugin we need, like actions/setup-python@v2, @ here means that its a version of the plugin. So here we using version 2 of setup-python plugin.





# SETTING - ACTIONS

We can configure our own runners as well, suppose we dont want to use github runner, we can click on New self-hosted runner, create our own runner (same as we created worker nodes in jenkins). This is commonly done when we are not happy with the default runners github is providing as they are light in nature, and dont have enough compute. In that case, we can create our own self hosted runners.


# Runners

Runners come in 2 flavours - Github hosted runner, and self hosted runners.