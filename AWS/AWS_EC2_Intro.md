# Amazon AWS

## Introduction

AWS (Amazon Web Services) provides a range of cloud services, including servers, storage, networking, remote computing, email, mobile development, and security. Below are some key components:

1. **EC2 (Elastic Compute Cloud)**: Virtual machines running in the cloud.
2. **EBS (Elastic Block Store)**: Storage for data, akin to virtual drives/disks.
3. **ELB (Elastic Load Balancing)**: Distributes incoming application traffic across multiple targets to ensure availability and reliability.
4. **ASG (Auto Scaling Group)**: Automatically scales EC2 instances up or down based on demand.

### Benefits of EC2

- **Scalability**: Easily scale resources up or down as needed.
- **Reliability**: High uptime and performance.
- **Integration**: Works seamlessly with other AWS services.
- **Cost-effective**: Pay only for what you use.

### Common Terminology

- **Instance**: A virtual server created in the cloud, not tied to physical hardware. It's an instance created inside a physical server.
- **Region**: The geographical area where AWS data centers are located.
- **AZ (Availability Zone)**: A specific physical data center within a region.
- **AMI (Amazon Machine Image)**: A pre-configured template containing software, OS, and configurations needed to launch an instance.
- **IAM (Identity and Access Management)**: Manages access permissions and identities in AWS.
- **Instance Store Volumes**: Temporary storage attached to an instance, similar to a physical PC's hard drive.

## Setting Up EC2

1. **Log in to the AWS Management Console**:
   - Visit the [AWS Management Console](https://console.aws.amazon.com/).
   - Sign up for an account if you don't have one.
   - AWS Free Tier offers 750 hours per month of select EC2 instances for free, for a year.

   > **Tip**: Change to the closest region to reduce latency.

2. **Access the EC2 Dashboard**:
   - Navigate via: All Services > Compute > EC2.
   - Or use the search bar to find EC2.

3. **Create an Instance**:
   - Click the “Create Instance” button.
   - For beginners, start with a `t2.micro` instance and the Amazon Linux 2 AMI (both are Free Tier eligible).

### EC2 Instance Setup Steps

1. **Choose an AMI** (server OS):
   - Amazon Linux 2 is recommended by AWS.
   - Some AMIs are Free Tier eligible.

2. **Choose Instance Type**:
   - Determines the power and capacity of the server.
   - Free Tier options are available.
   - More details on [AWS Instance Types](https://aws.amazon.com/ec2/instance-types/).

   > **Note**: You can click “Review and Launch” from this step onwards to skip to step 7.

3. **Configure Instance**:
   - Number of instances: Specify how many servers to create with the same settings.
   - Spot instances: Uncheck to create on-demand instances.
   - Subnet: Choose the data center where the instance will be created.

4. **Add Storage**:
   - Default settings are usually sufficient for starters.
   - You can attach EBS storage now or later.
   - Instance store volumes must be configured now.

5. **Add Tags**:
   - Tags help you name, organize, and identify your instances.

6. **Configure Security Group**:
   - Set firewall rules to control traffic.
   - Define SSH access rules.

   > **Warning**: Avoid using `0.0.0.0/0` for IP addresses, which leaves your server open to the entire internet.

7. **Review**:
   - Double-check all settings before proceeding.

8. **Launch**:
   - Create a new key pair or use an existing one for secure access.
   - Download the key pair file (usually `.pem`). Keep this file secure, as you will need it to access your instance via SSH.


   Once launched, check the instance state. 'Running' means it's ready. Use the right-click menu for instance state management:
   - **Start**: Launches a stopped instance.
   - **Stop**: Stops the instance (similar to shutting down a computer). The instance's data is retained, but charges for the instance do not accrue while it is stopped. However, storage (EBS volumes) still incurs costs.
   - **Reboot**: Restarts the instance without data loss or affecting storage volumes.
   - **Terminate**: Permanently deletes the instance, its settings, and data.

   > **Reminder:** Leaving instances running or unmonitored can lead to unexpected costs, as you are charged for the compute resources used. Regularly check and manage your instances to ensure you're only paying for what you need. Stopped instances do not incur compute charges, but associated storage (like EBS volumes) may still generate costs. Terminate unused instances to avoid unnecessary expenses.

## Connecting to Your EC2 Instance via SSH

### SSH Key Pair

- Create and download a key pair (`.pem` file) during instance setup.

### Using PuTTY for Windows

1. Use the format `user@ip-address` (e.g., `ec2-user@192.168.10.1`).
2. Configure PuTTY:
   - Navigate to `Connection > SSH > Auth`.
   - Load the `.pem` key file (converted to `.ppk` using PuTTYgen).

### EC2 Instance Connect (Browser-based Client)

1. Select your EC2 instance in the AWS Management Console.
2. Click “Connect.”
3. Use the default username (`ec2-user` for Amazon Linux).
4. Choose “EC2 Instance Connect” to open a browser-based terminal.

## Quick Tips

1. **Use the Global View**: Monitor all instances across regions to track resources and manage costs effectively.

2. **Take Advantage of the Free Tier**: Utilize the 750 hours/month free tier for exploring AWS services, but monitor usage to avoid extra charges.

3. **Choose the Right Region**: Select the region closest to your users to improve latency and reduce costs.

> **Remember** data transfer charges may vary between regions.

4. **Enable MFA**: Secure your account with Multi-Factor Authentication to prevent unauthorized access.

5. **Set Up Billing Alerts**: Configure billing threshold alerts to monitor your spending and avoid unexpected charges.

6. **Regularly Review Security Groups**: Frequently review and update security group settings. Ensure that only necessary ports are open and restrict access to specific IP addresses whenever possible. Avoid using 0.0.0.0/0 unless absolutely necessary, as this opens your instance to the entire internet.

7. **Backup Key Pair Files**: Store `.pem` key files securely and create backups to ensure access to your instances.

8. **Use IAM Roles**: Assign IAM roles for permissions instead of hard-coded credentials for secure and scalable security.

9. **Tag Resources**: Use tags to organize and identify resources by project, environment, or cost center.

10. **Check for Unused Resources**: Regularly identify and terminate idle instances and resources such as EC2 instances, EBS volumes, and Elastic IPs to save costs.

11. **Implement Auto Scaling**: Use Auto Scaling Groups to automatically adjust instances based on traffic demand, optimizing performance and cost.

12. **Monitor with CloudWatch**: Set up CloudWatch alarms to keep track of instance health, performance, and usage.

13. **Practice Safe Termination Procedures**: Always back up data and create AMIs of critical instances before terminating them.

14. **Use Elastic IPs Wisely**: Allocate Elastic IPs only when necessary and release unused ones to avoid charges.

15. **Leverage AWS Documentation**: Refer to AWS’s extensive documentation and tutorials for best practices and troubleshooting. 

---

### Sources:

1. Getting Started with Amazon EC2 - [AWS](https://aws.amazon.com/ec2/getting-started/).
2. Get started with Amazon EC2 - Amazon Elastic Compute Cloud - [AWS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html).
3. AWS EC2 for Beginners - [DataCamp](https://www.datacamp.com/tutorial/aws-ec2-beginner-tutorial).
4. How to Create EC2 Instance in AWS: Step by Step Tutorial - [Guru99](https://www.guru99.com/creating-amazon-ec2-instance.html).
5. How to Install an SSL/TLS Certificate In Amazon EC2 (AWS) - [GeeksforGeeks](https://www.geeksforgeeks.org/install-ssl-tls-certificate-in-amazon-ec2-aws/).
6. [AWS EC2 Console](https://console.aws.amazon.com/ec2/).
