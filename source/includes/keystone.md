# Keystone

**Service Endpoint URI**: `https://<my-company-platform9-url>/keystone`

Platform9 OpenStack Identity (Keystone) provides a central directory of users mapped to the OpenStack services they can access. It acts as a common authentication system across the cloud operating system.
The Keystone API is used to create an authentication token for use of all other APIs.

Check the [Usage](#usage) section for usage details.

## <a id="keystone-auth"></a>POST /v2.0/tokens

> JSON Request

```json
{
    "auth":{
        "passwordCredentials": {
            "username":"pf9@platform9.net",
            "password":"mypassword"
        },
        "tenantName":"service"
    }
}
```

> JSON Response

```json
{
    "access": {
        "token": {
            "issued_at": "2014-08-30T00:16:46.393238",
            "expires": "2014-08-31T00:16:46Z",
            "id": "MIIIcgYJKoZIhvcNAQcCoI...JdK0nj6beKUG",
            "tenant": {
                "description": "platform9 customer tenant",
                "enabled": true,
                "id": "1067aba5194246a5a61fbdb31708404b",
                "name": "service"
            }
        },
        "serviceCatalog": [
            {
                "endpoints": [
                    {
                        "adminURL": "http://127.0.0.1:9292",
                        "region": "RegionOne",
                        "internalURL": "http://127.0.0.1:9292",
                        "id": "5259c711e8e543ae96ac689ed71d83ff",
                        "publicURL": "http://127.0.0.1:9292"
                    }
                ],
                "endpoints_links": [],
                "type": "image",
                "name": "glance"
            },
            {
                "endpoints": [
                    {
                        "adminURL": "http://127.0.0.1:35357/v2.0",
                        "region": "RegionOne",
                        "internalURL": "http://127.0.0.1:5000/v2.0",
                        "id": "54c22a9d963f49a5b499b14e8806b3ff",
                        "publicURL": "http://127.0.0.1:5000/v2.0"
                    }
                ],
                "endpoints_links": [],
                "type": "identity",
                "name": "keystone"
            }
        ],
        "user": {
            "username": "pf9@platform9.net",
            "roles_links": [],
            "id": "7f5230ee5c5b4a8c92d8c9906138ca04",
            "roles": [
                {
                    "name": "admin"
                }
            ],
            "name": "pf9@platform9.net"
        },
        "metadata": {
            "is_admin": 0,
            "roles": ["b946d7cf71ea4716a042a4bedeabb03c"]
        }
    }
}
```

Authenticates user and generates a token to be used as an authentication token in subsequent API calls for the user. The *X-Auth-Token id* used in API calls is described in bold below.

To authenticate, you must provide either a user ID and password or a token in the body of the request.

If the authentication token has expired, a 401 response code is returned.

If the token specified in the request has expired, this call returns a 404 response code.

The Identity Service treats expired tokens as invalid tokens.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
username (Optional) | body | string | The user name. If you do not provide a user name and password, you must provide a token.
password (Optional) | body | string | The password of the user.
tenantName (Optional) | body | string | The tenant name. Both the tenantId and tenantName are optional, but should not be specified together. If both attributes are specified, the instance responds with a 400 Bad Request.
tenantId (Optional) | body | string | The tenant ID. Both the tenantId and tenantName are optional, but should not be specified together. If both attributes are specified, the instance responds with a 400 Bad Request.
token (Optional) | body | string | A token. If you do not provide a token, you must provide a user name and password.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
access | object | Contains information about the token, service endpoints, and user.
token | object | Auth token details. Properties include `issued_at`, the time that the token was issued, `expires`, the time that the token expires, `id`, **your X-Auth-Token used in API calls**, and `tenant`, explained in the table entry below.
tenant | object | Contains information about the user's tenant. `description` refers to a description of the tenant. `enabled` refers to whether the tenant is enabled or disabled. `id` refers to the unique ID of the tenant. `name` refers to the name of the tenant.
serviceCatalog | array | Contains information on endpoints. Properties include `endpoints`, explained in the table entry below, `type`, the endpoint type, and `name`, the endpoint name.
endpoints | array | Contains details on each endpoint. Properties include `adminUrl`, the admin URL for the endpoint, `region`, the endpoint region, `internalUrl`, the internal URL for the endpoint, `id`, the unique ID of the endpoint, and `publicUrl`, the public URL for the endpoint.
user | object | Contains information about the user being authenticated. Properties include `username`, the username of the user, `id`, the unique ID of the user, `roles`, explained in the table entry below, and `name`, the name of the user.
roles | array | A list of roles that belong to the user. The property `name` refers to the name of the role.
metadata | object | A metadata object.
roles | array |  A list of role IDs that belong to the user.

**Normal Response Codes**: 200, 203

**Error Response Codes**: identityFault (400, 500, …), userDisabled (403), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404) 

# <a id="keystone-admin"></a>Keystone Admin

**Service Endpoint URI**: `https://<my-company-platform9-url>/keystone_admin`

The Keystone Admin API is used to manipulate and retrieve data regarding **users**, **tenants**, and **roles**.

Check the [Usage](#usage) section for usage details.

## Users

List, create, update, and delete users.

## GET /v2.0/users

```json
{
    "users": [
        {
            "name": "nova",
            "id": "3b0d9950332e4e289a82e20da717ed8f",
            "enabled": true,
            "email": "nova@localhost",
            "tenantId": "99f45dcf2da541388f479caa30b2644a"
        },
        {
            "name": "pf9@platform9.net",
            "id": "7f5230ee5c5b4a8c92d8c9906138ca04",
            "enabled": true,
            "email": "pf9@platform9.net",
            "tenantId": "1067aba5194246a5a61fbdb31708404b"
        },
        ...,
        {
            "name": "some-user@example.com",
            "id": "fd6c159ee11d416fb63ac1d58635f487",
            "enabled": true,
            "email": "some-user@example.com",
            "tenantId": "1067aba5194246a5a61fbdb31708404b"
        }
    ]
}
```

Gets detailed information about all users.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
users | array | List of registered users.
name | string | The name of the user.
id | string | The unique ID of the user.
enabled | boolean | Returns `true` if the user is enabled, returns `false` if the user is disabled.
email | string | The email of the user.
tenantId | string | The unique ID of the tenant that the user is assigned to.

This operation does not require a request body.

**Normal Response Codes**: 200, 203

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404)

## POST /v2.0/users

> JSON Request

```json
{
    "user": {
        "username": "another-user@example.com",
        "email": "another-user@example.com",
        "password": "password",
        "tenantId": "1067aba5194246a5a61fbdb31708404b"
    }
}
```

> JSON Response

```json
{
    "user": {
        "email": "another-user@example.com",
        "enabled": true,
        "id": "d60b83b9a8b94fd2aaa14e296e6a6699",
        "name": "another-user@example.com",
        "tenantId": "1067aba5194246a5a61fbdb31708404b"
    }
}
```

Creates a new user. Assigns a default role of _member_.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
user (Optional) | body | object | The new user to be created.
username | body | string | The name/username of the new user.
email | body | string | The email of the new user.
password | body | string | The password of the new user.
tenantId | body | string | The unique ID of the tenant that the user is assigned to.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
email | string | The email of the new user.
enabled | boolean | Returns `true` if the user is enabled, returns `false` if the user is disabled.
id | string | The unique ID of the new user.
name | string | The name of the new user.
tenantId | string | The unique ID of the tenant that the user is assigned to.

**Normal Response Codes**: 201

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404), badMediaType (415) 

## DELETE /v2.0/users/​{userId}​

Deletes a user by unique ID.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
userId | URI | string | The unique ID of the user.

This operation does not require a request body and does not return a response body.

**Normal Response Codes**: 204

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404) 

## PUT /v2.0/users/​{userId}​

> JSON Request

```json
{
    "user": {
        "email": "another-user2@example.com",
        "name": "another-user2@example.com"
    }
}
```

> JSON Response

```json
{
    "user": {
        "email": "another-user2@example.com",
        "enabled": true,
        "extra": {
            "email": "another-user2@example.com",
            "id": "d60b83b9a8b94fd2aaa14e296e6a6699",
            "name": "another-user2@example.com",
            "tenantId": "1067aba5194246a5a61fbdb31708404b"
        }
    }
}
```

Update a user's name and email address by unique ID.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
userId | URI | string | The unique ID of the user.
email | body | string | The user's updated email.
name | body | string | The user's updated name.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
user | object | Contains information about the user.
email | string | The email of the user.
enabled | boolean | Returns `true` if the user is enabled, returns `false` if the user is disabled.
extra | object | Contains information about the user. `email` refers to the email of the user. `id` refers to the unique ID of the user. `name` refers to the name of the user. `tenantId` refers to the unique ID of the tenant that the user is assigned to.

**Normal Response Codes**: 200

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), badMediaType (415), itemNotFound (404)

## Tenants

Get information about tenants, users, or roles by tenant.

## GET /v2.0/tenants

```json
{
    "tenants_links": [],
    "tenants": [
        {
            "description": "platform9 customer tenant",
            "enabled": true,
            "id": "1067aba5194246a5a61fbdb31708404b",
            "name": "service"
        },
        {
            "description": "Tenant for the openstack services",
            "enabled": true,
            "id": "99f45dcf2da541388f479caa30b2644a",
            "name": "services"
        },
        {
            "description": "admin tenant",
            "enabled": true,
            "id": "ad1ad85e8db2495793fec230408e12fc",
            "name": "admin"
        }
    ]
}
```

Lists tenants which the specified token has access to.

**NOTE**: This will also return some internal tenants and users which are used for internal purposes only.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
tenants | array | The list of tenants.
description | string | The description of the tenant.
enabled | boolean | Returns `true` if the tenant is enabled, returns `false` if the tenant is disabled.
id | string | The unique ID of the tenant.
name | string | The name of the tenant.

This operation does not require a request body.

**Normal Response Codes**: 200, 203

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404)

## GET /v2.0/tenants/​{tenantId}​/users

```json
{
   "users": [
      {
         "name": "pf9@platform9.net",
         "id": "7f5230ee5c5b4a8c92d8c9906138ca04",
         "enabled": true,
         "email": "pf9@platform9.net",
         "tenantId": "1067aba5194246a5a61fbdb31708404b"
      },
      ...
   ]
}
```

Lists all of the users for a specified tenant.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenantId | URI | string | The unique ID of the tenant.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
users | array | List of users for specified tenant.
name | string | The name of the user.
id | string | The unique ID of the user.
enabled | boolean | Returns `true` if the user is enabled, returns `false` if the user is disabled.
email | string | The email of the user.
tenantId | string | The unique ID of the tenant the user belongs to.

This operation does not require a request body.

**Normal Response Codes**: 200, 203

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404)

## GET /v2.0/tenants/​{tenantId}​/users/​{userId}​/roles

> Admin Role

```json
{
    "roles": [
        {
            "id": "b946d7cf71ea4716a042a4bedeabb03c",
            "name": "admin"
        }
    ]
}
```

> Self-service User Role

```
{
    "roles": [
        {
            "description": "Default role for project membership",
            "enabled": "True",
            "id": "9fe2ff9ee4384b1894a90878d3e92bab",
            "name": "_member_"
        }
    ]
}
```

Lists roles for a specified user on a specified tenant.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenantId | URI | string | The unique ID of the tenant.
userId | URI | string | The unique ID of the user.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
roles | array | List of roles that belong to the user.
description | string | A description of the role.
id | string | The unique ID of the role.
name | string | The role of the user.

This operation does not require a request body.

**Normal Response Codes**: 200, 203

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404)

## PUT /v2.0/tenants/​{tenantId}​/users/​{userId}​/roles/OS-KSADM/​{roleId}​

> Admin Role

```json
{
    "role": {
        "id": "b946d7cf71ea4716a042a4bedeabb03c",
        "name": "admin"
    }
}
```

> Self-service User Role

```
{
    "roles": [
        {
            "description": "Default role for project membership",
            "enabled": "True",
            "id": "9fe2ff9ee4384b1894a90878d3e92bab",
            "name": "_member_"
        }
    ]
}
```

Assigns the specified role by ID to a user for a tenant.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenantId | URI | string | The unique ID of the tenant.
userId | URI | string | The unique ID of the user.
roleId | URI | string | The unique ID of the role.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
role | object | Contains information about the role.
description | string | A description of the role.
id | string | The unique ID of the role.
name | string | The role of the user.

This operation does not require a request body.

**Normal Response Codes**: 201

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), badMediaType (415), itemNotFound (404) 

## DELETE /v2.0/tenants/​{tenantId}​/users/​{userId}​/roles/OS-KSADM/​{roleId}​

Removes the specified role for a user in the given tenant.

### Request parameters

Parameter | Style | Type | Description
--------- | ----- | ---- | -----------
tenantId | URI | string | The unique ID of the tenant.
userId | URI | string | The unique ID of the user.
roleId | URI | string | The unique ID of the role.

This operation does not require a request body and does not return a response body.

**Normal Response Codes**: 204

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), itemNotFound (404)

## Roles

Retrieve a list of roles.

## GET v2.0/OS-KSADM/roles

```json
{
    "roles": [
        {
            "enabled": "True",
            "description": "Default role for project membership",
            "name": "_member_",
            "id": "9fe2ff9ee4384b1894a90878d3e92bab"
        },
        {
            "id": "b946d7cf71ea4716a042a4bedeabb03c",
            "name": "admin"
        }
    ]
}
```

Get information about available roles.

### Response parameters

Parameter | Type | Description
--------- | ---- | -----------
roles | array | List of roles.
description | string | The description of the role.
name | string | The name of the role.
id | string | The unique ID of the role.

This operation does not require a request body.

**Normal Response Codes**: 200, 203

**Error Response Codes**: identityFault (400, 500, …), badRequest (400), unauthorized (401), forbidden (403), badMethod (405), overLimit (413), serviceUnavailable (503), badMediaType (415), itemNotFound (404)