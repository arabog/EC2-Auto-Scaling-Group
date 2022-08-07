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

## Requirements
Load balancer
Target group

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

Provide the VPC, and subnets in which you want to create your EC2 instances.  
Specify the Group size with the desired, minimum, and maximum capacity, i.e., 
count of EC2 instances at any given moment. You can choose the default value, 
1, for each capacity.  
Use the default values in the remaining steps of this process.  

### Step 1: Choose launch template or configuration
Auto Scaling group name: your choice  

Launch template:  Click the refresh button in the Launch template box and select the template created in Step 1. Once selected, additional detail about the template will be visible. Change the Version from Default (1) to Latest (1). This change means that whenever we update this launch template and a new version is created in the future, our Auto Scaling group will automatically begin to use that latest version on the very next instance it needs to launch. Choose a Launch template that you have created in the previous step. 

### Step 2: Choose instance launch options
Select as many subnets as possible for maximum instance diversity.

### Step 3: Configure advanced options
Choose a load balancer to distribute incoming traffic for your application across instances to make it more reliable and easily scalable. You can also set options that give you more control over health check replacements and monitoring.

Load balancing: You have three options. No load balancer, Attach an existing load balancer and Attach to a new load balancer.
Select Attach to a new load balancer, since none was provided earlier

Load balancer type: Network Load Balancer, this helps
to distribute requests among the provisioned instances.

Load balancer scheme: internet-facing

![new-lb](new-lb.png?raw=true "new-lb")

![target-grp](target-grp.png?raw=true "target-grp")

### Step 4: Configure group size and scaling policies
Set the desired, minimum, and maximum capacity of your Auto Scaling group. You can optionally add a scaling policy to dynamically scale the number of instances in the group.

![scaling-policy](scaling-policy.png?raw=true "scaling-policy")

### Step 5: Add notifications
Optional


### Step 6: Add tags
Optional 

### The Auto Scaling Groups
![auto-scaling-grp](auto-scaling-grp.png?raw=true "auto-scaling-grp")

![auto-scaling-grp2](auto-scaling-grp2.png?raw=true "auto-scaling-grp2")

## Stage 3. Verify your Autoscaling group
On the Auto Scaling Group dashboard, select the Autoscaling group that you have created in the step above.

Verify the details that were provided while creating the Autoscaling group.

Verify that the Autoscaling group has launched the desired number of EC2 instance(s). The status of your instance(s) should be Successful, which means the instances are launched.

Verify that the instances have the desired lifecycle and health. Also, note the Instance ID for the next step.

## Stage 4. Test Autoscaling group
In this step, you will terminate only those instance(s) that were launched as a part of the Autoscaling group. And, then we will verify if the Autoscaling group automatically launches new instances to maintain the desired number of running instances.

Note - In addition to the instances that are launched as a part of the Autoscaling group, it is possible that you have additional instances already created in your EC2 dashboard.

Go to the EC2 Instances dashboard, and select the instance(s) that you want to terminate.  
Terminate the selected instance(s).  
Go back to the Auto Scaling Group service and select the Autoscaling group that you have created in the step above.  
While viewing the details of the Autoscaling group, review the history for the EC2 instance(s). You will see that the previous EC2 instance has been terminated, and a new one(s) are being instantiated.

## Stage 5. Delete Autoscaling group resources