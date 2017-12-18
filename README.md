# Overview
This module setups a VPN server for a VPC to connect to instances.

# Input variables

* **aws_access_key:** ex) AKIAXXXXXXXXXXXXXXXXXX
* **aws_secret_key:** ex) XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
* **aws_key_name:** SSH Key pair for VPN instance
* **vpc_id:** The VPC id
* **public_subnet_id:** One of the public subnets to create the instance
* **ami_id:** Amazon Linux AMI ID
* **instance_type:** Instance type of the VPN box (t2.small is mostly enough)
* **office_ip_cidrs:** List of office IP addresses that you can SSH and non-VPN connected users can reach temporary profile download pages
* AWS Tags
  * **tag_product**
  * **tag_env**
  * **tag_purpose**
  * **tag_role**

# Outputs
* **vpn_instance_private_ip_address:** Private IP address of the instance
* **vpn_public_ip_addres:** EIP of the VPN box
* **pritunl setup-key:** setup-key input initial consoles


# Usage

```

provider "aws" {
	region="eu-west-1"
}

module "app_pritunl" {
  source           = "github.com/opsgang/terraform_pritunl?ref=1.0.0"

  aws_access_key     = "AKIAXXXXXXXXXXXXXXXXXX"
  aws_secret_key     = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
  aws_key_name     = "org-eu-west-1"
  vpc_id           = "${module.vpc.vpc_id}"
  public_subnet_id = "${module.vpc.public_subnets[1]}"
  ami_id           = "ami-01ccc867"
  instance_type    = "t2.small"
  office_ip_cidrs  = [
                      "8.8.8.8/32"
  ]

  tag_product      = "vpn"
  tag_env          = "dev"
  tag_purpose      = "networking"
  tag_role         = "vpn"
}
```

**P.S. :** Yes, AMI id is hardcoded! This module meant to be used in your VPC template. Presumably, no one wants to destroy the VPN instance and restore the configuration after `terraform apply` against to VPC. There is no harm to manage that manually and keep people working during the day.

*There will be wiki link about initial setup of Pritunl*

# Refarence
https://github.com/opsgang/terraform_pritunl
