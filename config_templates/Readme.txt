This directory is used to store the userdata templates for controlling boot configuration on ec2 instances.
The template is used in conjunction with the main.tf userdata.tpl file that looks something like this:

If you change the userdata, and it requires a variable for each instance, then make sure you add the variable to the
template definition in main.tf. The terraform apply will complain if you don't.

data "template_file" "fgt_userdata_byol1" {
  template = file("./config_templates/fgt-userdata-byol.tpl")

  vars = {
    fgt_id                = var.fortigate_hostname_1
    Port1IP               = var.public1_ip_address
    Port2IP               = var.private1_ip_address
    Port3IP               = var.sync_subnet_ip_address_1
    Port4IP               = var.ha_subnet_ip_address_1
    PrivateSubnet         = var.private_subnet_cidr_1
    spoke1_cidr           = var.vpc_cidr_east
    spoke2_cidr           = var.vpc_cidr_west
    mgmt_cidr             = var.ha_subnet_cidr_1
    mgmt_gw               = cidrhost(var.ha_subnet_cidr_1, 1)
    fgt_priority          = "255"
    fgt_ha_password       = var.fgt_ha_password
    fgt_byol_license      = file("${path.module}/${var.fgt_byol_1_license}")
    fgt-remote-heartbeat  = var.sync_subnet_ip_address_2
    PublicSubnetRouterIP  = cidrhost(var.public_subnet_cidr1, 1)
    public_subnet_mask    = cidrnetmask(var.public_subnet_cidr1)
    private_subnet_mask   = cidrnetmask(var.private_subnet_cidr_1)
    sync_subnet_mask      = cidrnetmask(var.sync_subnet_cidr_1)
    hamgmt_subnet_mask    = cidrnetmask(var.ha_subnet_cidr_1)
    PrivateSubnetRouterIP = cidrhost(var.private_subnet_cidr_1, 1)
    fgt_admin_password    = var.fgt_admin_password
  }
}

