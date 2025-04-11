# lab_1
# This lab wont have a lot of code as we will use the openshift console in order
# to define pipelines and configurate all.
# Tough some CLI commands are used and I will detail each one of them.

## Ensure connection with OpenShift cluster

    oc config current-context

## The lab requires the installation of tasks created in the previous labs.
## Only those tasks that we created as there was no available version in
## Tekton tasks.

    ### Create tasks.yaml file and paste the provided code.

    ### Apply tasks.yaml

    ### Validate all tasks are present
        
        oc get tasks

## Go to Open OPenShigt console.

    ### In editor go to Skills Network toolbox -> Cloud -> OPenShift Console

## Go to administrator perspective ( change from developer to administrator )

## Go to storage -> PersistentVolumeClaims

    ### An error occurred for this task, just close OPenShift console and reopen

    ### Create PersistentVolumeClaim

        StorageClass: skills-network-learner
        PersistantVolumeClass: oc-lab-pvc
        Size: 1GB
        Single user (RWO)
        Filesystem

## Go back to developer perspective

## Go to pipelines and Create a new pipeline    

    ### Be sure to use pipeline builder and name the pipeline ci-cd-pipeline

    ### Create a new workspace called output (will be used by clone task)

## Before we made sure that out tasks (cleanup and nose) were added 
## using kubectl apply -f tasks.yaml

    ### Add this task in the pipeline builder

        #### Set the output workspace to this task

## Press create

## Start the pipeline
    
    ### Select actions dropdown -> start
    ### Select the type PersistentVolumeClaim and set it to our
    ### Previously generated pvc

    ### Start

    ### Check the logs for our tasks

## Add the clone task

   ### Add git-clone task from redhat 

    #### Add url (https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode)
    #### and the workspace.output to output

   ### Save, run the pipeline and check the logs

## Add the Flake8 task

   ### Add Flake8 task from redhat 

    #### Add image (python:3.9-slim)
    #### Add args: 
        --count
        --max-complexity=10
        --max-line-length=127
    #### Workspace.output = output

   ### Save, run the pipeline and check the logs

## Add the nose task

   ### Search for the nose task and install it and add it

    #### Workspace.output = output

   ### Save, run the pipeline and check the logs

## Add the buildah task

   ### Add buildah task from redhat ( probably will have to install)
    
   ### AS the namespace will be needed, in the lab environment console run
   ### echo $SN_ICR_NAMESAPCE (sn-labs-andrescischa)

   ### Add image, parametes and workspace for the task

    #### Add image ($(params.build-image))
    #### Workspace.output = output

   ### Click in the main page to close buildah config and add parameters in the 
   ### pipeline (not specific to the task)
    #### param name = build-name
    #### param default = image-registry.openshift-image-registry.svc:5000/SN_ICR_NAMESPACE/tekton-lab:latest
    #### Replace SN_ICR_NAMESPACE with the value above (sn-labs-andrescischa)

   ### Save, run the pipeline and check the logs

## Add the deploy task to OpenShift cluster using OpenShift client
## (oc deploy)

   ### Add openshift-client task from redhat ( probably will have to install)
    
   ### AS the namespace will be needed, in the lab environment console run
   ### echo $SN_ICR_NAMESAPCE (sn-labs-andrescischa)

   ### Add display name, SCRIPT

    #### display-name (deploy)
    #### SCRIPT = oc create deployment $(params.app-name) --image=$(params.build-image) --dry-run=client -o yaml | oc apply -f -

   ### Click in the main page to close buildah config and add parameters in the 
   ### pipeline (not specific to the task)
    #### param name = app-name
    #### param default = cicd-app

   ### Save, run the pipeline and check the logs

## Validate application 

    ### In the developer perspective go to Topology panel. Two applications 
        should appear. Click cicd-app.

    ### Go to Logs

        #### A message like SERVICERUNNING in the logs must appear indicating
             that the application is running
