# Lab 04
## Apply tasks and install git-clone task

    kubectl apply -f tasks.yaml
    tkn hub install task git-clone

## Check correct installatino of git-clone task

    tkn task ls

## Establish workspace

    kubectl apply -f pvc.yaml

## Note on cleanup task.
## After pthon code compilation, some files .pyc are generated 
## and only operable/owned by the specific user
## As git-clone does not have these privileges to delete these .pyc files
## The cleanup task is created to make this process.

## Install flake8 task from TektonHub

    tkn hub install task flake8

## Add a workspace required by flake8 in the lint task in pipeline.yaml

## Change lint task, to set the expected parameters by the flake8 task (pipeline.yaml)

## Apply the pipeline

    kubectl apply -f pipeline.yaml

## Run the pipeline

    tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git" \
    -p branch="main" \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog

## Create new task to perform tests using nosetests

## apply tasks so the new nosetests task is included

    kubectl apply -f tasks.yaml

## Change pipeline.yaml so that the test step uses
## nosetests (pipeline.yaml)


## Run the tests

    tkn pipeline start cd-pipeline \
        -p repo-url="https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git" \
        -p branch="main" \
        -w name=pipeline-workspace,claimName=pipelinerun-pvc \
        --showlog

## Check status

    tkn pipelinerun ls

## Check last pipeline execution logs

    tkn pipelinerun logs --last
