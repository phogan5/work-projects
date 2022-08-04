0.1: Two Windows EC2 instances are needed in order to adhere to security best practices and the principle of least privilege: 

1 machine that will be the ‘server’: the Windows machine generating logs to CloudWatch

1 machine that will be the ‘admin’: the Windows machine responsible for installing the CloudWatch agents

This way, the server machine can be configured to stream logs to CloudWatch without ever being directly interacted with. 

 

1: First, IAM roles will be created to give the necessary rights to each account, if roles are already assigned, add the following policies to the roles:

Server instance role:

AWS Service, EC2 Usecase

Policies:

CloudWatchAgentServerPolicy

AmazonEC2RoleforSSM [Soon to be deprecated, consider investigating alternative solution]

Admin instance role:

AWS Service, EC2 Usecase

Policies:

CloudWatchAgentAdminPolicy

AmazonEC2RoleforSSM

2: Assign admin and server roles to the admin and server instances

3: Commands will be run from AWS Systems Manager to install the CloudWatch agent on each instance. 

Command document: AWS-ConfigureAWSPackage

Package name: AmazonCloudWatchAgent

This command will be installed on both admin and server instances

[NOTE: If you don’t see your EC2 instance as an option for executing the command, check to make sure SSM (Session Manager) installed on the instance. This tool is what allows Systems Manager to remotely manage the EC2 instances. SSM generally comes installed by default on most AMI’s but given various security rules it may not. See this guide to installing SSM]

4: RDP into admin instance to run CloudWatch agent install wizard

Run file - C:\Program Files\Amazon\AmazonCloudWatchAgent\amazon-cloudwatch-agent-config-wizard.exe - via CMD or File Explorer

This will launch an installation wizard guiding you through installation and configuration of the log streaming. Configuration options include, but are not limited to:

Log expiration policy

Custom application metric collection using StatsD or collectd

Specify usage metrics collected (if any) using predefined CloudWatch config file or default AWS detail profiles

Specify Windows event logs to collect by log name, verbosity level, and urgency level (informational, warning, critical, etc.)

[Important] Log group name: the log group that will appear in the Cloudwatch console, name it something recognizable

Data format (XML, plaintext)

Parameter store name (remember this for later)

These parameters will automatically fill out and apply a CloudWatch config file that is viewable after finishing the wizard, if you want to make manual changes to the config or make one yourself that is also an option.

This config file is available through Systems Manager > Parameter Store > [Parameter store name], from here it can be altered at will to modify logs collected on the fly

5: Using AWS Systems Manager, run a command to install the CloudWatch config file created in Step 4 on the server machine.

Command document: AmazonCloudWatch-ManageAgent

Configuration location: [Parameter store name specified in Step 4 (default name is AmazonCloudWatch-windows)]

Location: [Server EC2 instance]

 

If everything worked successfully, there should be a new log stream accessible through CloudWatch > Log groups > Log streams
