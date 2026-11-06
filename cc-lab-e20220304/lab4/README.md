## Questions

### 1. How many IP addresses does each block contain, and why do the subnets not overlap?

The VPC (10.0.0.0/24) contains 256 IP addresses.
Each subnet (/25) contains 128 IP addresses, but AWS reserves 5 addresses, leaving 123 usable addresses per subnet.

The two subnets do not overlap because they have different IP ranges:
- Public subnet: 10.0.0.0 - 10.0.0.127
- Private subnet: 10.0.0.128 - 10.0.0.255

### 2. What makes the public subnet public and the private subnet private?

The public subnet has a route (0.0.0.0/0) pointing to the Internet Gateway, allowing instances with public IP addresses to communicate directly with the internet.

The private subnet has a route (0.0.0.0/0) pointing to the NAT Gateway instead. This allows instances to access the internet without receiving incoming connections directly from the internet.

### 3. Why is the NAT Gateway placed in the public subnet?

The NAT Gateway is placed in the public subnet because it needs access to the Internet Gateway.

It uses an Elastic IP address to communicate with the internet on behalf of private instances. If the NAT Gateway were placed in the private subnet without a route to the Internet Gateway, it would not provide internet access.

### 4. Can someone on the internet connect to lab4-ec2-02? Can it download updates?

No, someone on the internet cannot directly connect to lab4-ec2-02 because it does not have a public IP address and is located in the private subnet.

However, it can download software updates and access the internet through the NAT Gateway. We confirmed this by successfully running the update command using AWS Systems Manager.

### 5. Why did the instances need LabInstanceProfile?

LabInstanceProfile provides the IAM permissions required for AWS Systems Manager to manage EC2 instances.

It allows Systems Manager to communicate with the instances and execute commands remotely without using SSH or opening port 22.

In this lab, we used Systems Manager Run Command to test internet connectivity on both the public and private EC2 instances.