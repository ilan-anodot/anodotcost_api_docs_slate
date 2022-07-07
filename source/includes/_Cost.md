
> End Point **https://api.mypileus.io/api/v1**

## Cost Authentication
In order to start working with the Cost API, you need to perform two authentication steps. 

### Getting the Bearer Token

The following call will enable you to get an authentication token for the subsequent API calls. Please notice that the token recieved is valid for 24 hours.

You can either use basic authentication (recommended) or send the parameters in the body.

> Request example: Getting an authentication token

```shell
curl --location --request POST 'https://tokenizer.mypileus.io/prod/credentials' \
--header 'Authorization: Basic eWFyaXZAYW5vZG90LmNvbTpUd2Vyay0zMA==' \
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

After calling the Authentication, you will need to call the [Get Users](#get-users).
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

## Divisions
### Get Divisions 

**Summary:** retrieve divisions

#### HTTP Request 
`***GET*** /divisions` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |

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
curl --location --request GET 'https://api.mypileus.io/api/v1/invoices/cost-and-usage?groupby=service&startdate=2021-12-31&enddate=2022-07-01&periodGranLevel=month' \
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
| costType | query |  | No |  |
| isNetUnblended | query |  | No |  |
| isAmortized | query |  | No |  |
| isNetAmortized | query |  | No |  |
| wheres | query | conditions of type if x equals y when retrieving data | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data | No |  |

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

Filtering recommendations can be applied to the above calls using query parameters in the following manner: **filters[type]=recommendationType** where recommendationType is one of the recommendation types that are listed under the "Recommendation Types" section below. You can use the [Get Recommendation Types](get-recommendation-types) call to get the possible types.


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

**Possible Recommendation Types**

| Provider | Service | Recommendation | type |
| -------- | ------- | -------------- | ---- |
| **AWS** | EC2 | EC2 Reserved Instance | 'ri' |
| | | Operation System | 'operation-system' |
| | | OIP Unattached | 'ip-unattached' |
| | | EC2 Generation Upgrade | 'version-upgrade' |
| | | Idle EC2 instance | 'ec2-idle' |
| | | EC2 Right Sizing | 'ec2-low-cpu-usage' |
| | | EC2 Savings Plans | 'ec2-savings-plans' |
| | | Stopped EC2 instance | 'ec2-stopped-instance' |
| | | Unnecessary Data Transfer from EC2 instance | 'ec2-udt' |
| | RDS | RDS Generation Upgrade | 'rds-version-upgrade' |
| | | RDS Reserved-Instance | 'rds-ri' |
| | | RDS Type Change | 'rds-type-change' |
| | | Idle RDS Instance | 'rds-idle' |
| | DynamoDB | Idle Dynamo DB | 'dynamodb-idle' |
| | Load Balancer | Idle Load Balancer | 'idle-load-balancer' |
| | | Stop S3 Versioning | 's3-versioning' |
| | | Idle S3 | 's3-idle' |
| | EBS | Unattached EBS | 'ebs-unattached' |
| | | EBS Type Change | 'ebs-type-change' |
| | | Outdated EBS Snapshot | 'ebs-outdated-snapshot' |
| | | EBS Upgrade | 'ebs-upgrade' |
| | Redshift | Low utilization red shift cluster | 'redshift-util-low' |
| | Neptune DB | Neptune DB Idle | 'neptune-util-low' |
| | Elasticsearch | Elasticsearch Idle | 'es-util-low' |
| | NAT Gateway | NAT Gateway Idle | 'nat-gateway-util-low' |
| | ElastiCache | ElastiCache Idle | 'elasticache-util-low' |
| | DocumentDB | DocumentDB Idle | 'documentdb-util-low' |
| | Kinesis | Kinesis Idle | 'kinesis-util-low' |
| **Azure** | Disk | Disk Unattached | 'azure-disk-unattached' |
| | VM | Virtual Machine Reserved-Instance | 'azure-vm-ri' |
| | | Idle Virtual Machine | 'azure-vm-idle' |
| | Database | Datebase Reserved-Instance | 'azure-db-ri' |
| | Load Balancer | Idle Load Balancer | 'azure-idle-load-balancer' |
| | Disk | Disk Type Change | 'azure-disk-type-change' |
| | IP | IP Unattached | 'azure-ip-unattached' |
| | Cosmos DB | Cosmos DB Right Sizing | 'azure-cosmos-db-right-sizing' |
| **GCP** | VM | Idle Virtual Machine | 'gcp-vm-idle' | 
| | | Virtual Machine Right Sizing | 'gcp-vm-rightsizing' |



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
--header 'Authorization: {{bearer-token'
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

## /USAGE/ANOMALY-DETECTIONS/RULES
### ***GET*** 

**Summary:** Retrieves anomaly-detection rules for given user

**Description:** The call is used to retrieve anomaly-detection created by api-key defined user

#### HTTP Request 
`***GET*** /usage/anomaly-detections/rules` 

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

### ***PUT*** 

**Summary:** update anomaly-detection rule for given user

**Description:** The call is used to update anomaly detection rule created by api-key defined user

#### HTTP Request 
`***PUT*** /usage/anomaly-detections/rules` 

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

### ***POST*** 

**Summary:** create anomaly-detection rule for given user

**Description:** The call is used to create anomaly detection rule created by api-key defined user

#### HTTP Request 
`***POST*** /usage/anomaly-detections/rules` 

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

### ***DELETE*** 

**Summary:** update anomaly detection rule for given user

**Description:** The call is used to delete anomaly detection rule created by api-key defined user

#### HTTP Request 
`***DELETE*** /usage/anomaly-detections/rules` 

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
curl --location --request GET 'https://api.mypileus.io/api/v1/kubernetes/cost-and-usage?groupby=service&startdate=2022-01-01&enddate=2022-07-01&periodGranLevel=month' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{Bearer-token}}'
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
| costUsageType | query |  | No |  |
| usageType | query |  | No |  |
| metricType | query |  | No |  |
| isNetUnblended | query |  | No |  |
| isAmortized | query |  | No |  |
| isNetAmortized | query |  | No |  |
| wheres | query | conditions of type if x equals y when retrieving data | No |  |
| filters | query | conditions of type if x in (y,z,w) when retrieving data | No |  |
| excludeFilters | query | conditions of type if x not in (y,z,w) when retrieving data | No |  |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | successful retrieval |
| 400 | Invalid Parameters value |
| 500 | Server error |

### Get Kubernetes cost per pod

**Summary:** Retrieves kubernetes cost and usage from your cloud accounts invoice

**Description:** The call is used to retrieve kubernetes Pod cost and usage data grouped

#### HTTP Request 
`***GET*** /kubernetes/cost-and-usage/pod` 

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

