## Data Center Group Routed Network Terraform Module

This Terraform module will deploy a Data Center Group Routed Network into an existing VMware Cloud Director (VCD) Environment.  This module can be used to provision new Data Center Group Networks into [Rackspace Technology SDDC Flex](https://www.rackspace.com/cloud/private/software-defined-data-center-flex) VCD Data Center Regions.

## Requirements

| Name | Version |
|------|---------|
| terraform | ~> 1.2 |
| vcd | ~> 3.8 |

## Resources

| Name | Type |
|------|------|
| [vcd_vdc_group](https://registry.terraform.io/providers/vmware/vcd/3.8/docs/data-sources/vdc_group) | data source |
| [vcd_nsxt_edgegateway](https://registry.terraform.io/providers/vmware/vcd/3.8/docs/data-sources/nsxt_edgegateway) | data source |
| [vcd_network_routed_v2](https://registry.terraform.io/providers/vmware/vcd/3.8/docs/resources/network_routed_v2) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|----------|
| vdc_org_name | The name of the organization in VCD | string | N/A | yes |
| vdc_group_name | The name of the Data Center Group in VCD | string | N/A | yes |
| vdc_edge_name | Name of the Edge Gateway | string | N/A | yes |
| segments | A map of objects defining the segments to be created, including gateway, prefix length, DNS servers, DNS suffix, and IP pool ranges. | map(object({ gateway = string, prefix_length = number, dns1 = string, dns2 = string, dns_suffix = string, pool_ranges = list(object({ start_address = string, end_address = string })) })) | N/A | yes |

`NOTE:` The `segments` input requires a map of objects with specific attributes. Each segment object should have a `gateway`, `prefix_length`, `dns1`, `dns2`, `dns_suffix`, and `pool_ranges`. The `pool_ranges` should be a list of objects with `start_address` and `end_address`.

## Outputs

| Name | Description |
|------|-------------|
| segment_inputs | This output will provide a map of the segments created with their assigned attributes. |

## Example Usage

Here's an example of how to use this module in a `main.tf` file:

```terraform
module "org_vdc_routed_network" {
  source          = "github.com/global-vmware/vcd_network_routed_v2.git?ref=v2.0.1"
  
  vdc_group_name      = "<US1-VDC-GRP-NAME>"
  vdc_org_name        = "<US1-VDC-ORG-NAME>"
  vdc_edge_name       = "<US1-VDC-EDGE-NAME>"

  segments = {
    US1-Segment-01 = {
      gateway       = "172.16.0.1"
      prefix_length = 24
      dns1          = "192.168.255.228"
      dns2          = ""
      dns_suffix    = "mydomain.com"
      pool_ranges   = [
        {
          start_address = "172.16.0.50"
          end_address   = "172.16.0.100"
        }
      ]
    },
    US1-Segment-02 = {
      gateway       = "192.168.1.1"
      prefix_length = 24
      dns1          = "192.168.255.228"
      dns2          = ""
      dns_suffix    = "mydomain.com"
      pool_ranges   = [
        {
          start_address = "192.168.1.50"
          end_address   = "192.168.1.100"
        }
      ]
    }
  }
}
```

## Authors

This module is maintained by the [Global VMware Cloud Automation Services Team](https://github.com/global-vmware).
