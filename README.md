### Project Description
This project shows how to create vpc and how to secure a application within vpc in a production enviorment

### Pre-requesite:
- virtual private cloud(vpc)
- Two availability zones
- Auto scaling group
- Application load balancer
- Private subnet
- NAT gateway

### Project workflow:
Two ec2 server deployed in two availability zones by using auto scaling group and an application load balancer. for the more security purpose the two ec2 server will be deployed in private subnet. the server recieve reuests through the load balancer. the ec2 instances can connect to the internet by using a NAT gateway. after that the NAT gateway will be deployed in both availability zones.

![workflow](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/vpc-example-private-subnets.png)

### Create VPC
- VPC settings
1. Resource to create2
- VPC and more
2. NAT Gateways
- 1 per AZ
3. VPC endpoints
- None
  
![Screenshot-2024-01-22-234359.png](https://i.postimg.cc/nzby139J/Screenshot-2024-01-22-234359.png)

### Create auto scaling group
- Create launch template
  
![Screenshot-2024-01-22-235303.png](https://i.postimg.cc/FKmbgcF6/Screenshot-2024-01-22-235303.png)
- Network settings
- Create security group
1. Inbound security group rules
2. add security group rules

![Screenshot-2024-01-23-000359.png](https://i.postimg.cc/N0fDvmhH/Screenshot-2024-01-23-000359.png)

- Choose launch template
1. Networks - Vpc
2. Availability Zones and subnets - select Availability Zones and subnets - choose 2 private subnet
3. Scaling - desired capacity2 - Maximum desired capacity-4
   
![Screenshot-2024-01-23-002724.png](https://i.postimg.cc/52c2XWf4/Screenshot-2024-01-23-002724.png)
![Screenshot-2024-01-23-002831.png](https://i.postimg.cc/cJnj6P4S/Screenshot-2024-01-23-002831.png)

- Create ec2 instances
1. Network setting - VPC - auto assign public IP - Enable
   
![Screenshot-2024-01-23-003515.png](https://i.postimg.cc/sgr9pXz1/Screenshot-2024-01-23-003515.png)

- ssh to bastion-host
* `scp -i  /home/mobaxterm/Desktop/arbab/test.pem /home/mobaxterm/Desktop/arbab/test.pem ubuntu@54.242.200.185:/home/ubuntu`
  
![Screenshot-2024-01-23-004114.png](https://i.postimg.cc/1tghQMfg/Screenshot-2024-01-23-004114.png)
* ` ssh -i test.pem ubuntu@54.242.200.185`
Now login to ec2 private ip from bastion-host
* ` ssh -i test.pem ubuntu@10.0.135.26`
  
![Screenshot-2024-01-23-005628.png](https://i.postimg.cc/vBpkfNsW/Screenshot-2024-01-23-005628.png)
* `python3 -m http.server 8000`
  
![Screenshot-2024-01-23-005854.png](https://i.postimg.cc/25FhYqHh/Screenshot-2024-01-23-005854.png)

- Create load balancer
1. Ec2 - load balancer - application load balancer
2. vpc
   
![Screenshot-2024-01-23-010432.png](https://i.postimg.cc/rpp1c1HW/Screenshot-2024-01-23-010432.png)
- Create target group
1. Port 80
2. include as pending below
   
![Screenshot-2024-01-23-011150.png](https://i.postimg.cc/J7GQJ9j9/Screenshot-2024-01-23-011150.png)
![Screenshot-2024-01-23-011653.png](https://i.postimg.cc/BQBq4wtV/Screenshot-2024-01-23-011653.png)
1. Ec2 - SEcurity group - aws-prod-example
2. edit inbound traffic rule - http anywhere ipv4

- Browse from web browser
copy dns name
* `aws-prod-example-813467082.us-east-1.elb.amazonaws.com`
  
![Screenshot-2024-01-23-014228.png](https://i.postimg.cc/P5gTpZZv/Screenshot-2024-01-23-014228.png)
* `logout`
Now login to ec2 private ip from bastion-host
* ` ssh -i test.pem ubuntu@10.0.149.11`
  
![Screenshot-2024-01-23-015322.png](https://i.postimg.cc/kG2XpY11/Screenshot-2024-01-23-015322.png)

* `python3 -m http.server 8000`
- Browse from web browser and copy dns name
* `aws-prod-example-813467082.us-east-1.elb.amazonaws.com`
   
![Screenshot-2024-01-23-020057.png](https://i.postimg.cc/bwKgGwFC/Screenshot-2024-01-23-020057.png)
