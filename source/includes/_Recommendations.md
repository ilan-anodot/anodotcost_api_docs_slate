## Recommendations

### Get Recommendations

**Summary:** Retrieves current live recommendations

**Description:** The call is used to retrieve the current live recommendations

> Request Example: Getting recommendations

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations?filters[type]=ec2-idle' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```


**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| Authorization | header |  | Yes |  |
| apikey | header |  | Yes |  |
| filters | query | Filter recommendations based on type | No |  |

Filtering recommendations can be applied to the above calls using query parameters in the following manner:**filters[type]=recommendationType** where recommendationType is one of the recommendation types supported. You can use the *Get recommendation types* call to get the possible types.</br>

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

While each recommendation has different attributes that relate to the recommendation, they all share the following common fields:

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
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
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

### DEPRECATED - Get history of recommendations

<aside class="warning">
This request is deprecated and will be replaced in the future
</aside>

> Request Example: Getting recommendations history

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/history?filters[type]=ec2-idle' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
```

**Summary:** Retrieves the history of recommendations

**Description:** The call is used to retrieve historical recommendations</br>

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

### NEW - Get history of recommendations

> Request Example: Getting recommendations history by date range

```shell
curl --location --request POST 'https://api.mypileus.io/api/v1/recommendations/history?filters[type]=ec2-idle' \
--header 'Authorization: {{bearer-token}}' \
--header 'apikey: {{account-api-key}}' \
--header 'commonParams: {"isPpApplied":false}' \
--header 'Content-Type: application/json' \
--data '{
    "startDate": "2023-07-28 00:00:00",
    "endDate": "2023-08-27 23:59:59",
    "limit":"all"
}'
```

**Summary:** Retrieves the history of recommendations

**Description:** The call is used to retrieve historical recommendations</br>
The call supports pagination and it is recommended to use it with a date range.

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| filters | query | Filter recommendations, see table below | No | Strings from list of values |
| limit | Body | Overall number of records to retrieve. Use "all" to get all responses without pagination | Yes | Number |
| pageSize | Body | Number of records to return in each response. Default and Maximum is 80 entries. | No | Number |
| startDate | Body | Start date of the queried period. No default value. | No | 'YYYY-MM-DD hh:mm:ss' |
| endDate | Body | End date of the queried period. No default value. | No | 'YYYY-MM-DD hh:mm:ss' |
| lastEvaluatedKey | Body | The primary key of the last retrieved recommendation | No | {account_id, uuid} |

<aside class="success">
To filter by recommendation type use <b>filters[type]=recommendationType</b></br>
recommendationType is one of the recommendation types returned by the <b>Get recommendation types</b> call.</br>
The type are also listed in the recommendation documenation.
</aside>

**Filters**

| String | Description | Example | 
|-|-|-|
| region | The region related to the recommendation | us-east-1 |
| resource_id | The resource id related to the recommendation | export-schemas-poc |
| account_id | The payer account id | 114780013227 |
| linked_account_id	| The linked account id that the recommendation is relevant to | 114780013227 |
| type | Recommendation type | s3-idle |
| service | The service name related to the recommendation | Amazon Simple Storage Service |
| action | The general type of action related to the recommendation | Modify/Terminate/Upgrade/Delete/Buy |
| uuid | The recommendation id | 0034f235-fce2-418b-b4d4-215e2c0d6d89 |
| recommendation_status_id | The recommendation status | Completed - 1, Excluded - 4, Completed by system - 6 |



**Response fields**

| Name | Description |
| -- | -- |
| data | The recommendation data records |
| lastPageKey | The primary key of the last retrieved recommendation {account_id, uuid} |



### Get recommendation history types

**Summary:** Retrieves current history recommendations types

**Description:** The call is used to retrieve the history recommendations types

> Request Example: Getting recommendations history types

```shell
curl --location --request GET 'https://api.mypileus.io/api/v1/recommendations/history/types' \
--header 'apikey: {{account-api-key}}' \
--header 'Authorization: {{bearer-token}}' \
--header 'commonParams: {"isPpApplied":false}'
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

