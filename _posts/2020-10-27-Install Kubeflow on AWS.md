---
layout: post
title: Install Kubeflow on AWS - Step by Step Guide 
--- 



<h3>1. Prerequites:</h3>
<h4> 1.1 Create Kubernetes Cluster on EKS by following steps described in below link.</h4> 
Note cluster name <cluster_name> and region <region-code>, to be used to configure kubectl in coming steps.
Ensure kubernetes nodes can support atleast 45 pods if following below version of yaml file of kubeflow in step 2. this can be achieved with relevant minimum and maximum sizes of nodes in add nodes steps described below.
    <a href='https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html'>getting-started-console</a>
    
<h4> 1.2 Install aws cli and configure it with aws user credentials.</h4>
You can refer below link for ubuntu 18
<a href='https://linuxhint.com/install_aws_cli_ubuntu/>Install AWS</a>
    
<h4> 1.3 Install and Configure kubectl </h4>
<a href='https://kubernetes.io/docs/tasks/tools/install-kubectl/'>Install kubectl</a>

<a href='https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html'>Configure kubectl</a>
    aws --version
    aws eks --region <region-code> update-kubeconfig --name <cluster_name>
    kubectl get svc
    
Result will contain ClusterIp for <cluster_name> . If you want to manage kubeflow via UI you will also need load balancer to assign IP address to istio. You can follow below step for same.
       
<h3> 2. Kubeflow Installation </h3>
<h4> 2.1. Download Kubeflow tar from below link</h4>
<a href='https://www.kubeflow.org/docs/aws/deploy/install-kubeflow/'>Install kubeflow</a>
    
    tar -xvf kfctl_v1.1.0_<platform>.tar.gz

Add kfctl to PATH, to make the kfctl binary easier to use.

    export PATH=$PATH:"<path to kfctl>"

<h4> 2.2. Use the following kfctl configuration file for the AWS setup without authentication:</h4>
    export CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v1.1-branch/kfdef/kfctl_aws.v1.1.0.yaml"

Alternatively, use the following kfctl configuration if you want to enable authentication, authorization and multi-user:
    export CONFIG_URI="https://raw.githubusercontent.com/kubeflow/manifests/v1.1-branch/kfdef/kfctl_aws_cognito.v1.1.0.yaml"

<h4> 2.3. Set an environment variable for your AWS cluster name.</h4>
    export AWS_CLUSTER_NAME=<YOUR EKS CLUSTER NAME>

<h4> 2.4. Create the directory you want to store deployment, this has to be ${AWS_CLUSTER_NAME} </h4>
**${AWS_CLUSTER_NAME} - The name of your eks cluster. This will be picked by kfctl and set value to metadata.name. alb-ingress-controller requires correct value to provision application load balancers. Alb will be only created with correct cluster name.

    mkdir ${AWS_CLUSTER_NAME} && cd ${AWS_CLUSTER_NAME}

<h4> 2.5. Download your configuration files, so that you can customize the configuration before deploying Kubeflow. </h4>
    wget -O kfctl_aws.yaml $CONFIG_URI

<h4> 2.6 Get Rolename for authentication </h4>
    aws iam list-roles \
        | jq -r ".Roles[] \
        | select(.RoleName \
        | startswith(\"eksctl-$AWS_CLUSTER_NAME\") and contains(\"NodeInstanceRole\")) \
        .RoleName"
    
<h4> 2.7 Apply yaml file    </h4>
    kfctl apply -V -f kfctl_aws.yaml

<h4> 2.8 Wait for all the resources to become ready in the kubeflow namespace. </h4>
    kubectl -n kubeflow get all


<h4> 2.9 Access Kubeflow central dashboard </h4>
Run the following command to get your Kubeflow service’s endpoint host name and copy link in browser.

    kubectl get ingress -n istio-system
Paste the address value in yor browser to view Kubeflow UI
    
<h3> 3 References: </h3>
1. <a href='https://www.oreilly.com/library/view/kubeflow-operations-guide/9781492053262/'>https://www.oreilly.com/library/view/kubeflow-operations-guide/9781492053262/</a>
2. <a href='https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html'>https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html</a>
3. <a href='https://www.kubeflow.org/docs/aws/deploy/install-kubeflow/'>https://www.kubeflow.org/docs/aws/deploy/install-kubeflow/</a>
4. <a href='https://kubernetes.io/docs/tasks/tools/install-kubectl/'>https://kubernetes.io/docs/tasks/tools/install-kubectl/</a>
5. <a href='https://linuxhint.com/install_aws_cli_ubuntu/'>https://linuxhint.com/install_aws_cli_ubuntu/</a>


<h3> 4 Install SDK Pipeline to run ml models </h3>
pip3 install kfp --upgrade
export PATH=$PATH:~/.local/bin
