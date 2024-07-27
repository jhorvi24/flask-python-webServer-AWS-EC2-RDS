# Deploy a web server made in Python using the Flask framework on AWS EC2 and a MySQL database on AWS RDS.

## I'm going to deploy the next cloud architecture on AWS
![arquitectura aws](img/EC2-RDS.svg)

<hr>
1. I'm going to create the network architecture using the AWS VPC Service.
   - I create a VPC with the next IPv4 CIDR Block
     - 192.168.0.0/16
    
   - I create the three next subnets with the next names:
     - * PublicSubnetA:
       - Availability Zone: a
       - CIDR: 192.168.1.0/24
      
     - * PrivateSubnetA:
       - Availability Zone: a
       - CIDR: 192.168.2.0/24
      
     - * PrivateSubnetB:
       -Availability Zone: b
       -CIDR: 192.168.3.0/24

   I need the VPC have to an Internet connection so I need to configure an Internet Gateway.
   - When the Internet Gateway is created, I attach it to the VPC
   -   

    
     
   
    

     
