# Deploy a web server made in Python using the Flask framework on AWS EC2 and a MySQL database on AWS RDS.

## I'm going to deploy the next cloud architecture on AWS
![arquitectura aws](img/EC2-RDS.svg)

<hr>
1. I'll build the network architecture using the AWS VPC service.

   - I'm going to create a VPC with the following IPv4 CIDR Block
     - 192.168.0.0/16
    
   - I will create the three subnets with the next names:
     - **PublicSubnetA**:
          - Availability Zone: a
          - CIDR: 192.168.1.0/24
      
     - **PrivateSubnetA**:
          - Availability Zone: a
          - CIDR: 192.168.2.0/24
      
     - **PrivateSubnetB**:
          - Availability Zone: b
          - CIDR: 192.168.3.0/24

   I need the VPC to have an Internet connection so I need to configure an **Internet Gateway**.
   - When the Internet Gateway is created, I attach it to the VPC
   - I create a Route Table
   - In the Route Table I associate with the **Public Subnet** and I create a route for allow to connect to the Internet through the **Internet Gateway**  

<hr>
2. I'll use the AWS System Manager service to store the connection parameters that the web server will use to connect to the database configured on AWS RDS.

   - Using AWS System Manager with Parameter Store I create the following parameters:
      - /book/user: root
      - /book/password: *Test!2024* using the *SecureString* type
      - /book/database: books_db
      - /book/host: 192.168.1.23 *The IP address where the database is located. When creating the database on RDS I need to change this IP address to the endpoint provided by RDS*.
      
   - The web server will run in the EC2 instance and it needs to read the connection parameters to the database deployed on RDS, so I need to create an IAM role that has permission to EC2 to read the connection parameters from the System Manager service.
      - In the IAM service I create a role named *ec2RoleSSM*.
      - The Role has the following permission:
         - SSMFullAccess           

<hr>

3. I going to the EC2 service and I launch an EC2 instance in the **PublicSubnet** with the next configurations
     - AMI: *Amazon Linux 2023*
     - Instance Type: *t2.micro*
     - Key Pair: associate a key pair
     - Network settings:
        - VPC
        - Public Subnet: enable *Public IP*
        - Create a Security Group
     - Advanced details:
        - IAM instance profile: Associate the role created previously
        - User data: *copy the next following lines to the user data*
           ```
           #!/bin/bash
           sudo dnf install -y python3.9-pip
           pip install virtualenv
           sudo dnf install -y mariadb105-server
           sudo service mariadb start
           sudo chkconfig mariadb on
           pip install flask
           pip install mysql-connector-python
           pip install boto3
            ```
         

    
   
    

     
