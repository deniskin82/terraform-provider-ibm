---
layout: "ibm"
page_title: "IBM : lbaas"
sidebar_current: "docs-ibm-resource-lbaas-server-instance-attachment"
description: |-
  Attach server instances to IBM Cloud Load balancer.
---

# ibm\_lbaas\_server\_instance\_attachment

Provides a resource for attaching the server instance to IBM cloud load balancer. This allows to attach, detach and update server instances as loadbalancer members to IBM cloud load balancer.
 
## Example Usage

```hcl

resource "ibm_compute_vm_instance" "vm_instances" {
  count = "2"
  ....
}

resource "ibm_lbaas" "lbaas" {
  name        = "terraformLB"
  description = "delete this"
  subnets     = [1511875]

  protocols = [{
    frontend_protocol     = "HTTPS"
    frontend_port         = 443
    backend_protocol      = "HTTP"
    backend_port          = 80
    load_balancing_method = "round_robin"
    tls_certificate_id    = 11670
  },
    {
      frontend_protocol     = "HTTP"
      frontend_port         = 80
      backend_protocol      = "HTTP"
      backend_port          = 80
      load_balancing_method = "round_robin"
    },
  ]
}

resource "ibm_lbaas_server_instance_attachment" "lbaas_member" {
  private_ip_address = "${ibm_compute_vm_instance.vm_instances.ipv4_address_private}"
  weight             = 40
  lbaas_id           = "${ibm_lbaas.lbaas.id}"
}

```

## Argument Reference

The following arguments are supported:

* `private_ip_address` - (Required, string) The private IP address of a load balancer member.
* `weight` - (Optional, integer) The weight of a load balancer member.
* `lbaas_id` - (Required, string) The UUID of a load balancer.

## Attributes Reference

The following attributes are exported:

* `uuid` - The unique identifier of the load balancer member.
