## Data Center Group Routed Network Terraform Module
This Terraform module creates a Data Center Group Routed Network in an existing VMware Cloud Director (VCD) Environment.  This module is being used to provsion new Data Center Group Networks into the Rackspace SDDC Flex Data Center Regions.

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.2 |
| vcd | >= 3.8.2 |

## Resources

| Name | Type |
|------|------|
| [vcd_network_routed_v2](https://registry.terraform.io/providers/vmware/vcd/3.8.2/docs/resources/network_routed_v2) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|----------|
| vdc_group_name | Name of the Data Center Group | string | `"Data Center Group Name Format: <Account_Number>-<Region>-<Account_Name> <datacenter group>"` | yes |
| vdc_org_name | Name of the Data Center Group Organization | string | `"Data Center Group Name Format: <Account_Number>-<Region>-<Account_Name>"` | yes |
| vdc_edge_name | Name of the Data Center Group Edge Gateway | string | `"Edge Gateway Name Format: <Account_Number>-<Region>-<Edge_GW_Identifier>-<edge>"` | yes |
| prefix_length | CIDR notation prefix length for the Data Center Group Routed Network | string | `"24"` | no |
| segment_name | Name of the Data Center Group Routed Network | string | `"US1-Segment-01"` | no |
| segment_gateway | Gateway IP address for Data Center Group Routed Network | string | `"192.168.0.1"` | no |
| start_address | Start IP address of the static IP pool for the Data Center Group Routed Network | string | `"192.168.0.51"` | no |
| end_address | End IP address of the static IP pool for the Data Center Group Routed Network | string | `"192.168.0.100"` | no |
| dns1_address | Primary DNS server address for the Data Center Group Routed Network | string | `"192.168.255.228"` | no |
| dns2_address | Secondary DNS server address for the Data Center Group Routed Network | string | `""` | no |
| dns_suffix | DNS suffix for the Data Center Group Routed Network | string | `"segment01.local"` | no |

## Outputs

| Name | Description |
|------|-------------|
| network_name | The Name of the Data Center Group Routed Network |
| network_gateway | The Gateway Address of the Data Center Group Routed Network |
| network_static_ip_pool | The Static IP Pool Start and End Addresses |
| network_prefix_length | The Prefix Length of the Data Center Group Routed Network |
| network_dns1 | The IP Address of the Primary DNS Server |
| network_dns2 | The IP Address of the Secondary DNS Server |
| network_dns_suffix | The DNS Search Suffix |

## Example Usage
This is an example of a main.tf file that would use the "github.com/global-vmware/vcd-network-routed-v2" Module Source to create a Data Center Group Routed Network.

The Terraform code example for the main.tf file is below:

```terraform
module "org-vdc-routed-network" {
  source = "github.com/global-vmware/vcd-network-routed-v2.git?ref=v1.1.0"

  vdc_group_name = "US1-VDC-GRP-01"
  vdc_org_name = "US1-VDC-ORG-01"
  vdc_edge_name = "US1-VDC-EDGE-01"

  prefix_length = "24"

  segment_name = "US1-Segment-01"
  segment_gateway = "192.168.50.1"

  start_address = "192.168.50.51"
  end_address = "192.168.50.100"

  dns1_address = "192.168.50.10"
  dns2_address = "192.168.50.11"
  dns_suffix = "domain.local"
}
```