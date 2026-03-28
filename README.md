# AWS-Basics

Some Acronyms
EC2 -- Elastic Computing Cloud
VM -- Virtual Machine
AMI -- Amazon Machine Image
SSH -- Secure Shell --> When we create a Key Pair in AWS for windows we download .ppk, and for Mac, .pem file to access the EC2 instance remotely.
ARN -- Amazon Resource Name. For any resource we create in AWS, it creates unqiue identifier, and that is called ARN.
S3 -- Amazon Simple Storage Service.
IAM -- 
`Using chmod in Linux, modify the access permissions of files and directories

Using chmod 400 vins.pem we make the file secure`

Nginx -- If you don't want SSH in local Linux --> EC2 Instance Connect --> 

SSH port 22
HTTP port 80

Mapping the inbound port and the outbound port, and using -p, we do port mapping.
`sudo docker run -p 80:80 nginx`


**Question:**
How does S3 store files, and why is it called object storage and not file storage?
Everything stored in S3 is encrypted, and we can also encrypt the file and upload it if we don't trust AWS.
S3 presigned URL
