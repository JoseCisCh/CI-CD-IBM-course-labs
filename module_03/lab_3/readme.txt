# Lab 3

## Add the git-clone task

    tkn hub install task git-clone --version 0.8

    ### In case there was an error with the previous command use

        kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml

## Create the persistent volume claim (pvc.yaml)

## Apply the pvc

    kubectl apply -f pvc.yaml

## Add the workspace to the Pipeline (pipeline.yaml)

## Running the pipeline

    ### We can now run the popeline creating a Pipeline run
    ### In the following command we bind the workspace to the pvc-claim.
    ### and we also and all the input parameters to the pipeline

        tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git" \
    -p branch="main" \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog    

    ### Check the pipeline status

        tkn pipelinerun ls

    ### Check logs of the last run

        tkn pipelinerun logs --last
