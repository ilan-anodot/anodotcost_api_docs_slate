## Users and Groups

> End Point **/api/v2/users

The below end-points are meant to help you better understand how users in your account are using Anodot. This section will be growing in the near future. Stay tuned!

Authentication type: [Access Token Authentication] (#access-tokens).

### GET Users

> Request Example: Getting all users in the account  

```shell
curl -X GET 'https://app.anodot.com/api/v2/users' \
-H 'Authorization: Bearer {{data-token}}' \
-H 'Content-Type: application/json'
```

Get the list of all users in the account.
This request has no body

> Response Example:

```json
[
    {
        "_id": "608e9f23980ab6000ed6f696",
        "createdAt": "2021-05-02T12:46:27.823Z",
        "firstName": "Wiley",
        "lastName": "Coyote",
        "email": "wiley.coyote@acme.com",
        "roles": [
            "customer-user"
        ],
        "disabled": false,
        "ownerOrganization": "5ffc5ca1c4b329000e9d1063",
        "groups": [],
        "defaultGroup": ""
    },
    {
        "_id": "5ffc5ca3c4b329000e9d1065",
        "createdAt": "2021-01-11T14:11:47.052Z",
        "firstName": "Roger",
        "lastName": "Rabbit",
        "email": "roger.rabbit@acme.com",
        "roles": [
            "customer-admin"
        ],
        "disabled": false,
        "ownerOrganization": "5ffc5ca1c4b329000e9d1063",
        "groups": [
            "5ffc6662c4b329000e9d1066"
        ],
        "defaultGroup": "5ffc6662c4b329000e9d1066",
        "lastActive": "2021-04-05T11:23:15.476Z"
    }
]
```

#### Response Fields

The response is an array of "user" items. Each one of them has the following fields:

Field | Type | Description / Example
-|-|-
id | String ($uuid) | The unique user id
createAt | timestamp | Time when the user was created.
firtName | String | First name of the user
lastName | String | Last name of the user
email | String | email of the user
roles | enum | user role. Could be one of the following: {'customer-user', 'customer-admin'}
disabled | boolean | false if the user is active, true if the user is disabled. 
ownerOrganization | String ($uuid) | This is the customerID - this will be identical for all the users in your account. 
groups | array | List of groupIDs to which the user belongs (or empty if that user does not belong to any group).
defaultGroup | String ($uuid) | groupID of the default group to which the user belongs to.
lastActive | timestamp | Time when the user was last active. Will not be sent of the user was never active. 

> End Point **/api/v2/groups

The below end-points are meant to help you better manage groups in your Anodot account.

Authentication type: [Access Token Authentication] (#access-tokens).

### GET Groups

> Request Example: Getting list of all groups in the account

```shell
curl -X GET 'https://app.anodot.com/api/v2/groups' \
-H 'Authorization: Bearer {{data-token}}' \
-H 'Content-Type: application/json'
```

Get the list of all groups in the account.
This request has no body

> Response Example:

```json
[
    {
        "id": "6107947dac294a324f042789",
        "name": "Test Gr1",
        "created": "2021-08-02T06:45:17.817Z",
        "modified": "2021-09-09T12:39:47.177Z"
    },
    {
        "id": "610f6b0696d0a2478bef06f6",
        "name": "Test Gr2 - rename",
        "created": "2021-08-08T05:26:30.137Z",
        "modified": "2021-10-12T07:39:46.058Z"
    }
]
```

#### Response Fields

The response is an array of "group" items. Each one of them has the following fields:

Field | Type | Description / Example
-|-|-
id | String ($uuid) | The unique group id
name | String | Group name
created | timestamp | Time when the group was created.
modified | timestamp | Time when the group was modified.

