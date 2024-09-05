# HoneyPot-AWS
AWS CloudFormation script and instructions template to setup a honeypot 
The script provisions EC2 instance running Ubuntu and honeypot using Cowrie

This script will prompt you to input certain technical OS and networking details such as the instance type, SSH key, and VPC settings.
Before you run this, ensure you have an AWS account and AWS CLI installed and configured.

Steps:
Launch the CloudFormation Stack:
Save the above YAML script as honeypot-cloudformation.yml.
Go to AWS Management Console / CloudFormation / Create Stack.
Upload the template and provide values for InstanceType, SSHKeyName, VPC-Id, Subnet-Id, and other parameters as prompted.
Set Up and Access the Honeypot: Once the CloudFormation stack completes:

Connect to the EC2 instance via SSH using the key pair such as ssh -i /path/to/key.pem ubuntu@<public_ip_address>


1- Install Cowrie:
# Update the instance
sudo apt update && sudo apt upgrade -y

# Install dependencies
sudo apt install git python3-virtualenv libssl-dev libffi-dev build-essential python3-dev libpython3-dev -y

# Clone the Cowrie repository
git clone https://github.com/cowrie/cowrie.git

# Navigate to Cowrie folder
cd cowrie

# Set up a virtual environment for Cowrie
virtualenv --python=python3 cowrie-env
source cowrie-env/bin/activate

# Install Python dependencies
pip install -r requirements.txt

# Start Cowrie
./bin/cowrie start

2- Monitor the Honeypot:
Cowrie logs: to see attack attempts: tail log/cowrie.log

3- Security Monitoring:

Set up alerts for any suspicious activity. You can use AWS CloudWatch to trigger notifications based on specific patterns in the EC2 instance logs.
Stop the Honeypot (Optional): If needed, you can stop or terminate the EC2 instance through the AWS EC2 console.

Network Monitoring: You can integrate AWS VPC Flow Logs to monitor traffic to and from the honeypot.
Centralized Logging: You can configure AWS CloudWatch or S3 bucket storage for Cowrie logs to track them across multiple honeypot instances.

AWS SNS Alert System:
The script creates an SNS topic that sends an email to the AlertEmail address whenever a CloudWatch alarm is triggered.

Monitor and Respond
After the environment is set up, monitor the email inbox for alerts triggered by the honeypot or CPU spikes.
Adjust the thresholds for CloudWatch alarms based on actual system usage to reduce false positives.
