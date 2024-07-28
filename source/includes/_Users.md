## Users 

### Get List of Users 

**Summary:** retrieve users

> Request example: Getting list of users

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}'
```

> Response Example

```json
{
    "id": 606,
    "user_key": "c6d9e30e-cf36-4957-a41a-d08fbafa5093",
    "user_name": "yariv@anodot.com",
    "user_display_name": "yariv",
    "user_type": "1",
    "registration_time": "2020-04-27T20:58:36.000Z",
    "registration_status_id": null,
    "last_login_time": null,
    "first_name": "Yariv",
    "last_name": "Zur",
    "company_name": "anodot",
    "job_title": "",
    "pricing_plan_id": null,
    "trial_days": null,
    "payment_customer_id": null,
    "payment_billing_url": null,
    "slack_webhook_url": null,
    "pricing_amount_monthly": 400,
    "pricing_amount_yearly": 3840,
    "is_active": 1,
    "is_read_only": 0,
    "db_creation_time": "2020-09-22T19:41:31.000Z",
    "db_update_time": null,
    "remaining_trial_days": null,
    "userHash": "{{user-hash}}",
    "role_id": "34o48a5a",
    "accounts": "TBD",
    "root_user": true,
    "parent_level": 0,
    "is_parent": true,
    "settings": {
        "default_account_id": null
    },
    "childs": "TBD",
}
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

The response is comprised of an array of users, according to the following table:

**Response Codes**

| Code | Description |
| ---- | ----------- |
| 200 | User retrieval success |
| 404 | User not found |
| 500 | Server error |

### Get Users and Roles

**Summary:** retrieve users and their respective roles

> Request example: 

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users/with-roles' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}'
```

> Response Example

```json
{
    "accountKey": 11111,
    "creationDate": "2019-08-31T21:00:00.000Z",
    "email": "user@test1.com",
    "userKey": "11111111-23ab-479e-a4ee-b2ceccddef79",
    "role": {
        "roleType": "customerRole",
        "accounts": [
            {
                "linkedAccountIds": [
                    "1234567890"
                    ],
                "accountId": "123456",
                "label": "123456"
            }
        ]
    }
}
```

## Onboarding

This section lists calls that are related to automated onboarding.

### Get external ID

> Request example: Get external ID

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users/on-boarding/external-id/:cloudAccountId' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}'
```

**Summary:** Retrieve external ID

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| cloudAccoundID | Query params | The account we want the external ID for | Yes | String |


### Onboard AWS Account

> Request example: Onboard AWS account by an enterprise

```shell
curl --location '{DOMAIN}/api/v2/onboarding/aws/{ACCOUNT_ID}' \
--header 'apikey: {API_KEY}' \
--header 'authorization: {AUTH_TOKEN}' \
--header 'Content-Type: application/json' \
--data '{
    "accountName": "{ACCOUNT_NAME}",
    "bucketName":"custom-bucket10", 
    "bucketRegion": "us-east-1"
}'
```

> Request example: Onboard an AWS account for an MSP

```shell
curl --location '{DOMAIN}/api/v2/onboarding/aws/{ACCOUNT_ID}' \
--header 'apikey: {API_KEY}' \
--header 'authorization: {AUTH_TOKEN}' \
--header 'Content-Type: application/json' \
--data '{
    "accountName": "{ACCOUNT_NAME}",
    "bucketName":"custom-bucket10", 
    "bucketRegion": "us-east-1", 
    "resellerCustomer": "test", 
    "resellerCustomerDomain": "test.com", 
    "autoAssignLinkedAccounts": 1, 
    "accountType": "dedicated", 
    "isCustomerSelfManaged": 1, 
    "excludedLinkedAccountMatch": "SP*SP*"
}'
```

Use this  to onboard an AWS account in Anodot Cost.</br>
As an enterprise customer, provide the account id and name.</br>
The MSP request contains slightly more information.<br>


**Parameters**

| Name | Located In | Description | Required |
| ---- | -----------| ----------- | ---------|
| account_id | Query | The payer account ID to onboard to Anodot | Yes |
| bucketName | body | The bucket name to be created. Default is "cur-{account_ID}", provide your own name if you do not want to create it with the default name | optional |
| bucketRegion | body | Region for bucket creation. Default is "us-east-1" | optional |

**Parameters for MSPs**

| Name | Located In | Description | Required |
| ---- | -----------| ----------- | ---------|
| accountType | body | Values: "dedicated" (default) or "shared" | optional |
| resellerCustomer | body |  Display name for the reseller customer. In case accountType = dedicated | optional |
| isCustomerSelfManaged | body | Relevant for accountType = dedicated, Values: 1 - Self managed, 0 - not self managed | optional |
| resellerCustomerDomain | body | Domain for reseller customer. Will be used for SSO. The domain should match existing domains for the customer, or be a new domain in Anodot's records | optional |
| autoAssignLinkedAccounts | body |  Relevant for accountType = dedicated. Values: 1 - auto assign, 0 - do not auto assign | optional |
| excludedLinkedAccountMatch | body |  Exclude all linked accounts that matches the string, example "SP*", relevant in case autoAssignLinkedAccounts is "1" | optional | 

**Response**

This API request will create a script you need to run in AWS.
Instructions to run this script [appear here](https://cloudcost.anodot.com/hc/en-us/articles/10273724354076-AWS-Automatic-and-Manual-Onboarding#h_01J1WG131M7GSAKXWM0S7ER079)

## SSO

Anodot Cost enables Single Sign On using different providers.
To configure SSO, please refer to the [product documentation](https://cloudcost.anodot.com/hc/en-us) 

### Get SSO Client ID

> Request Example: Get client ID from email (note we're using a POST request for this)


```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/users/sso' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": {{email}}
}'
```


**Summary:** Retrieve the client ID from the user's email

**Parameters**

| Name | Located In | Description | Required |
| ---- | -----------| ----------- | ---------|
| username | Body | The user email we want the client ID for | Yes |

**Response**

This request returns the client ID for an SSO domain based user
The request returns NULL when an SSO domain is not defined

## Roles

> Endpoint **https://api.mypileus.io/api/v2/roles**


The Roles endpoint enables Role CRUD actions as well updating user roles.

### Get Roles

> Request example: Get Roles

```shell
curl --location --request GET 'https://api.mypileus.io/api/v2/roles' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}'
```

> Response example:

```json
[{
        "id": "test",
        "name": "test role",
        "creationTime": "2024-07-04",
        "accounts": [
            {
                "linkedAccountIds": [
                    "1234567"
                    ],
                "accountId": "213"
            },
           
        ]
    }
]
```

**Summary:** Retrieve all roles

**Description**: Retrieves all the roles available in the account

**Parameters:**

| Name | Located in | Required | 
| ---- | ---------- | -------- | 
| Authorization | header | Yes | 
| apikey | header | Yes |


### Create Role

> Request example:

```shell
curl --location --request POST 'https://api.mypileus.io/api/v2/roles' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data '{
	"name": "Role Name",
    "type": "EDITOR",
    "accounts": [{ "accountId": "123", "linkedAccountIds": "all"}]
}'
```

> Response example:

```json
{
	"id": "1234",
    "name": "Role Name",
    "type": "EDITOR",
    "accounts": [{ "accountId": "123", "linkedAccountIds": "all"}]
}
```

**Summary:** Create a Role

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| name | body | The role's name | Yes | String |
| type | body | The role's type, Values: EDITOR /  VIEWER | Yes | String |
| accounts | body | Payer and linked accounts accessible for the role | Yes | Array of Strings |


**Response Fields:** The role's fields and the ID assigned by Anodot Cost.

### Update Role

> Request example:

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v2/roles/{RoleID}' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data '{
	"name": "Role Name",
    "type": "Viewer",
    "accounts": [{ "accountId": "123", "linkedAccountIds": "all"}]
}'
```

> Response example:

```json
{
	"id": "1234",
    "name": "Role Name",
    "type": "Viewer",
    "accounts": [{ "accountId": "123", "linkedAccountIds": "all"}]
}
```

**Summary:** Update role by ID

**Desription:** Refer to a role by its ID and change relevant fields.

The request uses the roleID in the header (see the example) and is otherwise identical to the Create role request.

### Create user in Role

> Request example:

```shell
curl --location --request POST 'https://api.mypileus.io/api/v2/roles/{RoleID}/user' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "test@test.com"
}'
```

**Summary:** Add a user to role by roleID and user email

**Description:** Assign a user to a role

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| roleID | header | The role's ID | Yes | String |
| email | body | The user's email | Yes | String |

### Change user Role

> Request example:

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v2/roles/{RoleID}/user' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data '{
    "email": "test@test.com"
}'
```

**Summary:** Change user role by roleID

**Description:** Change a user's assigned role

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| roleID | header | The role's ID | Yes | String |
| email | body | The user's email | Yes | String |


### Delete Role

> Request example:

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v2/roles/{RoleID}' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data ''
```

**Summary:** Delete role by roleID

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| roleID | header | The role's ID | Yes | String |

