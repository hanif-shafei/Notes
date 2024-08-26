# AMAZON AWS

## Intro

AWS provides servers, storage, networking, remote computing, email, mobile development, and security

1. EC2 - Elastic Compute Cloud : the virtual machine
2. EBS - Elastic Block Store : data storage / virtual drive/disk
3. ELB - Elastic Load Balancing :  distributes incoming application traffic across multiple targets
4. ASG - Auto Scaling Group : automatically launch and terminate EC2 instances based on traffic

### EC2 benefit

- Easy to scale
- Reliable
- use with other service
- Pay per use

### Common jargon

**INSTANCE**

> The virtual server itself. Since it's cloud-based, your server is not a physical machine; it's an instance created inside a physical server.

**REGION**

> The geographical location where data centers are located.

**AZ - AVAILABILITY ZONE**

> A physical data center building or infrastructure available within a region

**AMI - AMAZON MACHINE IMAGE**

> A template containing the software configuration required to launch an instance

**IAM - IDENTITY and ACCESS AMANAGEMENT**

> Allows you to create and assign permissions for trusted identities to perform actions in AWS

**INSTANCE STORE VOLUMES**

> Storage comparable to the primary hard drive in a physical PC.

## EC2 setup

- login to AWS Managment Console:
    - Visit the [AWS Management Console](https://console.aws.amazon.com/).
    - If you don't have an account, sign up
    - With the AWS Free Tier, you get 750 hours/month of select EC2 instances for free.

> **IMPORTANT :** change to closest location/region. This can improve latency greatly

- ways to get to EC2 dashboard:-
    - All service > Compute > EC2
    - menu (service) > Compute
    - search EC2

- click create instance button

> For your first instance, I recommend using a low-cost, general-purpose instance type like `t2.micro` and the Amazon Linux 2 AMI (both free-tier eligible).

### EC2 instance setup step

1. Choose AMI (server OS)
    - Amazon linux 2 - recommended by Amazon
    - some OS have free tier

2. Choose Instance type
    - basically its how powerful your server will be
    - free tier are available
    - [AWS Instance Types](https://aws.amazon.com/ec2/instance-types/).

> `Review and Launch` button available from this steps onwards which will skips to step 7

3. Configure Instance
    - no. of instance : how many server to create (with same setting)
    - request spot instance : un-tick to create on-demand instance
    - subnet : in which data center the instance will be created

4. Add Storage
    - default are ok for starters
    - EBS can be attach now or after launch
    - instance store volumes can only be configured now

5. Add Tags
    - tag to name or verify your instance or maybe groups it

6. Configure Security Group
    - firewall rules
    - can define SSH rule here

>**NEVER** leave IP at  0.0.0.0/0 : your server is open to the world

7. Review
    - just double check

8. **Launch**
    - create key pair or use existing
    - this key pair is to enable secure connection to the instance

Once launched, check the instance state. 'Running' means its ready. The instance state can be change by right click menu. Available Instance state option:
  - Start : start the instance
  - Stop : stop the instance - alike to shutting down the computer
  - Reboot : restart the instance - like restarting the computer
  - Terminate : Delete the instance wholly. this will delete the instance and all settings and all data

## SSH

### SSH key

  - Create a key pair and save it to a local file.

### PUTTY

- Use the format `user@ip-address` (e.g., `root@192.168.10.1`).
- Configure PuTTY under `Connection > SSH > Auth > Credentials`
  -  get the key pair file

### EC2 Instance Connect (AWS browser-based client)

- Select the EC2 instance you created.
- Key in user name / use default username
- Choose “Connect.”
- Click “EC2 Instance Connect” to establish a connection.

## Quick Tips

- Use the EC2 global view to check overall instances owned
