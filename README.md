 Ansible Inventory Lab
Static vs Dynamic Inventory in AWS Environments


This lab explains the difference between Static Inventory and Dynamic Inventory in Ansible, and why dynamic inventory is essential in cloud environments like AWS.


 What is Ansible Inventory?


Ansible Inventory is the source that contains the list of servers managed by Ansible.


It allows Ansible to know:


Which servers to manage
How to group servers (web, db, etc.)
SSH connection details (user, key, etc.)
Variables assigned to hosts or groups
 Static Inventory


Static Inventory means manually defining servers inside a file such as inventory.ini or hosts.yml.


Example:
[webservers]
web1 ansible_host=52.1.1.10
web2 ansible_host=52.1.1.11


[dbservers]
db1 ansible_host=52.1.1.12
✅ Advantages of Static Inventory
Simple and easy to understand
Good for small projects and labs
Easy to set up manually
❌ Limitations of Static Inventory
Relies on fixed IP addresses
In AWS, public IPs may change after restart
Requires manual updates
Not suitable for dynamic environments
☁️ Dynamic Inventory


Dynamic Inventory allows Ansible to automatically discover servers from external systems such as:


AWS EC2
Azure
Google Cloud
Kubernetes
APIs and cloud platforms
💡 How It Works


Instead of manually defining hosts, Ansible:

Connects to AWS
Retrieves EC2 instances
Filters them using tags
Builds the inventory dynamically
 Why Dynamic Inventory is Important


In cloud environments:


Instances are frequently created and deleted
IP addresses can change
Auto Scaling Groups add/remove servers automatically


 Manual inventory becomes unreliable


 AWS Tags + Dynamic Inventory


AWS Tags are key-value pairs assigned to EC2 instances:

Name = webserver
Environment = dev


Ansible uses these tags instead of relying on IP addresses.


Example Dynamic Inventory (AWS EC2)
plugin: amazon.aws.aws_ec2


regions:
  - us-east-1


filters:
  tag:Environment: dev
  instance-state-name: running


keyed_groups:
  - key: tags.Name
    prefix: tag
What This Does:
Connects to AWS
Searches in us-east-1 region
Filters only running instances
Selects instances with Environment=dev tag
Groups hosts based on Name tag



⚖️ Static vs Dynamic Inventory
Static Inventory
Manually defined hosts
Uses fixed IPs
Suitable for small labs
Requires manual updates
Dynamic Inventory
Automatically discovered hosts
Uses cloud APIs
Ideal for production environments
Scalable and flexible




🧪 Real-World Scenario
AWS EC2 Instance:
Name = webserver
Environment = dev
Public IP = 52.1.1.10


After restart:


Public IP = 3.2.2.20
❌ Static Inventory Behavior


Ansible still tries:


52.1.1.10 ❌ (connection fails)
✅ Dynamic Inventory Behavior




Ansible:

Queries AWS again
Finds the instance using tags
Gets updated IP automatically
Connects successfully
 Useful Commands
View inventory structure
ansible-inventory -i aws_ec2.yml --graph
List all hosts
ansible-inventory -i aws_ec2.yml --list
Test connection
ansible -i aws_ec2.yml all -m ping
Run playbook
ansible-playbook -i aws_ec2.yml playbook.yml



⚙️ Requirements

To use AWS Dynamic Inventory:

Ansible installed
AWS CLI configured
boto3 and botocore installed
amazon.aws Ansible collection
AWS credentials with EC2 read permissions
Properly tagged EC2 instances
Install AWS Collection
ansible-galaxy collection install amazon.aws
Install dependencies
pip install boto3 botocore
Configure AWS
aws configure
 Key Concept


Dynamic Inventory + AWS Tags = Automatic Discovery + Zero Manual Updates


 Summary


In this lab, you learned:


What Ansible Inventory is
Difference between Static and Dynamic Inventory
Why static inventory fails in cloud environments
How AWS EC2 dynamic inventory works
How tags help identify instances
Why dynamic inventory is the standard in DevOps
