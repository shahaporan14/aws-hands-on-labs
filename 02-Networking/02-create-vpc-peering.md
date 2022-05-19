# Create VPC using AWS Management Console

## Step-01: Create VPC - A
- **Name:** DEMO-VPC-A
- **IPv4 CIDR Block:** 10.200.0.0/16
- Rest all defaults
- Click on **Create VPC**

## Step-03: Create Internet Gateway and Associate it to VPC
- **Name Tag:** DEMO-IGW-A
- Click on **Create Internet Gateway**
- Click on Actions -> Attach to VPC -> DEMO-VPC-A

## Step-04: Create Subnets
### Step-04-01-01: Public Subnet
- **VPC ID:** DEMO-VPC-A
- **Subnet Name::**DEMO-PUBLIC-SUBNET-A
- **Availability zone:** us-east-1a
- **IPv4 CIDR Block:** 10.200.0.0/24

### Step-04-01-02: Create Public Route Table
- **Name tag:** DEMO-PUBLIC-RT-A
- **vpc:** DEMO-VPC-A
- Click on **Create**

### Step-04-01-03: Associate Subnet with Public RT
- Click on **Edit Subnet Associations**
- Select on **DEMO-PUBLIC-SUBNET-1**
- Click on **Save associations**

### Step-04-01-04: Create Public Route in Route Table
- Click on **Add Route**
- **Destination:** 0.0.0.0/0
- **Target:** DEMO-IGW-A
- Click on **Save Route**


## Step-05: Create VPC in different region- B
- **Name:** DEMO-VPC-B
- **IPv4 CIDR Block:** 10.100.0.0/16
- Rest all defaults
- Click on **Create VPC**

### Step-05-01: Private Subnet
- **Subnet Name::** DEMO-PRIVATE-SUBNET-B
- **Availability zone:** us-west-1a
- **IPv4 CIDR Block:** 10.100.0.0/24
- Click on **Create Subnet**


### Step-05-02: Create Private Route Table
- **Name tag:** DEMO-PRIVATE-RT-B
- **vpc:** DEMO-VPC-B
- Click on **Create**

### Step-05-03:  Associate Subnet with Private RT
- Click on **Edit Subnet Associations**
- Select on **DEMO-PRIVATE-SUBNET-B**
- Click on **Save associations**


## Step-06: Create VPC peering Connections from VPC A
- **Name :** DEMO-VPC-A-TO-VPC-B
- **vpc REQ:** VPC-A
- **Account** Account->My account
- **Region** Any region
- **VPC Accepter** VPC-B
- Click on **Create**


## Step-07: Create Security Group for Public EC2 Intance - A
- Click on **Security**
- **Name:**DEMO-PUBLIC-A-SG
- **Description:**DEMO-PUBLIC-A-SG
- **VPC:** DEMO-VPC
- **Inbound security:**
    - TYPE: SSH 
    - PORT :  22 
    - Source: Anywhere
    - TYPE: ICMP 
    - PORT :   0-65535 
    - Source: 10.100.0.0/16
- Click on **Create security group**

## Step-08: Create Public EC2 
  - Name: PUBLIC-EC2-A
  - Number of instances: 1
  - AMI: Linux (free tier)
  - Instance type: t2.micro
  - KeyName: demo.pem
  - Network settings: 
    - VPC: DEMO-VPC-A
    - Subnet:DEMO-PUBLIC-SUBNET-A
    - Auto-assign: Enable
    - Security: DEMO-PUBLIC-A-SG
  - Configure storage: Default
  - Advanced Settings: Default

## Step-09: Create Security Group for Private Ec2 Intance
- Click on **Security**
- **Name:** DEMO-PRIVATE-SG-B
- **Description:** DEMO-PRIVATE-SG-B
- **VPC:** DEMO-VPC-B
- **Inbound security:**
    - TYPE: SSH && ALL ICMP
    - PORT :  22  & 0-65535
    - Source: 10.200.0.0/16
- Click on **Create security group**

## Step-10: Create Private EC2 
  - Name: PRIVATE-EC2-B
  - Number of instances: 1
  - AMI: Linux (free tier)
  - Instance type: t2.micro
  - KeyName: demo.pem
  - Network settings: 
    - VPC: DEMO-VPC-B
    - Subnet: DEMO-PRIVATE-SUBNET-B
    - Auto-assign: disabled
    - Security: DEMO-PRIVATE-SG-B
  - Configure storage: Default
  - Advanced Settings: Default

## Step-11: Verify with Public Ec2 instance 
 ``` 
 chmod 400 demo.pem 
 ssh -i "demo.pem" ec2-user@3.82.129.8 
 ping 10.200.1.216
 ```
 ## Step-12: Verify with private Ec2 instance through the Public Ec2 instance
- EC2 Private 
- open your **Key** into text editor 
- copy the **key**
- Terminal 
  ```
  vi demo.pem 
  chmod 400 demo.pem 
  ssh -i "demo.pem" ec2-user@10.200.1.216
  ping ip
  ```

## Step-13: Create VPC peering Connections from VPC A
- Click on **Peer Connection** -> Action->Accept Request

## Step-14: Update Routes in Route Table in VPC A
- Click on **Add Route**
- **Destination:** 10.100.0.0/24
- **Target:** VPC-PEERING-ID
- Click on **Save Route**

## Step-15: Update Routes in Route Table in VPC B
- Click on **Add Route**
- **Destination:** 10.200.0.0/24
- **Target:** VPC-PEERING-ID
- Click on **Save Route**


## Step-11: Delete everything
- Delete `Instance`
- Delete `VPC peering`
- Delete `VPC`


