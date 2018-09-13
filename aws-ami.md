# Objective:
## AWS-AMI-RHEL7 & AWS-AMI-WIN16 - Getting the latest AMI using terraform


## Steps:

### (i)Create a terraform main.tf file with the following resource:


	provider "aws" {

	  region = "<region>"
	}

	data "aws_ami" "search" {

	  most_recent = true
	  filter {
	    name   = "name"
	    values = ["<filter value>"]
	  }
	}


### (1) The resource "aws_ami" :

	(a) To get the recent AMI created by packer, set "most_recent = true" parameter. so that we can get the most recent AMI created by packer.

	(b) To get the recent AMI of RHEL7, use the filter given below,
		
		data "aws_ami" "search" {
		  most_recent = true

		  filter {
		    name   = "name"
		    values = ["RHEL-7.4_HVM_GA_*"]
		  }

		}

	(c) To get the recent AMI of Windows 2016, use the filter below,

		data "aws_ami" "search" {
		  most_recent = true

		  filter {
		    name   = "name"
		    values = ["Windows_Server-2016_*"]
		  }

		}

### (ii)Create outputs.tf file to display the AMI Id:
	
	output "ami_id"{

	  value = "${data.aws_ami.search.image_id}"
	}

Use the above terraform script to get the AMI Id based on the search results.


### (iii) We can directly pass the AMI Id to launch an Instance : Ref [aws_instance](https://www.terraform.io/docs/providers/aws/r/instance.html)

	resource "aws_instance" "main" {

	  ami  = "${data.aws_ami.search.image_id}"

	  instance_type = "t2.micro"

	  tags {
	    Name = "launch-instance"
	  }
	}






