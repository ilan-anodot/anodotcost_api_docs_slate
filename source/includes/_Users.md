## Users and Groups

The below end-points are meant to help you better understand how users in your account are using Anodot. This section will be growing in the near future. Stay tuned!

Authentication type: [Access Token Authentication] (#access-tokens).

### Get list of Users

> Request Example: Getting all users in the account  

```shell
curl -X GET 'https://app.anodot.com/api/v2/users' \
-H 'Authorization: Bearer {{bearer-token}}' \
-H 'Content-Type: application/json'
```

Get the list of all users in the account.
This request has no body

**Response Fields**

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

### Create User

> Request Example: Create a new user in the account  

```shell
curl --location --request POST 'https://app.anodot.com/api/v2/users' \
--header 'Authorization: Bearer {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "emails": [
        "wiley.coyote@acme.com"
    ],
    "groups": [
            "62b8622e3f062e000f8be297"
        ],
    "defaultGroup": "62b8622e3f062e000f8be297",
    "role": "customer-user",
    "personalMsg": "Welcome to Anodot"
}'
```

Create a user in the account

**Request Fields**

Field | Type | Description / Example
-|-|-
emails | String | User email
groups | array | List of groupIDs to which the user belongs (or empty if that user does not belong to any group).
defaultGroup | String ($uuid) | groupID of the default group to which the user belongs to.
role | enum | which role should the user be assigned. Available options are **"customer-user"** and **"customer-read-only"**. We do not recommend creating admin users using API so this is blocked. 
personalMsg | String | Welcome message the user will get in their registration email. 

**Response Fields**

> Response Example: invalid email

```json
{
    "users": null,
    "groups": null,
    "validationResult": {
        "passed": false,
        "failures": [
            {
                "email": "wiley.coyote@acmecom",
                "message": "Some mail addresses are not valid."
            }
        ]
    }
}
```

The response is a "user" object. See the [Get User](#get-users) call for details. 

### Get List of Groups

The below end-points are meant to help you better manage groups in your Anodot account.
Authentication type: [Access Token Authentication] (#access-tokens).



> Request Example: Getting list of all groups in the account

```shell
curl -X GET 'https://app.anodot.com/api/v2/groups' \
-H 'Authorization: Bearer {{bearer-token}}' \
-H 'Content-Type: application/json'
```

Get the list of all groups in the account.
This request has no body

**Response Fields**

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

The response is an array of "group" items. Each one of them has the following fields:

Field | Type | Description / Example
-|-|-
id | String ($uuid) | The unique group id
name | String | Group name
created | timestamp | Time when the group was created.
modified | timestamp | Time when the group was modified.

### Create Group

> Request Example: Creating a new group in the account

```shell
curl -X POST 'https://app.anodot.com/api/v2/groups' \
-H 'Authorization: Bearer {{data-token}}' \
-H 'Content-Type: application/json'
--data-raw '{
    "name": "AnodotOps"
}'
```

Create a group in the account. 

**Request Parameters** 
Field | Type | Description / Example
-|-|-
name | String | Group name


**Response Fields**

> Response Example:

```json
{
    "colorSchema": "gray",
    "users": [],
    "_id": "62e8e76d6e2440000f38f117",
    "ownerOrganization": "5fc35adc94bd29000ef14b3d",
    "createdBy": "60f943f57fb4d5000f919610",
    "name": "AnodotOps",
    "created": "2022-08-02T08:59:25.737Z",
    "modified": "2022-08-02T08:59:25.737Z",
    "__v": 0
}
```

The response is an a group object with all the parameters. 

Field | Type | Description / Example
-|-|-
colorSchema | enum | Color which is assigned to the group. Default is "gray".
users | array | Array of users assigned to the group. Since this group is new, it should be empty.
_id | String ($uuid) | The unique group id
ownerOrganization | String ($uuid) | ID of the account. 
name | String | Group name
created | timestamp | Time when the group was created.
modified | timestamp | Time when the group was modified.

