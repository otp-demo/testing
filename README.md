# Testing


## Prerequisites

* when first deploying a hub cluster no apps should be included. Comment out the apps repository in `0-bootstrap/single-cluster/kustomize.yaml`

* There are several necessary components for EVERY RHACM install in 1-infra and 2-services that MUST be deployed. They are as follows: 
1- infra: 
    - argocd/consolenotification.yaml
    - argocd/namespace-sealed-secrets.yaml
    - argocd/namespace-openshift-acm.yaml
    - argocd/namespace-openshift-acm-observability.yaml
    - argocd/namespace-openshift-rhacm-credentials.yaml
    - argocd/namespace-policies.yaml
    - argocd/namespace-sso.yaml
    - argocd/namespace-sso-integration.yaml
* Other items may be deployed as needed (according to comments and READMEs) but you should comment out the things you do not need
2-services: 
     - argocd/operators/cert-manager.yaml
     - argocd/instances/cert-manager.yaml
     - argocd/instances/sealed-secrets.yaml
     - argocd/operators/external-secrets.yaml
     - argocd/operators/openshift-pipelines.yaml
     - argocd/operators/openshift-acm.yaml
     - argocd/instances/openshift-acm-instance.yaml
     - argocd/instances/openshift-acm-observability-instance.yaml
 
## Installing 

* The script to modify infra currently only works on MacOS    

### Installing and Configuring OpenShift GitOps 

* Creating RoleBindings and ArgoCD instances may show a 'no resource found' error message, but ultimately the command does work - you can check inside the cluster or simply run the command again to verify. 
                                           

### GitHub Tokens 
* Committing tokens to GitHub will cause an issue (token found in commit has been revoked) - be sure that you remove any tokens, or add them to a GitIgnore before committing 

## Dependencies 


## Issues

* Git set and unset scripts will not work for any forks other than direct forks of the upstream repo 
    * This issue is currently being resolved but ultimately this step must be performed manually 
* RHACM health check is not correct for ACM 
* Azure healthchecks show sync ok even while the provision job is running. Might be better to have this app show as syncing until the job is finished
* AWS Healthcheck shows healthy and synced before the cluster is provisioned. This causes sync wave issues that need to be accounted for in following jobs       
* We need to document EXACTLY what (if any) values Helm/Kustomize expects for each app  
* the ClusterSet is created and clusters are added, but they cannot be moved between ClusterSets (as the label is configured at time of provisioning)
* The clusterset may not show up in RHACM until the ClusterSet object is created - we need to create the clusterset in RHACM 

### Resolutions 

* create.sh script (make_template.sh script) now correctly adjusts the values.yaml file for aws
* Submariner now waits for a cluster as well as a submariner deployment prior to applying a NAT patch
* RHSSO Sync waves adjusted 
* We need to be careful of elastic IP limits in AWS and region constraints in Azure. Might be best to start with a decently clean slate before we film demos 

* Infra script not tabbing values cirrectly
```
ComparisonError
rpc error: code = Unknown desc = `kustomize build /tmp/https___github.com_otp-demo-test_otp-gitops/0-bootstrap/single-cluster/1-infra --enable-alpha-plugins` failed exit status 1: Error: error converting YAML to JSON: yaml: line 34: did not find expected '-' indicator
``` 
* resolved by tabbing infra deployments correctly

