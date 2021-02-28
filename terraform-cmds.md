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
aws_eip.one
aws_instance.web-server-instance
aws_internet_gateway.gw
aws_network_interface.web-server-nic
aws_route_table.prod-route-table
aws_route_table_association.a
aws_security_group.allow_web
aws_subnet.subnet-1
aws_vpc.prod-vpc

# $ terraform state show aws_instance.web-server-instance
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

# `terraform output` printout all the outputs
$ terraform output
server_info = [
  "ip-10-0-1-50.us-east-2.compute.internal",
  "subnet-0a3b8c48f70b1eb2c",
]
server_public_ip = "3.141.188.212"

# using a variable without default value will trigger prompt to input:
$ terraform apply --auto-approve
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

# another option is to define the variable in `terraform.tfvars` which is the default place for variable configuration.