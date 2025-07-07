# Getting Started
This project was a simple one to gain some experience on using AWS services. 
- Launch an instance of EC2 and run an app on it
- Get comfortable using IAM to create roles, security groups, and use key pairs
- Gain familiarity with AWS sdks

## Notes: 

- EC2 key pair (.pem): used on local machine w/ssh to ssh into the EC2 instance.
- Access Keys (AWS CLI): used on local machine with `aws configure` to authenticate local aws cli to interact with AWS services (S3, EC2)
- IAM Role (for EC2): attached to the EC2 instance. Gives the EC2 instance itself permissions to use AWS services


## Steps: 
1. package app as a .jar: 
   1. `mvn clean package`
   2. generates a .jar in the target/ folder
2. Launch EC2 instance:
   1. use free tier AMI and instance type
   2. if creating a new key pair, download the .pem
   3. use default vpc
   4. launch 
3. Create a new security group and attach to EC2 instance:
   1. HTTP port 8080 source anywhere
   2. Security inbound group should allow: 
      1. SSH TCP 22 0.0.0.0/0

4. SSH into EC2 instance;
   1. `chmod 400 key.pem`
   2. `ssh -i key.pem ec2-user@<ec2-public-ip>`
5. install java:
   1. `sudo amazon-linux-extras enable corretto17
      sudo yum install java-17-amazon-corretto -y
      java -version`
   2. Note: Amazon Linux 2023 (AL2023) does not include , run `sudo dnf install java-17-amazon-corretto -y`
6. upload app: `scp -i your-key.pem target/file-upload-0.0.1-SNAPSHOT.jar ec2-user@<your-ec2-ip>:/home/ec2-user`
7. Create an IAM role for Ec2 instance to give S3 permissions: 
   1. UseCase is EC2
   2. Add `AmazonS3FullAccess`
   3. Attach role to EC2 instance
8. run app: `java -jar file-upload-0.0.1-SNAPSHOT.jar`

9. Use postman to hit the endpoint at: `http://<ec2-public-ip>:8080/api/files/upload`
   1. Use form-data
   2. Key: file, Type: file, Value: <select a file>

10. Clean up resources:
    1. Terminate EC2 instance
    2. Delete up S3 bucket
    3. Delete IAM access key / user

Note: Whitelabel error means the app is working. It's spring's default error page when url exists but doesn't have a handler defined (i.e. there is no controller mapped to /)