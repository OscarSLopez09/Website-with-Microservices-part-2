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

<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng01.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>

 Private RT AZ1 is created. Now I need to configure the routes. 

* Select Routes – Edit routes. 
* Add route: 0.0.0.0/0 - Target: Nat Gateway AZ1 
* Save the changes.

<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng02.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>

I need to create a Subnet association: 
* Select Subnet association – Edit subnet association. 
* Select Private App Subnet AZ1, Private Data Subnet AZ1 
* Save association 

<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng03.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>
 
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

<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng06.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>

I need to create a Subnet association: 
* Select Subnet association – Edit subnet association. 
* Select Private App Subnet AZ2, Private Data Subnet AZ2 
* Save association 
 
<img src="https://github.com/OscarSLopez09/Website-with-Microservices-part-2/blob/main/Images/ng07.PNG?raw=true" height="70%" width="70%" alt="Disk Sanitization Steps"/>


