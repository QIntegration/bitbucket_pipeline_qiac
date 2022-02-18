# Qualys IaC Bitbucket pipeline script

## Description
Qualys IaC Bitbucket pipeline script is used to scan the Infrastructure-as-Code templates from your Bitbucket repository using Qualys CloudView (Cloud Security Assessment). It checks for security issues using the Qualys Cloud Infrastructure as Code scan and displays the failed checks as pipeline annotations.

Note: Qualys IaC Bitbucket pipeline script supports the below file formats for scanning.
* Terraform supported extensions: `.tf`, `.json`
* CloudFormation supported extensions: `.template`, `.yml`, `.yaml`


## How to use the Qualys IaC Bitbucket pipeline script

1. Bitbucket cloud provides a pipelines option where we can add Qualys IaC Bitbucket Pipeline script to check security issues.
2. Subscribe to Qualys CloudView and obtain Qualys credentials.
3. Create repository variables on the Bitbucket server for Qualys URL, Qualys Username, and Qualys Password.
For more information on how to set up repository variables, refer to [repository variables](https://support.atlassian.com/bitbucket-cloud/docs/variables-and-secrets/#Variablesinpipelines-Repositoryvariables).
4. Copy bitbucket-pipelines.yml script from `QIntegration/bitbucket_pipelines@main` into your Bitbucket cloud repo.
5. Now, every event of repo scan will trigger for security check.


## Qualys IaC Bitbucket pipeline script

```
image: qualys/qiac_security_cli

pipelines:
  custom: 
    qualys: 
      - step:
          script:
            - export ScheduleBuildTrigger=true
            - sh /home/qiac/bitbucket.sh $ScheduleBuildTrigger
  default:
    - step:
        name: Qualys
        caches:
          - pip
        script:
          - export ScheduleBuildTrigger=false
          - sh /home/qiac/bitbucket.sh $ScheduleBuildTrigger 

```

* The pipeline script runs on Qualys qiac docker image from docker hub.
* From the above script, the custom Qualys section will be triggered manually or by a scheduled event. For all other events, the default section will be executed.
* Configure ScheduleBuildTrigger to true if you want the pipeline script to trigger as per your schedule.
