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
              
