
## Tasks

Task 1:

1. Create a StackSet in your Master account and deploy resources to multiple target accounts in all regions (atleast 6 regions).
2. If you don't have multiple accounts, you can use a single account and deploy stacks to all regions in the same account. StackSet should be preferrably in us-east-1.
3. Your stack should deploy an IAM Role and Instance Profile if region is us-east-1, and an EC2 instance resource in all regions that will use the InstanceProfile created in us-east-1.
4. Optional: You can create a simple webserver with httpd installed and display a simple welcome page when we hit the public IP of EC2.
