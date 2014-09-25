# Resource Manager

**Service Endpoint URI**: `https://<my-company-platform9-url>/resmgr`

The resource manager is source of truth for:

* Hosts allocated to Platform9.
* Their roles in the Platform9 system.
* (Optionally) which hosts are approved by user for specific role in Platform9.

Check the [Usage](#usage) section for usage details.

## GET /v1/roles

```json
[
    {
        "name": "roleA",
        "display_name": "Role A",
        "description": "Simple role A",
        "active_version": "3.0.9-1"
    },
    {
        "name": "roleB",
        "display_name": "Role B",
        "description": "Another role B",
        "active_version": "1.0.2-5"
    }
]
```

Returns a list of Platform9 roles available in the system.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
name | string | The name of the role.
display_name | string | The user-friendly name for the role.
description | string | A description of the role.
active_version | string | The version of the role.

## GET /v1/roles/{role_name}

```json
{
    "name": "roleA",
    "display_name": "Role A",
    "description": "Simple role A",
    "active_version": "3.0.9-1"
}
```

Returns a Platform9 role corresponding to `role_name`.

### Request Parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
role_name | URI | string | The name of the role.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
name | string | The name of the role.
display_name | string | The user-friendly name for the role.
description | string | A description of the role.
active_version | string | The version of the role.

## GET /v1/hosts

```json
[
    {
        "id": "rsc_1",
        "state": "active",
        "info": {
            "hostname": "leb-centos-1.platform9.sys",
            "os_family": "Linux",
            "arch": "x86_64",
            "os_info": "centos 6.4 Final",
            "responding": true,
            "last_response_time": "2014-08-29T18:04:42Z"
        },
        "roles": ["role_1", "role2"],
        "role_status": "ok"
    },
    {
        "id": "rsc_2",
        "state": "activating",
        "info": {
            "hostname": "leb-centos-1.platform9.sys",
            "os_family": "Linux",
            "arch": "x86_64",
            "os_info": "centos 6.4 Final",
            "responding": false,
            "last_response_time": "2014-08-29T18:04:42Z"

        },
        "roles": ["role_2", "role5"],
        "role_status": "ok"
    },
]
```

Returns a list of hosts in the Platform9 system with their assigned roles.

The `last_response_time` values are used when the host is not responding. Otherwise, this value may be null.

For unauthorized hosts, `responding`, `last_response_time`, and `role_status` are not exposed.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
id | string | The unique ID of the host.
state | string | The current state of the host. Can be `active` (host currently has a role assigned to it), `inactive` (host currently has no roles assigned to it), or `activating` (host is currently being assigned a role).
hostname | string | The name of the host.
os_family | string | The operating system family of the host.
arch | string | The architecture of the host.
os_info | string | Information about the operating system.
responding | boolean | If `true`, it indicates that the host is currently responding.
last_response_time | dateTime | The time of the last response by the host.
roles | array | A list of roles by name that belong to the host.
role_status | string | Can be `ok`, `converging`, `retrying`, or `failed`.

## GET /v1/hosts/{id}

```json
{
    "id": "rsc_1",
    "info": {
        "hostname": "leb-centos-1.platform9.sys",
        "os_family": "Linux",
        "arch": "x86_64",
        "os_info": "centos 6.4 Final",
        "responding": true,
        "last_response_time": "2014-08-29T18:04:42Z"

    },
    "state": "active",
    "roles": ["role4", "role3"],
    "role_status": "ok"
}
```

Returns details for a single host.

The `last_response_time` value is used when the host is not responding. Otherwise, this value may be null.

For unauthorized hosts, `responding`, `last_response_time`, and `role_status` are not exposed.

### Request Parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
id | URI | string | The unique ID of the host.

### Response Parameters

Parameter | Type | Description
--------- | ---- | -----------
id | string | The unique ID of the host.
state | string | The current state of the host. Can be `active` (host currently has a role assigned to it), `inactive` (host currently has no roles assigned to it), or `activating` (host is currently being assigned a role).
hostname | string | The name of the host.
os_family | string | The operating system family of the host.
arch | string | The architecture of the host.
os_info | string | Information about the operating system.
responding | boolean | If `true`, it indicates that the host is currently responding.
last_response_time | dateTime | The time of the last response by the host.
roles | array | A list of roles by name that belong to the host.
role_status | string | Can be `ok`, `converging`, `retrying`, or `failed`.