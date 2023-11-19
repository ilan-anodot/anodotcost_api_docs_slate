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


## SSO

Anodot Cost enables Single Sign On using different providers.
To configure SSO, please refer to the [product documentation](https://cloudcost.anodot.com/hc/en-us) 

### Get SSO Client ID

> Request Example: Get client ID from email

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users/sso?username={{email}}'
```

**Summary:** Retrieve the client ID from the user's email

**Parameters**

| Name | Located In | Description | Required |
| ---- | -----------| ----------- | ---------|
| email | Query | The user email we want the client ID for | Yes |

**Response**

This request returns the client ID for an SSO domain based user
The request returns NULL when an SSO domain is not defined

