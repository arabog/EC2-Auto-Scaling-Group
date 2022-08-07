# EC2 AUTO SCALING GROUP

## Introduction
This project entails using Auto Scaling to automatically launch Amazon EC2 
instances in response to the conditions being specified. And how auto-scaling
automatically provisions replacement instance(s).

![autoscalinggrp](EC2-Auto-Scaling-Grp.png?raw=true "autoscalinggrp")

## Definition
An Autoscaling group contains *a collection of EC2 instances that share similar 
characteristics* and are treated as a logical group.
It helps to maintain application availability and allows scaling of Amazon EC2 
capacity up or down automatically according to conditions defined. Auto Scaling 
helps to ensure that the desired number of Amazon EC2 instances are running 
during demand spikes to maintain performance and decrease capacity during 
lulls to reduce costs.

All EC2 instances that are provisioned as a part of auto-scaling have the *same 
configuration* because they are instantiated from a *Launch template*.

## Preparatory Steps
Sign in to the AWS Management Console.
Navigate to the EC2 dashboard. On the left pane, click Auto Scaling groups under 
Auto Scaling section.
In this exercise, we will use the following two services available in the EC2 dashboard:  
For Instances → *Launch templates*  
For Auto Scaling → *Auto Scaling Groups*  

## Stage 1. Create a Launch template
A Launch template specifies instance configuration information, such as, AMI ID, the instance type, a key pair, security groups, and the other parameters that you use to launch EC2 instances.

![launchtemplate](launch-template.png?raw=true "launchtemplate")

Go to the Launch templates service under Instances section on the left pane, and 
create a new template by specifying the following parameters:  
Template name, and description of your choice.  
Auto Scaling guidance: Provide guidance to help me set up a template that I 
can use with EC2 Auto Scaling  
Template contents: AMI ID: Amazon Linux 2 AMI (HVM), SSD volume type  
Instance type: t2.micro  
Key-pair for logging in to the EC2 instances: Your choice of key-pair  
Network settings: Choose a Virtual Private Cloud (VPC), and a subnet(NB: Remove subnet for EC2 Auto Scaling templates) in which the network interface is located. Choose a security group accordingly, because this step will ensure that a public IP address is assigned automatically to every instance.  
Subnet: Don't include in launch template. Why? When you specify a subnet, a network interface is automatically added to your template.  
Storage (volumes), tags, and network interfaces: Default  

![template-created](template-created.png?raw=true "template-created")

NB: Notice the Next steps in the above image

## Stage 2. Create an Autoscaling group
Go to the Auto Scaling Groups service, and create a new Autoscaling group. 
Creating an Autoscaling group is a multi-step process, in which you specify the configuration settings, group size, scaling policies (step/simple/target scaling 
policy), notifications, and tags. 

Specify the following minimal set of configurations:  
Choose a Launch template that you have created in the previous step.  
Provide the VPC, and subnets in which you want to create your EC2 instances.  
Specify the Group size with the desired, minimum, and maximum capacity, i.e., 
count of EC2 instances at any given moment. You can choose the default value, 
1, for each capacity.  
Use the default values in the remaining steps of this process.  






