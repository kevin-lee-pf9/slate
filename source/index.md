---
title: Platform9 API Reference

language_tabs:
  - python

toc_footers:
  - <a href="http://www.platform9.com">Platform9 Systems Inc</a>

includes:
  - keystone
  - glance
  - nova
  - resmgr

search: true
---
# Overview

Platform9 is the simplest way for enterprises to implement a private cloud, with intelligent, self-service provisioning of workloads onto their computing infrastructure. Platform9's cloud service enables the use of existing and new virtualized servers to power private clouds in a minutes, while interoperating seamlessly with existing processes and management products.

Platform9 is based on OpenStack and is 100% compatible with core OpenStack APIs. This documentation describes Platform9's REST APIs, which include APIs for core OpenStack components that Platform9 supports such as Nova, Glance and Keystone, along with APIs for Platform9's proprietary components such as Resource Manager.  

<aside class="notice">
To use Platform9's REST API, you are **required** to get an authentication token, which must be passed into all API calls as an `X-Auth-Token` request header. All REST API calls are in JSON format. Check the [Usage](#usage) section for more information.
</aside>

## <a id="service-endpoint"></a> Terminology

* **Service Endpoint**: A Service Endpoint is a distinct web service exposed by Platform9. For eg, Nova, Glance, Keystone and Resource Manager are the distinct Service Endpoints. Each Service Endpoint has a distinct URL that follows the format: 

`https://<my-company-platform9-url>/<service-endpoint-name>`

Example: 'https://foo.platform9.net/keystone'

where:

&nbsp;&nbsp;&nbsp;&nbsp;&#9702; **my-company-platform9-url** is the URL corresponding to the Platform9 account created for your company. This URL is typically of the format https://{company-name}.platform9.net. Eg https://foo.platform9.net

&nbsp;&nbsp;&nbsp;&nbsp;&#9702; **service-endpoint-name** is the distinct name for the given service endpoint. For eg the service-endpoint-name for Keystone Service Endpoint is 'keystone'. The corresponding documentation sections for each Service Endpoint such as Nova, Glance, Keystone and Resource Manager will list the distinct Service Endpoint URL at the beginning.

A REST API request to a Service Endpoint always includes the version of the API being referenced, immediately after the Service Endpoint name. 

Example: 'https://foo.platform9.net/keystone/v2.0'

where '2.0' is the current supported version of keystone API.

* **Tenant**: Platform9 Private Cloud is designed to be shared by multiple users and teams of users. The concept of a 'Tenant' is designed so that Administrators can create a distinct Tenant per team of users that will be using the Platform9 Private Cloud. For eg, an Administrator might want to create distinct Tenants for 'Development' and 'QA' teams to definte different logical pools of resources that each of these teams will utilize. At any given point, a user logged into Platform9 is always operating in the context of a distinct Tenant. An Administrator can assign a given user to one or more Tenants. (NOTE: Platform9 currently supports a single tenant only. The ability to create multiple tenants and map users to them is coming soon.)
* **Role**: Role-based access controls what functionality a user has access to, within a given Tenant. Platform9 currently supports two roles.
    * **Administrator**: As an Administrator user, you have complete access to all functionality exposed by Platform9. You can add or remove servers to Platform9, configure networking, populate image catalog, create and destroy instances etc. 
    * **Self-Service User**: A Self-Service user does not have access to infrastructure. He cannot see or manipulate servers, storage or networking configuration. But he does have access to all instances running within a given tenant, as well as Images in the catalog that the tenant has access to. He can create a new instance from Images in the catalog, view all instances running within given tenant, perform lifecycle operations on them, or delete them. 
* **Flavor**: A flavor is a pre-defined instance configuration that determines the amount of CPU, Memory and Storage that will be allocated to an instance. Flavors allow the Administrators to standardize on one or more resource configurations for instances within their environment and ensure that all instances are created using one of these standard configurations only. Platform9 ships with a default set of useful flavors, but Administrators can create their own set of flavors that better suit their environments. 

## <a id="usage"></a>Usage

```python
# Copyright (c) Platform9 Systems. All rights reserved

# This python sample code demonstrates how to get the authentication-token from Platform9 Keystone Service by passing username and password credentials. 

import requests
import json

# Platform9 currently ships with a default tenant with name 'service'. Ability to create new tenants 
# is not supported yet but coming soon.

TENANT = 'service'

def login(username, password):

    data = {
        "auth": {
            "tenantName": TENANT,
            "passwordCredentials": {
                "username": username,
                "password": password
            }
        }
    }

    req = requests.post("https://my-company.platform9.net/keystone/v2.0/tokens",
                    json.dumps(data),
                    verify=False,
                    headers={'Content-Type': 'application/json'})

    if req.status_code != 200 :
        raise Exception("Platform9 login returned %d, body:%s" %
                (r.status_code, r.text))
    else :
        return req.json()

# This is the Platform9 username and password associated with your Platform9 account. 
username = <my-platform9-username>
password = <my-platform9-password>
auth = login(username, password)
auth_token = auth['access']['token']['id']

```

A typical Platform9 REST API call involves following two steps:

* All Platform9 REST API calls use JSON as content type. Set the content-type property to be "application/json" in the request header for all Platform9 REST API calls. 
* Get an authorization token from keystone by making a POST request to:

`https://<my-company-platform9-url>/keystone/v2.0/tokens`
 
with your credentials in the request body. (Please check the [documentation](#keystone-auth) for this endpoint to see the request format.)

<aside class="notice">
NOTE: The authentication tokens generated by Platform9 Keystone are valid for 24 hours today. Once the token expires, the corresponding request will return a 401 Unauthorized response code. At this point, you must regenerate a new authentication token and pass it to the API calls.  
</aside>
 
* Make your REST API call by setting the 'X-AuthToken' parameter in request header to the authentication token you just received in the step above. Each REST API call URL will follow the syntax:

`http://<service-endpoint-url>/<api-call-parameters>`

&nbsp;&nbsp;&nbsp;&nbsp;&#9702; **service-endpoint-url** is described as part of definition of Service Endpoint [here](#service-endpoint)

&nbsp;&nbsp;&nbsp;&nbsp;&#9702; **api-call-parameters** are defined per API request and are described below. 

For example, if you wanted to get a list of users on a specified tenant, you would make a GET request to: 

`https://my-company.platform9.net/keystone_admin/v2.0/tenants/{tenantId}/users`

replacing {tenantId} with the correct identifier of the Tenant you are operating in the context of.

## Example Create Instance Request

```python
import json
import requests

# Copyright (c) Platform9 Systems. All rights reserved.

# This python sample code demonstrates how to create a virtual machine instance using the Nova 
# Service, after logging into Platform9. 

# Use the login method from the previous sample
username = <my-platform9-username>
password = <my-platform9-password>
auth = login(username, password)
auth_token = auth['access']['token']['id']
tenant_id = auth['access']['token']['tenant']['id']

config = {'server': {
             # name of the instance being created
		     'name': 'test_provision',
		     # ID of the image from which the instancce is to be created. Acquired by querying 
		     # the Glance Service.
		     'imageRef': image_id,
		     # ID of the flavor to be used for instance creation. Acquired by querying 
		     # the Nova Service. 
		     'flavorRef': flavor_id
		 }}

resp = requests.post("https://my-company.platform9.net/nova/v2/%s/servers" % (tenant_id),
                     data=json.dumps(config),
                     verify=False,
                     headers={'X-Auth-Token': auth_token,
                              'Content-Type': 'application/json'})

if resp.status_code != 202:  # CREATED
        raise RuntimeError("instance creation returned unexpected response code %s"
                % resp.status_code)

instance_id = resp.json()['server']['id']

# Upon instance creation, user should wait for instance status to be 'ACTIVE' before proceeding with 
# any operations on the instance. 

```

Example request for [creating an instance](#create-instance) is shown on the right.