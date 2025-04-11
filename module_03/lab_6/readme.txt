##Lab_6

## Install git-clone and flake8 task

    tkn hub install task git-clone
    tkn hub install task flake8

## Apply tasks and pvc yamls

    kubectl apply -f tasks.yaml
    kubectl apply -f pvc.yaml

## Check all tasks are installed

    tkn task ls

## Check if clusterTask has openshift-client installed

    tkn clustertask ls

## Reference openshift task in pipeline.yaml (deploy task)

    ### Indicate that the kind: property is ClusterTask
        otherwise it wont work (only if installed in the cluster)

## Set the parameters for the openshift-client task.
## in this case the parameter is SCRIPT and the value
   is 

    oc create deployment {name} --image={image-name}

## Add app-name parameter to the whole pipeline, this parameter is
## used in the openshift-client command..


## Apply the pipeline (pipeline.yaml)

## Run the pipeline


    tkn pipeline start cd-pipeline \
    -p repo-url="https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode.git" \
    -p branch=main \
    -p app-name=hitcounter \
    -p build-image=image-registry.openshift-image-registry.svc:5000/$SN_ICR_NAMESPACE/tekton-lab:latest \
    -w name=pipeline-workspace,claimName=pipelinerun-pvc \
    --showlog

## Check status

    tkn pipelinerun ls

## Check last pipeline execution logs

    tkn pipelinerun logs --last

## The follwing line indicates that the application was deployed successfully

   [deploy : oc] deployment.apps/hitcounter created 

## Check the deployment (running state)

    kubectl get all -l app=hitcounter

    NAME                              READY   STATUS    RESTARTS   AGE
pod/hitcounter-599585f774-xfmz5   1/1     Running   0          3m7s

NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hitcounter   1/1     1            1           3m8s

NAME                                    DESIRED   CURRENT   READY   AGE
replicaset.apps/hitcounter-599585f774   1         1         1       3m8s
