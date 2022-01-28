The purpose of this project/tutorial is to host a highly availalbe and scaliable Word Press site on AWS. 
I will first create a custom VPC to host the EC2 instances, RDS, an Elastic File Storage instance and Load Balancer.
The environment will also include an Auto Scaling group to increase computing power during high traffic times.

It should take no longer than 15-20 minutes to get this setup up and running.

Network Topology:

![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/network%20topology.PNG)

1. I will first create a custom VPC with two public and two private subnets in different availability zones.
        
   VPC Settings:
      
      - Name - WP-VPC
      - IPv4 CIDR block - IPv4 CIDR manual input
      - IPv4 CIDR - 12.0.0.0/16
      - IPv6 CIDR block - No IPv6 CIDR block
      - Tenancy - Default
      
      ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC-1.PNG)
      
    Press the Create VPC button to create the VPC. 
    
      - Enable DNS hostnames and DNS resolution from the VPC management page.
      
      ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC-dnshr.PNG)
      
    
    Now create the public and private subnets inside the WP-VPC.
    
    Subnets:
           
      - VPC ID - WP-VPC
      - Subnet Name - Public-Subnet-1
      - Availability Zone - us-east-1a
      - IPv4 CIDR block - 12.0.1.0/24
              
      - Subnet Name - Public-Subnet-2
      - Availability Zone - us-east-1b
      - IPv4 CIDR block - 12.0.2.0/24
      
      ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC-3.PNG)        
      
      - Subnet Name - Private-Subnet-1
      - Availability Zone - us-east-1a
      - IPv4 CIDR block - 12.0.3.0/24
              
      - Subnet Name - Private-Subnet-2
      - Availability Zone - us-east-1b
      - IPv4 CIDR block - 12.0.4.0/24
      
      ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC-4.PNG)
              
      - Enable auto assign IPv4 settings on the public subnets.
      
      ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/publiciv4psunet.PNG)

2. Create an Internet Gateway.
             
              Create an Internet gatway and attach the IGW to my VPC. 
             
              
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

6. Create database subnet group and RDS Instance for Word Press
                
                Create subnet group to place the RDS instance in a private subnet. Select two AZ and the private subnets for high availability.
                Image DBSNG
                
                Create a MYSQL RDS Instance for Word Press.
                ImagesRDS1-6
                
7. Create EFS instance
                
                Images EFS
                
8. Create an EC2 instance, install Apache, PHP 7.2, Word Press and upgrade to PHP 8
                
                Create an EC2 instance and select Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type as the AMI.
                Select a t2.mirco as the instnace type.
                Include the EFS in the EC2 configuration to auto mount the filesystem upon launch and change the mount point to /var/www/html, the location where our                 webserver and wordpress will be installed
                Click next then click the launch button to create the instance.
                After SSH to the recently launched EC2, you see that the EFS has auto mounted with a mount point of /var/www/html.
                Install Apache from the CLI using the following yum command: sudo yum install -y httpd
                Start Apache, set Apache to auto start upon rebooting the server and check the status of Apache
                Confirm that Apache is up and running by accessing the EC2's DNS endpoint from web browser.
                
                Run the following commands from the CLI to upgarde to PHP 8
                        sudo yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
                        sudo yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
                        sudo yum makecache
                        sudo yum -y install yum-utils
                        sudo yum-config-manager --disable 'remi-php*'
                        sudo amazon-linux-extras enable php8.0
                        sudo yum clean metadata
                        sudo yum install php-{pear,cgi,pdo,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip}
                        sudo systemctl stop php-fpm
                        sudo systemctl start php-fpm
                        sudo systemctl restart httpd
              
              Download WordPress archive and extract it
                       cd /home/ec2-user/
                       wget https://wordpress.org/latest.tar.gz
                       tar -xzf latest.tar.gz 
              Copy config file sample to config.php         
                       cd wordpress
                       cp wp-config-sample.php wp-config.php          
                       
                       Use Nano or Vi to edit the wp-config.php file. Input the RDS DNS endpoint as the DB_Host name.
                       Input your DB_NAME,_USER and _PASSWORD values into the respective fields.
                       
                       Copy Word Press directory to /var/www/html from /home/ec2-user
                       sudo cp -r wordpress/* /var/www/html/
                       
                       Change user/group permissions of /var/www so that Apache can access files at that location
                       sudo chown -R apache:apache /var/www/*
                       
                       Make the following changes in /etc/httpd/conf/httpd.conf:

                      Scroll to line 102 (line numbers maybe different on your EC2)Replace the following lines

                        <Directory />
                          AllowOverride none
                          Require all denied
                        </Directory>

                      with the
                       <Directory />
                         Options FollowSymLinks
                         AllowOverride All
                       </Directory>

                       scroll down to line no 125 and change AllowOverride None to AllowOverride All

                        scroll down to line no 151 and change AllowOverride None to AllowOverride All

                        Restart Apache Web Server

                        sudo systemctl restart httpd
                        
                        Word Press install page now displays.
                        
                        Install Word Press
                        
                        Word Press is live on our EC2.
                       
                      
                        Stop the EC2 instance Create an imaage of the web server EC2. This image will be used to launch instances in the Auto Scaling group.
                
                
7. Auto Scaling Group
8. Load Balancer

               
                

                
                
