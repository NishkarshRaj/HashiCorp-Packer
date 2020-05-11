# HashiCorp Packer - Infrastructure Build Tool

![Image](img/packer.png)

## Why use Packer?

Configuration Management and Infrastructure-as-a-Service is trending because of the surge of remote deployments of services and their access over network to the consumers via Cloud or containers techniques.
The biggest problem to infrastructure or pre-baked images is that they are written by development team and thus operations team are not too flexible to make changes to them due to **high learning curve**. Furthermore, it is difficult to **automate** infrastructure using pre-baked images.

* Create identical machine images for multiple platforms from a single source configuration. Packer automates the creation of any type of machine image.
* **Multi-provider portability:** Packer creates identical images which can be deployed on public cloud (AWS,GCP,Azure etc), private cloud (OpenStack etc.) or local desktop machines (Virtual Box, VWWare)
* Fast: Entire infrastructure is configured and provisioned in seconds 

## What are the alternatives to Packer?

Configuration Management tools like Chef and Puppet are the alternatives but Packer is becoming more popular because:

1. Easy to learn and use
2. Automate IaaS
3. Compatible with other tools and frameworks like Chef and Puppet

## Key Features of Packer

* **Fast infrastructure deployment** - provision infrastructure and configures its dependencies within seconds.

* **Multi-Provider Portability** - identical images for all platforms

* **Improved Stability** - configure dependency on image creation rather than configuration of infrastructure

* **Greater Testability** - Easy to smoke test one machine and that guarentees it will work everywhere due to high portability.

## Some Use-cases of Packer

* **Continuous Delivery** - Lightweight, Portable, CMD driven. Can be used to create portable image to be configured by Ansible, Chef.

* **Dev-Prod Parity** - Tries to reduce the differences between the various environments in software development lifecycle.

* **Appliance-Demo creation** - Very suitable for alpha and beta testing as user environment can be replicated very easily.

## Installation of Packer in Linux machines

* Install Golang on your machine
```
$ apt install golang-go
```

* Check version of golang for verifying successful installation
```
$ go version
```

* Clone packer in your `GOPATH`
```
$ mkdir -p $(go env GOPATH)/src/github.com/hashicorp && cd $_
$ git clone https://github.com/hashicorp/packer.git
$ cd packer
```

* Extract and install binaries using Make utility
```
$ make dev
```

* Verify successful Packer installation
```
$ packer --version
```

![packer](img/version.png)

## Direct installation of Packer for Debian based systems

```
$ apt-get upgrade && apt-get update
$ apt install packer
$ packer --version
```

## Launching an AWS EC2 instance using Packer

* Create a json script - `example.json`
```
{
  "variables": {
    "aws_access_key": "",
    "aws_secret_key": ""
  },
  "builders": [
    {
      "type": "amazon-ebs",
      "access_key": "{{user `aws_access_key`}}",
      "secret_key": "{{user `aws_secret_key`}}",
      "region": "us-east-1",
      "source_ami_filter": {
        "filters": {
          "virtualization-type": "hvm",
          "name": "ubuntu/images/*ubuntu-xenial-16.04-amd64-server-*",
          "root-device-type": "ebs"
        },
        "owners": ["099720109477"],
        "most_recent": true
      },
      "instance_type": "t2.micro",
      "ssh_username": "ubuntu",
      "ami_name": "packer-example {{timestamp}}"
    }
  ]
}
```

* Validate the Packer Script
```
$ packer validate example.json
```

![aws](img/aws1.png)

![aws](img/aws2.png)

![aws](img/aws3.png)

![aws](img/aws4.png)

![aws](img/aws5.png)

![aws](img/aws6.png)

* Create IAM User and generate tokens to be passed as variables to the script file
```
$ export AWS_ACCESS_KEY_ID=AKIAYNZB56UH67JQO5XJ 
$ export AWS_SECRET_ACCESS_KEY=fwQqvr7Fad60aSG/ekKjeNi5nYEC5RXUNEsmUYJl
```

![export](img/export.png)

* Build the Packer script
```
$ packer build example.json
```

![build](img/b1.png)

![build](img/b2.png)

![build](img/AMI.png)

![build](img/EC2.png)

# Automation HashiCorp Packer using GitHub Actions

* Create a Dockefile in **action-a/** folder to specify Packer installation

![Image](img/Dockerfile.png)

* Write the main configuration file of GitHub Actions that executes the above created Dockerfile

![Image](img/Actions.png)

* Check for the current installation of Packer in GitHub Actions UI

![Image](img/Installation.png)

![Image](img/Installation2.png)

* Pass the AWS IAM Credentials as encrypted secrets to GitHub workflow

![Image](img/secret.png)

![Image](img/validate.png)

* Modify the Dockerfile to build the packer for AWS example.json file

```
RUN ADD example.json /example.json
RUN packer build example.json
```

**Note:** You might get a build error stating there is no default VPC is the user you have authenticated has no direct VPC attached

![Image](img/BuildError.png)

* Go to AWS - VPC services and create a default VPC.

![Image](img/DefaultVPC.png)

* Re-run the GitHub Actions workflow 

![Image](img/success.png)

* The workflow ran successfully and gives the AMI of the provisioned AWS EC2 instance.

![Image](img/success2.png)

![Image](img/success3.png)

* Visit AWS - EC2 services to see the snapshot in execution

![Image](img/success4.png)



