# terraform init
# terraform plan
# terraform apply
# terraform destroy
# terraform refresh #shows the states/outputs without really applying changes

# terraform apply --auto-approve # automatically apply without confirmation

# delete/apply only one resource
# terraform apply -target aws_instance.web-server-instance
# terraform destroy -target aws_instance.web-server-instance

# $ terraform state list
```
aws_eip.one
aws_instance.web-server-instance
aws_internet_gateway.gw
aws_network_interface.web-server-nic
aws_route_table.prod-route-table
aws_route_table_association.a
aws_security_group.allow_web
aws_subnet.subnet-1
aws_vpc.prod-vpc
```

# $ terraform state show aws_instance.web-server-instance
```
# aws_instance.web-server-instance:
resource "aws_instance" "web-server-instance" {
    ami                          = "ami-02aa7f3de34db391a"
    arn                          = "arn:aws:ec2:us-east-2:818472496193:instance/i-0588e17dfc6ef07d9"
    associate_public_ip_address  = true
    availability_zone            = "us-east-2a"
    cpu_core_count               = 1
    cpu_threads_per_core         = 1
    disable_api_termination      = false
    ebs_optimized                = false
    get_password_data            = false
    hibernation                  = false
    id                           = "i-0588e17dfc6ef07d9"
    instance_state               = "running"
    instance_type                = "t2.micro"
    ipv6_address_count           = 0
    ipv6_addresses               = []
    key_name                     = "terraform-main-key"
    monitoring                   = false
    primary_network_interface_id = "eni-0c715d436ed817045"
    private_dns                  = "ip-10-0-1-50.us-east-2.compute.internal"
    private_ip                   = "10.0.1.50"
    public_ip                    = "3.141.188.212"
    secondary_private_ips        = []
    security_groups              = []
    source_dest_check            = true
    subnet_id                    = "subnet-0a3b8c48f70b1eb2c"
    tags                         = {
        "Name" = "terraform-prod"
    }
    tenancy                      = "default"
    user_data                    = "b1012ca44c304a108cf032bccc08286b0ccced2b"
    vpc_security_group_ids       = [
        "sg-050c279e907acbc56",
    ]

    credit_specification {
        cpu_credits = "standard"
    }

    enclave_options {
        enabled = false
    }

    metadata_options {
        http_endpoint               = "enabled"
        http_put_response_hop_limit = 1
        http_tokens                 = "optional"
    }

    network_interface {
        delete_on_termination = false
        device_index          = 0
        network_interface_id  = "eni-0c715d436ed817045"
    }

    root_block_device {
        delete_on_termination = true
        device_name           = "/dev/sda1"
        encrypted             = false
        iops                  = 100
        tags                  = {}
        throughput            = 0
        volume_id             = "vol-0c485640be0046d0a"
        volume_size           = 8
        volume_type           = "gp2"
    }
}
```

# `terraform output` printout all the outputs
```
$ terraform output
server_info = [
  "ip-10-0-1-50.us-east-2.compute.internal",
  "subnet-0a3b8c48f70b1eb2c",
]
server_public_ip = "3.141.188.212"
```
# using a variable without default value will trigger prompt to input:
$ terraform apply --auto-approve
```
var.available_zone
  AWS zone to pin up the resources

  Enter a value: us-east-2a

aws_vpc.prod-vpc: Refreshing state... [id=vpc-08a37582990a53a1a]
aws_internet_gateway.gw: Refreshing state... [id=igw-0a5ca052966e66515]
aws_subnet.subnet-1: Refreshing state... [id=subnet-0a3b8c48f70b1eb2c]
aws_security_group.allow_web: Refreshing state... [id=sg-050c279e907acbc56]
aws_route_table.prod-route-table: Refreshing state... [id=rtb-0d5d06ccc05222fb3]
aws_network_interface.web-server-nic: Refreshing state... [id=eni-0c715d436ed817045]
aws_route_table_association.a: Refreshing state... [id=rtbassoc-0234639c258aa17c1]
aws_eip.one: Refreshing state... [id=eipalloc-0c71ff81db2a11a94]
aws_instance.web-server-instance: Refreshing state... [id=i-0a8dd582e3bf0566d]

Apply complete! Resources: 0 added, 0 changed, 0 destroyed.

Outputs:

server_info = [
  "ip-10-0-1-50.us-east-2.compute.internal",
  "subnet-0a3b8c48f70b1eb2c",
]
server_public_ip = "3.141.188.212"

# or pass the variable through command line:
$ terraform apply -var "available_zone=us-east-2a" --auto-approve
aws_vpc.prod-vpc: Creating...
aws_vpc.prod-vpc: Creation complete after 2s [id=vpc-0e661cf851805f26b]
aws_internet_gateway.gw: Creating...
aws_subnet.subnet-1: Creating...
aws_security_group.allow_web: Creating...
aws_subnet.subnet-1: Creation complete after 1s [id=subnet-0b2ee50019518aec9]
aws_internet_gateway.gw: Creation complete after 1s [id=igw-01c066d3ca97b7aa2]
aws_route_table.prod-route-table: Creating...
aws_route_table.prod-route-table: Creation complete after 2s [id=rtb-0629eecd1ed74ddd1]
aws_route_table_association.a: Creating...
aws_security_group.allow_web: Creation complete after 3s [id=sg-06e88a4b20f658a07]
aws_network_interface.web-server-nic: Creating...
aws_route_table_association.a: Creation complete after 0s [id=rtbassoc-08ef0caa60ef7c32a]
aws_network_interface.web-server-nic: Still creating... [10s elapsed]
aws_network_interface.web-server-nic: Still creating... [20s elapsed]
aws_network_interface.web-server-nic: Still creating... [30s elapsed]
aws_network_interface.web-server-nic: Creation complete after 31s [id=eni-079c3df12b2c6d3e1]
aws_eip.one: Creating...
aws_instance.web-server-instance: Creating...
aws_eip.one: Creation complete after 1s [id=eipalloc-0b9ee5928cc2cf20e]
aws_instance.web-server-instance: Still creating... [10s elapsed]
aws_instance.web-server-instance: Still creating... [20s elapsed]
aws_instance.web-server-instance: Creation complete after 22s [id=i-00a017c557860442c]

Apply complete! Resources: 9 added, 0 changed, 0 destroyed.

Outputs:

server_info = [
  "ip-10-0-1-50.us-east-2.compute.internal",
  "subnet-0b2ee50019518aec9",
]
server_public_ip = "3.140.163.30"
```
# another option is to define the variable in `terraform.tfvars` which is the default place for variable configuration.

# passing variable definition file
$ terraform destroy -var-file credential.tfvars
```
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # aws_eip.one will be destroyed
  - resource "aws_eip" "one" {
      - associate_with_private_ip = "10.0.1.50" -> null
      - association_id            = "eipassoc-09f6bdce2764f8a58" -> null
      - domain                    = "vpc" -> null
      - id                        = "eipalloc-0ea7b996239a1a855" -> null
      - instance                  = "i-00c72dc123564f371" -> null
      - network_border_group      = "us-east-2" -> null
      - network_interface         = "eni-08a1d304d53dab607" -> null
      - private_dns               = "ip-10-0-1-50.us-east-2.compute.internal" -> null
      - private_ip                = "10.0.1.50" -> null
      - public_dns                = "ec2-3-23-245-115.us-east-2.compute.amazonaws.com" -> null
      - public_ip                 = "3.23.245.115" -> null
      - public_ipv4_pool          = "amazon" -> null
      - tags                      = {} -> null
      - vpc                       = true -> null
    }

  # aws_instance.web-server-instance will be destroyed
  - resource "aws_instance" "web-server-instance" {
      - ami                          = "ami-02aa7f3de34db391a" -> null
      - arn                          = "arn:aws:ec2:us-east-2:818472496193:instance/i-00c72dc123564f371" -> null
      - associate_public_ip_address  = false -> null
      - availability_zone            = "us-east-2a" -> null
      - cpu_core_count               = 1 -> null
      - cpu_threads_per_core         = 1 -> null
      - disable_api_termination      = false -> null
      - ebs_optimized                = false -> null
      - get_password_data            = false -> null
      - hibernation                  = false -> null
      - id                           = "i-00c72dc123564f371" -> null
      - instance_state               = "running" -> null
      - instance_type                = "t2.micro" -> null
      - ipv6_address_count           = 0 -> null
      - ipv6_addresses               = [] -> null
      - key_name                     = "terraform-main-key" -> null
      - monitoring                   = false -> null
      - primary_network_interface_id = "eni-08a1d304d53dab607" -> null
      - private_dns                  = "ip-10-0-1-50.us-east-2.compute.internal" -> null
      - private_ip                   = "10.0.1.50" -> null
      - secondary_private_ips        = [] -> null
      - security_groups              = [] -> null
      - source_dest_check            = true -> null
      - subnet_id                    = "subnet-0df6415eba8051111" -> null
      - tags                         = {
          - "Name" = "terraform-prod"
        } -> null
      - tenancy                      = "default" -> null
      - user_data                    = "b1012ca44c304a108cf032bccc08286b0ccced2b" -> null
      - vpc_security_group_ids       = [
          - "sg-0724c2a148596c59c",
        ] -> null

      - credit_specification {
          - cpu_credits = "standard" -> null
        }

      - enclave_options {
          - enabled = false -> null
        }

      - metadata_options {
          - http_endpoint               = "enabled" -> null
          - http_put_response_hop_limit = 1 -> null
          - http_tokens                 = "optional" -> null
        }

      - network_interface {
          - delete_on_termination = false -> null
          - device_index          = 0 -> null
          - network_interface_id  = "eni-08a1d304d53dab607" -> null
        }

      - root_block_device {
          - delete_on_termination = true -> null
          - device_name           = "/dev/sda1" -> null
          - encrypted             = false -> null
          - iops                  = 100 -> null
          - tags                  = {} -> null
          - throughput            = 0 -> null
          - volume_id             = "vol-0154502ba6974e116" -> null
          - volume_size           = 8 -> null
          - volume_type           = "gp2" -> null
        }
    }

  # aws_internet_gateway.gw will be destroyed
  - resource "aws_internet_gateway" "gw" {
      - arn      = "arn:aws:ec2:us-east-2:818472496193:internet-gateway/igw-039fb3f5561bba8af" -> null
      - id       = "igw-039fb3f5561bba8af" -> null
      - owner_id = "818472496193" -> null
      - tags     = {} -> null
      - vpc_id   = "vpc-04b5635b546d50c65" -> null
    }

  # aws_network_interface.web-server-nic will be destroyed
  - resource "aws_network_interface" "web-server-nic" {
      - id                 = "eni-08a1d304d53dab607" -> null
      - ipv6_address_count = 0 -> null
      - ipv6_addresses     = [] -> null
      - mac_address        = "02:d8:e4:38:d8:7c" -> null
      - private_ip         = "10.0.1.50" -> null
      - private_ips        = [
          - "10.0.1.50",
        ] -> null
      - private_ips_count  = 0 -> null
      - security_groups    = [
          - "sg-0724c2a148596c59c",
        ] -> null
      - source_dest_check  = true -> null
      - subnet_id          = "subnet-0df6415eba8051111" -> null
      - tags               = {} -> null

      - attachment {
          - attachment_id = "eni-attach-0a02d9401393c44da" -> null
          - device_index  = 0 -> null
          - instance      = "i-00c72dc123564f371" -> null
        }
    }

  # aws_route_table.prod-route-table will be destroyed
  - resource "aws_route_table" "prod-route-table" {
      - id               = "rtb-09690b471c3386e25" -> null
      - owner_id         = "818472496193" -> null
      - propagating_vgws = [] -> null
      - route            = [
          - {
              - cidr_block                = ""
              - egress_only_gateway_id    = ""
              - gateway_id                = "igw-039fb3f5561bba8af"
              - instance_id               = ""
              - ipv6_cidr_block           = "::/0"
              - local_gateway_id          = ""
              - nat_gateway_id            = ""
              - network_interface_id      = ""
              - transit_gateway_id        = ""
              - vpc_endpoint_id           = ""
              - vpc_peering_connection_id = ""
            },
          - {
              - cidr_block                = "0.0.0.0/0"
              - egress_only_gateway_id    = ""
              - gateway_id                = "igw-039fb3f5561bba8af"
              - instance_id               = ""
              - ipv6_cidr_block           = ""
              - local_gateway_id          = ""
              - nat_gateway_id            = ""
              - network_interface_id      = ""
              - transit_gateway_id        = ""
              - vpc_endpoint_id           = ""
              - vpc_peering_connection_id = ""
            },
        ] -> null
      - tags             = {
          - "Name" = "terraform-prod"
        } -> null
      - vpc_id           = "vpc-04b5635b546d50c65" -> null
    }

  # aws_route_table_association.a will be destroyed
  - resource "aws_route_table_association" "a" {
      - id             = "rtbassoc-06f7952cfef1a085e" -> null
      - route_table_id = "rtb-09690b471c3386e25" -> null
      - subnet_id      = "subnet-0df6415eba8051111" -> null
    }

  # aws_security_group.allow_web will be destroyed
  - resource "aws_security_group" "allow_web" {
      - arn                    = "arn:aws:ec2:us-east-2:818472496193:security-group/sg-0724c2a148596c59c" -> null
      - description            = "Allow Web inbound traffic" -> null
      - egress                 = [
          - {
              - cidr_blocks      = [
                  - "0.0.0.0/0",
                ]
              - description      = ""
              - from_port        = 0
              - ipv6_cidr_blocks = []
              - prefix_list_ids  = []
              - protocol         = "-1"
              - security_groups  = []
              - self             = false
              - to_port          = 0
            },
        ] -> null
      - id                     = "sg-0724c2a148596c59c" -> null
      - ingress                = [
          - {
              - cidr_blocks      = [
                  - "0.0.0.0/0",
                ]
              - description      = "HTTP"
              - from_port        = 80
              - ipv6_cidr_blocks = []
              - prefix_list_ids  = []
              - protocol         = "tcp"
              - security_groups  = []
              - self             = false
              - to_port          = 80
            },
          - {
              - cidr_blocks      = [
                  - "0.0.0.0/0",
                ]
              - description      = "HTTPS"
              - from_port        = 443
              - ipv6_cidr_blocks = []
              - prefix_list_ids  = []
              - protocol         = "tcp"
              - security_groups  = []
              - self             = false
              - to_port          = 443
            },
          - {
              - cidr_blocks      = [
                  - "0.0.0.0/0",
                ]
              - description      = "SSH"
              - from_port        = 22
              - ipv6_cidr_blocks = []
              - prefix_list_ids  = []
              - protocol         = "tcp"
              - security_groups  = []
              - self             = false
              - to_port          = 22
            },
        ] -> null
      - name                   = "allow_web_traffic" -> null
      - owner_id               = "818472496193" -> null
      - revoke_rules_on_delete = false -> null
      - tags                   = {
          - "Name" = "allow_web"
        } -> null
      - vpc_id                 = "vpc-04b5635b546d50c65" -> null
    }

  # aws_subnet.subnet-1 will be destroyed
  - resource "aws_subnet" "subnet-1" {
      - arn                             = "arn:aws:ec2:us-east-2:818472496193:subnet/subnet-0df6415eba8051111" -> null
      - assign_ipv6_address_on_creation = false -> null
      - availability_zone               = "us-east-2a" -> null
      - availability_zone_id            = "use2-az1" -> null
      - cidr_block                      = "10.0.1.0/24" -> null
      - id                              = "subnet-0df6415eba8051111" -> null
      - map_customer_owned_ip_on_launch = false -> null
      - map_public_ip_on_launch         = false -> null
      - owner_id                        = "818472496193" -> null
      - tags                            = {
          - "Name" = "terraform-prod"
        } -> null
      - vpc_id                          = "vpc-04b5635b546d50c65" -> null
    }

  # aws_vpc.prod-vpc will be destroyed
  - resource "aws_vpc" "prod-vpc" {
      - arn                              = "arn:aws:ec2:us-east-2:818472496193:vpc/vpc-04b5635b546d50c65" -> null
      - assign_generated_ipv6_cidr_block = false -> null
      - cidr_block                       = "10.0.0.0/16" -> null
      - default_network_acl_id           = "acl-0bac7f032c4d9654b" -> null
      - default_route_table_id           = "rtb-0705a0809e1615551" -> null
      - default_security_group_id        = "sg-071163714343681f1" -> null
      - dhcp_options_id                  = "dopt-255e894e" -> null
      - enable_dns_hostnames             = false -> null
      - enable_dns_support               = true -> null
      - id                               = "vpc-04b5635b546d50c65" -> null
      - instance_tenancy                 = "default" -> null
      - main_route_table_id              = "rtb-0705a0809e1615551" -> null
      - owner_id                         = "818472496193" -> null
      - tags                             = {
          - "Name" = "terraform-prod"
        } -> null
    }

Plan: 0 to add, 0 to change, 9 to destroy.

Changes to Outputs:

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

aws_route_table_association.a: Destroying... [id=rtbassoc-06f7952cfef1a085e]
aws_eip.one: Destroying... [id=eipalloc-0ea7b996239a1a855]
aws_instance.web-server-instance: Destroying... [id=i-00c72dc123564f371]
aws_route_table_association.a: Destruction complete after 0s
aws_route_table.prod-route-table: Destroying... [id=rtb-09690b471c3386e25]
aws_route_table.prod-route-table: Destruction complete after 1s
aws_eip.one: Destruction complete after 1s
aws_internet_gateway.gw: Destroying... [id=igw-039fb3f5561bba8af]
aws_instance.web-server-instance: Still destroying... [id=i-00c72dc123564f371, 10s elapsed]
aws_internet_gateway.gw: Still destroying... [id=igw-039fb3f5561bba8af, 11s elapsed]
aws_internet_gateway.gw: Destruction complete after 11s
aws_instance.web-server-instance: Still destroying... [id=i-00c72dc123564f371, 20s elapsed]
aws_instance.web-server-instance: Still destroying... [id=i-00c72dc123564f371, 30s elapsed]
aws_instance.web-server-instance: Still destroying... [id=i-00c72dc123564f371, 40s elapsed]
aws_instance.web-server-instance: Destruction complete after 40s
aws_network_interface.web-server-nic: Destroying... [id=eni-08a1d304d53dab607]
aws_network_interface.web-server-nic: Destruction complete after 1s
aws_subnet.subnet-1: Destroying... [id=subnet-0df6415eba8051111]
aws_security_group.allow_web: Destroying... [id=sg-0724c2a148596c59c]
aws_security_group.allow_web: Destruction complete after 1s
aws_subnet.subnet-1: Destruction complete after 1s
aws_vpc.prod-vpc: Destroying... [id=vpc-04b5635b546d50c65]
aws_vpc.prod-vpc: Destruction complete after 0s

Destroy complete! Resources: 9 destroyed.
```