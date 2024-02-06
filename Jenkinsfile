#!groovy

@Library('jenkins-shared-library') _

def configMap = [
    application: "nodejsVM"
    component: "catalogue"
]

if (!env.BRANCH_NAME.equalsIgnoreCase('main')) {
    pipelineDecission.decidePipeline(configMap)
}
else {
    echo "This is PRODUCTION, deal with CR process"
}