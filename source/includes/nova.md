# Nova

**Service Endpoint URI**: `https://<my-company-platform9-url>/nova/`

Platform9 OpenStack Compute (Nova) is a cloud computing fabric controller, which is the main part of an IaaS system. It is designed to manage and automate pools of computer resources.
The Nova API is used to manipulate and retrieve data regarding hypervisors and instances, as well as interact with them.

Check the [Usage](#usage) section for usage details.

## Instances

List, create, get details for, update, and delete instances (servers).

## GET /v2/​{tenant\_id}​/servers

```json
{
    "servers": [
        {
            "id": "a807443a-c9e3-45d4-8b71-149470773286",
            "links": [
                {
                    "href": "http://127.0.0.1:8774/v2/1067aba5194246a5a61fbdb31708404b/servers/a807443a-c9e3-45d4-8b71-149470773286",
                    "rel": "self"
                },
                {
                    "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/servers/a807443a-c9e3-45d4-8b71-149470773286",
                    "rel": "bookmark"
                }
            ],
            "name": "test-instance"
        },
        ...,
        {
            "id": "948ef2d6-6342-46f1-9466-f4fa52a0a405",
            "links": [
                {
                    "href": "http://127.0.0.1:8774/v2/1067aba5194246a5a61fbdb31708404b/servers/948ef2d6-6342-46f1-9466-f4fa52a0a405",
                    "rel": "self"
                },
                {
                    "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/servers/948ef2d6-6342-46f1-9466-f4fa52a0a405",
                    "rel": "bookmark"
                }
            ],
            "name": "testbed-vm-311615"
        }
    ]
}
```

Lists IDs, names, and links for all instances.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
servers | array | List of instances with their `id`, `links`, and `name`.
id | string | The unique ID of a particular instance.
links | array | Contains information for the REST API link for a particular instance.
href | string | The URL to get detailed information about a particular instance.
name | string | The name of a particular instance.

**Normal Response Codes**: 200, 203

**Error Response Codes**: computeFault (400, 500, …), serviceUnavailable (503), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413)

## <a id="create-instance"></a>POST /v2/​{tenant\_id}​/servers

> JSON Request

```json
{
    "server": {
        "flavorRef": "2",
        "imageRef": "7084c035-1f93-45d7-8aca-afe74f20cfee",
        "name": "test-instance",
        "networks": [
            {
                "uuid": "4b903d1d-e6ad-4618-8652-cce4d3cf2f42"
            }
        ]
    }
}
```

> JSON Response

```json
{
    "server": {
        "security_groups": [
            {
                "name": "default"
            }
        ],
        "OS-DCF:diskConfig": "MANUAL",
        "id": "a807443a-c9e3-45d4-8b71-149470773286",
        "links": [
            {
                "href": "http://127.0.0.1:8774/v2/1067aba5194246a5a61fbdb31708404b/servers/a807443a-c9e3-45d4-8b71-149470773286",
                "rel": "self"
            },
            {
                "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/servers/a807443a-c9e3-45d4-8b71-149470773286",
                "rel": "bookmark"
            }
        ],
        "adminPass": "3JwnM52WPyxG"
    }
}
```

Creates a new instance from an image with specified flavor configuration that is connected to the specified networks.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.
server | body | object | Contains information about the instance.
imageRef | body | string | The image reference for the desired image for your instance. Specify as an ID.
flavorRef | body | string | The flavor reference for the desired flavor for your instance. Specify as an ID.
name | body | string | The name for your created instance.
networks | body | array | Array of objects that contains the `uuid`, or unique identifier, of the desired networks.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
server | object | Contains information about the instance being created.
id | string | The unique ID of the instance being created.
links | array | Contains information for the REST API link for the instance being created.
href | string | The URL to get detailed information about the instance being created.

**Normal Response Codes**: 202

**Error Response Codes**: computeFault (400, 500, …), UnprocessableEntity (422), serviceUnavailable (503), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), itemNotFound (404), badMediaType (415), serverCapacityUnavailable (503)

## <a name="server-details"></a>GET /v2/​{tenant\_id}​/servers/detail

```json
{
  "servers": [
    {
      "status": "ACTIVE",
      "OS-EXT-PF9-SRV-RES-INFO:res_info": {
        "cfg_disk_gb": 0.0,
        "vcpus": 1,
        "cfg_memory_mb": 256,
        "cpu_mhz": 1000
      },
      "updated": "2014-08-29T18:04:42Z",
      "hostId": "5abc7966cca161c9926009429ed27b91abfd8a7a43dd47c3cb9a3fa6",
      "OS-EXT-SRV-ATTR:host": "eeeca4cc-ee71-4dc5-8689-4441337abd59",
      "addresses": {
        "Network-virbr0": [
          {
            "OS-EXT-IPS-MAC:mac_addr": "00:16:3e:54:8f:49",
            "version": 4,
            "addr": "192.168.122.187",
            "OS-EXT-IPS:type": "fixed"
          }
        ]
      },
      "links": [
        {
          "href": "http://127.0.0.1:8774/v2/1067aba5194246a5a61fbdb31708404b/servers/d7b47df2-c855-4ff0-b65b-bcb692988e32",
          "rel": "self"
        },
        {
          "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/servers/d7b47df2-c855-4ff0-b65b-bcb692988e32",
          "rel": "bookmark"
        }
      ],
      "key_name": null,
      "image": "",
      "OS-EXT-STS:task_state": null,
      "OS-EXT-STS:vm_state": "active",
      "OS-EXT-SRV-ATTR:instance_name": "instance-00000005",
      "OS-SRV-USG:launched_at": "2014-08-29T18:04:42.000000",
      "OS-EXT-SRV-ATTR:hypervisor_hostname": "testbed-klee-20140829-1057111.platform9.sys",
      "flavor": {
        "id": "94086",
        "links": [
          {
            "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/flavors/94086", 
            "rel": "bookmark"
          }
        ]
      },
      "id": "d7b47df2-c855-4ff0-b65b-bcb692988e32",
      "OS-SRV-USG:terminated_at": null,
      "OS-EXT-AZ:availability_zone": "nova",
      "user_id": "7f5230ee5c5b4a8c92d8c9906138ca04",
      "name": "testbed-vm-926240",
      "created": "2014-08-29T18:04:42Z",
      "tenant_id": "1067aba5194246a5a61fbdb31708404b",
      "OS-DCF:diskConfig": "MANUAL",
      "os-extended-volumes:volumes_attached": [],
      "accessIPv4": "",
      "accessIPv6": "",
      "progress": 0,
      "OS-EXT-STS:power_state": 1,
      "config_drive": "",
      "metadata": {}
    },
    ...
  ]
}
```

Lists details of all instances.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
servers | array | Contains list of instances and details about them.
status | string | The current status of the instance. An instance can be `ACTIVE`, `REBOOTING`, etc.
OS-EXT-PF9-SRV-RES-INFO:res\_info | object | Contains information about the instance's configuration. `cfg_disk_gb` refers to the configured disk size of the instance. `vcpus` refers to the number of virtual cpus on the instance. `cfg_memory_mb` refers to the configured amount of memory in MB on the instance. `cpu_mhz` refers to the virtual cpu speed in MHz of the instance.
updated | dateTime | Last time the instance was updated.
hostId | string | The unique ID of the instance's hypervisor.
addresses | object | List of networks the instance connects to. Each network has one or more interface specifications, which have the following properties. `OS-EXT-IPS-MAC:mac_addr` refers to the network's mac address. `version` refers to the IP version. `addr` refers to the IP address of the interface. `OS-EXT-IPS:type` refers to the network type, which can be **fixed** or **floating**.
links | array | Contains information for the REST API link of a the instance.
href | string | The URL to get detailed information about the instance.
image | string | The unique ID of the image from which the instance was created.
OS-EXT-STS:task\_state | string | A description of the task currently being run on the instance. If no task is being run, returns `null`.
OS-EXT-STS:vm\_state | string | The current state of the instance.
OS-EXT-SRV-ATTR:instance\_name | string | The hypervisor-specific name of the instance.
OS-SRV-USG:launched\_at | dateTime | Time that the instance was launched.
OS-EXT-SRV-ATTR:hypervisor\_hostname | string | The name of the instance's hypervisor.
flavor | object | Contains information about the instance's flavor. `id` refers to the flavor's unique ID. `href` contains the URL to a description of the flavor.
id | string | The unique ID of the instance.
OS-SRV-USG:terminated\_at | dateTime | Time that the instance was terminated.
user_id | string | The unique ID of the user who created the instance.
name | string | The name of the instance.
created | dateTime | Time that the instance was created.
tenant_id | string | The unique ID of the tenant the instance belongs to.
progress | integer | The percentage completion of the current task on the instance.
OS-EXT-STS:power\_state | integer | The current power state of the instance.

This operation does not require a request body.

**Normal Response Codes**: 202

## GET /v2/​{tenant\_id}​/servers/​{server\_id}​

```json
{
    "server":
    {
        "status": "ACTIVE",
        "OS-EXT-PF9-SRV-RES-INFO:res_info": {
            "cfg_disk_gb": 20.0,
            "vcpus": 1,
            "cfg_memory_mb": 2048,
            "cpu_mhz": 1000
        },
        "updated": "2014-09-02T19:13:40Z",
        "hostId": "e3a9e3c95dd3f14b1cbed018e8fd5082b5ae797297aba6d59a656fac",
        "OS-EXT-SRV-ATTR:host": "8fb7a370-c69b-4c6d-89ca-a90ca7de8901",
        "addresses": {"Network-br_isolated2": []},
        "links": [
            {
                "href": "http://127.0.0.1:8774/v2/1067aba5194246a5a61fbdb31708404b/servers/a807443a-c9e3-45d4-8b71-149470773286",
                "rel": "self"
            },
            {
                "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/servers/a807443a-c9e3-45d4-8b71-149470773286",
                "rel": "bookmark"
            }
        ],
        "key_name": null,
        "image": {
            "id": "7084c035-1f93-45d7-8aca-afe74f20cfee",
            "links": [
                {
                    "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/images/7084c035-1f93-45d7-8aca-afe74f20cfee",
                    "rel": "bookmark"
                }
            ]
        },
        "OS-EXT-STS:task_state": null,
        "OS-EXT-STS:vm_state": "active",
        "OS-EXT-SRV-ATTR:instance_name": "instance-00000006",
        "OS-SRV-USG:launched_at": "2014-09-02T19:13:39.000000",
        "OS-EXT-SRV-ATTR:hypervisor_hostname": "testbed-klee-20140829-1057112.platform9.sys",
        "flavor": {
            "id": "2",
            "links": [
                {
                    "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/flavors/2",
                    "rel": "bookmark"
                }
            ]
        },
        "id": "a807443a-c9e3-45d4-8b71-149470773286",
        "security_groups": [
            {
                "name": "default"
            }
        ],
        "OS-SRV-USG:terminated_at": null,
        "OS-EXT-AZ:availability_zone": "nova",
        "user_id": "7f5230ee5c5b4a8c92d8c9906138ca04",
        "name": "test-instance",
        "created": "2014-09-02T19:12:22Z",
        "tenant_id": "1067aba5194246a5a61fbdb31708404b",
        "OS-DCF:diskConfig": "MANUAL",
        "os-extended-volumes:volumes_attached": [],
        "accessIPv4": "",
        "accessIPv6": "",
        "progress": 0,
        "OS-EXT-STS:power_state": 1,
        "config_drive": "",
        "metadata": {}
    }
}
```

Gets details for a specified instance by unique ID.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.
server\_id | URI | string | The unique ID of the instance.

### Response parameters

Refer to response parameters [here](#server-details).

This operation does not require a request body.

**Normal Response Codes**: 200, 203

**Error Response Codes**: computeFault (400, 500, …), serviceUnavailable (503), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), itemNotFound (404)

## DELETE /v2/​{tenant\_id}​/servers/​{server\_id}​

Deletes a specified instance by unique ID.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.
server\_id | URI | string | The unique ID of the instance.

This operation does not require a request body and does not return a response body.

**Normal Response Codes**: 204

**Error Response Codes**: computeFault (400, 500, …), serviceUnavailable (503), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), itemNotFound (404), buildInProgress (409)

## Server Actions

Performs actions for a specified instance, including starting, stopping, resuming, suspending, and rebooting a instance, as well as creating a instance image.

## <a id="create-image"></a>POST /v2/​{tenant\_id}​/servers/{server\_id}/action

### Start a instance

> JSON Request

```json
{
    "os-start": null
}
```

Returns a **STOPPED** instance to **ACTIVE**. Specify the `os-start` action in the request body.

### Stop a instance

```json
{
    "os-stop": null
}
```

Stops a running instance. Changes status to **STOPPED**. Specify the `os-stop` action in the request body.

### Resume a instance

```json
{
    "resume" : null
}
```

Resumes a suspended instance. Changes status from **SUSPENDED** to **ACTIVE**. Specify the `resume` action in the request body.

### Suspend a instance

```json
{
    "suspend" : null
}
```

Suspends a instance. Specify the `suspend` action in the request body.

### Reboot a instance

```json
{
    "reboot" : {
        "type" : "SOFT"
    }
}
```

Reboots the specified instance. Specify the `reboot` action in the request body.

### Create an image

```json
{
    "createImage" : {
        "name" : "test-image",
        "metadata": {
            "pf9_description": "description of image"
        }
    }
}
```

Creates a new image. Specify the `createImage` action in the request body.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.
server\_id | URI | string | The unique ID of the instance.
os-start | body | N/A | Starts the instance.
os-stop | body | N/A | Stops the instance.
resume | body | N/A | Resumes a suspended instance.
suspend | body | N/A | Suspends the instance.
reboot | body | object | Reboots the instance. Contains the property `type` which refers to the type of reboot. A **SOFT** reboot restarts the same instance, while a **HARD** reboot terminates the instance and starts a new instance.
createImage | body | object | Creates an image from an instance. The createImage specification contains the properties `name`, which refers to the name of the image, and `metadata`, with the `pf9_description`, which is a description of the image.

This operation does not return a response body.

**Normal Response Codes**: 202

**Error Response Codes**: computeFault (400, 500, …), serviceUnavailable (503), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), itemNotFound (404), badMediaType (415), buildInProgress (409)

## Flavors

Retrieve information about available flavors.

## GET /v2/​{tenant\_id}​/flavors/detail

```json
{
  "flavors": [
    {
      "name": "m1.tiny",
      "links": [
        {
          "href": "http://127.0.0.1:8774/v2/1067aba5194246a5a61fbdb31708404b/flavors/1",
          "rel": "self"
        },
        {
          "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/flavors/1",
          "rel": "bookmark"
        }
      ],
      "ram": 512,
      "OS-FLV-DISABLED:disabled": false,
      "vcpus": 1,
      "swap": "",
      "os-flavor-access:is_public": true,
      "rxtx_factor": 1.0,
      "OS-FLV-EXT-DATA:ephemeral": 0,
      "disk": 1,
      "id": "1"
    },
    {
      "name": "m1.small",
      "links": [
        {
          "href": "http://127.0.0.1:8774/v2/1067aba5194246a5a61fbdb31708404b/flavors/2",
          "rel": "self"
        },
        {
          "href": "http://127.0.0.1:8774/1067aba5194246a5a61fbdb31708404b/flavors/2",
          "rel": "bookmark"
        }
      ],
      "ram": 2048,
      "OS-FLV-DISABLED:disabled": false,
      "vcpus": 1,
      "swap": "",
      "os-flavor-access:is_public": true,
      "rxtx_factor": 1.0,
      "OS-FLV-EXT-DATA:ephemeral": 0,
      "disk": 20,
      "id": "2"
    },
    ...
  ]
}
```

Lists available flavors for a specified tenant.

**NOTE**: The Unknown flavor is used by Platform9 to associate with discovered VMs and should not be deleted. 

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
flavors | array | List of all available flavors.
name | string | The name of the flavor.
links | array | Contains information for the REST API link of the flavor.
href | string | The URL to get detailed information about the flavor.
ram | integer | Amount of RAM allocated in MB.
vcpus | integer | The number of virtual cpus on the flavor.
swap | integer | The swap space in MB.
os-flavor-access:is_public | boolean | If set to `true`, anybody can access this flavor. If set to `false`, only the owner can access the flavor.
OS-FLV-EXT-DATA:ephemeral | integer | The ephemeral disk space that the instance created from this flavor can use.
disk | integer | The configured disk space in GB.
id | integer | The flavor id.

This operation does not require a request body.

**Normal Response Codes**: 200

## Hypervisors

Retrieve information about hypervisors.

## GET /v2/​{tenant\_id}​/os-hypervisors/detail

```json
{
  "hypervisors": [
    {
      "OS-EXT-PF9-HYP-ATTR:host_id": "8fb7a370-c69b-4c6d-89ca-a90ca7de8901",
      "service": {
        "host": "8fb7a370-c69b-4c6d-89ca-a90ca7de8901",
        "id": 11
      },
      "vcpus_used": 2,
      "hypervisor_type": "QEMU",
      "local_gb_used": 0,
      "host_ip": "10.4.118.51",
      "hypervisor_hostname": "testbed-klee-20140829-1057112.platform9.sys",
      "memory_mb_used": 1024,
      "memory_mb": 3832,
      "current_workload": 0,
      "vcpus": 1,
      "cpu_info": '{"vendor": "AMD", "features": ["osvw", "3dnowprefetch", "misalignsse", "sse4a", "abm", "cr8legacy", "extapic", "3dnow", "3dnowext", "pdpe1gb", "fxsr_opt", "mmxext", "hypervisor", "popcnt", "x2apic", "vme"], "clock_rate": [{"cores": 1, "mhz": 2300}], "model": "Opteron_G2", "arch": "x86_64", "topology": {"cores": 1, "threads": 1, "sockets": 1}}',
      "running_vms": 2,
      "free_disk_gb": 17,
      "hypervisor_version": 12001,
      "disk_available_least": 13,
      "local_gb": 17,
      "free_ram_mb": 2808,
      "id": 1,
      "OS-EXT-PF9-HYP-ATTR:ip_info": [
        {
          "ip": "10.4.118.51",
          "if_name": "eth0"
        },
        {
          "ip": "192.168.122.1",
          "if_name": "virbr0"
        }
      ],
      "OS-EXT-PF9-HYP-RES": {
        "net_sent_mibps": "0",
        "disk_total_gb": "17",
        "net_recvd_pkts_k": "0",
        "mem_used_percent": "10.64",
        "net_recvd_mibps": "0",
        "cpu_util_percent": "4.3",
        "net_sent_pkts_k": "0",
        "disk_used_gb": "1"
      }
    },
    ...
  ]
}
```

Shows information for all hypervisors for a specified tenant.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The ID for the tenant or account in a multi-tenancy cloud.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
hypervisors | array | A list of hypervisors that belong to the specified tenant.
OS-EXT-PF9-HYP-ATTR:host_id | string | The Platform9 ID of the hypervisor.
vcpus_used | integer | The number of vcpus used for running instances.
hypervisor_type | string | The type of hypervisor, including `QEMU`, `DOCKER`, `VMWARE`, etc.
local_gb_used | integer | The disk used in GB for running instances.
host_ip | string | The IP address of the hypervisor.
hypervisor_hostname | string | The host name of the hypervisor.
memory_mb_used | integer | The memory in MB used.
memory_mb | integer | The total available memory in MB.
vcpus | integer | The number of virtual CPUs available.
cpu_info | string | String representation of the CPU.
running_vms | integer | The number of instances running on the hypervisor.
free_disk_gb | integer | The amount of free disk space available on the hypervisor.
hypervisor_version | integer | The version number of the hypervisor.
free_ram_mb | integer | The amount of RAM free on the hypervisor in MB.
OS-EXT-PF9-HYP-ATTR:ip_info | array | Contains information about network connections of the hypervisor. `ip` refers to the hypervisor's IP address on that network. `if_name` refers to the name of the interface.
OS-EXT-PF9-HYP-RES | object | Describes the resource utilization of the hypervisor. `net_sent_mibps` refers to the total data sent on the network. `disk_total_gb` refers to the disk space in GB. `net_recvd_pkts_k` refers to the total number of packets received in thousands. `mem_used_percent` refers to the percent memory utilization. `net_recvd_mibps` refers to the total data received on the network. `cpu_util_percent` refers to the percent CPU utilization. `net_sent_pkts_k` refers to the total number of packets sent in thousands. `disk_used_gb` refers to the disk space utilized in GB.

This operation does not require a request body.

**Normal Response Codes**: 200

## Networks

Retrieve information about networks.

## GET /v2/​{tenant\_id}​/os-networks

```json
{
  "networks": [
    {
      "range_end": null,
      "bridge": "br_isolated2",
      "vpn_public_port": null,
      "dhcp_start": null,
      "bridge_interface": null,
      "updated_at": null,
      "id": "4b903d1d-e6ad-4618-8652-cce4d3cf2f42",
      "cidr_v6": null,
      "deleted_at": null,
      "gateway": null,
      "rxtx_base": null,
      "label": "Network-br_isolated2",
      "priority": null,
      "project_id": "1067aba5194246a5a61fbdb31708404b",
      "netmask_v6": null,
      "range_start": null,
      "description": null,
      "deleted": 0,
      "vlan": null,
      "broadcast": null,
      "netmask": null,
      "injected": false,
      "cidr": null,
      "vpn_public_address": null,
      "multi_host": false,
      "dns2": null,
      "dns1": null,
      "host": null,
      "gateway_v6": null,
      "vpn_private_address": null,
      "created_at": "2014-08-29T18:04:22.000000"
    },
    ...
  ]
}
```

Lists all networks that are available to the tenant.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenant\_id | URI | string | The unique identifier of the tenant or account.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
networks | array | A list of network configurations that belong to the specified tenant.
bridge | string | The name of the Linux bridge device that this network attaches to.
id | string | The network's unique ID.
gateway | string | The gateway address.
project_id | string | The ID of the Keystone tenant or project that this network is associated with.
range_start | string | The first address of the IP pool used for IP address allocation. The pool always resides within the subnet defined by the cidr field.
range_end | string | The last address of the IP pool used for IP address allocation. If range_start and range_end are both defined (not `null`), then they specify the start and end of the IP pool within the subnet defined by the cidr field.
description | string | The network's description.
cidr | string | If `null`, the network is interpreted as a DHCP network. Else, instances provisioned with this network will have an IP address allocated from a pool of addresses. The cidr field defines the IPv4 subnet containing the pool. It is in CIDR format, i.e. IP_Address/Prefix_Length. For example: 192.168.100.0/24. If the range_start and range_end fields are null, then the pool will include all addresses in the specified CIDR subnet. 
dns1 | string | The address of the first DNS server.
dns2 | string | The address of the second DNS server.

This operation does not require a request body.

**Normal Response Codes**: 200