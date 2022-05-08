# Create VPC using AWS Management Console
## Step-01: Introduction
- Create Demo VPC
- Create Internet Gateway and Associate to VPC
- Create Public and Private Subnets
- Create NAT Gateway in Public Subnet
- Create Public Route Table, Add Public Route via Internet Gateway and Associate Public Subnet
- Create Private Route Table, Add Private Route via NAT Gateway and Associate Private Subnet
- Create Ec2 Instance
- Verify 

## Step-02: Create VPC
- **Name:** demo-vpc
- **IPv4 CIDR Block:** 10.200.0.0/16
- Rest all defaults
- Click on **Create VPC**

## Step-03: Create Internet Gateway and Associate it to VPC
- **Name Tag:** demo-igw
- Click on **Create Internet Gateway**
- Click on Actions -> Attach to VPC -> demo-vpc

## Step-04: Create Subnets
### Step-04-01-01: Public Subnet
- **VPC ID:** demo-vpc
- **Subnet Name::** demo-public-subnet-1
- **Availability zone:** us-east-1a
- **IPv4 CIDR Block:** 10.200.0.0/24

### Step-04-01-02: Create Public Route Table
- **Name tag:** demo-public-rt
- **vpc:** demo-vpc
- Click on **Create**

### Step-04-01-03: Associate Subnet with Public RT
- Click on **Edit Subnet Associations**
- Select on **demo-public-subnet-1**
- Click on **Save associations**

### Step-04-01-04: Create Public Route in Route Table
- Click on **Add Route**
- **Destination:** 0.0.0.0/0
- **Target:** my-igw
- Click on **Save Route**


### Step-04-02-01: Private Subnet
- **Subnet Name::** demo-private-subnet-1
- **Availability zone:** us-east-1b
- **IPv4 CIDR Block:** 10.200.1.0/24
- Click on **Create Subnet**

## Step-04-02-02:  Create NAT Gateway
- **Name:** demo-nat-gateway
- **Subnet:** demo-public-subnet-1
- **Allocate Elastic Ip:** click on that
- Click on **Create NAT Gateway**

### Step-04-02-03: Create Private Route Table
- **Name tag:** demo-private-rt
- **vpc:** demo-vpc
- Click on **Create**

### Step-04-02-04:  Associate Subnet with Private RT
- Click on **Edit Subnet Associations**
- Select on **demo-private-subnet-1**
- Click on **Save associations**

### Step-04-02-05: Create Private Route Route Table
- Click on **Add Route**
- **Destination:** 0.0.0.0/0
- **Target:** demo-nat-gateway
- Click on **Save Route**

## Step-05: Create Security Group for Public Ec2 Intance
- Click on **Security**
- **Name:** demo-public-sg
- **Description:** demo-public-sg
- **VPC:** demo-vpc
- **Inbound security:**
    - TYPE: SSH 
    - PORT :  22 
    - Source: Anywhere
- Click on **Create security group**

## Step-06: Create Public EC2 
  - Name: public-instance
  - Number of instances: 1
  - AMI: Linux (free tier)
  - Instance type: t2.micro
  - KeyName: demo.pem
  - Network settings: 
    - VPC: demo-vpc
    - Subnet: demo-public-subnet-1
    - Auto-assign: Enable
    - Security: public-demo-sg
  - Configure storage: Default
  - Advanced Settings: Default

## Step-07: Create Security Group for Private Ec2 Intance
- Click on **Security**
- **Name:** demo-private-sg
- **Description:** demo-private-sg
- **VPC:** demo-vpc
- **Inbound security:**
    - TYPE: SSH && ALL ICMP
    - PORT :  22  & 0-65535
    - Source: 10.200.0.0/24
- Click on **Create security group**

## Step-08: Create Private EC2 
  - Name: public-instance
  - Number of instances: 1
  - AMI: Linux (free tier)
  - Instance type: t2.micro
  - KeyName: demo.pem
  - Network settings: 
    - VPC: demo-vpc
    - Subnet: demo-private-subnet-1
    - Auto-assign: disabled
    - Security: private-demo-sg
  - Configure storage: Default
  - Advanced Settings: Default

## Step-09: Verify with Public Ec2 instance 
 ``` 
 chmod 400 demo.pem 
 ssh -i "demo.pem" ec2-user@3.82.129.8 
 ping 10.200.1.216
 ```
## Step-10: Verify with private Ec2 instance through the Public Ec2 instance
- EC2 Private 
- open your **Key** into text editor 
- copy the **key**
- Terminal 
  ```
  vi demo.pem 
  chmod 400 demo.pem 
  ssh -i "demo.pem" ec2-user@10.200.1.216
  ping google.com
  ```
## Step-11: Delete everything
- Delete `Instance`
- Delete `demo-nat-gateway`
- Wait till NAT Gateway is deleted
- Delete `demo-vpc`


