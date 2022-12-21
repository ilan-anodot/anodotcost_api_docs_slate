
> End Point **https://api.mypileus.io/api/v1**

## Cost Authentication
In order to start working with the Cost API, you need to perform two authentication steps. 

### Getting the Bearer Token

The following call will enable you to get an authentication token for the subsequent API calls. Please notice that the token recieved is valid for 24 hours.

You can either use basic authentication (recommended) or send the parameters in the body.

> Request example: Getting an authentication token

```shell
curl --location --request POST 'https://tokenizer.mypileus.io/prod/credentials' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "coyote@acme.com",
    "password": "thisismypassword"
}'
```

> Response example - invalid credentials.

```json
{
    "Error": "Failed Retrieving Authentication Token"
}
```


> Response example - token

```json
{
   "Authorization": "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",  
 	"apikey": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX:-1" 
}
```

### Forming the API Key

After calling the Authentication, you will need to call the [Get Users](#users).
From the users API response you will get an array of accounts (See example on the right)

> Sample reponse from the users API

```json
[
  {
    "accounts": [
        {
            "accountKey":"{{accountkey}}",
            "accountId": "1234567",
            "accountTypeId": 1,
            "accountName": "anodot",
            "cloudTypeId": 0,
            "userKey": "{{user-key}}",
            "firstProcessTime": "2020-09-22T12:00:00.000Z",
            "lastProcessTime": "2022-06-30T12:00:00.000Z",
            "divisionId": "{{divisionId}}",
            "divisionsIds": [
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0,
                0
            ]
        }
  }
]
```

Choose the **accountkey** which is relevant for the subsequent API calls and the **divisionID**. 

Then - replace the '-1' at the end of the API key you got with accountKey:divisionId

Example: if the API key you initially got is this:

*c6249e30e-cf36-4957-a41a-d08fbafa5093:-1*

It will now be:

*c6249e30e-cf36-4957-a41a-d08fbafa5093:{{acountKey}}:{{divisionId}}*

In the following calls, we refer to this new string as **{{account-api-key}}**

## Pagination

Today, Each API call is limited to returning **5000** rows. In case you need to get more results, you can use the pagination capability of the APIs


> Pagination Call Example: First Request

```json
{ 
  "data": "[item1, item2, … , item5000]",
   "nextToken": 2 
}
```


> Pagination Call Example: Calling with token=2

```json
{ 
  "data": "[item5001, item5002, … , item6000]",
   "nextToken": null 
}
```

In order to get the full date (in case **next_token** is not null), you need to make the same API request with **token=next_token_value** until **nexToken** returns with a null value.

The following APIs currently support pagination:

* [Cost and Usage](#get-cost-amp-usage)
* [Kubernetes Cost and Usage](#get-kubernetes-cost)

<aside class="warning">
Important - In order to use pagination, you need to use /v2 on the API calls instead of /v1.
</aside>

To use pagination the user needs to add an additional parameter called **token** to the query parameter. You do not need to attach **token** for the first call. 
However - you will see in the response of the relevant calls a field called **nextToken** (see example on the right)

## Users 

### Get List of Users 

**Summary:** retrieve users

> Request example: Getting list of users

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users' \
--header 'apikey: {{apikey}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |


**Responses**

The response is comprised of an array of users, according to the following table:

**TBD**

| Code | Description |
| ---- | ----------- |
| 200 | User retrieval success |
| 404 | User not found |
| 500 | Server error |

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

## Cost Centers 

### Get List of Cost Centers 

**Summary:** retrieve list of cost centers

> Request example: Getting list of cost centers

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users/cost-centers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

> Response Example

```json
[
    {
        "id": 1,
        "name": "New anodot Cost Center",
        "code": "",
        "accountId": "932213950603",
        "linkedAccounts": [
            {
                "linkedAccountId": "247699517797",
                "linkedAccountName": "anodot-linked-account-test"
            },
            {
                "linkedAccountId": "835083597758",
                "linkedAccountName": "tests"
            },
            {
                "linkedAccountId": "932213950603",
                "linkedAccountName": "Pileus"
            }
        ]
    },
    {
        "id": 2,
        "name": "newCostCenterEdited",
        "code": "388",
        "accountId": "932213950603",
        "linkedAccounts": [
            {
                "linkedAccountId": "750699721763",
                "linkedAccountName": "Test"
            },
            {
                "linkedAccountId": "837447010640",
                "linkedAccountName": "reservation"
            }
        ]
    }
]
```

**Responses**

The response is comprised of an array of cost centers, according to the following table:

| Name | Type | Description | 
| ---- | ---- | ----------- | 
| id | ID | ID of the cost center |
| name | String | Cost Center Name | 
| code | String | Cost Center Code (optional) |
| accountId | ID | ID of the Anodot Cost Account | 
| linkedAccounts | Array | All the accounts linked to this cost center |

**Return Codes**

| Code | Description |
| ---- | ----------- |
| 200 | User retrieval success |
| 404 | User not found |
| 500 | Server error |

### Create Cost Center

**Summary:** Create a new cost center

> Request example: Create cost center

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/users/cost-centers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--data-raw '{
    "name": "SEs",
    "linkedAccountsIds": ["552822412346"],
    "code": "code blue"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| name | body | Cost center name | yes | String |
| code | body | Cost center code | no | String |
| linkedAccountIds | body | All the linked accounts which should be attached to the cost center | Yes | Array |


**Responses**

The response is comprised of an array of cost centers, according to the following table:

**TBD**

| Code | Description |
| ---- | ----------- |
| 200 | Cost center creation success |
| 404 | Error in parameters |
| 500 | Server error |

> Response Example

**TBD**


### Update Cost Center

**Summary:** Update an existing cost center

> Request example: Update cost center

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/users/cost-centers/{{costCenterId}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--data-raw '{
    "name": "SEs",
    "linkedAccountsIds": ["552822412346"],
    "code": "code blue"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| costCenterID | header | ID of the cost center to update | Yes | String |
| name | body | Cost center name | yes | String |
| code | body | Cost center code | no | String |
| linkedAccountIds | body | All the linked accounts which should be attached to the cost center | Yes | Array |

**Responses**

The response is comprised of an array of cost centers, according to the following table:

**TBD**

| Code | Description |
| ---- | ----------- |
| 200 | Cost center creation success |
| 404 | Error in parameters |
| 500 | Server error |

> Response Example

**TBD**

### Delete Cost Center

**Summary:** Delete a cost center

> Request example: Delete a cost center

```shell
curl --location --request DELETE 'api.mypileus.io/api/v1/users/cost-centers/{{costCenterId}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| costCenterID | header | Cost center ID | yes | String |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Cost center Deletion successull |
| 404 | Error in parameters |
| 500 | Server error |


## Divisions
### Get Divisions 

<aside class="warning">
The Get Divisions API has been depcreated. Please use the Get Cost Centers API instead.
</aside>

**Summary:** retrieve divisions

> Request Example: Get Divisions

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/divisions' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

> Response Example: TBD


**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Divisions retrival success |
| 404 | Divisions not found |
| 500 | Server error |

## Cost & Usage
### Get Cost & Usage 

**Summary:** Retrieves cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve cloud invoice data grouped and filters by different values

> Request Example - Getting cost and usage

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/invoices/cost-and-usage?groupby=service&startDate=2021-12-31&endDate=2022-07-01&periodGranLevel=month&costType=discount' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| groupBy | query | The group by value that will be used for the results | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | Yes |  |
| periodGranLevel | query | Granularity level of the period in the output data | Yes |  |
| costType | query | Type of cost to get the report for. Possible options are: cost, discount, refund, credit | Yes |  |
| isNetUnblended | query |  | No |  |
| isAmortized | query |  | No |  |
| isNetAmortized | query |  | No |  |
| wheres | query | conditions of type if x equals y when retrieving data | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data | No |  |

> Response Example 

```json
[
    {
        "group_by": "undefined",
        "usage_date": "2022-05",
        "account_id": "932213950603",
        "total_cost": 36564.952178,
        "total_usage_quantity": 1096079113.54,
        "resource_name": "",
        "resource_id": ""
    },
    {
        "group_by": "undefined",
        "usage_date": "2022-06",
        "account_id": "932213950603",
        "total_cost": 41454.924133,
        "total_usage_quantity": 1247146351.53,
        "resource_name": "",
        "resource_id": ""
    },
    {
        "group_by": "undefined",
        "usage_date": "2022-07",
        "account_id": "932213950603",
        "total_cost": 75231.838587,
        "total_usage_quantity": 2346800877.42,
        "resource_name": "",
        "resource_id": ""
    },
    {
        "group_by": "undefined",
        "usage_date": "2022-08",
        "account_id": "932213950603",
        "total_cost": 11307.24461,
        "total_usage_quantity": 141920221.13,
        "resource_name": "",
        "resource_id": ""
    }
]
```

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## Recommendations
### Get Recommendations

**Summary:** Retrieves current live recommendations

**Description:** The call is used to retrieve the current live recommendations

> Request Example: Getting recommendations

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations?filters[type]=ec2-idle' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | query | Filter recommendations based on type | No |  |

Filtering recommendations can be applied to the above calls using query parameters in the following manner: **filters[type]=recommendationType** where recommendationType is one of the recommendation types supported. You can use the *Get recommendation types* call to get the possible types.</br>
To get the recommendation types list please contact Anodot Support.


**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

> Response Example: Recommendation

```json
[
    {
        "max_cpu_util": "0.8",
        "region": "us-east-1",
        "operation_system": "Linux/UNIX",
        "resource_name": "mongodb-rs1.k8s-production.anodot.com",
        "total_cost_recommended": "0",
        "resource_tags": {
            "createdBy": "AssumedRole:AROAIFFBAISDNBXFCKHYE:tcohen@anodot.com",
            "Resource_Name": "mongodb-rs1.k8s-production.anodot.com",
            "stack": "production"
        },
        "initial_creation_date": "2022-06-12",
        "resource_id": "i-0fbec9ade54277c5f",
        "linked_account_name": "Production",
        "signature": "340481513670#|#340481513670#|#ec2-idle#|#i-0fbec9ade54277c5f",
        "total_cost_current": "389.86",
        "sp_coverage": "0.0",
        "instance_type_size": "2xlarge",
        "network_out": 1,
        "recommendation_creation_time": "2022-06-30 22:34:25",
        "max_network": 10,
        "ri_coverage": "100.0",
        "network_in_statistics_usage": {
            "2022-06-08": "0.22"
        },
        "sp_savings": false,
        "instance_type_model": "r5ad",
        "starting_time": "2022-06-30 22:34:04",
        "last_process_date": "2022-06-29",
        "num_of_days": 30,
        "action": "Terminate",
        "cpu_util_statistics_usage": {
            "2022-06-08": "0.6683333333333333"
        },
        "account_id": "340481513670",
        "ri_savings": true,
        "linked_account_id": "340481513670",
        "service": "Amazon Elastic Compute Cloud",
        "network_in": 1,
        "recommendation_update_time": "2022-06-30 22:34:25",
        "uuid": "92ab0dcc-5dee-4df2-b186-4b246293b3c8",
        "potential_savings": "100.0",
        "recommendation_status_id": 0,
        "network_out_statistics_usage": {
            "2022-06-08": "1.3"
        },
        "type": "ec2-idle"
    }
]
```

While each recommendation has different attributes that relat to the recommendation, they all share the following common fields:

| Name | Description | 
| ---- | ----------- | 
| account_id | The payer account id |
| linked_account_id | The linked account id that the recommendation is relevant to (can be the payer account id also) |
| linked_account_name | The linked account name that the recommendation is relevant to (can be the payer account name also) |
| type | The recommendation type (See Recommendations Types Below) |
| total_cost_current | The corresponding monthly cost without executing the recommendation |
| total_cost_recommended | The corresponding monthly cost after executing the recommendation |
| potential_savings | The monthly savings percent in case the recommendation is executed |
| action | The general type of action related to the recommendation - Modify/Terminate/Upgrade/Delete/Buy |
| recommendation_creation_time | The time the recommendation was created |
| uuid | The recommendation id |
| recommendation_status_id |The recommendation status (See Recommendations Statuses Below) |
| recommendation_update_time | The time the recommendation was updated with one of the statuses. |

**Recommendation Statuses**

Available Recommendation Statuses are: 

* Open Recommendation (Recommendation which no action have been made on it): Type ID: 0
* Completed Recommendation: Type ID: 1
* Excluded Recommendation (Recommendation which excluded action has been made on it): Type ID: 4

### Get Recommendations types

**Summary:** Retrieves current live recommendations types

**Description:** The call is used to retrieve the current live recommendations types

Example:

> Request Example: Getting recommendations types

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/types' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

> Response Example: Recommendation Types

```json
[
    "ebs-unattached",
    "ip-unattached",
    "ec2-idle",
    "dynamodb-idle",
    "version-upgrade",
    "rds-version-upgrade",
    "ec2-low-cpu-usage",
    "rds-ri",
    "ebs-upgrade",
    "elasticache-ri",
    "ec2-savings-plans",
    "nat-gateway-util-low",
    "ebs-outdated-snapshot",
    "rds-idle",
    "ec2-stopped-instance"
]
```
| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

**For a complete list of all recommendation types, please contact support@anodot.com**


### Get history of recommendations

**Summary:** Retrieves history of recommendations

**Description:** The call is used to retrieve history recommendations

> Request Example: Getting recommendations history

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/history?filters[type]=ec2-idle' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | query | Filter recommendations based on type | No |  |

Filters are the same as the live [Get Recommendations](#get-recommendations) call

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Get recommendation history types

**Summary:** Retrieves current history recommendations types

**Description:** The call is used to retrieve the history recommendations types

> Request Example: Getting recommendations history types

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/history/types' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |


## Budgets
### Get Budget 

**Summary:** Retrieves budgets for given user

**Description:** The call is used to retrieve budgets created by api-key defined user


> Request Example: Getting a budget


```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/budgets' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**
The response is an array of budgets defined in the account. 

> Response Example 

```json
[
    {
        "budgetId": "f30de497a71f45b7a5dd5e0953bc43e6",
        "budget_amounts": [],
        "end_date": "2023-06-01 00:00:00",
        "creation_time": "2022-07-03 11:48:33",
        "user_key": "ff55ffaf-42d7-402d-95c8-bdd6488392f3",
        "budget_amount_type": "fixed",
        "budget_type": "recurring",
        "flexible": 1,
        "alerts": [
            {
                "budget_percent_to_alert_from": 80,
                "alert_granularity": [
                    "daily"
                ],
                "recipients": [
                    "yariv@anodot.com"
                ],
                "when_to_alert": [
                    "forecasted"
                ]
            }
        ],
        "filters": {
            "include": {
                "linkedaccname": [
                    "Anodot Customer Success"
                ]
            },
            "exclude": {}
        },
        "account_id": "340481513670",
        "start_date": "2022-06-01 00:00:00",
        "division_id": "0",
        "budget_amount": 50000,
        "is_active": 1,
        "account_key": "649",
        "budget_name": "Customer Success",
        "is_valid": "1"
    }
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit Budget 

**Summary:** update budget for given user

**Description:** The call is used to update budget created by api-key defined user

> Request Example: Update a budget 

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/budgets?budgetId={{budgetId}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "budgetName": "string",
  "budgetAmount": 0,
  "budgetAmounts": [
    {
      "date": "string",
      "amount": 0
    }
  ],
  "budgetAlerts": [
    {
      "alertPercent": 0,
      "alertEmail": "string",
      "whenToAlert": [
        "actualUsage"
      ],
      "alertGranularity": [
        "daily"
      ]
    }
  ],
  "includeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "budgetType": "expiring",
  "budgetAmountType": "fixed",
  "startDate": "string",
  "endDate": "string",
  "isFlexible": true,
  "budgetId": "string"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Create Budget

**Summary:** create budget for given user

**Description:** The call is used to create budgets created by api-key defined user

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/budgets' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "budgetName": "string",
  "budgetAmount": 0,
  "budgetAmounts": [
    {
      "date": "string",
      "amount": 0
    }
  ],
  "budgetAlerts": [
    {
      "alertPercent": 0,
      "alertEmail": "string",
      "whenToAlert": [
        "actualUsage"
      ],
      "alertGranularity": [
        "daily"
      ]
    }
  ],
  "includeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "linkedaccid"
    ]
  },
  "budgetType": "expiring",
  "budgetAmountType": "fixed",
  "startDate": "string",
  "endDate": "string",
  "isFlexible": true,
  "budgetId": "string"
}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete Budget

**Summary:** Delette budget for given user

**Description:** The call is used to delete budget created by api-key defined user

> Request Example: Delete a budget

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/budgets?budgetId={{budget-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| budgetId | query | The budget Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## Commitments 
### Get Utilization 

**Summary:** Retrieves the utilization of an RI (Reserved Instance)

**Description:** The call is used to retrieve the utilization of RIs for a specific date.

Note: Currently this is only supported in Microsoft Azure accounts.

> Request Example: Getting a utilization


```shell
curl --location -g --request GET 'https://api.mypileus.io/api/v1/commitment/utilization/?date=2022-07-29&linkedAccount={{linked-account-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| date | query | Report date (YYYY-MM-DD) format | Yes | String |
| linkedAccount | query | Linked account ID only (No linked account name) | No | String |

**Responses**
The response is a utilization object showing the utilization for each RI. 

> Response Example 

```json
[
    {
        "minUtilizationPercentage": "0.0",
        "maxUtilizationPercentage": "100.0",
        "usageDate": "2022-08-01T00:00:00",
        "accountId": "ABC-123",
        "usedHours": "560.3",
        "reservedHours": "650.0",
        "linkedAccountName": "LINKED_ACC_NAME",
        "linkedAccountId": "LINKED_ACC_ID",
        "avgUtilizationPercentage": "86.2",
        "reservationOrderId": "asxc1234526fssz",
        "reservationId": "1234-5676-23243-12123",
        "skuName": "Test_acv2_sa"
    },
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## Business Mapping
### Get Business Mappings 

**Summary:** Retrieves the business mapping rules defined in the account. 

**Description:** This call is used to retrieve the business mapping rules defined in an account. 

> Request Example: Getting business mappings


```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**
The response is an array of business mapping rules according to the following structure:


> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    },
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Create Business Mapping 

**Summary:** Create a new business mapping rule 

**Description:** This call is used to create a business mapping rule in the account. 

> Request Example: Creating a business mapping rule


```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
--header 'Content-Type: application/json' \
--data-raw '
    {
        "name": "API",
        "rank": 38,
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            }
        ],
        "split": "true",
        "splitOptions": {
        "mappings": [
                "92b818e1-33ce-495e-a39d-11a5862e11b7",
                "a5052a4d-1529-4680-8e89-22f890adb849",
                "545c005f-a185-453d-a56b-a2d9101ca2fd",
                "2f3138dc-3f5b-41bd-8d52-f2d39dcb754e",
                "fd0bda77-b275-40ef-9b21-7cd17b10df10",
                "c5e03b1d-ba33-4530-966b-bc91a8edf219"
            ]
        }
    }
```

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| name | body | mapping name | Yes | string |
| rank | body | ranked order of this rule | Yes | Number |
| rules | body | Filters which define the mapping | Yes | Array of filter fields. See structure below |
| split | body | Determine whether the cost should be split | No | Boolean |
| splitOptions | body | Split type | No | One of "EQUAL", "RATIO" |
| mappings | body | Mapping ids to be split | No | Array of mapping IDs |

**Filter Structure**: Each filter has the following structure:

* "field": Name of filter field (e.g. service, customertags, etc.)
* "operator": one of IS, IS_NOT, STARTS_WITH, ENDS_WITH, LIKE, NOT_LIKE
* "values": should be an array when operator = IS, IS_NOT or a string otherwise
* "prefix": when field=customertags or k8slabels - this field should contain tag key


**Responses**
The response is the business mapping object created. 

> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    }
]
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Update Business Mapping 

> Request Example: Updating a business mapping rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings/{{businss-mapping-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
--header 'Content-Type: application/json' \
--data-raw '
    {
        "name": "API",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            }
        ],
        "split": "true",
        "splitOptions": {
        "mappings": [
                "92b818e1-33ce-495e-a39d-11a5862e11b7",
                "a5052a4d-1529-4680-8e89-22f890adb849",
                "545c005f-a185-453d-a56b-a2d9101ca2fd",
                "2f3138dc-3f5b-41bd-8d52-f2d39dcb754e",
                "fd0bda77-b275-40ef-9b21-7cd17b10df10",
                "c5e03b1d-ba33-4530-966b-bc91a8edf219"
            ]
        }
    }
```

**Summary:** Update a business mapping rule

**Description:** This call is used to update a business mapping rule in the account. 

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| business mapping id | header | ID of the business mapping to be updated | Yes | UUID |

Rest of the parameters are the same as the [Create Business Mapping call](#create-business-mapping).

> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    }
]
```

**Responses**
The response is the business mapping object created. 

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete Business Mapping 

**Summary:** Delete a business mapping rule

**Description:** This call is used to delete a business mapping rule in the account. 

> Request Example: Deleting  a business mapping rule


```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings/{{businss-mapping-id}}' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| business mapping id | header | ID of the business mapping to be deleted | Yes | UUID |


**Responses**
The response is the business mapping object created. 

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Update Business Mapping Order

> Request Example: Updating a business mapping order

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/usage/business-mapping/mappings/rank' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
--header 'Content-Type: application/json' \
--data-raw '--data-raw '[
    { 
        "uuid": "918029b6-0wellwwwf55-b8bf-6ce1f3983wek", 
        "rank": 1 
        },
    { 
        "uuid": "i39029b6-0161-4qwekq-b8bf-6ce1f3983sdk2", 
        "rank": 2 
        },
    { 
        "uuid": "019029b6-016lqoqoee55-b8bf-6ce1fqwemq3", 
        "rank": 3 
    }
]'
```

**Summary:** Update a business mapping order

**Description:** This call is used to update the order of the business mappings. 

**Parameters:**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| ranks | body | list of business mapping IDs with their new rank | Yes | Array

**Note:** You need to provide the full list of the mappings in the account for the ranking to work. 

> Response Example 

```json
[
    {
        "updateTime": "2022-08-23 07:56:36",
        "split": false,
        "creationTime": "2022-08-04 15:37:35",
        "dbUpdateTime": "2022-11-23 15:19:11",
        "createdBy": "user@pileuscloud.com",
        "userKey": "ae853bda-2f0b-479d-85cc-226e61381cdc",
        "accountId": "932213950603",
        "name": "Dev",
        "rank": 1,
        "splitOptions": null,
        "uuid": "10f3351f-0780-4fe2-ae82-6647dda43c3b",
        "divisionId": "0",
        "accountKey": "21",
        "rules": [
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "env",
                        "values": [
                            "dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "customtags",
                        "operator": "IS",
                        "prefix": "Env",
                        "values": [
                            "dev",
                            "Dev"
                        ]
                    }
                ]
            },
            {
                "filters": [
                    {
                        "field": "service",
                        "operator": "IS",
                        "values": [
                            "AWS Support [Business]",
                            "AWS Support [Developer]"
                        ]
                    }
                ]
            }
        ]
    }
]
```

**Responses**
The response is the business mapping object created. 

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## Anomaly Detection
### Get Anomaly Detection Rules

**Summary:** Retrieves anomaly-detection rules for given user

**Description:** The call is used to retrieve anomaly-detection created by api-key defined user

> Request example: Get anomaly detection rules

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules' \
--header 'apikey: {{account-api-token}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit Anomaly Detection Rules

**Summary:** update anomaly-detection rule for given user

**Description:** The call is used to update anomaly detection rule created by api-key defined user

> Request Example: Editing an Anomaly Detection Rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
--data-raw '{
  "ruleName": "string",
  "minPercentChange": 0,
  "minAmountChange": 0,
  "includeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "ruleId": "string"
}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful delete |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Create Anoamly Detection Rules

**Summary:** create anomaly-detection rule for given user

**Description:** The call is used to create anomaly detection rule created by api-key defined user

> Request Example: Creating an anomaly detection rule

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules' \
--header 'apikey: {{account-api-token}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "ruleName": "string",
  "minPercentChange": 0,
  "minAmountChange": 0,
  "includeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "excludeFilters": {
    "filter_name": [
      "string"
    ]
  },
  "ruleId": "string"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete Anomaly Detection Rules 

**Summary:** update anomaly detection rule for given user

**Description:** The call is used to delete anomaly detection rule created by api-key defined user

> Request Example: Delete an anomaly detection rule

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/usage/anomaly-detections/rules?ruleId=sdfsd' \
--header 'apikey: {{account-api-token}}' \
--header 'Authorization: {{Bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| ruleId | query | The anomaly detection rule Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## Kubernetes
### Get Kubernetes cost 

**Summary:** Retrieves kubernetes cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve kubernetes data grouped and filtered by different values

> Request Example: Get Kubernetes Cost

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/kubernetes/cost-and-usage?groupBy=cluster&startDate=2022-05-01&endDate=2022-08-01&periodGranLevel=month&costUsageType=compute' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| groupBy | query | The group by value that will be used for the results. Possible values: Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | Yes |  |
| periodGranLevel | query | Granularity level of the period in the output data. Available values : month, week, day | Yes |  |
| costUsageType | query | Available valuye: Available values : compute, dataTransfer, storage | Yes |  |
| usageType | query | Available values : CPU, Memory | No |  |
| metricType | query | Available values : actual, requirements | No |  |
| isNetUnblended | query | Available values : true, false | No |  |
| isAmortized | query | Available values : true, false | No |  |
| isNetAmortized | query | Available values : true, false | No |  |
| wheres | query | conditions of type if x equals y when retrieving data. Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data. Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data. Available values : cluster, region, linkedaccname, linkedaccid, namespace, node, instancetype, labels, customtags | No |  |

**Responses**

> Response example: Getting Kubernetes Cost

```json
{
    "data": [
        {
            "groupBy": "us-east-1:932213950603:cw-test-2",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:cw-test-2",
            "totalCost": 61.9,
            "totalUsage": 253883145259.66
        },
        {
            "groupBy": "us-east-1:932213950603:my-cluster-name",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:my-cluster-name",
            "totalCost": 7.74,
            "totalUsage": 31338272009.87
        }
    ],
    "nextPage": null
}
```

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Get Kubernetes cost per pod

**Summary:** Retrieves kubernetes cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve kubernetes Pod cost and usage data grouped

> Request Example: Getting Kubernetes cost per pod / cluster 

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/kubernetes/cost-and-usage/pod?startDate=2022-05-01&endDate=2022-08-01&periodGranLevel=month&clusterName=us-east-1:932213950603:cw-test-2' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| startDate | query | Start date of the cost and usage data examination | Yes |  |
| endDate | query | End date of the cost and usage data examination | No |  |
| clusterName | query | The cluster name that the pod running on. cluster name formatting should be in following format- [CLUSTER_REGION]:[CLUSTER_ACCOUNT_ID]:[CLUSTER_NAME] for example us-east-1:123456789012:prod-cluster | No |  |
| nodeName | query |  | No |  |
| podName | query | pod name formatting should be in following format- [POD_NAMESPACE]:[POD_NAME] for example kube-system:aws-node-k5jgh | Yes |  |

> Response Example: 

```json
{
    "data": [
        {
            "groupBy": "us-east-1:932213950603:cw-test-2",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:cw-test-2",
            "totalCost": 61.9,
            "totalUsage": 253883145259.66
        },
        {
            "groupBy": "us-east-1:932213950603:my-cluster-name",
            "usageDate": "2022-05",
            "accountId": "932213950603",
            "clusterName": "us-east-1:932213950603:my-cluster-name",
            "totalCost": 7.74,
            "totalUsage": 31338272009.87
        }
    ]
}
```

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

## Cost Events
### Get Cost Events 

**Summary:** Retrieves events for given user

**Description:** The call is used to retrieve events created by api-key defined user

> Request Example: Get Events

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/users/events?startDate=2022-01-01&endDate=2022-08-01' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| startDate | query | Start date of events | Yes | date |
| endDate | query | Start date of events | Yes | date |

**Responses**

> Response Example: Events

```json
[
    {
        "date": "2022-07-07",
        "creationTime": "2022-07-07 12:46:08",
        "createdBy": "PileusUser",
        "uuid": "eventId",
        "divisionId": "0",
        "accountKey": "649",
        "userKey": "userKey",
        "accountId": "accountID",
        "description": "Its time to get the API going!",
        "title": "The Big API Palooza"
    }
]
```


| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

The response is an array of events within the given time frame.

**Event Structure**

| Field | Description |
| ----- | ----------- |
| account_id | The payer account id |
| uuid | The event id |
| creation_time | Time and date when this event was created in format YYYY-MM-DD hh:mm:ss |
| date | Date of the event in format YYYY-MM-DD |
| description | The description of the event |
| title | The title of the event |
| user_key | Identifier of user, who created this event |
| updating_time | The last time and date when this event was updated in format YYYY-MM-DD hh:mm:ss |

### Create Cost Event 

**Summary:** create event for given user

**Description:** The call is used to create events created by api-key defined user

> Request Example: Create Event

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/users/events' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "title": "The Big API Palooza II",
  "description": "Its time to get the API going!",
  "date": "2022-07-07",
  "createdBy": "Anodot User"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| title | body | Event title | Yes | String |
| description | body | Event description | Yes | String |
| date | body | Event date | Yes | date |
| createdBy | body | Name of user creating the event | Yes | String |

**Responses**

> Response Example - Event Creation

```json
{
    "account_id": "{{acccountID}}",
    "uuid": "fd50e8f0-823a-456e-9df8-7e14f8df3fb7",
    "account_key": "{{accountKey}}",
    "division_id": "0",
    "user_key": "{{user-Key}}",
    "creation_time": "2022-07-07 13:00:50",
    "created_by": "Anodot User",
    "title": "The Big API Palooza II",
    "description": "Its time to get the API going!",
    "date": "2022-07-07"
}
```

| Code | Description |
| ---- | ----------- |
| 201 | successful Creation |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit Cost Event 

**Summary:** update event for given user

**Description:** The call is used to update budget created by api-key defined user

> Request Example - Edit an event

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/users/events/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
  "title": "The Big API Palooza III",
  "description": "Its time to get the API going!",
  "date": "2022-07-07",
  "createdBy": "Ragnar Lothbrok"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The event ID to update | Yes |  |

**Responses**

The response will contain the updated event in the regular structure.

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete Cost Event

**Summary:** delete event for given user

**Description:** The call is used to delete event created by api-key defined user

> Request Example: Delete event

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/users/events/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The event ID to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |

## MSP Customers
### Get MSP Customers

**Summary:** Retrieves list of customers for an MSP

**Description:** The call is used to retrieve list of customers for an MSP

> Request Example: Get MSP Customers

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/msp/customers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| startDate | query | Start date of events | Yes | date |
| endDate | query | Start date of events | Yes | date |

**Responses**

> Response Example: Events

```json
[
    {
        "date": "2022-07-07",
        "creationTime": "2022-07-07 12:46:08",
        "createdBy": "PileusUser",
        "uuid": "eventId",
        "divisionId": "0",
        "accountKey": "649",
        "userKey": "userKey",
        "accountId": "accountID",
        "description": "Its time to get the API going!",
        "title": "The Big API Palooza"
    }
]
```


| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

The response is an array of events within the given time frame.

**Event Structure**

| Field | Description |
| ----- | ----------- |
| account_id | The payer account id |
| uuid | The event id |
| creation_time | Time and date when this event was created in format YYYY-MM-DD hh:mm:ss |
| date | Date of the event in format YYYY-MM-DD |
| description | The description of the event |
| title | The title of the event |
| user_key | Identifier of user, who created this event |
| updating_time | The last time and date when this event was updated in format YYYY-MM-DD hh:mm:ss |

### Create MSP Customer

**Summary:** Create MSP customer

**Description:** The call is used to create a new customer for an MSP

> Request Example: Create MSP Customer

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/msp/customers' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "customerName",
    "linkedAccountsIds": [“19923818823”, “342134123”],
    "code": "customerCode"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| title | body | Event title | Yes | String |
| description | body | Event description | Yes | String |
| date | body | Event date | Yes | date |
| createdBy | body | Name of user creating the event | Yes | String |

**Responses**

> Response Example - Event Creation

```json
{
    "account_id": "{{acccountID}}",
    "uuid": "fd50e8f0-823a-456e-9df8-7e14f8df3fb7",
    "account_key": "{{accountKey}}",
    "division_id": "0",
    "user_key": "{{user-Key}}",
    "creation_time": "2022-07-07 13:00:50",
    "created_by": "Anodot User",
    "title": "The Big API Palooza II",
    "description": "Its time to get the API going!",
    "date": "2022-07-07"
}
```

| Code | Description |
| ---- | ----------- |
| 201 | successful Creation |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit MSP Customer

**Summary:** Update an MSP customer

**Description:** The call is used to update an MSP customer

> Request Example - Edit an MSP customer

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/msp/customers/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "name": "customerName",
    "linkedAccountsIds": [“19923818823”, “342134123”],
    "code": "customerCode"
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer ID to update | Yes |  |

**Responses**

The response will contain the updated customer in the regular structure.

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete MSP Customers

**Summary:** delete an MSP customer

**Description:** The call is used to delete an MSP customer

> Request Example: Delete MSP Customer

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/msp/customers/fd50e8f0-823a-456e-9df8-7e14f8df3fb7' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |

## MSP Billing Rules
### Get MSP Billing Rules

**Summary:** Retrieves list of MSP Billing Rules

**Description:** The call is used to retrieve list of billing rules for an MSP

> Request Example: Get MSP Billing Relues

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/msp/billing-rules' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

**Responses**

> Response Example: Get MSP Billing Rules

```json
[
   {
       "id": "2eee54d8",
       "name": "test1",
       "accountId": "932213950603",
       "creationDate": "2022-07-07",
       "frequency": "one time",
       "customers": "ALL",
       "effectiveMonth": "2022-06",
       "marginType": "custom-service-calc-cost",
       "margin": "20",
       "disableMinimumFee": false,
       "excludeAwsMarketplace": true,
       "customService": "Custom Service Name",
       "regions": [
           "af-south-1"
       ],
       "excludeService": true,
       "usageTypes": "ALL",
       "usageTypeOperator": "IS",
       "cloudFrontRegions": [],
       "services": [
           "Amazon Athena",
           "Amazon CloudFront",
           "Amazon Detective"
       ]
   }
]
```


| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

The response is an array of rules for the MSP.

**Event Structure**

| Field | Description |
| ----- | ----------- |
| account_id | The payer account id |
| uuid | The event id |
| creation_time | Time and date when this event was created in format YYYY-MM-DD hh:mm:ss |
| date | Date of the event in format YYYY-MM-DD |
| description | The description of the event |
| title | The title of the event |
| user_key | Identifier of user, who created this event |
| updating_time | The last time and date when this event was updated in format YYYY-MM-DD hh:mm:ss |

### Create MSP Billing Rules

**Summary:** Create an MSP Billing Rule

**Description:** The call is used to create a new billing rule for an MSP

> Request Example: Create MSP Billing Rule

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/msp/billing-rules' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-Token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "frequency": "recurring",
   "name": "Billing Rule 1",
   "linkedAccounts": "ALL",
   "startMonth": "2022-06",
   "endMonth": "2022-07",
   "marginType": "calc-cost",
   "margin": -50,
   "excludeAwsMarketplace": false,
   "regions": "ALL",
   "disableMinimumFee": false,
   "excludeService": false,
   "usageTypes": "ALL",
   "usageTypeOperator": "IS",
   "cloudFrontRegions": [],
   "services": ["AWS Premium Support"]
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| title | body | Event title | Yes | String |
| description | body | Event description | Yes | String |
| date | body | Event date | Yes | date |
| createdBy | body | Name of user creating the event | Yes | String |

**Responses**

> Response Example - TBD

| Code | Description |
| ---- | ----------- |
| 201 | successful Creation |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Edit MSP Billing Rules

**Summary:** Update an MSP Billing Rule

**Description:** The call is used to update an existing MSP Billing Rule

> Request Example - Edit an MSP Billing Rule

```shell
curl --location --request PUT 'https://api.mypileus.io/api/v1/msp/billing-rules/23454' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
   "frequency": "recurring",
   "name": "Billing Rule 1",
   "linkedAccounts": "ALL",
   "startMonth": "2022-06",
   "endMonth": "2022-07",
   "marginType": "calc-cost",
   "margin": -50,
   "excludeAwsMarketplace": false,
   "regions": "ALL",
   "disableMinimumFee": false,
   "excludeService": false,
   "usageTypes": "ALL",
   "usageTypeOperator": "IS",
   "cloudFrontRegions": [],
   "services": ["AWS Premium Support"]
}'
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer ID to update | Yes |  |

**Responses**

The response will contain the updated customer in the regular structure.

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Delete MSP Billing Rules

**Summary:** delete an MSP Billing Rule

**Description:** The call is used to delete an MSP Billing Rule

> Request Example: Delete MSP Billing rule

```shell
curl --location --request DELETE 'https://api.mypileus.io/api/v1/msp/billing-rules/234254' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw ''
```

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| id | path | The customer Id to remove | Yes |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful deletion |
| 400 | Invalid Parameters value |
| 500 | Server error |
