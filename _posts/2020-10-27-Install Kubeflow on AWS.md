---
layout: post
title: Install Kubeflow on AWS - Step by Step Guide 
--- 



# 1. Prerequites:
# 1.1 Create Kubernetes Cluster on EKS by following steps described in below link.
Note cluster name <cluster_name> and region <region-code>, to be used to configure kubectl in coming steps.
Ensure kubernetes nodes can support atleast 45 pods if following below version of yaml file of kubeflow in step 2. this can be achieved with relevant minimum and maximum sizes of nodes in add nodes steps described below.
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html
    
# 1.2 Install aws cli and configure it with aws user credentials.
You can refer below link for ubuntu 18
https://linuxhint.com/install_aws_cli_ubuntu/
    
# 1.3 Install and Configure kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl/

https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html
    aws --version
    aws eks --region <region-code> update-kubeconfig --name <cluster_name>
    kubectl get svc
    
Result will contain ClusterIp for <cluster_name> . If you want to manage kubeflow via UI you will also need load balancer to assign IP address to istio. You can follow below step for same.
       
# 2. Kubeflow Installation
# 2.1. Download Kubeflow tar from below link
https://www.kubeflow.org/docs/aws/deploy/install-kubeflow/
    
    tar -xvf kfctl_v1.1.0_<platform>.tar.gz

Add kfctl to PATH, to make the kfctl binary easier to use.

    export PATH=$PATH:"<path to kfctl>"

# 2.2. Use the following kfctl configuration file for the AWS setup without authentication:
    export CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v1.1-branch/kfdef/kfctl_aws.v1.1.0.yaml"

Alternatively, use the following kfctl configuration if you want to enable authentication, authorization and multi-user:
    export CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v1.1-branch/kfdef/kfctl_aws_cognito.v1.1.0.yaml"

# 2.3. Set an environment variable for your AWS cluster name.
    export AWS_CLUSTER_NAME=<YOUR EKS CLUSTER NAME>

# 2.4. Create the directory you want to store deployment, this has to be ${AWS_CLUSTER_NAME}
**${AWS_CLUSTER_NAME} - The name of your eks cluster. This will be picked by kfctl and set value to metadata.name. alb-ingress-controller requires correct value to provision application load balancers. Alb will be only created with correct cluster name.

    mkdir ${AWS_CLUSTER_NAME} && cd ${AWS_CLUSTER_NAME}

# 2.5. Download your configuration files, so that you can customize the configuration before deploying Kubeflow.
    wget -O kfctl_aws.yaml $CONFIG_URI

# 2.6 Get Rolename for authentication
    aws iam list-roles \
        | jq -r ".Roles[] \
        | select(.RoleName \
        | startswith(\"eksctl-$AWS_CLUSTER_NAME\") and contains(\"NodeInstanceRole\")) \
        .RoleName"
    
# 2.7 Apply yaml file    
    kfctl apply -V -f kfctl_aws.yaml

# 2.8 Wait for all the resources to become ready in the kubeflow namespace.
    kubectl -n kubeflow get all


# 2.9 Access Kubeflow central dashboard
Run the following command to get your Kubeflow service’s endpoint host name and copy link in browser.

    kubectl get ingress -n istio-system
Paste the address value in yor browser to view Kubeflow UI
    
# 3 References:
1. https://www.oreilly.com/library/view/kubeflow-operations-guide/9781492053262/
2. https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html
3. https://www.kubeflow.org/docs/aws/deploy/install-kubeflow/
4. https://kubernetes.io/docs/tasks/tools/install-kubectl/
5. https://linuxhint.com/install_aws_cli_ubuntu/


# 4 Install SDK Pipeline to run ml models
pip3 install kfp --upgrade
export PATH=$PATH:~/.local/bin
