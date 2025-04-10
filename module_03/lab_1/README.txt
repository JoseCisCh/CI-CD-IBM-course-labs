Instruction to execute each one of the laboratiores

# Lab 2

## Setting forwward port to port 8090
## this is important to call in localhost

    kubectl port-forward service/el-cd-listener  8090:8080

## In a second terminal execute the following curl command:

    curl -X POST http://localhost:8090 \
      -H 'Content-Type: application/json' \
      -d '{"ref":"main","repository":{"url":"https://github.com/ibm-developer-skills-network/wtecc-CICD_PracticeCode"}}'

## Check the command status with 

    tkn pipelinerun ls

## Examine the PipelineLogs with the following command
## -L/--last option is for the last.

    tkn pipelinerun logs --last
