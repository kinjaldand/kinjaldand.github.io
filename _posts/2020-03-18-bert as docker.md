---
layout: post
title: Run Bert as service using docker
--- 

This post gives steps to run Bert as a service using Docker


1. Download repo and model
  1.1. git clone https://github.com/hanxiao/bert-as-service.git
  1.2. mkdir bertmodels
  1.3. ls bertmodels
  1.4. wget https://storage.googleapis.com/bert_models/2018_10_18/uncased_L-24_H-1024_A-16.zip
  1.5. unzip uncased_L-24_H-1024_A-16
2. Install docker
  2.1 Follow required ssteps from https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04
3. Runtime NVDIA for docker
  3.1 Follow from https://stackoverflow.com/questions/59008295/add-nvidia-runtime-to-docker-runtimes
  3.2 Run commnds from https://nvidia.github.io/nvidia-container-runtime/
  3.3 Run commands from https://github.com/NVIDIA/nvidia-container-runtime#installation
4. Run docker run command
5. Test with bert as client
