# Highly Available and Scaliable LAMP Environment To Host Word Press 


The purpose of this project/tutorial is to host a highly availalbe and scaliable Word Press site on AWS. 
I will first create a custom VPC to host the EC2 instances, RDS, an Elastic File Storage instance and Load Balancer.
The environment will also include an Auto Scaling group to increase computing power during high traffic times.

It should take no longer than 15-20 minutes to get this setup up and running.

Network Topology:

![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/network%20topology.PNG)

# 1. VPC

 Create a custom VPC with two public and two private subnets in different availability zones
        
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
      
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC3.PNG)        
      
   - Subnet Name - Private-Subnet-1
   - Availability Zone - us-east-1a
   - IPv4 CIDR block - 12.0.3.0/24
              
   - Subnet Name - Private-Subnet-2
   - Availability Zone - us-east-1b
   - IPv4 CIDR block - 12.0.4.0/24
      
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC4.PNG)
              
   - Enable auto assign IPv4 settings on the public subnets by clicking on 'Edit Subnet Settings' from the Action menu
   - Click the checkbox next 'Enable auto-assign public IPv4 address'
   - Press the Save button
      
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/publiciv4psunet.PNG)
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/vpcsubnetenableipv4.PNG)

# 2. Internet Gateway
             
   - Create an Internet gatway and attach the IGW to my VPC
      
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC5.PNG)       
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC6.PNG)
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC7.PNG)   

# 3. Route Table
       
   - Create two route tables (public and private) and select the WP-VPC
      
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC9.PNG)
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC10.PNG)   
                
# 4. Routes & Route Tables
                
   - Create a route to IGW in public subnet route table 
   
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC11.PNG)
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC12.PNG)               
   
   - Press the save button to create the route from the public route to the IGW so that assets in the public subnets can access the Internet            
                
   - Assign the public subnets to public route table
   
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC13.PNG)
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/VPC14.PNG)              
   
   - Press the 'Save Associations' button

# 5. Security Groups
                
   - Create three security groups the webserver (EC2), the database (RDS) and the network file storage (EFS) 
     
     - Web server security group
       
       ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/WebSG1.PNG)
     
     - Database security group
       
       ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/DBSG.PNG)
    
     - Network file storage security group
       
       ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/efssg.PNG)

# 6. RDS
     
   - Create a database subnet group to place the RDS instance in a private subnet. Select two AZ in the private subnets for high availability
     
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/DBSNG.PNG)
                
   - Create a MYSQL RDS Instance for Word Press.
     
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/RDS1.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/RDS2.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/RDS3.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/RDS4.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/RDS5.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/RDS6.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/RDS7.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/rdsep.PNG)
                
# 7. EFS

  - Create EFS instance for file storage across avaliablity zones
               
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EFS1.PNG)
     ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EFS2.PNG)
                
# 8. EC2
                
  - Launch an EC2 instance and select Amazon Linux 2 AMI (HVM) - Kernel 5.10, SSD Volume Type as the AMI
  
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EC21.PNG)
  
  - Select a t2.mirco as the instnace type.
  
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EC22.PNG)
  
  - Select the WP-VPC as the network, Public-Subnet-l as the subnet (the auto assign IP and enable resource-based IPv4 options can be left disabled/unchecked. the public subnet will auto assign an IPv4 during the creation process)
  
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EC23.PNG)
 
 - Scroll down to find 'User data' textarea and copy/paste the following script to auto mount the EFS drive, install, start and enable Apache web server
   
   ```
   #!/bin/bash
   yum update -y
   mkdir -p /var/www/html
   mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-05dc68312508654fb.efs.us-east-1.amazonaws.com:/ /var/www/html/
   yum install -y amazon-efs-utils
   yum install -y httpd
   systemctl start httpd
   systemctl enable httpd
   echo "<mount name>.efs.<region>.amazonaws.com:/ /var/www/html nfs defaults,_netdev 0 0" >> /etc/fstab
   ```
   
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EC23.PNG)
                
 - Continue clicking the Next button until being prompted to select a security group for the EC2 instance and select webserver security group
 - Click until arriving at 'Step 7: Review Instance Launch', create a key pair, download the private key and press the launch button to create the EC2 instance
  
  ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EC24.PNG)
  
 - SSH to the instance using the downloaded private key and confirm that EFS auto mounted by using the following command:
   ```
   df -T -h
   ```
 - The EFS instance will display as a filesystem   
            
 - Confirm that Apache is up and running by accessing the EC2's DNS endpoint from a web browser 
 - The Apache test page will display
   
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EC27.PNG)
                
 - Run the following commands from the CLI to install PHP 8 on the server
   ```
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
   ```          
 - Restart Apache
   ```
   sudo systemctl restart httpd
   ```
 
 - Download the latest WordPress archive to the EC2 instance's home directory and extract it
   ```
   cd /home/ec2-user/
   wget https://wordpress.org/latest.tar.gz
   tar -xzf latest.tar.gz
   ```
   
 - Copy config file sample to wp-config.php         
   ```
   cd wordpress
   cp wp-config-sample.php wp-config.php          
   ```

 - Use a text editor to edit the wp-config.php file. Input the RDS DNS endpoint as the DB_Host name.
 - Input your DB_NAME,_USER and _PASSWORD values into the respective fields.
   
   ![](https://github.com/bwilliams4428/AWS-Projects/blob/main/AWS-WordPress/Images/EC291.png)
                       
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

               
                

                
                
