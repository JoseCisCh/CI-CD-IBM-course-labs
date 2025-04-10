## Install git-clone and flake8

    tkn hub install task git-clone
    tkn hub install task flake8

## Apply tasks.yaml

    kubectl apply -f tasks.yaml

## Check all tasks are installed

    tkn task ls

## Check if the task needed is already present in the ClusterTask

    tkn clustertask ls

## Add cluster to task in pipeline.yaml

    ### For this it was needed to add a new parameter to the whole pipeline
        called build-image. This build-image is passed to buildah as IMAGE
        for it to make the build

## Apply the pipeline.yaml and the pvc.yaml

    kubectl apply -f pipeline.yaml
    kubectl apply -f pvc.yaml

## Run the pipeline

    tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git" \
    -p branch=main \
    -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:latest \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog

## Check status

    tkn pipelinerun ls

## Check last pipeline execution logs

    tkn pipelinerun logs --last
