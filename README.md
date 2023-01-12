# Website-with-Microservices-part-2
This is the continuation of the Website Deployment with Microservices
##

A NAT gateway is a Network Address Translation (NAT) service. You can use a NAT gateway so that instances in a private subnet can connect to services outside the VPC. In the following section I’m going to create two Nat Gateways to connect the private subnets to the internet. One on each public Subnet, (Public Subnet AZ1, Public Subnet AZ2). 
##

## To create a Nat Gateway: 

* Select VPC and click on Nat gateways. 
* Nat gateway settings – Name: Nat Gateway AZ1 
* Subnent: Public Subnet AZ1 
* Connectivity type: Public 
* Allocate Elastic IP and select Create Nat Gateway. 

<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng00.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>
 
## Route Table creation: 

On the left side of the VPC dashboard select Route Tables. 
* Route table settings: 
* Name: Private RT AZ1 
* VPC: Dev VPC 
* Click create Route Table. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng01.PNG?raw=true)

 Private RT AZ1 is created. Now I need to configure the routes. 

* Select Routes – Edit routes. 
* Add route: 0.0.0.0/0 - Target: Nat Gateway AZ1 
* Save the changes.

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng02.PNG?raw=true)

I need to create a Subnet association: 
* Select Subnet association – Edit subnet association. 
* Select Private App Subnet AZ1, Private Data Subnet AZ1 
* Save association 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng03.PNG?raw=true)
 
## Second Nat Gateway: 

* Create Nat Gateway – Nat Gateway settings. 
* Name: Nat Gateway AZ2 
* Subnet: Public Subnet AZ2 
* Connectivity type: Public 
* Allocate Elastic IP and select create Nat Gateway. 

<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng04.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>
 
## Second Route Table: 

* Route table settings: 
* Name: Private RT AZ2 
* VPC: Dev VPC 
* Click create Route Table. 

<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng05.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>

After Route table: Private RT AZ2 is created I need to configure the routes. 

* Select Routes – Edit routes. 
* Add route: 0.0.0.0/0 - Target: Nat Gateway AZ2  
* Save the changes. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng06.PNG?raw=true)

I need to create a Subnet association: 
* Select Subnet association – Edit subnet association. 
* Select Private App Subnet AZ2, Private Data Subnet AZ2 
* Save association 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng07.PNG?raw=true)

##

In this section of the project, I’m going to create the security groups needed. 
##

On the AWS console go to VPC and then Security groups. 
* Name: ALB SG 
* Description: ALB SG 
* VPC: Dev VPC 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/sg.PNG?raw=true)

* Select add rules: 
* Port: 80 – source: 0.0.0.0/0 
* Port: 443 – source: 0.0.0.0/0 
* Select create Security group. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/sg0.PNG?raw=true)
 
Second security group: 
* Name: Container SG 
* Description: Container SG 
* VPC: Dev VPC 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/sg1.PNG?raw=true)

* Select add rules: 
* Port: 80 – source: ALB SG 
* Port: 443 – source: ALB SG 
* Select create Security group. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/sg2.PNG?raw=true)

##

Application Load Balancer operates at the request level (layer 7), routing traffic to targets (EC2 instances, containers, IP addresses, and Lambda functions) based on the content of the request.   

##

To create a load balancer, go to the EC2 Dashboard and on the left side look for Load Balancer tab and select it. 

* Application load balancer: create 
* Load balancer name: Dev-ALB 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/alb.PNG?raw=true)

* Network mapping – VPC: Dev VPC 
* Availability zone: US-East-1A subnet (Public Subnet AZ1) 
* Availability zone: US-East-1B subnet (Public Subnet AZ2) 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/alb0.PNG?raw=true)

* Security group: ALB SG 
* Listener and routing: 
* Protocol: HTTP – 80 forward to: Dev-TG  
* Finally click create load balancer. 

##

We have pushed the container image to ECR repository. We’re ready to run the ECS fargate containers. To run the fargate container I need to create an ECS cluster.

## 
 
On the AWS console go to ECS and on the left side select cluster. 
* Cluster name: jupiter-cluster 
* VPC: Dev VPC 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/cluster.PNG?raw=true)

* Subnets: Private App Subnet AZ1, Private App Subnet AZ2 
* Infrastructure: AWS Fargate (Serverless) 
* Select create cluster.

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/cluster0.PNG?raw=true)

##
After creating the ECS cluster the next thing I need to do is create a task definition. The task definition is required to run Docker containers in Amazon ECS. You can define multiple containers in a task definition. 
##
 
To create a task definition, I first go to ECR and on the left side select task definitions. 
* Select create new task definition. 
* Task definition family: jupiter-task-definition 
* Name: jupiter and input the image URI 
* Click next 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/task.PNG?raw=true)

* App environment: AWS Fargate (Serverless) 
* Operating system: Linux/X86_64 
* CPU: .25 vCPU Memory: .5 GB 
* Scroll down and click next, then select create task. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/task0.PNG?raw=true)

##
You can use an Amazon ECS service to run and maintain a specified number of instances of a task definition simultaneously in an Amazon ECS cluster. If one of your tasks fails or stops, the Amazon ECS service scheduler launches another instance of your task definition to replace it.
##

To start an ECS service go to cluster on ECS and then select cluster. 
* Select services and click on create. 
* Family: jupiter-task-definition 
* Service name: jupiter-service 
* Desired task: 2 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/service0.PNG?raw=true)

* Networking – VPC: Dev VPC 
* Subnets: Private App Subnet AZ1, Private App Subnet AZ2 
* Security groups: Container SG 
* Disabled: Public IP 
* Load balancer: Dev-ALB 
* Target group: Dev-TG – Protocol: HTTP 
* Click on create 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/service0.PNG?raw=true)
 
To check if the ECS service deployed work I change console to EC2.  
* On EC2 dashboard look for load balancer and click on it. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/service1.PNG?raw=true)

* Get the DNS name and input it on a new tab. 
* The Webserver is working. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/service2.PNG?raw=true)

##
In this section of the project, I’m going to create a record set on Route 53. 

##
On the AWS console look for Route 53. 
* Route 53 dashboard select: DNS management 1 hosted zone. 
* Select hosted zone (Richarlison017.click) 
* The hosted zone details page will be displayed and click on create. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/route53.PNG?raw=true)

* Configuration settings: 
* Record name: www 
* Record Type: A – Route traffic to an IPv4 address and some AWS resources. 
* Select Alias record 
* Route traffic to: Alias to Application and Classic load balancer 
* Choose a region: US-East-North Virginia 
* Choose Load balancer:  dualstack.Dev-ALB-528652584.us-east-1.elb.amazonaws.com 
* Finally click on create records. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/route530.PNG?raw=true)
 
To test the record register, I open a new browser tab and copy and paste the domain: www.Richarlison017.click. The test was successful. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/route531.PNG?raw=true)

##
 
In this section of the project, I will create an SSL certificate in AWS Certificate manager.
##

On the AWS console type certificate manager, then select request certificate. On the certificate type select – Request a public certificate and click next. 

 ![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ssl.PNG?raw=true)
 
Configure settings: 
* Domain names – Fully qualify domain name: richarlison017.click. 
* Validate method: DNS validation 
* Scroll down and select Request. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ssl0.PNG?raw=true)

Now I need to validate our domain name with Route 53, this is done by clicking on create records in Route 53. 
* Select the domain name (richarlison017.click) and click Create records. 

![](https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ssl1.PNG?raw=true)
 

 


 

