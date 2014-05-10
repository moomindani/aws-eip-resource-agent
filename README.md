# aws-eip-resource-agent

Resource Agent for AWS EC2 EIP for Pacemaker

This resource agent script is intended for use with Amazon EC2 instances running Linux operating systems and Pacemaker.

The scripts have been tested on the following Amazon Machine Images (AMIs) version:

* Red Hat Enterprise Linux 6.5

The scripts have been tested on the following Pacemaker version:

* Pacemaker 1.0.13-1.2

## Prerequisites

This script requires [AWS Command Line Interface](http://aws.amazon.com/cli/).

### Install AWS Command Line Interface

If pip is not installed, install pip with following commands:
```
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
```
To install awscli, use pip:

```
pip install awscli
```

## Getting Started

Download "eip" or clone repository: 

```
git clone https://github.com/moomindani/aws-eip-resource-agent.git
cd aws-eip-resource-agent
```

Install eip resource agent:

```
mv eip /usr/lib/ocf/resource.d/heartbeat/
chown root:root /usr/lib/ocf/resource.d/heartbeat/eip 
chmod 0755 /usr/lib/ocf/resource.d/heartbeat/eip
```

After setting up pacemaker, configure crm setting to make eip resource agent ready. 
Here's an example setting:

```
property \
    no-quorum-policy="ignore" \
    stonith-enabled="false" \
    crmd-transition-delay="0s"

primitive eip ocf:heartbeat:eip \
    params \
        elastic_ip="54.178.202.118" \
    op start   timeout="60s" interval="0s"  on-fail="stop" \
    op monitor timeout="60s" interval="10s" on-fail="restart" \
    op stop    timeout="60s" interval="0s"  on-fail="block"
```

## Parameter

Name                       | Description
-------------------------- | -------------------------------------------------
elastic_ip                 | The Elastic IP address.
