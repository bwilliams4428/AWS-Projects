The purpose of this project is to host a highly availalbe and scaliable Word Press site on AWS. The hosting environment will use a custom VPC, RDS, EC2, EFS, ALB and ASG technologies.

1. I will first create a custom VPC with two public and two private subnets in different availability zones.
        
        VPC Settings:
            
            Name - WP-VPC
            IPv4 CIDR block - IPv4 CIDR manual input
            IPv4 CIDR - 12.0.0.0/16
            IPv6 CIDR block - No IPv6 CIDR block
            Tenancy - Default
            
            Subnets:
              
              VPC ID - WP-VPC
              
              Subnet Name - Public-Subnet-1
              Availability Zone - us-east-1a
              IPv4 CIDR block - 12.0.1.0/24
              
              Subnet Name - Public-Subnet-2
              Availability Zone - us-east-1b
              IPv4 CIDR block - 12.0.2.0/24
              
              Subnet Name - Private-Subnet-1
              Availability Zone - us-east-1a
              IPv4 CIDR block - 12.0.3.0/24
              
              Subnet Name - Private-Subnet-2
              Availability Zone - us-east-1b
              IPv4 CIDR block - 12.0.4.0/24
              
              Enable DNS hostnames and DNS resolution.
              
              IMAGES VPC1-4

2. Create an Internet Gateway and NAT Gateway.
             
              Create an Internet gatway and attach the IGW to my VPC. 
              Create a NAT Gateway, selec Private-Subnet-1, set the connectivity type to public and assign an elastic IP to the NGW.
              
              IMAGES VPC5-8

3. Create Route Tables for the public and private subnets.
                
                Create two route tables (public and private) and select the WP-VPC.
                IMAGES VPC9-10
                
4. Create routes in route tables and assign subnets to route tables.
                
                Create a route to IGW in public subnet route table 
                IMAGE VPC11-12
                
                Assign public subnets to public route table
                Image VPC13-14
                
                Create a route to NGW in private subnet route table
                Image VPC15
                
                Assign private subnets to private subnet route table
                Image VPC16

5. Create Security Groups for http, ssh and database traffic.
                
                Create security group for web and ssh traffic.
                Set the source for http traffic to anywhere ipv4 and the source for ssh traffic to my IP.
                IMAGE WBSG1
                
                Create security for database traffic. Set the source to as the type to MYSQL/Aurora and source as the web traffic security group.
                IMAGE DBSG
                

                
                
