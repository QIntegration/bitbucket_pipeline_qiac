# Qualys IaC Bitbucket pipeline script

## Description
Qualys IaC Bitbucket pipeline script is used to scan the Infrastructure-as-Code templates in your Bitbucket repository using Qualys CloudView (Cloud Security Assessment). It checks for security issues using the Qualys Cloud Infrastructure as Code Scan and displays the failed checks as pipeline annotations.

Note: Qualys IaC Bitbucket pipeline script supports the below file formats for scanning.
* Terraform supported extensions: `.tf`, `.json`
* CloudFormation supported extensions: `.template`, `.yml`, `.yaml`


## How to use the Qualys IaC Bitbucket pipeline script

1. Bitbucket cloud provides a pipelines option where we can add Qualys IaC Bitbucket Pipeline Script to check security issues.
2. Subscribe to Qualys CloudView and obtain Qualys credentials.
3. Create repository variables on the bitbucket server for Qualys URL, Qualys Username, and Qualys Password.
Refer to [repository variables](https://support.atlassian.com/bitbucket-cloud/docs/variables-and-secrets/#Variablesinpipelines-Repositoryvariables) for more details on how to setup repository variables.
4. Copy bitbucket-pipelines.yml script from `QIntegration/bitbucket_pipelines@main` into your bitbucket cloud repo.
5. Now every event of repo scan will trigger for security check.


## Qualys IaC Bitbucket pipeline script

```
image: qualys/qiac_security_cli

pipelines:
  custom: 
    qualys: 
      - step:
          script:
            - export ScheduleBuildTrigger=true
            - sh /bitbucket.sh $ScheduleBuildTrigger
  default:
    - step:
        name: Qualys
        caches:
          - pip
        script:
          - export ScheduleBuildTrigger=false
          - sh /bitbucket.sh $ScheduleBuildTrigger 

```

* The pipeline script runs on Qualys qiac docker image from docker hub.
* In the above script custom Qualys section will be triggered only manually or by a scheduled event. And for all other events default section will execute.
* Configure ScheduleBuildTrigger to true if you want the pipeline script to trigger as per your schedule.
