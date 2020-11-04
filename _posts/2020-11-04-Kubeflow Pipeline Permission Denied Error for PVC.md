---
layout: post
title: Kubeflow Pipeline Permission Denied Error for PVC
--- 

Scenario: 
Running pipeline from Kubeflow Notebook server
Error : Permission Denied while writing files on PVC
Reason : Folders on PVC created with root user group, pipeline runs as another user 
Solution/Temporary Workaround : New Container-OP with bash image and linux commands executed to change folder permission. Similar to initContainer
>
Solution File : <a href='https://github.com/kinjaldand/kinjaldand.github.io/blob/master/codefiles/KF_Fashion_MNIST%20(1).ipynb'>Notebook</a>
