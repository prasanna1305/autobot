# autobot
Tool to create AWS EC2 instances from commandline in bulk

Introduction:
AWS offers various services related to EC2 instances like 
• Creating nodes 
• Attaching volumes
• Stopping, starting, and terminating existing instances. 

If the number of instances are more; performing these operations can be very tricky and time consuming. 

When Teuthology is used on AWS nodes, the nodes should first be configured to run ceph related tests like BST through Teuthology. Preparing node for BST run involves number of tests:
• Root access to sudo user.
• SSH key generation without any paraphrase.
• Generated SSH keys of all the nodes should be listed in target file in Teuthology Admin Node.
• Installing some modules.
• Giving Passwordless SSH Access to Teuthology Admin Node.
• Adding the disk names on which OSDs will be mounted in /scratch_devs file.
• Hostname resolution in Teuthology Admin Node.

With Autobot any number of AWS nodes can be created with single input file. All the steps to make the nodes ready for BST runs are also performed using Autobot.

Pre-requisite
1. Instance type and AMI ID of the OS image should be known.
2. Keypair should be created and saved on local machine from where this tool will be invoked.
3. Python-boto, python-docopt modules should be installed.

Configuration file
Autobot requires input configuration file which is in following format:

[AWS]
instance_name=example_instance
owner_email=email_ID@sandisk.com
Usage=purpose_to_create_instances
instance_type=m3.xlarge
ami_id=ami-6ac2a85a
instance_num=100
root_vol_size=30
ebs_vol_num=1
ebs_vol_size=30
key=/home/ubuntu/example.pem

Instance_name: Name of the instance to be created or already created instance name
Owner_email: Email ID of the owner of instances
Usage: Purpose to create the instances
instance_type: Type of AWS instances
ami_id: AMI ID of the operating system of the instances
instance_num: Number of instances
root_vol_size: Size of Root Volume
ebs_vol_num: Number of volumes attached to instance apart from root volume
ebs_vol_size: Size of EBS volume attached
key: Path to Keypair to connect to instance

Usage

$ python autobot.py --help
Usage:
   autobot.py (-h | --help)
   autobot.py (create|start|stop|terminate) <config> [--configure]
   
   Commands:
   create                  Creates the instances
   start                   start the instances
   stop                    Stop the instances
   terminate               Terminate the instances
   
If --configure option is provided, Autobot will run all the pre-requisite commands on created nodes to prepare those nodes for teuthology.
