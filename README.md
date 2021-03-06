# Setup IBM MQ with Tekton Pipeline on Openshift

## Requirements

1. Openshift 4.6
2. [IBM Cloud Pak for Integration V2021.2.1](https://www.ibm.com/common/ssi/ShowDoc.wss?docURL=/common/ssi/rep_ca/1/899/ENUSLP21-0291/index.html)
3. [IBM MQ V 9.2.3.0-r1 and mq.ibm.com/v1beta1 Operator ibm-mq.v1.6.0](https://www.ibm.com/support/knowledgecenter/SSFKSJ_9.2.0/com.ibm.mq.ctr.doc/ctr_api_v1beta1_QueueManager.htm)
4. A namespace called where the MQ Operator ^ is installed
5. An entitlement key called `ibm-entitlement-key` [Get from here for IBM Employees](https://myibm.ibm.com/products-services/containerlibrary)
6. A Github token [see here for creating one](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)


# Steps

## Step 1: Fork this repository 
- Follow [GitHub’s](https://docs.github.com/en/github/getting-started-with-github/fork-a-repo_) documentation on how to fork a repo to create your own fork of this repo.
## Step 2: Clone the forked repository onto your machine
## Step 3: Modify and run the install script 
1. Open the file `./install/install.sh` and insert your namespace where MQ is installed (chopperxx...) also your Git token and git usernname

    - `MQ_NS`= 
    - `PN_NS`= 
    - `GIT_TOKEN`=paste git token here and remove brackets
    - `GIT_USERNAME`=paste github username here and remove brackets
   
2. Update the Pipeline `mq-pipeline` (`/tekton/pipelines/mq-pipeline.yaml`) and change the `TARGET_NAMESPACE` to be your namespace where MQ is installed (chopperxx...), change 'QM_NAME' and 'QM_POD_NAME' to your queue manager name

3. Update the `PipelineResouce` to point to the url of your forked repository
    - update line 8 of this file `./tekton/resources/mq-git-repo-resource.yaml` and change the `value`
4. Make the install script executable `chmod +x ./install/install.sh`
5. Run the install script `./install/install.sh`

## Step 4: Add the route to Github WebHook

[Follow steps here to add a webhook](https://docs.github.com/en/developers/webhooks-and-events/creating-webhooks#payload-url)

[Or here for pictures](https://developer.ibm.com/tutorials/tekton-triggers-101/)

- Open your fork of the this repository in GitHub and navigate to the Webhooks menu. Then click the Add webhook button.

- Set the Payload URL to the EventListener Route: `oc get route el-el-cicd-mq-hook-route --template='http://{{.spec.host}}'`
- Set the Content type to application/json.
- You aren’t using the Secret in this tutorial, so you can leave it blank.
- Select **Just the Push Event**

# Step 5: Test it out by commiting a change!
